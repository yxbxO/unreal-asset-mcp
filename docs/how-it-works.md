# How it works

## Reading the game's packages

Unreal games ship their content in `.pak` files, or in `.utoc`/`.ucas` pairs. The tool opens them with [CUE4Parse](https://github.com/FabianFG/CUE4Parse), the library behind FModel, and turns what's inside (Blueprints, DataTables, levels, widgets, curves, config) into JSON your assistant can read.

Blueprint logic is compiled to bytecode. The tool decodes it into readable steps, so the assistant can follow what a function does and where a value is set.

## Usmap from the exe

UE5 games store most properties without their names or types, to save space. Reading them needs a mapping file, a `.usmap`, that describes every class. Normally you'd make one by injecting a dumper into the running game.

This tool builds it from the game's exe instead, without running anything. The engine compiles its class descriptions into tables inside the exe. The tool finds those tables and works out which record layout this engine version uses, by scoring each known layout against the data. It only accepts a layout when exactly one fits cleanly. Then it writes the usmap and checks it by reading sample packages.

If the exe doesn't match any known layout, the tool refuses and says why, rather than producing a usmap that silently misreads values. You can always supply your own `.usmap` instead.

| Engine | Usmap from the exe |
|---|---|
| UE 5.0 – 5.8 | Yes, automatically |
| UE 4.x | Not needed: UE4 packages carry their own property names |

## Native classes and code

The same tables give the size of every native class, the offset of each of its properties, and its function list. That's how `read_class` can show you the layout of `Pawn` or `Character` (UE 4.25 – 5.8).

For native functions, the tool locates each function's code in the exe and classifies it:

| Status | Meaning | If you want to hook it |
|---|---|---|
| **REAL** | It calls a separate body that does the work. | Hook the body (`bodyRva`). |
| **INLINE** | The work is compiled directly into the function's entry point. | Hook the entry point (the thunk). |
| **EMPTY_BODY**, **STUB**, **NO_EFFECT**, **RETURNS_CONSTANT** | It does nothing, or only returns a fixed value. | Hooking it changes nothing; look for the code that does the work elsewhere. |

It can also disassemble a function with names filled in, and find its callers.

## Engine version detection

The tool works out the engine version from the exe's own version string. If that's missing, it falls back to the package format and the engine paths compiled into the exe. You can override it with `ueVersion` if needed.
