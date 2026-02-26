Introducción

En esta práctica se desarrolla un escenario procedural en Blender utilizando Python mediante la API bpy.

El proyecto consiste en generar automáticamente un pasillo compuesto por un tramo recto y un tramo curvo. Además, se implementa la animación de una cámara que recorre el camino de manera fluida, simulando un desplazamiento en primera persona.

La construcción del tramo curvo se basa en cálculos trigonométricos utilizando funciones seno y coseno, lo que permite posicionar los objetos siguiendo una trayectoria circular precisa.

Este proyecto integra conceptos de:

Generación procedural

Transformaciones geométricas

Trigonometría aplicada

Animación por fotogramas clave (Keyframes)

Iluminación 3D

Código en Python
import bpy
import random
import math 

def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.use_nodes = True
    bsdf = mat.node_tree.nodes["Principled BSDF"]
    bsdf.inputs['Base Color'].default_value = (*color_rgb, 1.0)
    bsdf.inputs['Roughness'].default_value = 0.7
    return mat

def generar_escenario():
    # Limpiar escena
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()

    # Materiales
    mat_pared_a = crear_material("ParedOscura", (0.1, 0.1, 0.1))
    mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0))

    # Parámetros
    largo_pasillo = 10
    ancho_pasillo = 4
    radio_curva = 12 

    # Tramo recto
    for i in range(largo_pasillo):
        bpy.ops.mesh.primitive_cube_add(location=(-ancho_pasillo, i * 2, 1))
        pared_izq = bpy.context.active_object
        
        if i % 2 == 0:
            pared_izq.data.materials.append(mat_pared_a)
        else:
            pared_izq.data.materials.append(mat_pared_b)
            pared_izq.scale.z = 1.5

        bpy.ops.mesh.primitive_cube_add(location=(ancho_pasillo, i * 2, 1))
        pared_der = bpy.context.active_object
        pared_der.data.materials.append(mat_pared_a)

    # Tramo curvo
    cy = (largo_pasillo - 1) * 2 
    cx = radio_curva 

    for j in range(1, largo_pasillo + 1):
        angulo = math.pi - (j * (math.pi / 2) / largo_pasillo)
        rotacion_z = math.pi - angulo 

        x_izq = cx + (radio_curva + ancho_pasillo) * math.cos(angulo)
        y_izq = cy + (radio_curva + ancho_pasillo) * math.sin(angulo)

        bpy.ops.mesh.primitive_cube_add(
            location=(x_izq, y_izq, 1),
            rotation=(0, 0, rotacion_z)
        )

        pared_izq_curva = bpy.context.active_object
        pared_izq_curva.data.materials.append(mat_pared_a)

        x_der = cx + (radio_curva - ancho_pasillo) * math.cos(angulo)
        y_der = cy + (radio_curva - ancho_pasillo) * math.sin(angulo)

        bpy.ops.mesh.primitive_cube_add(
            location=(x_der, y_der, 1),
            rotation=(0, 0, rotacion_z)
        )

        pared_der_curva = bpy.context.active_object
        pared_der_curva.data.materials.append(mat_pared_a)

    # Suelo
    bpy.ops.mesh.primitive_plane_add(size=1, location=(radio_curva/2, cy/2 + radio_curva/2, 0))
    suelo = bpy.context.active_object
    suelo.scale.x = (ancho_pasillo * 2) + radio_curva + 10
    suelo.scale.y = (largo_pasillo * 2) + radio_curva + 10

    # Luces
    bpy.ops.object.light_add(type='SUN', location=(10, 10, 10))
    bpy.context.active_object.data.energy = 3 

    # Cámara
    bpy.ops.object.camera_add(location=(0, -6, 1.5), rotation=(math.radians(90), 0, 0))
    camara = bpy.context.active_object
    bpy.context.scene.camera = camara
    camara.rotation_mode = 'XYZ'

    # Animación
    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = 250

    camara.keyframe_insert(data_path="location", frame=1)
    camara.keyframe_insert(data_path="rotation_euler", frame=1)

    camara.location = (0, cy, 1.5)
    camara.keyframe_insert(data_path="location", frame=100)
    camara.keyframe_insert(data_path="rotation_euler", frame=100)

generar_escenario()
