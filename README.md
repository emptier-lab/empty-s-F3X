# F3X API

A Lua library for programmatic control of Building Tools by F3X in Roblox.

## Overview

This API provides a wrapper around F3X building tools, enabling developers to create, manipulate, and manage 3D objects programmatically in Roblox environments.

## Installation

1. Require the module in your script:
```lua
local F3X = require(path.to.F3XModule)
```

## Core Functions

### F3X.Object(object)
Wraps an existing Roblox instance with F3X functionality.

```lua
local part = workspace.SomePart
local f3xPart = F3X.Object(part)
```

### F3X.Edit(objects, properties)
Modifies properties of one or more objects.

```lua
F3X.Edit(part, {
    Color = Color3.fromRGB(255, 0, 0),
    Transparency = 0.5
})
```

### F3X.new(className, parent)
Creates a new part with the specified class name.

```lua
local newPart = F3X.new("Part", workspace)
newPart.Size = Vector3.new(4, 1, 2)
```

### F3X:Clone(object)
Clones an object and places it in the workspace.

```lua
local clonedPart = F3X:Clone(originalPart)
```

### F3X:FakeClone(object)
Creates a visually identical copy without using F3X systems.

```lua
local fakePart = F3X:FakeClone(originalPart)
```

## Object Methods

When you wrap an object with `F3X.Object()`, the returned proxy has these methods:

### For BaseParts

#### Adding Features
```lua
local mesh = part:AddMesh()
local decal = part:AddDecal()
local texture = part:AddTexture()
local smoke = part:AddSmoke()
local fire = part:AddFire()
local sparkles = part:AddSparkles()
local spotlight = part:AddSpotLight()
local pointLight = part:AddPointLight()
local surfaceLight = part:AddSurfaceLight()
```

#### Joints and Welds
```lua
-- Weld to specific parts
part:WeldTo(otherPart)
part:WeldTo({part1, part2, part3})

-- Auto-weld to touching parts
part:MakeJoints()

-- Remove all welds
part:BreakJoints()
```

#### Destruction
```lua
part:Destroy()  -- or part:Remove()
```

## Property Handling

F3X objects support direct property access and modification:

```lua
-- Read properties
local color = f3xPart.Color
local size = f3xPart.Size

-- Write properties
f3xPart.Transparency = 0.5
f3xPart.Anchored = true
```

## Supported Properties

### BaseParts
- `Color`
- `Material`
- `Reflectance`
- `Transparency`
- `Anchored`
- `CanCollide`
- `Shape`
- `Size`
- `CFrame`
- `BackSurface`
- `BottomSurface`
- `FrontSurface`
- `LeftSurface`
- `RightSurface`
- `TopSurface`

### SpecialMesh
- `MeshType`
- `Scale`
- `Offset`
- `MeshId`
- `TextureId`
- `VertexColor`

### Decals and Textures
- `Face`
- `Texture`
- `Transparency`
- `StudsPerTileU` (Texture only)
- `StudsPerTileV` (Texture only)

### Decorations
- **Smoke**: `Color`, `Opacity`, `Size`, `RiseVelocity`
- **Fire**: `Color`, `SecondaryColor`, `Heat`, `Size`
- **Sparkles**: `SparkleColor`

### Lighting
- **SpotLight/SurfaceLight**: `Color`, `Range`, `Brightness`, `Angle`, `Face`, `Shadows`
- **PointLight**: `Color`, `Range`, `Brightness`, `Shadows`

## Examples

### Creating and Positioning a Part
```lua
local part = F3X.new("Part", workspace)
part.Size = Vector3.new(10, 1, 10)
part.CFrame = CFrame.new(0, 10, 0)
part.Color = Color3.fromRGB(45, 180, 45)
part.Material = Enum.Material.Grass
part.Anchored = true
```

### Building a Simple Structure
```lua
-- Create floor
local floor = F3X.new("Part", workspace)
floor.Size = Vector3.new(20, 1, 20)
floor.CFrame = CFrame.new(0, 0, 0)
floor.Anchored = true

-- Create walls
local function createWall(position)
    local wall = F3X.new("Part", workspace)
    wall.Size = Vector3.new(20, 10, 1)
    wall.CFrame = position
    wall.Anchored = true
    return wall
end

local walls = {
    createWall(CFrame.new(0, 5, 10)),
    createWall(CFrame.new(0, 5, -10)),
    createWall(CFrame.new(10, 5, 0) * CFrame.Angles(0, math.rad(90), 0)),
    createWall(CFrame.new(-10, 5, 0) * CFrame.Angles(0, math.rad(90), 0))
}

-- Add a roof
local roof = F3X.new("Part", workspace)
roof.Size = Vector3.new(20, 1, 20)
roof.CFrame = CFrame.new(0, 10, 0)
roof.Anchored = true
```

### Decorating Objects
```lua
local pillar = F3X.new("Part", workspace)
pillar.Size = Vector3.new(2, 15, 2)
pillar.CFrame = CFrame.new(5, 7.5, 5)
pillar.Anchored = true

-- Add effects
local fire = pillar:AddFire()
fire.Heat = 25
fire.Size = 5

local light = pillar:AddPointLight()
light.Color = Color3.fromRGB(255, 170, 0)
light.Range = 15
light.Brightness = 2
```

## Limitations

- All operations are performed through the F3X ServerEndpoint
- Some complex operations may require administrative access in the game
- Performance may degrade when manipulating many objects simultaneously

## Advanced Usage

### Batch Operations
For efficiency when modifying multiple objects:

```lua
local parts = {part1, part2, part3, part4}
F3X.Edit(parts, {
    Material = Enum.Material.Neon,
    Transparency = 0.3
})
```

## License

This API is designed for use with Building Tools by F3X. Review the F3X license terms before using this in production environments.
