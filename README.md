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

# -----------------------------------
# FUNCIÓN PARA CREAR MATERIAL
# -----------------------------------
def crear_material(nombre, color_rgb):
    mat = bpy.data.materials.new(name=nombre)
    mat.use_nodes = True
    bsdf = mat.node_tree.nodes["Principled BSDF"]
    bsdf.inputs["Base Color"].default_value = (*color_rgb, 1)
    bsdf.inputs["Roughness"].default_value = 0.7
    return mat


# -----------------------------------
# FUNCIÓN PARA GENERAR ESCENARIO
# -----------------------------------
def generar_escenario():

    # Limpiar escena
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete(use_global=False)

    # Materiales
    mat_pared_a = crear_material("ParedOscura", (0.1, 0.1, 0.1))
    mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0))

    # Parámetros
    largo_pasillo = 10
    ancho_pasillo = 4
    radio_curva = 12

    # -----------------------------------
    # TRAMO RECTO
    # -----------------------------------
    for i in range(largo_pasillo):
        bpy.ops.mesh.primitive_cube_add(location=(-ancho_pasillo, i * 2, 1))
        pared_izq = bpy.context.active_object
        pared_izq.scale = (0.2, 1, 2)
        pared_izq.data.materials.append(mat_pared_a)

        bpy.ops.mesh.primitive_cube_add(location=(ancho_pasillo, i * 2, 1))
        pared_der = bpy.context.active_object
        pared_der.scale = (0.2, 1, 2)
        pared_der.data.materials.append(mat_pared_a)


    # -----------------------------------
    # CREAR CÁMARA
    # -----------------------------------
    bpy.ops.object.camera_add(location=(0, -5, 2))
    cam = bpy.context.active_object
    cam.rotation_euler = (math.radians(75), 0, 0)

    bpy.context.scene.camera = cam

    # -----------------------------------
    # ANIMACIÓN DE CÁMARA
    # -----------------------------------
    cam.location = (0, -5, 2)
    cam.keyframe_insert(data_path="location", frame=1)

    cam.location = (0, 20, 2)
    cam.keyframe_insert(data_path="location", frame=120)

    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = 120


# Ejecutar función
generar_escenario()
