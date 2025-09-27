# Ballistic Penetration System

Advanced physics-based bullet penetration system for Godot 4 that simulates realistic projectile behavior through various materials. The system calculates damage reduction based on material thickness, hardness, and penetration depth. Uses raycasting, making it suitable for hitscan weapons.



https://github.com/user-attachments/assets/93f6dd95-38ff-462b-9120-626388da3e35



## Key Features:
- **Material-based Physics**: Different penetration properties for wood, metal, concrete, etc.
- **Thickness-aware Damage**: Damage scales with material thickness and density
- **Multi-layer Penetration**: Bullets can penetrate multiple objects in sequence
- **Performance Optimized**: Efficient collision detection and raycasting
- **Customizable**: Easy to configure material properties and penetration rules, with code that can be easily modified to add additional features like ricochet

## Performance
The system is optimized for real-time use. Below are typical performance metrics for different shot counts with 3rd-level penetration:


* **1024 shots with 3rd-level penetration** — ~45–49 ms per frame
* **512 shots with 3rd-level penetration** — ~12.9–22 ms per frame
* **8 shots with 3rd-level penetration** — ~0.37 ms per frame
* **1 shot with 3rd-level penetration** — ~0.070–0.130 ms per frame

*Note: Higher penetration levels require more calculations (for rays and other parameters). This dependency is non-linear, similar to the relationship with the number of shots per frame.*

## Usage

### Basic Setup
1. Add the penetration system script or scene (more flexibility) as an autoload/singleton in your project
2. Configure material properties in the `penetration_data` dictionary
3. Assign materials to your objects. You can do this in two ways:
   - **Via code:** `collider.set_meta(&"material", &"wood")`
   - **Via the Inspector:** Add a `meta` property named `"material"` with StringName type to your scene objects

### Basic Implementation
```gdscript
# Simple firing example
# G_PenetrationSystem - autoload/singleton
@onready var ray_cast_3d: RayCast3D = $path  # or unique name %name

func _shoot():
    var origin: Vector3 = self.global_position
    var direction: Vector3 = PenetrationSystem.get_raycast_global_direction(ray_cast_3d)
    var max_penetrations: int = 3

    G_PenetrationSystem.base_damage = 30.0
    G_PenetrationSystem.max_distance = 15.0

    # Call the penetration system
    G_PenetrationSystem.fire_bullet(origin, direction, max_penetrations, G_PenetrationSystem.penetration_data)
```

### Advanced Usage
```gdscript
# Custom material configuration
# Materials can be configured directly in PenetrationSystem or passed as parameter
# G_PenetrationSystem - autoload/singleton
var custom_materials = {
    "glass": {
        "max_thickness": 0.8,
        "damage_multiplier": 0.9,
        "penetration_cost": 0.2
    },
    "armor": {
        "max_thickness": 0.05,
        "damage_multiplier": 0.1,
        "penetration_cost": 5.0
    }
}

# Option 1: Configure directly in the singleton
G_PenetrationSystem.penetration_data = custom_materials

# Option 2: Pass as parameter when firing
func _shoot_advanced():
    var origin : Vector3 = self.global_position
    var direction : Vector3 = PenetrationSystem.get_raycast_global_direction(ray_cast_3d)
    var max_penetrations : int = 3

    G_PenetrationSystem.base_damage = 30.0
    G_PenetrationSystem.max_distance = 15.0

    # Call the penetration system
    G_PenetrationSystem.fire_bullet(origin, direction, max_penetrations, custom_materials)
```

### Damage Handling
Implement the take_damage method in your objects:
```gdscript
func take_damage(damage: float, hit_position: Vector3):
    health -= damage
    # Add visual effects, sound, etc.
    if health <= 0:
        destroy()
```

## Realistic Simulation:
- **Energy Loss Based on Material Hardness**: Bullets lose energy proportionally to the hardness of penetrated materials
- **Progressive Damage Reduction**: Damage decreases with each successive penetration
- **Smart Termination**: Automatic stopping when damage becomes negligible to optimize performance
- **Advanced Geometry Support**: Works with complex collision shapes and CSG geometry
  
## License
MIT License - see LICENSE file for details.
