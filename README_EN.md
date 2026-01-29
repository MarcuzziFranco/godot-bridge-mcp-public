# Godot MCP Bridge

A bridge that allows **MCP-compatible AIs** (Cursor, Claude Desktop, Windsurf, etc.) to interact directly with the **Godot 4.x Editor** through the MCP (Model Context Protocol).

With this bridge, you can ask the AI to create scenes, add nodes, configure materials, write scripts, and much more, all from your favorite editor.

---

## ✨ Features

- 🎮 **Full editor control**: Create/open/save scenes, run the game
- 🌳 **Node manipulation**: Add, remove, duplicate, rename nodes
- 🎨 **Visual resources**: Materials, textures, sprites, lights, cameras
- 📜 **Scripts**: Read, write and assign GDScript scripts
- ⌨️ **Input Map**: Configure input actions and key bindings
- 💥 **Physics**: Create 2D/3D collision shapes
- 🔊 **Audio**: Create audio players
- 🌍 **Environment**: Configure WorldEnvironment

---

## 🔌 Compatibility

This project has **two levels of compatibility**:

### MCP Clients (Ready to use)

The MCP server uses Anthropic's **Model Context Protocol**:

| Client | Support |
|--------|---------|
| **Cursor** | ✅ Ready |
| **Claude Desktop** | ✅ Ready |
| **Windsurf** | ✅ Ready |
| **Zed** | ✅ With plugin |
| **VS Code** | ⚠️ Requires MCP extension |
| **Other IDEs** | ❌ Only if they implement MCP |

### Direct WebSocket Connection (For developers)

The Godot addon exposes a **WebSocket JSON-RPC server** independent of MCP:

```
ws://127.0.0.1:49631
```

