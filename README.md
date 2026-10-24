# Unreal Asset MCP

Like FModel, but your AI assistant does the browsing. Ask about an Unreal Engine game's Blueprints, default values, DataTables, levels and native code, and get answers taken straight from the game's files.

Offline and read-only: it never runs, injects into or changes the game.

> **Early release.** It is tested on a handful of games. If it reads something wrong, please [open an issue](../../issues).

## What it looks like

> **You:** Load the game at `D:\Games\MyGame` and tell me where sprint speed is set.
>
> **Assistant:** It's in `BP_ThirdPersonCharacter`. Its two `IA_Sprint` input events set `CharacterMovement.MaxWalkSpeed`: one to the variable `speed sprint`, which defaults to **800**, and the other back to **500**.

## What you can ask

- *"What Blueprints does this game have for enemies?"*
- *"Show me the player character's default values."*
- *"Where is the sprint speed set, and what changes it?"*
- *"List the weapons DataTable rows where Damage is over 50."*
- *"What are the key bindings?"*
- *"What does `Character.Jump` actually do in the exe? Who calls it?"*
- *"What's the memory layout of `Pawn`?"*
- *"Which actors are placed in the first level?"*

## Install

You need Windows 10/11 (x64) and an AI client that supports MCP servers, such as Claude Code, Claude Desktop, Cursor or VS Code with Copilot.

1. Download `unreal-asset-mcp-vX.Y.Z-win-x64.zip` from [Releases](../../releases).
2. Right-click the zip, open **Properties**, tick **Unblock** and click OK (the exe isn't code-signed).
3. Unzip it somewhere permanent, such as `C:\Tools\unreal-asset-mcp\`.
4. Add it to your AI client:

**Claude Code**
```sh
claude mcp add unreal-assets --scope user -- "C:\Tools\unreal-asset-mcp\unreal-asset-mcp.exe"
```

**Claude Desktop**: in *Settings → Developer → Edit Config*, add:
```json
{
  "mcpServers": {
    "unreal-assets": {
      "command": "C:\\Tools\\unreal-asset-mcp\\unreal-asset-mcp.exe"
    }
  }
}
```

**Cursor**: the same block, in `.cursor/mcp.json`.

**VS Code (Copilot)**: in `.vscode/mcp.json`:
```json
{
  "servers": {
    "unreal-assets": {
      "type": "stdio",
      "command": "C:\\Tools\\unreal-asset-mcp\\unreal-asset-mcp.exe"
    }
  }
}
```

Restart the client, then ask: *"Load the game at …"*, giving the game's install folder.

To update, close your AI client before replacing the exe.

## Supported games

| Engine | What works |
|---|---|
| UE 5.0 – 5.8 | Assets, Blueprints, native class layouts and native code. The `.usmap` file UE5 games need is built from the game's exe automatically. |
| UE 4.25 – 4.27 | Assets, Blueprints, native class layouts and native code. |
| Older UE4 | Assets and Blueprints should read, but this is untested. Native class layouts aren't available. |
| Encrypted games | The AES key is often found in the exe automatically; otherwise you can supply it. |

Games with heavy obfuscation or custom engine forks may not read cleanly. When the tool isn't sure, it says so rather than guessing.

## Good to know

- **It reads what shipped, not what's running.** Values that change during play (your current health, for example) aren't in the files, and "this function does work" means its code does something, not how it behaves in game.
- **No network calls.** Your files stay on your machine.
- **The exe is ~95 MB** because it carries its own .NET runtime.

## More

- [How it works](docs/how-it-works.md)
- [All tools](docs/tools.md)
- [Releases](../../releases)

## License

Licensed under [MIT](LICENSE). Built on [CUE4Parse](https://github.com/FabianFG/CUE4Parse); licenses of the third-party libraries used are listed in [NOTICE](NOTICE).
