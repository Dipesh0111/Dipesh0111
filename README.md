import bpy

def create_fuselage():
    bpy.ops.mesh.primitive_cylinder_add(
        radius=1.975,  # Diameter / 2
        depth=37.57,
        vertices=64,
        location=(0, 0, 18.785)  # Half of fuselage length
    )
    bpy.context.object.name = "Fuselage"

def create_wing():
    bpy.ops.mesh.primitive_plane_add(size=1, location=(0, 0, 0))
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={"value":(0, 0, 0.5)})
    bpy.ops.transform.resize(value=(17.9, 2.5, 1))
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.context.object.name = "Wing"
    add_control_surfaces()

def add_control_surfaces():
    bpy.ops.mesh.primitive_plane_add(size=1, location=(9, 0, 0))
    aileron = bpy.context.object
    aileron.scale = (4.5, 1.0, 0.1)
    aileron.name = "Aileron"
    
    bpy.ops.mesh.primitive_plane_add(size=1, location=(9, 0, 0))
    flap = bpy.context.object
    flap.scale = (4.5, 1.0, 0.1)
    flap.name = "Flap"

def create_tail():
    bpy.ops.mesh.primitive_cylinder_add(
        radius=2.5,
        depth=7.5,
        location=(0, -20, 3.75)
    )
    bpy.context.object.name = "Vertical Stabilizer"
    
    bpy.ops.mesh.primitive_cylinder_add(
        radius=2.5,
        depth=0.5,
        location=(0, -20, -0.25)
    )
    bpy.context.object.name = "Horizontal Stabilizer"

def create_engine_nacelles():
    for pos in [10.0, 15.0]:
        bpy.ops.mesh.primitive_cylinder_add(
            radius=0.6,
            depth=2.5,
            location=(pos, 0, 0)
        )
        bpy.context.object.name = f"Engine Nacelle {pos}"

def create_landing_gear():
    bpy.ops.mesh.primitive_cylinder_add(
        radius=0.25,
        depth=2.0,
        location=(10, 0, -1.0)
    )
    bpy.context.object.name = "Main Landing Gear"
    
    bpy.ops.mesh.primitive_cylinder_add(
        radius=0.25,
        depth=2.0,
        location=(0, 0, -1.0)
    )
    bpy.context.object.name = "Nose Landing Gear"

def create_a320_model():
    bpy.ops.object.select_all(action='DESELECT')
    bpy.ops.object.select_by_type(type='MESH')
    bpy.ops.object.delete()
    
    create_fuselage()
    create_wing()
    create_tail()
    create_engine_nacelles()
    create_landing_gear()
    
    # Combine all parts
    bpy.ops.object.select_all(action='DESELECT')
    for obj_name in ["Fuselage", "Wing", "Vertical Stabilizer", "Horizontal Stabilizer", "Engine Nacelle 10.0", "Engine Nacelle 15.0", "Main Landing Gear", "Nose Landing Gear"]:
        bpy.data.objects[obj_name].select_set(True)
    
    bpy.context.view_layer.objects.active = bpy.data.objects["Fuselage"]
    bpy.ops.object.join()
    bpy.ops.object.transform_apply(location=True, rotation=True, scale=True)
    bpy.ops.object.shade_smooth()

create_a320_model()

<!---
Dipesh0111/Dipesh0111 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
