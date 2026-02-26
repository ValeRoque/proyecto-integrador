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

---
6. Resultados

Se obtuvo un escenario tridimensional compuesto por:

Un pasillo recto con alternancia de materiales.

Un tramo curvo generado mediante funciones trigonométricas.

Iluminación básica para generar sombras.

Cámara animada que recorre el camino de manera progresiva.

El movimiento es fluido y mantiene orientación coherente durante la curva.
