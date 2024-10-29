# Project Introduction

## Table of Contents

### Lua Code Assistance

The Lua code assistance feature in VSCode aims to streamline the development process. Key functionalities currently implemented include:

- **Auto Completion**
- **Snippet Completion**
- **Definition Jump**
- **Find Reference**
- **Type Inference**
- **Comment Generation**
- **Linting**: Depends on [luacheck](https://github.com/mpeterv/luacheck)
- **Formatting**: Depends on [lua-fmt](https://github.com/trixnz/lua-fmt)

**Feature Demonstration: Code Suggestions and Definition Jump**

![Code Suggestions and Definition Jump](../Res/feature-introduction/codeDefAndCompleting.gif)

**Feature Demonstration: Comment Generation**

![Comment Generation](../Res/feature-introduction/generateComments.gif)

### Notes for Using Code Suggestions

1. Open a folder containing Lua files in VSCode, rather than individual Lua files.
2. Ensure that the file extension is correctly associated with the Lua type; for example, if the Lua file has a `.txt` extension, it should be associated with Lua to activate the plugin.

### Lua Code Debugging

The debugger is divided into two parts: the [VSCode extension] and the [debugger core] that runs within the Lua process. They communicate over TCP, which allows for real-device and remote debugging; thus, your project must include luasocket.

#### Debugger Architecture

![Debugger Architecture](../Res/feature-introduction/debug-arch2.png)

*Image source: [Visual Studio Code API](https://code.visualstudio.com/api/extension-guides/debugger-extension)*

LuaPanda's debugger features a dual architecture of Lua and C, balancing the efficiency of C with the flexibility of Lua. The C extension library loads automatically based on the context, and users can continue debugging even if it fails to load.

### Debugging Use Cases

- **Dynamic Distribution**: Avoids debugging issues after game packaging, suitable for post-release use.
- **C Modules**: Ideal for efficient debugging during development.

#### Debugging Interface in VSCode

![Debugging Interface](../Res/feature-introduction/debugui.png)

## Features

### Multi-Platform Support

- **Mac**: Console + Lua 5.1
![Debug on Console](../Res/feature-introduction/debugon-console.png)

- **Windows**: slua-unreal + Lua 5.3
![Debug on SLua UE](../Res/feature-introduction/debugon-slua-ue.png)

### Display Metatables and Upvalues

Shows the number of members in a table and the metatable, as well as upvalues in functions.
![Show Metatable](../Res/feature-introduction/show-metatable.png)

### Expression Monitoring and Debug Console

Monitor expressions in the variable watch area.

![Expression Monitoring](../Res/feature-introduction/REPL-watch.png)

Use the debug console at breakpoints to input expressions, execute functions, or observe variable values.

![Debug Console](../Res/feature-introduction/debug-console.png)

### Attach Mode Support

This mode allows for running the Lua project first and then attaching the debugger to it later.

![Attach Mode](../Res/feature-introduction/attach_mode.GIF)

### Conditional Breakpoints and Log Points

Set regular breakpoints, conditional breakpoints, or log points by right-clicking in the line number area.

![Add Condition Breakpoint](../Res/feature-introduction/add_condition_bk.png)

For example, entering a condition like `a == 2` will evaluate the expression. The result of `nil` or `false` is treated as false, while others are true.

Log points will print logs upon execution, with output shown in the `DebugConsole - OUTPUT - LuaPanda Debugger`.

![Log Output](../Res/feature-introduction/print_log.png)

### Variable Assignment

Users can modify variable values at breakpoints or assign values through the debug console.

![Set Variable Value](../Res/feature-introduction/set-var-value.gif)

### Single File Debugging

Easily debug individual Lua files within your project.

![Debug File](../Res/debug-file.GIF)