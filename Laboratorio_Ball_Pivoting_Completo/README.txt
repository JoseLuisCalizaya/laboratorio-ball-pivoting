LABORATORIO COMPLETO: BALL PIVOTING, OPENGL Y TRANSICION A CUDA

Objetivo general
----------------
Comprender progresivamente cómo una nube de puntos puede convertirse en una
malla triangular, desde ejemplos pedagógicos 2D/3D hasta el Stanford Bunny.

IMPORTANTE
----------
Los programas del Stanford Bunny incluidos aquí usan centrado automático
solamente para la visualización OpenGL. Las coordenadas originales utilizadas
por Ball Pivoting no se modifican.

ESTRUCTURA
----------

01_BPA_2D
---------
BallPivoting2D_OpenGL.cpp
    Ejemplo pedagógico inicial: puntos, círculo y avance del contorno.

BallPivoting2D_Malla_OpenGL.cpp
    Ejemplo 2D que genera triángulos progresivamente.

02_BPA_3D_Pedagogico
---------------------
BallPivoting3D_Pedagogico_OpenGL.cpp
    Ejemplo 3D con 9 puntos, triángulos, esfera alambrada y rotación.

Nube3D_20Puntos_OpenGL.cpp
    Nube de 20 puntos aleatorios sobre una superficie controlada.

03_Ejemplo_Terreno
------------------
nube9_terreno.xyz
malla9_terreno.obj
    Ejemplo opcional para discutir triangulación de terreno y Delaunay.

04_Stanford_Bunny
-----------------
Stanford_Bunny_2503.xyz
    Nube de 2503 puntos sin conectividad.

Stanford_Bunny_2503.obj
    Malla de referencia.

01_Visualizador_Stanford_Bunny_CENTRADO.cpp
    Visualizador OpenGL centrado:
        P = puntos
        W = malla alambrada
        F = malla sólida
        Mouse/Flechas = rotar
        +/- = zoom

05_BPA_Bunny_2503
-----------------
Stanford_Bunny_2503.xyz
02_Stanford_Bunny_BPA_2503_CENTRADO.cpp
    Ball Pivoting pedagógico sobre 2503 puntos.
    Incluye corrección de tolerancia geométrica, vecindad local
    y centrado automático de la visualización.

06_CPU_vs_CUDA
--------------
Stanford_Bunny_20000.xyz
Stanford_Bunny_50000.xyz
03_Stanford_Bunny_BPA_MULTISEMILLA_CENTRADO.cpp
    Versión para nubes más grandes.
    Usa múltiples semillas cuando una frontera deja de crecer.

    Estos archivos permiten mostrar el aumento del costo computacional
    y motivar la paralelización con CUDA.

SECUENCIA RECOMENDADA
---------------------
1. BPA 2D: comprender el pivote y la conectividad.
2. Malla 2D: generar triángulos.
3. BPA 3D pedagógico: pasar de círculo a esfera.
4. Nube aleatoria 3D: dejar los ejemplos perfectamente simétricos.
5. Stanford Bunny: observar una nube realista y su malla de referencia.
6. Bunny 2503: ejecutar Ball Pivoting.
7. Bunny 20000: observar el incremento del tiempo de ejecución.
8. Bunny 50000: usar como prueba de estrés.
9. Comparar posteriormente CPU vs CUDA.

ARCHIVOS EXCLUIDOS A PROPOSITO
------------------------------
No se incluyen:
- Stanford_Bunny_1000.xyz, porque no fue usado.
- Versiones antiguas con errores o sin centrado.
- Primeras versiones del BPA del Bunny que fallaban al encontrar la semilla.

NOTA SOBRE CUDA
---------------
En este paquete todavía no se incluye una implementación CUDA.
La siguiente etapa natural es paralelizar las partes más costosas:
- cálculo de distancias,
- construcción de vecindades,
- verificación de esfera vacía,
- evaluación de candidatos.

CREDITO DEL MODELO
------------------
Stanford Bunny:
Stanford University Computer Graphics Laboratory.
Uso académico/no comercial según las condiciones del Stanford 3D Scanning Repository.
