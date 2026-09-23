# 01. Fundamentos Teóricos de Redes Ópticas DWDM

## 1. Definición y Principio de Multiplexación: DWDM vs. CWDM
La multiplexación por división de longitud de onda (*Wavelength Division Multiplexing*, WDM) es una técnica que permite transmitir múltiples canales de información independientes y simultáneos sobre una única fibra óptica, asignando a cada canal una longitud de onda ($\lambda$) de luz portadora distinta.

* **CWDM (Coarse WDM - Multiplexación Espaciada):** Definido por el estándar ITU-T G.694.2. Utiliza un espaciamiento amplio de **20 nm** entre canales adyacentes, cubriendo un rango espectral desde 1270 nm hasta 1610 nm (hasta 18 canales nominales). No requiere láseres con control térmico activo (refrigeración Peltier), lo que abarata los costos de los transceptores; sin embargo, no puede ser amplificado por amplificadores ópticos de fibra dopada con erbio (EDFA) y su alcance está limitado a distancias metropolitanas cortas.
* **DWDM (Dense WDM - Multiplexación Densa):** Definido por la recomendación ITU-T G.694.1. Utiliza un espaciamiento espectral muy estrecho definido en frecuencia (típicamente **100 GHz** o **50 GHz**, equivalentes a ~0.8 nm y ~0.4 nm respectivamente). Opera fundamentalmente en la **Banda C** (1530 nm a 1565 nm), aprovechando el punto de mínima atenuación de la sílice y la banda de amplificación de los EDFA, permitiendo enlaces de ultra-larga distancia y capacidades de terabits por segundo.

---

## 2. Espectro Óptico y Grillas Espectrales

### 2.1 Bandas Ópticas de Transmisión
El espectro de transmisión en fibra óptica monomodo se divide en seis bandas normalizadas:
* **Banda O (Original):** 1260 nm – 1360 nm. Región de mínima dispersión cromática para fibras G.652 estándar.
* **Banda E (Extendida):** 1360 nm – 1460 nm. Históricamente afectada por el "pico de agua" (alta atenuación producida por la absorción del ión hidroxilo $OH^-$ a 1383 nm).
* **Banda S (Short / Corta):** 1460 nm – 1530 nm.
* **Banda C (Conventional / Convencional):** 1530 nm – 1565 nm. Región crítica de mínima atenuación (~0.2 dB/km) y ganancia plana de amplificadores EDFA.
* **Banda L (Long / Larga):** 1565 nm – 1625 nm. Utilizada para ampliar la capacidad de canales cuando la Banda C está saturada.
* **Banda U (Ultra-long / Ultra-larga):** 1625 nm – 1675 nm. Principalmente reservada para mantenimiento y monitoreo con OTDR.

### 2.2 Grilla Espectral ITU-T G.694.1
La norma ITU-T G.694.1 estipula la grilla de frecuencias para sistemas DWDM, tomando como referencia absoluta la frecuencia central de **193.100 THz (1552.52 nm)**.

Las frecuencias centrales de los canales se determinan mediante la expresión:
$$f = 193.100 \text{ THz} \pm (n \times \Delta f)$$
Donde $\Delta f$ define el espaciamiento:
* **100 GHz (~0.8 nm):** Empleado en la maqueta de laboratorio.
* **50 GHz (~0.4 nm):** Permite hasta 80 canales en Banda C.
* **Flexible Grid (Grilla Flexible):** Asignación de franjas espectrales en pasos de 12.5 GHz para modular portadoras coherentes de alta tasa (400G / 800G).

### 2.3 Canales Asignados en la Maqueta HT6000
| Ubicación en Chasis | Canal DWDM | Canal ITU | Frecuencia (THz) | Longitud de Onda (nm) |
|---|---|---|---|---|
| OTU UP CH1 | C21 | 21 | 192.100 | 1560.61 |
| OTU UP CH2 | C22 | 22 | 192.200 | 1559.79 |
| OTU UP CH3 | C23 | 23 | 192.300 | 1558.98 |
| OTU UP CH4 | C24 | 24 | 192.400 | 1558.17 |
| OTU DOWN CH1 | C25 | 25 | 192.500 | 1557.36 |
| OTU DOWN CH2 | C26 | 26 | 192.600 | 1556.56 |
| OTU DOWN CH3 | C27 | 27 | 192.700 | 1555.75 |
| OTU DOWN CH4 | C28 | 28 | 192.800 | 1554.94 |

---

## 3. Modulación Óptica y Conversión Electro-Óptica

* **Conversión Óptica-Eléctrica-Óptica (O-E-O):** Los módulos transpondedores (OTU) reciben señales ópticas de cliente (ej. 850 nm multimodo o 1310 nm monomodo de corto alcance), las convierten a pulsos eléctricos para regenerar y re-temporizar el reloj (3R: Reamplificación, Reshaping y Retiming), y modulan un láser DFB estabilizado en una longitud de onda fija del grid DWDM.
* **NRZ (Non-Return-to-Zero):** Esquema binario directo (OOK) donde un bit "1" representa presencia de luz y "0" ausencia de luz. Utilizado ampliamente en canales de hasta 10 Gbps por su simplicidad.
* **PAM4 (Pulse Amplitude Modulation 4):** Modulación de cuatro niveles de amplitud que codifica 2 bits por símbolo, reduciendo el ancho de banda analógico necesario a la mitad. Estándar en interconexiones 100G/400G de media distancia.
* **Modulación Coherente (DP-QPSK / m-QAM):** Modula amplitud y fase en dos estados ortogonales de polarización (Dual Polarization). En recepción se combina con un oscilador local y un procesador de señales digital (DSP) de alta velocidad, permitiendo compensar por software la dispersión cromática (CD) y la dispersión por modo de polarización (PMD) en enlaces de 100 Gbps a 800 Gbps.

---

## 4. Estándares y Normativas de Referencia

* **ITU-T G.694.1:** Define la grilla espectral WDM en frecuencias fijas (100 GHz, 50 GHz, etc.) y grilla flexible para sistemas DWDM.
* **ITU-T G.709 (OTN - Optical Transport Network):** Define la jerarquía de transporte óptico y la estructura de enmarcado digital (OTU/ODU/OPU). Introduce algoritmos de Corrección de Errores Hacia Adelante (FEC - *Forward Error Correction*), permitiendo corregir errores de bit en recepción y tolerar menores niveles de relación señal a ruido óptica (OSNR).
* **ITU-T G.652 (Standard Single-Mode Fiber - SSMF):** Fibra monomodo estándar utilizada en planta externa y en las bobinas de la maqueta. Posee longitud de onda de dispersión nula ($\lambda_0$) cerca de 1310 nm y coeficiente de atenuación aproximado de 0.20 dB/km en 1550 nm. La variante G.652.D elimina el pico de agua en la banda E.
* **ITU-T G.655 (Non-Zero Dispersion-Shifted Fiber - NZ-DSF):** Diseñada específicamente para enlaces DWDM de larga distancia; mantiene un valor pequeño pero no nulo de dispersión cromática en Banda C para mitigar efectos ópticos no lineales perjudiciales, como la Mezcla de Cuatro Ondas (FWM - *Four-Wave Mixing*).
