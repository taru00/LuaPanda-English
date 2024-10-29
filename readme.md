**LuaPanda** is a versatile Lua code tool designed for use with Visual Studio Code, focusing on ease of use and integration with various development frameworks. Here are the key features and recent updates of LuaPanda:

### Key Features
- **Code Completion**: Offers suggestions for code snippets and completions to speed up coding.
- **Debugging Capabilities**: Supports single-step debugging, breakpoints, conditional breakpoints, and coroutine debugging.
- **Cross-Version Support**: Compatible with Lua versions 5.1 to 5.4 and frameworks like slua, xlua, and slua-unreal.
- **REPL Integration**: Allows expression monitoring and execution at breakpoints.
- **Automatic Hook Frequency Adjustment**: Enhances efficiency based on breakpoint density.
- **Multi-target Debugging**: Enables debugging of multiple Lua processes simultaneously.

### Recent Updates
- **Version 3.3.0**: Fixed issues with plugin execution errors on VSCode 1.82, improved compatibility with macOS and Windows, and resolved various bugs related to coroutine debugging and dynamic attachment during debugging.
- **Version 3.2.0**: Introduced case-insensitive code suggestions, enhanced multi-target debugging capabilities, and optimized the real device debugging process.

### Dependencies
The debugging functionality relies on **luasocket**, which is essential for environments integrated with slua and xlua. Additional libraries included with the plugin are:
- [luaparse](https://github.com/oxyc/luaparse)
- [luacheck](https://github.com/mpeterv/luacheck)
- [lua-fmt](https://github.com/trixnz/lua-fmt)

### Contribution and Support
LuaPanda encourages contributions, whether through documentation, bug fixes, or feature enhancements. Users can report issues on the [GitHub Issues page](https://github.com/Tencent/LuaPanda/issues).

For detailed documentation, including setup and usage instructions, you can refer to the [LuaPanda project documentation](./Docs/Manual/feature-introduction.md).