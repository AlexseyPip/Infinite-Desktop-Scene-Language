# IDSL - Infinite Desktop Scene Language

IDSL is a declarative language for describing infinite, zoomable scenes for desktop environments. Instead of storing gigabytes of static textures, IDSL describes scenes mathematically - planets, continents, cities, and interactive workspaces that appear and disappear based on zoom level and camera position.

## Philosophy

IDSL turns your desktop background from a static image into a living, interactive world. The language is designed to be:

- **Lightweight** - Parser and runtime under 35MB
- **Modular** - Can be used outside of FuturaOS
- **Human-readable** - Simple syntax with comments
- **Powerful** - Hierarchical scenes, animations, system reactivity

## Features

### Infinite Zoom Without Quality Loss
Objects are defined mathematically (points, polygons, curves). Zoom in 1000x and everything stays sharp - no pixelation.

### Hierarchical Worlds
Build complex scenes with parent-child relationships: Universe → Galaxy → Planet → Continent → City → Workspace

### Zoom-Dependent Visibility
Objects appear and disappear based on zoom level. At low zoom you see the big picture, at high zoom you see details.

```
body Earth {
    min_zoom: 0.3    # Visible when zoom >= 0.3
}

node Moscow {
    min_zoom: 1.5    # Only visible when zoomed in close
}
```

### Animations
Simple animations without complex code: rotation, orbital motion, pulsing, and twinkling stars.

```
body Sun {
    rotation_speed: 0.0005 rad/s
    pulse_speed: 0.1 Hz
}

body Earth {
    orbit: Sun
    orbit_radius: 2800
    orbit_speed: 0.002 rad/s
}
```

### System Reactivity
Colors and sizes can respond to system state: CPU load, memory usage, time of day.

```
body CPU_Indicator {
    color_system: "cpu_load"
    color_gradient: [
        { value: 0, color: #22c55e },
        { value: 50, color: #eab308 },
        { value: 80, color: #ef4444 }
    ]
}
```

### Workspace Integration
Define interactive zones where windows automatically snap and organize.

```
node Dev_Cluster {
    workspace: "development"
    layout: "tiling"
    snap_radius: 80
    color: #3b82f6
}
```

## Syntax Examples

### Minimal Scene
```
scene {
    bg_color: #0f172a
}

grid {
    spacing: 50
    color: #1e293b
}

body Home {
    pos: 0, 0
    radius: 4
    color: #3b82f6
}
```

### Solar System with Workspaces
```
scene {
    name: "Solar System"
    bg_color: #000008
    default_zoom_min: 0.05
    default_zoom_max: 20.0
}

grid DeepSpace {
    spacing: 2000
    color: #1a1a2e
    min_zoom: 0.0
    max_zoom: 0.8
}

body Sun {
    pos: 0, 0
    radius: 400
    color: #fbbf24
    glow: 80
    glow_color: #fcd34d
    rotation_speed: 0.0005 rad/s
}

body Earth {
    parent: Sun
    orbit_radius: 2800
    orbit_speed: 0.002 rad/s
    radius: 180
    color: #0f172a
    glow: 15
    glow_color: #38bdf8
    min_zoom: 0.2
    label: "Earth"
    label_offset: 0, -220
}

polygon Eurasia {
    parent: Earth
    color: #1e3a5f
    border_color: #3b82f6
    border_width: 2
    min_zoom: 0.8
    points: [
        (-140, 200), (-80, 240), (20, 260),
        (100, 240), (180, 200), (240, 120),
        (260, 40), (200, -40), (120, -60),
        (40, -20), (-40, 20), (-100, 80)
    ]
}

node Moscow {
    parent: Eurasia
    pos: 20, 110
    type: point
    radius: 12
    color: #ef4444
    glow: 8
    min_zoom: 1.5
    label: "Dev Cluster"
    workspace: "development"
    layout: "tiling"
    snap_radius: 80
}
```

### Cyberpunk City
```
scene {
    bg_color: #0a0015
}

grid {
    spacing: 100
    color: #7c3aed
    opacity: 0.15
}

polygon City {
    points: [(-800, -600), (800, -600), (800, 600), (-800, 600)]
    color: #1a0a2e
    border_color: #7c3aed
    border_width: 2
    glow: 30
    glow_color: #8b5cf6
}

path NeonLines {
    points: [(-500, -300), (-200, -100), (100, -400), (400, 200)]
    color: #ec4899
    width: 3
    glow: 20
    glow_color: #f472b6
    min_zoom: 0.5
}

node Workspace {
    pos: 200, 100
    radius: 30
    color: #06b6d4
    glow: 25
    workspace: "neon-dev"
    min_zoom: 0.8
}
```

## Integration with Window Managers

IDSL provides APIs for window managers to read scene data:

- Get workspace at current camera position
- Get layout preferences for windows
- Query snap zones
- React to scene changes

```c
#include <idsl/idsl.h>

idsl_scene_t* scene = idsl_load_scene("scene.idsl");
idsl_workspace_t* ws = idsl_get_workspace(scene, camera_x, camera_y);
if (ws) {
    printf("Workspace: %s, Layout: %s\n", ws->name, ws->layout);
}
```

## Installation

```bash
# Basic build
make

# Install system-wide
sudo make install

# Build with GPU support
make GPU=1
```

## Command Line Tools

- `idsl-validate scene.idsl` - Validate syntax
- `idsl-preview scene.idsl` - Preview scene in a window
- `idsl-compile scene.idsl scene.idslb` - Compile to binary format
- `idsl-watch scene.idsl` - Watch for changes and auto-render

## Performance

- Scene load time: < 10ms for 1000 objects
- CPU renderer: ~30fps at 1080p
- GPU renderer: 60fps+ at 4K
- Memory usage: ~2MB for 10,000 objects

## License

MIT License - See LICENSE file for details.

## Contributing

Contributions welcome! Areas for contribution:

- Parser improvements
- New renderer backends (Vulkan, DirectX)
- More built-in shapes
- Performance optimizations
- Documentation

## Authors
Alexsey Pipichin (Алексей Пипичин)

## Links

- Website: https://futuraos.ru
- GitHub: https://github.com/alexseypip/idsl
- Email: alexsey.websites.work@gmail.com
