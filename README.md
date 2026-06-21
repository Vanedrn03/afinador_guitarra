# Afinador Digital de Guitarra con FFT

Proyecto del curso de Análisis Numérico 1 — Ciclo 01-2026  
Universidad Centroamericana José Simeón Cañas (UCA)

**Integrantes:**
- Pedro José Manzanares Santos — 00230422
- Douglas Alexander Alfaro Díaz — 00229020
- Vanessa Saraí Durán Cruz — 00025822

---

## ¿De qué trata?

Implementación de un afinador digital para guitarra acústica usando la Transformada Rápida de Fourier (FFT). El sistema toma un audio grabado de cada cuerda, aplica una ventana de Hanning para reducir el spectral leakage, calcula la FFT y detecta la frecuencia fundamental para compararla con la afinación estándar.

También se incluye una implementación manual de la DFT (O(N²)) para contrastarla con la FFT de NumPy (O(N log N)) y mostrar la diferencia de rendimiento.

## Contenido del repositorio

```
afinador_guitarra/
├── afinador_guitarra.ipynb   # notebook principal
├── audioswav/                # audios grabados (no incluidos en git)
└── README.md
```

> Los archivos `.wav` no están en el repositorio porque son muy pesados. Para reproducir los experimentos, grabar cada cuerda al aire en formato WAV (44100 Hz o 48000 Hz, mono) y colocarlos en la carpeta `audioswav/` con los nombres que aparecen en la celda de configuración del notebook.

## Cómo correrlo

1. Instalar dependencias:
   ```
   pip install numpy scipy matplotlib ipykernel
   ```
2. Abrir `afinador_guitarra.ipynb` en VS Code o Jupyter.
3. Correr las celdas en orden.

Las celdas de señales sintéticas y comparación DFT vs FFT funcionan sin necesitar los archivos de audio.

## Estructura del notebook

| Sección | Contenido |
|---------|-----------|
| Imports y parámetros | Configuración global: fs, N, frecuencias teóricas |
| Funciones | Carga de WAV, ventana de Hanning, DFT manual, FFT, HPS |
| DFT vs FFT | Comparación de tiempos con señal sintética a 440 Hz |
| Ventaneo de Hanning | Visualización del spectral leakage |
| Análisis de audios reales | Detección de frecuencia en las 6 cuerdas grabadas |
| Señales sintéticas | Prueba del algoritmo sin necesidad de audios |
| Trade-offs de N | Resolución vs latencia vs costo computacional |

## Afinación estándar de referencia

| Cuerda | Nota | Frecuencia (Hz) |
|--------|------|----------------|
| 6 (más gruesa) | Mi (E2) | 82.41 |
| 5 | La (A2) | 110.00 |
| 4 | Re (D3) | 146.83 |
| 3 | Sol (G3) | 196.00 |
| 2 | Si (B3) | 246.94 |
| 1 (más delgada) | Mi (E4) | 329.63 |
