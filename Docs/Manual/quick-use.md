# Quick Start Guide for Debugger

### Code Assistance Tool

No configuration is required for the code assistance tool; just keep the following points in mind:

1. Open a folder containing `.lua` files in VSCode, not just a single `.lua` file.
2. Ensure correct file suffix and type association in VSCode. If your Lua file has a `.txt` or `.lua.txt` suffix, associate it with Lua to activate the plugin.
3. Avoid Lua errors in the file; these can interfere with symbol generation.

Once configured, open the Explorer tab in VSCode and expand the Outline at the bottom left. If it shows information, debugging is ready.

### Lua Debugger

**Note:** The VSCode debugging plugin mechanism allows only one active debugger per language. Disable other Lua debugging plugins and restart VSCode.

To try the debugger in different frameworks, first install the `LuaPanda` plugin from the VSCode Extension Store.

### Console Debugging

Ensure `luasocket` is installed.

- **Check**: Run `lua` in the console and `require "socket.core"`. If no error appears, `luasocket` is installed.

1. **Install VSCode Plugin**: Search for `LuaPanda` in the VSCode Store and install it.
2. **Add Debugged Files**: Create a folder named `LuaPandaTest` and add the Lua file to debug.
3. **Configuration**: Open `LuaPandaTest` in VSCode, go to the Debug tab (Shift + Cmd/Ctrl + D), click the gear icon, and select LuaPanda to auto-generate a configuration file.
   - If Lua is executable from a specific directory, update `luaPath` with the Lua executable's path.
4. **Start Debugging**: Open the file in the editor, select `LuaPanda-DebugFile` in the Debug tab, and click the green arrow to run and debug the file in the VSCode terminal.

   If there are errors, check the active window and `luaPath` configuration.

### slua Debugging

1. **Download slua Project**: Get it from [slua on GitHub](https://github.com/pangweiwei/slua).
2. **Set Up slua**: Open slua in Unity, switch platform to Android/iOS, select Slua -> All -> Make, and choose `Slua/Editor/example/Circle` scene.
3. **Add Debugger File**: Copy `LuaPanda.lua` from `/Debugger` on GitHub to `Slua/Resources/` in the project and change the file extension to `.txt`.
4. **Configure**: Open `Slua/Resources/` in VSCode, go to the Debug tab, and select LuaPanda. Change `luaFileExtension` in the configuration to `"txt"`.
5. **Start Debugging**: Add `require("LuaPanda").start("127.0.0.1",8818)` to `Slua/Resources/circle/circle.txt`. In VSCode, select `LuaPanda` in the Debug tab and click the green arrow. Unity will run, stopping automatically at the `require` statement.

### xlua Debugging

1. **Download xlua Project**: [xlua on GitHub](https://github.com/Tencent/xLua).
2. **Add Debugger File**: Copy `LuaPanda.lua` to `\XLua\Examples\07_AsyncTest\Resources` and rename it to `.lua.txt`.
3. **Configure**: Open `\XLua\Examples\07_AsyncTest\Resources` in VSCode, select LuaPanda in the Debug tab, and set `luaFileExtension` to `"lua.txt"`.
4. **Start Debugging**: Add `require("LuaPanda").start("127.0.0.1",8818)` to `async_test.lua.txt`. Select `LuaPanda` in the Debug tab and click the green arrow. Unity will run and stop at `require`.

### slua-unreal Debugging

1. **Download slua-unreal**: [slua-unreal on GitHub](https://github.com/Tencent/sluaunreal).
2. **Add Debugger File**: Copy `LuaPanda.lua` to `sluaunreal/Content/Lua/`.
3. **Configure**: Open `sluaunreal/Content` in VSCode and select LuaPanda in the Debug tab.
4. **Start Debugging**: Add `require("LuaPanda").start("127.0.0.1",8818)` in the Lua code. In VSCode, select `LuaPanda` and click the start arrow. UE4 will run, stopping at `require`.

### unlua Debugging

1. **Install luasocket**:
   - Recommended source: [luasocket on GitHub](https://github.com/diegonehab/luasocket).
   - Mac: Copy `socket` and `mime` folders to `/usr/local/lib/lua/5.3/`. Verify with `require("socket.core")`.
   - Windows: Place in a custom location and modify `package.cpath` to locate `.dll` files.

2. **Add Debugger File**: Copy `LuaPanda.lua` to `unlua/Content/Script/`, alongside `UnLua.lua`.
3. **Configure**: Open `Script` in VSCode, select LuaPanda, and set `stopOnEntry` to `false`.
4. **Start Debugging**: Add `require("LuaPanda").start("127.0.0.1",8818)` in the Lua code. Select `LuaPanda` in VSCode and start debugging; UE4 will run and stop at `require`.

### cocos2dx Debugging

*Note*: LuaPanda currently supports standard Lua VM; LuaJIT in cocos2dx may cause skip steps during tail calls.

1. Download cocos2dx and create a new project.
2. **Add Debugger File**: Copy `LuaPanda.lua` to `/src`, alongside `main.lua`.
3. **Configure**: Open `src` in VSCode and select LuaPanda.
4. **Start Debugging**: Add `require("LuaPanda").start("127.0.0.1",8818)` before `require "cocos.init"` in `main.lua`. Select `LuaPanda` in VSCode and start debugging.

### tolua Debugging

Documentation for debugging in tolua is available [here](https://github.com/Arthur-qi/LuaPandaTutorialForToLua/blob/master/LuaPandaTutorialForToLua-LuaFramework_UGUI.txt).