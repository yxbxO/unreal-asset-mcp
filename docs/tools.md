# Tools

## Getting started

| Tool | What it does |
|---|---|
| `initialize_game` | Loads a game. Give it the install folder; for UE5 it builds the `.usmap` from the exe. |
| `get_status` | Shows which game is loaded, its engine version and usmap. |
| `validate_usmap` | Checks that the usmap really fits this game. |
| `get_guide` | Tells the assistant how to use these tools well. |
| `unload_game` | Frees the memory. |
| `list_supported_versions` | Lists engine versions you can force if detection gets it wrong. |

## Finding things

| Tool | What it does |
|---|---|
| `search_files` | Finds packages by name, or by class, parent class or tag through the asset registry. |
| `list_files` | Lists the packages in a folder. |
| `find_row` | Finds which DataTables hold a given row name. |
| `gameplay_tags` | Lists gameplay tags, and finds where one is used. |
| `text_lookup` | Turns a localized text key into its string, or finds where a string is used. |

## Reading assets

| Tool | What it does |
|---|---|
| `read_asset` | Reads a package: its objects and their property values, such as a Blueprint's defaults. |
| `read_datatable_rows` | Reads DataTable rows, with filters like "Damage > 50". |
| `read_curve` | Reads a curve and evaluates it at given points. |
| `read_config` | Reads the game's `.ini` config files. |
| `read_input` | Reads Enhanced Input key bindings. |
| `read_widget_tree` | Reads a UI widget's layout and the text it shows. |
| `read_gas` | Reads Gameplay Ability System effects, abilities and attribute sets. |
| `read_level_actors` | Lists what a level places, and where. |
| `find_actor_placements` | Finds every level that places a given kind of actor. |
| `read_savegame` | Reads a `.sav` save file. |

## Blueprint logic

| Tool | What it does |
|---|---|
| `read_class` | Shows a class's properties, functions or components. Works for native engine classes too. |
| `read_function` | Shows what a Blueprint function does, as readable steps. |
| `search_bytecode` | Finds every Blueprint that reads, writes or calls something. |
| `find_call_sites` | Finds every Blueprint call of a function, with the arguments passed. |
| `find_bytecode_refs` | Finds the functions in one package that touch a property. |

## Native code (needs the game's exe)

The `ue_` prefix marks tools that read the game's compiled C++ code in the exe, rather than its packages.

| Tool | What it does |
|---|---|
| `ue_exec_status` | Says whether a native function really does work, or is empty or a stub. |
| `ue_native_fn` | Finds a native function's code in the exe. |
| `ue_disasm` | Shows that code as annotated assembly, with field and function names filled in. |
| `ue_callers` | Finds what calls a native function. |

## Encrypted games

| Tool | What it does |
|---|---|
| `find_aes_key` | Searches the exe for the encryption key and tests it. Loading the game already does this. |
| `submit_aes_key` | Adds a key you already have. |
