# Implementation Plan for Some Features

## Overview

LuaPanda is a Lua code debugger based on VSCode, mainly consisting of three parts.

![](../Res/debugger-principle/mode_pic.png)

- The debugger frontend is a VSCode extension that notifies VSCode to display information and facilitates data exchange.

- The debugger backend is a Lua module responsible for implementing the main debugging functionalities, developed using Lua.

- Additionally, there is an optional C-hook extension module designed to enhance debugging efficiency. This module relies on the debugger and cannot be used independently.

## Debugger Working Principle

The Lua language provides basic debugging interfaces. The debugger's role is to analyze, consolidate, and control code execution based on debug information obtained from the Lua virtual machine.

Key interfaces that the debugger relies on include:

```
debug.sethook ([thread,] hook, mask [, count])
```

The Lua virtual machine accepts a hook function set by the debugger, which is called when the code execution reaches specified states (mask: call, line, return), accompanied by the following information:

```
typedef struct lua_Debug {
  int event;
  const char *name;           /* (n) Function name */
  const char *namewhat;       /* (n) Explanation of the name field. Can be "global", "local", "method", "field", "upvalue", or "" (empty string). Depends on how the function is called */
  const char *what;           /* (S) Type lua/c/main */
  const char *source;         /* (S) Path */
  int currentline;            /* (l) Current line */
  int linedefined;            /* (S) Function definition line */
  int lastlinedefined;        /* (S) Function end line */
  unsigned char nups;         /* (u) Number of upvalues */
  unsigned char nparams;      /* (u) Number of parameters */
  char short_src[LUA_IDSIZE]; /* (S) A "printable version" of the source */
  /* private part */
  other fields
} lua_Debug;
```

The hook function is implemented by the debugger, where breakpoint hit judgment, single-step operations, etc., are processed.

If a breakpoint is hit, the debugger blocks the execution of the debugged Lua code and retrieves variable and stack information to send to the frontend for display.

## Hook State Classification

The goal of the hook state classification is to reduce the number of hooks without affecting judgment accuracy.

Lua supports three hook states: lcr (line, call, return). Initially, the plan was to use all three states for hooking.

![](../Res/debugger-principle/state-pic.png)

The debugger has multiple execution states, and user-perceived stuttering occurs during the Run state. As shown in the state diagram, the work under the Run state hook is to determine if a breakpoint is hit.

If the user has not set breakpoints, there is no need for breakpoint judgment; the system should just periodically receive messages (like new breakpoints added by the user).

If the user has set breakpoints but the current executing function has none, the hook is only needed during function transitions (call, return), adjusting the hook state based on whether there are breakpoints in the new function.

Based on this reasoning, we established a hook state machine, categorizing hooks into the following states:

![](../Res/debugger-principle/state-table.png)

Every time the user sets a breakpoint or a function call or return occurs, a recheck is needed to switch to the corresponding state according to the breakpoint conditions to avoid missing breakpoints.

## Attach Mode

The lack of multithreading in Lua poses challenges for network connections, as synchronous waiting at network requests can freeze the interface.

The initial idea was that when the debugger is referenced, it would attempt a connection to VSCode upon startup. If it fails, no further connections would be made. This approach requires establishing a connection at Lua startup, but how to implement an attach mode for the debugger?

Since pure Lua lacks asynchronous capabilities and timers, requests must be sent continuously during runtime. We designed the hook to attempt a connection after a specified number of calls:

```lua
debug.sethook(this.debug_hook, "r", 1000000);
```

Due to performance costs associated with connection attempts and time considerations, no reconnection will be attempted if less than 1 second has passed since the last attempt. 

This method allows the debugger to be attached for debugging at any time during runtime. However, it has the drawback of attempting reconnections every second, incurring performance overhead.

To address performance concerns, we also provided a setting for users to choose whether to use attach mode.

## Executing Expressions and Variable Assignments

When debugging at a breakpoint, users can input variables or expressions to observe execution results, as shown below:

![](../Res/debugger-principle/dostring-pic.png)

`loc_num` is a local variable that the user wishes to check the value of `loc_num + 1` at the breakpoint.

Initially, the approach was to use `loadstring` to execute the string and obtain a closure. However, since the closure’s environment is `_G`, the value of `loc_num` could not be accessed, resulting in execution errors and unexpected results.

To solve this, I realized it was necessary to obtain the user space's environment and set it in the execution environment of `loadstring`. Yet, I encountered another challenge: if the user did not actively set an env for the function, the retrieved environment would still be `_G`.

Ultimately, we adopted a method using the `__index` function in the metatable for dynamic lookup.

First, we call `debugger_loadString` (5.1 uses `loadstring`, while 5.3 uses `load`) to convert the string provided by the user into a closure:

```lua
local f = debugger_loadString(expression);
```

Then, we set the environment for the closure:

```lua
if type(f) == "function" then
    if _VERSION == "Lua 5.1" then
        setfenv(f, env);
    else
        debug.setupvalue(f, 1, env);
    end
end
```

The `env` table's `__index` is a function that can retrieve the local variables and upvalues corresponding to the user's call stack level:

```lua
local env = setmetatable({}, {
    __index = function(tab1, varName)
        local ret = this.getWatchedVariable(varName, _G.VSCodeLuaDebug.curStackId, false);
        return ret;
    end
});
```
