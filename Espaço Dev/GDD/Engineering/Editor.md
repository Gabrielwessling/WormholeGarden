#### 1. Editor Scene Structure
	- EditorManager: Main script that manages editor state, UI, and tools.
	- Map: The current map being edited (ScriptableObject or runtime instance).
	- GridVisualizer: Draws the tile grid and handles selection.
	- Tool System: For switching between paint, entity placement, etc.
	- UI: Panels for tools, properties, save/load, etc.
### 2. Core Features & How to Approach Each
	1. A. Create Maps (new scene + grid)
		1. UI Button: “New Map”
		2. Action: Instantiate a new Map object (ScriptableObject or runtime class), create a grid of Tile objects.
		3. Optional: Prompt for map size, name, etc.
	2. B. Paint Tile Heights
		1. Tool: “Height Brush”
		2. Action: On mouse over tile, adjust Tile.vertexHeights[] (show preview, update mesh/visual).
		3. UI: Slider for brush strength/height.
	3. C. Place Entities (walls, furniture, lights, etc.)
		1. Tool: “Entity Placement”
		2. Action: Select entity type from UI, click grid to place at anchor (center, border, intersection).
		3. Prefab Palette: List of available entity prefabs.
	4. D. Place Transition Events (teleporters)
		1. Tool: “Teleporter Tool”
		2. Action: Click to place, set destination map/position in UI.
	5. E. Edit Map Metadata & Properties
		1. Panel: “Map Properties”
		2. Fields: Name, environmental settings, flags, etc.
		3. Bind: Directly to your Map’s metadata fields.
	6. F. Save/Load/Export
		1. Buttons: “Save”, “Load”, “Export”
		2. Action: Serialize Map data to file (JSON, ScriptableObject, etc.), load from file, or export for game use.
### 3. Next Steps: What to Build First?
	Grid Visualizer: Draw the tile grid, allow tile selection.
	New Map UI: Button to create a new map and set its size.
	Height Painting Tool: Click/drag to raise/lower tile heights.
	Once you have these, you can add entity placement, metadata editing, and save/load.