This allows creating custom clients in any language (Python, Node.js, C#, etc.) without needing the MCP protocol.

---

## 📋 Requirements

- **Godot 4.x** (tested with 4.4.1)
- **Python 3.9+**
- **MCP Client**: Cursor, Claude Desktop, Windsurf, or other compatible

---

## 🚀 Installation

### Step 1: Install the Addon in Godot

1. Copy the `godot-addon/addons/godotbridge` folder to your Godot project:

```
your-project/
├── addons/
│   └── godotbridge/
│       ├── plugin.cfg
│       ├── godotbridge_plugin.gd
│       ├── rpc_server.gd
│       ├── rpc_handlers.gd
│       ├── serializers.gd
│       └── validators.gd
```

2. In Godot, go to **Project → Project Settings → Plugins**

3. Enable the **"GodotBridge"** plugin

4. Verify in the console that it shows:
```
[GodotBridge] Plugin initialized on port 49631
```

5. The plugin generates a token file at:
   - Windows: `%APPDATA%/Godot/app_userdata/YOUR_PROJECT/godotbridge_token.txt`
   - Linux: `~/.local/share/godot/app_userdata/YOUR_PROJECT/godotbridge_token.txt`
   - macOS: `~/Library/Application Support/Godot/app_userdata/YOUR_PROJECT/godotbridge_token.txt`

### Step 2: Install the MCP Server

1. Navigate to the server folder:

```bash
cd mcp-server
```

2. Install dependencies:

```bash
pip install websockets>=10.0
```

### Step 3: Configure Cursor

1. Open the MCP configuration in Cursor:
   - **Windows/Linux**: `%USERPROFILE%/.cursor/mcp.json` or `~/.cursor/mcp.json`
   - **macOS**: `~/.cursor/mcp.json`

2. Add the server configuration:

```json
{
  "mcpServers": {
    "godot": {
      "command": "python",
      "args": ["C:/FULL/PATH/mcp-server/src/main.py"],
      "env": {
        "GODOT_TOKEN_FILE": "C:/Users/YOUR_USER/AppData/Roaming/Godot/app_userdata/YOUR_PROJECT/godotbridge_token.txt"
      }
    }
  }
}
```

> ⚠️ **Important**: Replace the paths with the correct ones for your system.

3. Restart Cursor to load the configuration.

---

## 🎯 Usage

Once configured, you can talk to Cursor AI and ask it to interact with Godot:

### Example commands:

```
"Create a new 3D scene called Level1"

"Add a MeshInstance3D with a cube to the root node"

"Configure WASD keys for movement"

"Write a movement script for the player"

"Run the game"
```

---

## 🛠️ Available Tools

### Project
| Tool | Description |
|------|-------------|
| `godot_get_project_info` | Get project information |
| `godot_add_input_action` | Add input action with key binding |
| `godot_remove_input_action` | Remove input action |

### Editor
| Tool | Description |
|------|-------------|
| `godot_get_editor_state` | Current editor state |
| `godot_open_scene` | Open a scene |
| `godot_save_scene` | Save the current scene |

### Scenes
| Tool | Description |
|------|-------------|
| `godot_create_scene` | Create new scene |
| `godot_instance_scene` | Instance a scene |
| `godot_get_scene_tree` | Get scene tree |

### Nodes
| Tool | Description |
|------|-------------|
| `godot_list_nodes` | List child nodes |
| `godot_get_node_properties` | Get node properties |
| `godot_set_node_properties` | Set properties |
| `godot_add_node` | Add a node |
| `godot_remove_node` | Remove a node |

### Meshes and Materials
| Tool | Description |
|------|-------------|
| `godot_create_mesh` | Create mesh (Box, Sphere, etc.) |
| `godot_create_material` | Create and apply material |
| `godot_set_material` | Assign existing material |

### Sprites and Textures
| Tool | Description |
|------|-------------|
| `godot_set_sprite_texture` | Assign texture to sprite |
| `godot_set_modulate` | Change color/modulation |

### Physics
| Tool | Description |
|------|-------------|
| `godot_create_collision_shape` | Create collision shape |

### Lighting and Camera
| Tool | Description |
|------|-------------|
| `godot_create_light` | Create light (Directional, Omni, Spot, Point2D) |
| `godot_configure_camera` | Configure camera |
| `godot_create_environment` | Create WorldEnvironment |

### Audio
| Tool | Description |
|------|-------------|
| `godot_create_audio_player` | Create audio player |

### Scripts
| Tool | Description |
|------|-------------|
| `godot_read_script` | Read script content |
| `godot_write_script` | Write script |
| `godot_assign_script` | Assign script to node |

### Execution
| Tool | Description |
|------|-------------|
| `godot_run_main` | Run main scene |
| `godot_run_current` | Run current scene |
| `godot_stop` | Stop execution |

### Files
| Tool | Description |
|------|-------------|
| `godot_search_files` | Search files in project |

---

## 🖼️ Sprite Sheet MCP (New)

This project includes a **second independent MCP** for intelligent spritesheet analysis using OpenCV.

### Features
- 🔍 **Automatic frame detection** via alpha channel or background color
- 🎬 **Animation grouping** by rows, columns, or spatial clustering
- 📐 **Normalization** of sizes and pivots
- 📤 **Godot-friendly export** with pre-built commands

### Additional Installation

```bash
cd sprite-sheet-mcp
pip install -r requirements.txt
```

### Configuration in mcp.json

```json
{
  "mcpServers": {
    "godot": { ... },
    "sprite-sheet-tools": {
      "command": "python",
      "args": ["C:/PATH/sprite-sheet-mcp/src/main.py"]
    }
  }
}
```

### Workflow

1. `sprite_analyze_sheet` → Detect frames automatically
2. `sprite_group_animations` → Group into animations
3. `sprite_export_godot_json` → Generate JSON with Godot commands
4. Use the JSON with `godot_atlas_batch_create` and `godot_spriteframes_*`

See [sprite-sheet-mcp/TOOLS.md](sprite-sheet-mcp/TOOLS.md) for full documentation.

---

## 📁 Project Structure

```
godot-mcp-bridge/
├── godot-addon/
│   └── addons/
│       └── godotbridge/
│           ├── plugin.cfg           # Plugin metadata
│           ├── godotbridge_plugin.gd # Main plugin
│           ├── rpc_server.gd        # WebSocket server
│           ├── rpc_handlers.gd      # RPC handlers
│           ├── serializers.gd       # Data serialization
│           └── validators.gd        # Input validation
├── mcp-server/
│   ├── src/
│   │   └── main.py                  # Godot MCP server
│   ├── requirements.txt             # Python dependencies
│   ├── TOOLS.md                     # Tools documentation
│   └── INFORME_ERRORES.md          # Debugging history
├── sprite-sheet-mcp/                # Spritesheet analysis MCP
│   ├── src/
│   │   ├── main.py                  # MCP server
│   │   ├── detector.py              # Frame detection (OpenCV)
│   │   ├── grouper.py               # Animation grouping
│   │   ├── normalizer.py            # Normalization
│   │   └── exporter.py              # Godot export
│   ├── requirements.txt             # opencv-python, numpy, etc.
│   └── TOOLS.md                     # Documentation
└── README.md
```

---

## 🔧 Advanced Configuration

### Change Port

The default port is `49631`. To change it:

1. In Godot: **Project Settings → General → GodotBridge → Network → Port**
2. In the MCP server, add the environment variable:
```json
"env": {
  "GODOT_WS_URL": "ws://127.0.0.1:NEW_PORT"
}
```

### Debug Mode

To see detailed MCP server logs:
```json
"env": {
  "GODOT_MCP_VERBOSE": "1"
}
```

---

## 🔒 Security

- The server only accepts connections from `localhost` (127.0.0.1)
- Session token authentication is used
- RPC methods are on an allowlist

---

## ❓ Troubleshooting

### MCP server doesn't connect

1. Verify that Godot is open with the plugin active
2. Verify that the token path is correct in `mcp.json`
3. Check the Godot console for errors

### Tools don't appear in Cursor

1. Restart Cursor after modifying `mcp.json`
2. Verify that Python is in the PATH
3. Check Cursor logs (Help → Toggle Developer Tools)

### "Node not found" error

- Use node names relative to the scene root (e.g., `Player`, `Player/Sprite2D`)
- Don't include the root name if it's the same as the node (e.g., use `Player`, not `Game/Player`)

---

## 📄 License

MIT License

---

## 🤝 Contributing

Contributions are welcome! Please open an issue or pull request.
