📘 Proyecto Integrador – Unidad I
Escenario Procedural con Animación de Cámara en Blender
---
1. Introducción

El presente proyecto integrador tiene como objetivo aplicar los fundamentos de la graficación por computadora mediante la generación de un escenario tridimensional procedural utilizando Python en Blender.

Se desarrolla un pasillo compuesto por un tramo recto y uno curvo, generados automáticamente mediante cálculos matemáticos. Además, se implementa la animación de una cámara que recorre el camino de forma continua y fluida, simulando una experiencia de recorrido en primera persona.

Este proyecto integra conceptos de:

Modelado 3D

Transformaciones geométricas

Trigonometría aplicada

Animación por fotogramas clave (Keyframes)

Iluminación digital

Generación procedural

---
2. Objetivo General

Desarrollar un escenario tridimensional procedural en Blender utilizando Python, incorporando animación de cámara a lo largo de un camino recto y curvo.

---
3. Objetivos Específicos

Generar estructuras 3D mediante código.

Aplicar materiales y sombreado a objetos.

Utilizar funciones trigonométricas para construir trayectorias curvas.

Implementar animación mediante keyframes.

Integrar iluminación básica en la escena.

---
4. Marco Conceptual

4.1 Generación Procedural

La generación procedural consiste en crear contenido digital utilizando algoritmos matemáticos en lugar de modelado manual.

En este proyecto, el pasillo es generado automáticamente mediante ciclos for y cálculos trigonométricos.

4.2 Transformaciones Geométricas

Se aplican transformaciones como:

Traslación (cambio de posición)

Rotación (orientación en el espacio)

Escalamiento (modificación de tamaño)

Estas transformaciones permiten construir tanto el tramo recto como el tramo curvo.

4.3 Trigonometría Aplicada

Para generar la curva se utilizan las funciones:

cos(θ)

sin(θ)

Estas funciones permiten calcular posiciones en un movimiento circular:

x = cx + r cos(θ)
y = cy + r sin(θ)

Donde:

r es el radio de la curva

cx, cy es el centro de la circunferencia

4.4 Animación por Keyframes

La animación se logra insertando fotogramas clave (keyframes), que almacenan:

Posición (location)

Rotación (rotation_euler)

Blender interpola automáticamente entre estos puntos para generar movimiento fluido.

---
5. Desarrollo del Proyecto

El proyecto se divide en las siguientes etapas:

Limpieza de escena

Creación de materiales

Generación del tramo recto

Generación del tramo curvo

Creación del suelo

Incorporación de luces

Creación de cámara

Animación del recorrido

La animación tiene una duración de 250 fotogramas, simulando aproximadamente 10 segundos de recorrido.

📌 Código Completo
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
    # 1. Limpiar la escena previa
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()

    # 2. Definir materiales
    mat_pared_a = crear_material("ParedOscura", (0.1, 0.1, 0.1))
    mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0))

    # 3. Parámetros del escenario
    largo_pasillo = 10
    ancho_pasillo = 4
    radio_curva = 12 

    # 4a. Tramo Recto
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

    # 4b. Tramo Curvo
    cy = (largo_pasillo - 1) * 2 
    cx = radio_curva 

    for j in range(1, largo_pasillo + 1):
        angulo = math.pi - (j * (math.pi / 2) / largo_pasillo)
        rotacion_z = math.pi - angulo 

        # Pared izquierda curva
        x_izq = cx + (radio_curva + ancho_pasillo) * math.cos(angulo)
        y_izq = cy + (radio_curva + ancho_pasillo) * math.sin(angulo)

        bpy.ops.mesh.primitive_cube_add(
            location=(x_izq, y_izq, 1),
            rotation=(0, 0, rotacion_z)
        )

        pared_izq_curva = bpy.context.active_object

        if j % 2 == 0:
            pared_izq_curva.data.materials.append(mat_pared_a)
        else:
            pared_izq_curva.data.materials.append(mat_pared_b)
            pared_izq_curva.scale.z = 1.5

        # Pared derecha curva
        x_der = cx + (radio_curva - ancho_pasillo) * math.cos(angulo)
        y_der = cy + (radio_curva - ancho_pasillo) * math.sin(angulo)

        bpy.ops.mesh.primitive_cube_add(
            location=(x_der, y_der, 1),
            rotation=(0, 0, rotacion_z)
        )

        pared_der_curva = bpy.context.active_object
        pared_der_curva.data.materials.append(mat_pared_a)

    # 5. Agregar suelo
    bpy.ops.mesh.primitive_plane_add(
        size=1,
        location=(radio_curva/2, cy/2 + radio_curva/2, 0)
    )

    suelo = bpy.context.active_object
    suelo.scale.x = (ancho_pasillo * 2) + radio_curva + 10
    suelo.scale.y = (largo_pasillo * 2) + radio_curva + 10

    # 6. Luces
    bpy.ops.object.light_add(
        type='SUN',
        location=(10, 10, 10),
        rotation=(math.radians(-45), math.radians(30), 0)
    )
    bpy.context.active_object.data.energy = 3 

    bpy.ops.object.light_add(type='POINT', location=(0, 5, 4))
    bpy.context.active_object.data.energy = 500 

    bpy.ops.object.light_add(
        type='POINT',
        location=(radio_curva, cy + radio_curva/2, 4)
    )
    bpy.context.active_object.data.energy = 800

    # 7. Cámara
    bpy.ops.object.camera_add(
        location=(0, -6, 1.5),
        rotation=(math.radians(90), 0, 0)
    )

    camara = bpy.context.active_object
    bpy.context.scene.camera = camara
    camara.rotation_mode = 'XYZ'

    # 8. Animación
    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = 250

    # Inicio
    camara.keyframe_insert(data_path="location", frame=1)
    camara.keyframe_insert(data_path="rotation_euler", frame=1)

    # Final tramo recto
    camara.location = (0, cy, 1.5)
    camara.keyframe_insert(data_path="location", frame=100)
    camara.keyframe_insert(data_path="rotation_euler", frame=100)

    # Curva
    for frame in range(101, 251):
        progreso = (frame - 100) / 150.0
        angulo_curva = math.pi - (progreso * (math.pi / 2))

        x_cam = cx + radio_curva * math.cos(angulo_curva)
        y_cam = cy + radio_curva * math.sin(angulo_curva)
        rot_z = -progreso * (math.pi / 2)

        camara.location = (x_cam, y_cam, 1.5)
        camara.rotation_euler = (math.radians(90), 0, rot_z)

        camara.keyframe_insert(data_path="location", frame=frame)
        camara.keyframe_insert(data_path="rotation_euler", frame=frame)

generar_escenario()

---
6. Resultados

Se obtuvo un escenario tridimensional compuesto por:

Un pasillo recto con alternancia de materiales.

Un tramo curvo generado mediante funciones trigonométricas.

Iluminación básica para generar sombras.

Cámara animada que recorre el camino de manera progresiva.

El movimiento es fluido y mantiene orientación coherente durante la curva.
