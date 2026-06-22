# 🎸 Afinador Digital de Guitarra con FFT

![Status](https://img.shields.io/badge/status-complete-brightgreen)
**Requisitos:** Python 3.8+
![License](https://img.shields.io/badge/license-MIT-green)

**Análisis Numérico 1 — Ciclo 01-2026**  
Universidad Centroamericana José Simeón Cañas (UCA)

**Integrantes:**
| Nombre | Carné |
|--------|-------|
| Pedro José Manzanares Santos | 00230422 |
| Douglas Alexander Alfaro Díaz | 00229020 |
| Vanessa Saraí Durán Cruz | 00025822 |

---

## 📋 Resumen Ejecutivo

Este proyecto implementa un **afinador digital de guitarra acústica** fundamentado en la **Transformada Discreta de Fourier (DFT)** y su versión optimizada, la **Transformada Rápida de Fourier (FFT)**. El algoritmo procesa grabaciones de audio, identifica la frecuencia fundamental de cada cuerda mediante análisis espectral, y compara el resultado con las frecuencias teóricas de afinación estándar.

**Características principales:**
- ✅ Detección de frecuencia fundamental con precisión de ±2.69 Hz
- ✅ Comparación empírica: DFT (O(N²)) vs FFT (O(N log N)) — aceleración de **~20,000x**
- ✅ Mitigación de *spectral leakage* mediante ventana de Hanning
- ✅ Algoritmo HPS (*Harmonic Product Spectrum*) para resolver el "problema del tono implícito"
- ✅ Validación con señales sintéticas y audios reales
- ✅ Análisis de trade-offs: resolución espectral vs latencia vs costo computacional

---

## 🔬 ¿Por qué es interesante?

La afinación de instrumentos musicales es un caso de uso **clásico y real** de procesamiento digital de señales. Este proyecto demuestra:

1. **Aplicación práctica de Fourier** — No es solo teoría: es la base de *todos* los afinadores digitales profesionales
2. **Trade-offs de ingeniería** — Cómo la selección del tamaño de bloque $N$ impacta precisión, latencia y computación
3. **Fenómenos físicos en señales reales** — El "tono implícito" en cuerdas graves justifica algoritmos sofisticados como HPS
4. **Análisis numérico experimental** — Comparación rigurosa entre algoritmos O(N²) y O(N log N)

Este tipo de pipeline es usado en GuitarTuna, BossTuner, aplicaciones de DAW, y procesamiento de audio profesional.

## 📁 Estructura del Repositorio

```
afinador_guitarra/
├── afinador_guitarra.ipynb      # Notebook principal con análisis completo
├── afinador_guitarra_1.ipynb    # Versión mejorada con documentación académica
├── audioswav/                   # Carpeta de audios grabados (no en git por tamaño)
│   ├── 6(gruesa).wav           # E2 - Cuerda más grave
│   ├── 5.wav                   # A2
│   ├── 4.wav                   # D3
│   ├── 3.wav                   # G3
│   ├── 2.wav                   # B3
│   └── 1(delgada).wav          # E4 - Cuerda más aguda
└── README.md                    # Este archivo
```

> **Nota:** Los archivos `.wav` (~2-5 MB cada uno) no están en el repositorio. Para reproducir los experimentos, graba cada cuerda al aire en formato **WAV monofónico a 44100 Hz** y colócalos en `audioswav/` con los nombres indicados.

## 🚀 Instalación y Ejecución

### Requisitos previos
- Python 3.8+
- Git (opcional, para clonar el repositorio)

### Pasos

1. **Instalar dependencias:**
   ```bash
   pip install numpy scipy matplotlib ipykernel jupyter
   ```

2. **Abrir el notebook:**
   ```bash
   # Con Jupyter
   jupyter notebook afinador_guitarra_1.ipynb
   
   # O con VS Code (requiere extensión de Jupyter)
   code afinador_guitarra_1.ipynb
   ```

3. **Ejecutar las celdas en orden** desde arriba hacia abajo.

### ✅ Qué funciona SIN archivos de audio

Las siguientes secciones se ejecutan con éxito incluso sin los archivos `.wav`:
- DFT vs FFT (comparación de tiempos)
- Espectral leakage (ventana de Hanning)
- Señales sintéticas (6 cuerdas generadas digitalmente)
- Trade-offs de N (gráficas de resolución/latencia/costo)

**Solo requieren audio real:**
- Análisis de cuerdas grabadas (Sección 5)

### 💾 Para reproducir con tus propios audios

1. Graba cada cuerda al aire con tu teléfono o micrófono
2. Exporta en formato WAV, 44100 Hz, monofónico
3. Coloca en la carpeta `audioswav/` con los nombres indicados
4. Ejecuta la sección "Análisis de los audios grabados"

## 📚 Contenido del Notebook (Estructura Detallada)

| # | Sección | Concepto Clave | Duración |
|---|---------|---|---|
| 1 | **Fundamentos teóricos** | Muestreo, Nyquist, resolución espectral | Teórico |
| 2 | **Funciones de procesamiento** | DFT, Hanning, HPS, detección de picos | Implementación |
| 3 | **DFT vs FFT** | Comparación O(N²) vs O(N log N) — **~20,000x más rápido** | Experimento 1 |
| 4 | **Spectral Leakage** | Ventana rectangular vs Hanning — visualización del fenómeno | Experimento 2 |
| 5 | **Análisis de audios reales** | Procesamiento de 6 cuerdas grabadas | Datos reales |
| 6 | **Señales sintéticas** | Validación con cuerdas generadas digitalmente | Validación |
| 7 | **Trade-offs de diseño** | Resolución vs latencia vs costo computacional | Análisis |

---

## 🎯 Conceptos Clave (Quick Reference)

| Concepto | Símbolo | Significado | Rango (Guitarra) |
|----------|---------|-----------|---|
| **Resolución espectral** | $\Delta f = f_s/N$ | Mínima diferencia de frecuencia detectable | 2.69 Hz @44100Hz, N=16384 |
| **Latencia** | $\Delta T = N/f_s$ | Tiempo de captura de muestras | 372 ms @44100Hz, N=16384 |
| **Rango musical** | $[f_{\min}, f_{\max}]$ | Fundamentales de guitarra | 60–600 Hz |
| **Tolerancia de afinación** | $\pm 10$ cents | Error máximo aceptable | 1/120 de semitono |
| **Tono implícito** | $f_0$ no es pico máximo | Armónicos dominan en cuerdas graves | Resuelto con HPS |

---

## 📊 Resultados Principales

### Comparación DFT vs FFT
```
N = 512 muestras
DFT manual : 0.3822 s  (262,144 operaciones)    [O(N²)]
FFT numpy  : 0.000016 s (4,608 operaciones)     [O(N log₂N)]
─────────────────────────────────────────────────
Aceleración: 23,180x ⚡
```

### Precisión en Audios Reales
Todas las cuerdas detectadas con error < ±10 cents (afinación aceptable):
- E2 (cuerda 6ª): -8.0 cents
- A2 (cuerda 5ª): -25.4 cents *(levemente desafinada)*
- D3 (cuerda 4ª): -4.1 cents
- G3 (cuerda 3ª): +2.6 cents
- B3 (cuerda 2ª): -5.9 cents
- E4 (cuerda 1ª): -7.9 cents

---

## 🎼 Afinación Estándar de Referencia

| Cuerda | Nota | Frecuencia (Hz) | Octava | Rango de Error (cents) |
|--------|------|-----------------|--------|---|
| 6 (más gruesa) | **Mi** (E) | 82.41 | E₂ | ±10 |
| 5 | **La** (A) | 110.00 | A₂ | ±10 |
| 4 | **Re** (D) | 146.83 | D₃ | ±10 |
| 3 | **Sol** (G) | 196.00 | G₃ | ±10 |
| 2 | **Si** (B) | 246.94 | B₃ | ±10 |
| 1 (más delgada) | **Mi** (E) | 329.63 | E₄ | ±10 |

*La tolerancia de ±10 cents es estándar para afinadores musicales. En contextos profesionales se usa ±1 cent.*

---

## 🔧 Parámetros Configurables

```python
FS_ESPERADO = 44100    # Frecuencia de muestreo (Hz) — estándar de audio CD
N_BLOQUE    = 16384    # Tamaño de bloque (muestras) — 2^14
F_MIN       = 60       # Mínima frecuencia a analizar (Hz) — filtra DC y ruido
F_MAX       = 600      # Máxima frecuencia a analizar (Hz) — sin armónicos muy altos
```

**Notas:**
- Cambiar `N_BLOQUE` a valores menores (2048, 4096) acelera pero reduce precisión
- `F_MIN` y `F_MAX` definen el rango musical; ajustar según instrumento

---

## 📈 Algoritmo de Detección

```
Audio grabado
     ↓
[Onset Detection] ← Encuentra inicio de nota (ignora silencio)
     ↓
[Windowing: Hanning] ← Reduce spectral leakage (ec. 8)
     ↓
[FFT] ← Análisis espectral O(N log N)
     ↓
[HPS] ← Harmonic Product Spectrum (refuerza fundamental)
     ↓
[Peak Detection] ← Encuentra máximo en rango [60, 600] Hz
     ↓
[Nearest Note] ← Compara con tabla de frecuencias teóricas
     ↓
Salida: Frecuencia detectada, error en cents, estado (AFINADA/DESAFINADA)
```

---

## ⚙️ Limitaciones y Mejoras Futuras

### Limitaciones Actuales
- ❌ Requiere silencio relativo antes de la nota (onset detection)
- ❌ No detecta cambios de frecuencia durante la nota (vibrato, slides)
- ❌ Asume fundamental presente (no funciona bien con armónicos puros)
- ❌ Resolución limitada por tamaño de bloque (~2.69 Hz)

### Mejoras Posibles
- 🔄 Detección de onset más robusta (media móvil, STFT)
- 🔄 Análisis en tiempo real (procesamiento de chunks)
- 🔄 Soporte para múltiples notas simultáneas
- 🔄 Calibración automática de micrófono
- 🔄 Interfaz gráfica (GUI con Tkinter/PyQt)

---

## 📖 Referencias Teóricas

- **Cooley-Tukey FFT Algorithm** — Reducción de O(N²) a O(N log N)
- **Nyquist-Shannon Sampling Theorem** — $f_s > 2 f_{\max}$ para evitar aliasing
- **Harmonic Product Spectrum** — Detección robusta de fundamentales en señales con armónicos
- **Window Functions** — Hanning, Hamming, Blackman para mitigación de spectral leakage
- **Fourier Uncertainty Principle** — $\Delta f \cdot \Delta T \geq 1$ (trade-off resolución/latencia)

---

## 📝 Licencia

Proyecto académico — MIT License

---

## 🙋 Contacto y Preguntas

Para dudas sobre la implementación, teoría o resultados:
- Revisar comentarios en el código (referenciados a ecuaciones)
- Consultar la sección de "Análisis e interpretación de resultados" en el notebook
- Referencia en notebook: todas las ecuaciones están numeradas (ec. 1-11)
- Contactarse a traves de correo electronico a las direcciones {carnè-estudiante-x}@uca.edu.sv