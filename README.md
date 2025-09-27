# Ballistic Penetration System

Advanced physics-based bullet penetration system for Godot 4 that simulates realistic projectile behavior through various materials. The system calculates damage reduction based on material thickness, hardness, and penetration depth.

## Key Features:
- **Material-based Physics**: Different penetration properties for wood, metal, concrete, etc.
- **Thickness-aware Damage**: Damage scales with material thickness and density
- **Multi-layer Penetration**: Bullets can penetrate multiple objects in sequence
- **Performance Optimized**: Efficient collision detection and raycasting
- **Customizable**: Easy to configure material properties and penetration rules

## Realistic Simulation:
- Bullets lose energy based on material hardness
- Damage decreases with each penetration
- Automatic stopping when damage becomes negligible
- Support for complex collision shapes and CSG geometry