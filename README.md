# Laboratorio: reconstrucción de superficies con Ball Pivoting

Práctica de computación gráfica que recorre el paso de una nube de puntos a una malla triangular: ejemplos pedagógicos en 2D y 3D, Stanford Bunny, reconstrucción con Ball Pivoting y medición del costo computacional.

## Contenido

```text
Laboratorio_Ball_Pivoting_Completo/
├── 01_BPA_2D/                  Ejemplos 2D y construcción progresiva de triángulos
├── 02_BPA_3D_Pedagogico/       Ejemplo 3D y nube aleatoria de 20 puntos
├── 03_Ejemplo_Terreno/         Nube y malla de referencia de un terreno
├── 04_Stanford_Bunny/          Nube, malla de referencia y visualizador OpenGL
├── 05_BPA_Bunny_2503/          Ball Pivoting con 2503 puntos
└── 06_CPU_vs_CUDA/             Nubes de 20 000 y 50 000 puntos y benchmark
```

La carpeta `evidencias/` contiene las capturas, métricas y resúmenes de las actividades. Los informes y las secciones LaTeX se mantienen fuera del repositorio.

## Requisitos

- Compilador C++ con soporte C++17.
- OpenGL, GLU y FreeGLUT.
- OpenMP para la versión paralela.

En Arch Linux:

```bash
sudo pacman -S base-devel freeglut glu mesa mesa-utils
```

En Ubuntu/Debian:

```bash
sudo apt install build-essential freeglut3-dev libglu1-mesa-dev libgl1-mesa-dev mesa-utils
```

## Compilación

Cada programa se compila desde su carpeta, porque carga los archivos `.xyz` y `.obj` con rutas relativas.

Ejemplo 2D:

```bash
cd Laboratorio_Ball_Pivoting_Completo/01_BPA_2D

g++ -O2 -std=c++17 BallPivoting2D_Malla_OpenGL.cpp \
    -o bpa2d -lglut -lGL -lGLU -lm

./bpa2d
```

Visualizador del Stanford Bunny:

```bash
cd Laboratorio_Ball_Pivoting_Completo/04_Stanford_Bunny

g++ -O2 -std=c++17 01_Visualizador_Stanford_Bunny_CENTRADO.cpp \
    -o bunny -lglut -lGL -lGLU -lm

./bunny
```

## Benchmark de la Parte F

La versión paralela está en `06_CPU_vs_CUDA/03_Stanford_Bunny_BPA_MULTISEMILLA_PARALELO.cpp`. Usa OpenMP en `buscarPuntoBPA` y `esferaVacia`, agrega medición con `std::chrono` y permite ejecutar sin ventana mediante `--bench`.

```bash
cd Laboratorio_Ball_Pivoting_Completo/06_CPU_vs_CUDA

make
make benchmark
cat resultados/resumen.csv
```

El Makefile ejecuta 2503 y 20 000 puntos con 1, 2, 4 y 6 hilos. También permite probar los 12 hilos lógicos:

```bash
make benchmark THREADS="1 2 4 6 12"
```

Prueba de estrés con 50 000 puntos:

```bash
make benchmark-50k THREADS="6" TIMEOUT=180
```

## Resultados principales

Con radio `0.007`, la versión serial tarda aproximadamente:

| Puntos | Tiempo serial | Triángulos |
|---:|---:|---:|
| 2 503 | 39 ms | 6 564 |
| 20 000 | 10 136 ms | 40 801 |
| 50 000 | no completó en 15 min | — |

La construcción de vecindad es `O(n²)`, pero en 20 000 puntos representa solo el 3.7 % del tiempo total. El cuello de botella está en el crecimiento de la malla y en la búsqueda multi-semilla.

La versión OpenMP conserva la misma malla y reduce su propio tiempo de 1 hilo, pero no supera a la versión serial original. El costo de crear regiones paralelas por cada arista pesa más que la ganancia. La conclusión práctica es que una aceleración real requiere un diseño de grano más grueso o una implementación CUDA.

## Actividades

- **Parte A:** ejemplo pedagógico 2D.
- **Parte B:** transición a 3D y proyección frontal.
- **Parte C:** nube aleatoria de 20 puntos.
- **Parte D:** Stanford Bunny en modos puntos, alambrado y sólido.
- **Parte E:** Ball Pivoting con 2503 puntos y tres radios.
- **Parte F:** 20 000 y 50 000 puntos, tiempos y análisis de paralelización.

## Créditos

Stanford Bunny: Stanford University Computer Graphics Laboratory. Modelo utilizado con fines académicos según las condiciones del Stanford 3D Scanning Repository.

## Nota

No se incluye una implementación CUDA. El equipo de desarrollo no dispone de una GPU NVIDIA; la Parte F analiza qué funciones conviene paralelizar y por qué el cuello de botella no es solo el cálculo de distancias.
