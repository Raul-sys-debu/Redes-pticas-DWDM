# 03 - Manual de Configuración

## 1. Medidas de Seguridad Óptica y Manipulación de Conectores

### 1.1 Seguridad Láser (Clases 1M y 3R)
1. **Peligro Invisible:** La luz utilizada en sistemas DWDM opera en Banda C (~1550 nm), en el espectro infrarrojo no visible por el ojo humano. La ausencia de luz perceptible no implica ausencia de radiación.
2. **Daño Ocular Irreversible:** La radiación infrarroja atraviesa la córnea y se enfoca directamente sobre la retina sin activar el reflejo de parpadeo, pudiendo provocar quemaduras retinianas permanentes.
3. **Regla de Oro:** **NUNCA mirar directamente** el extremo de una fibra óptica, conector de parcheo, ferrule o puerto de salida de equipos activos (transpondedores, líneas DWDM) sin verificar previamente la ausencia de potencia óptica con un medidor de potencia o verificar que el láser esté apagado.
4. Uso de tapas protectoras antipolvo en todos los puertos y patch cords no conectados.

### 1.2 Limpieza de Conectores
* Utilizar limpiadores mecánicos tipo pluma (*one-click cleaner*) adecuados al diámetro del ferrule (2.5 mm para SC/FC/ST y 1.25 mm para LC).
* Inspeccionar visualmente el extremo mediante microscopio óptico de inspección antes de conectar. La presencia de polvo o grasa incrementa drásticamente la atenuación de inserción y puede quemar el extremo de la fibra debido a la densidad de potencia del láser.

### 1.3 Incompatibilidad Crítica: Conectores APC vs. UPC
* **UPC (Ultra Physical Contact):** Color **AZUL**. Pulido plano con curvatura convexa ligera. Reflexión de retorno (*Back Reflection*): $< -55\text{ dB}$.
* **APC (Angled Physical Contact):** Color **VERDE**. Pulido en ángulo de 8°. Reflexión de retorno (*Back Reflection*): $< -65\text{ dB}$. La luz reflejada se desvía hacia el revestimiento (*cladding*).
* **ADVERTENCIA TÉCNICA OBLIGATORIA:** **NUNCA conectar un conector APC (verde) a un puerto o conector UPC (azul)**. Debido al bisel de 8°, el contacto no es uniforme, provocando una cámara de aire (*air gap*), pérdidas de inserción severas (> 10 dB) y daños mecánicos permanentes e irreversibles en las caras de pulido de las férulas.

---

## 2. Guía de Aprovisionamiento del Chasis HT6000

1. **Conexión:** Conectar un cable de red Ethernet RJ-45 desde la PC de gestión (`192.168.1.10/24`) a la interfaz de gestión NMS del chasis DWDM 1 (`192.168.1.101`) o DWDM 2 (`192.168.1.102`).
2. **Acceso:** Abrir un navegador web (Chrome/Firefox) e ingresar a `http://192.168.1.101`. Autenticarse con `admin` / `admin`.
3. **Verificación de Módulos:** En el árbol de inventario del chasis verificar el estado de las tarjetas OTU (Transpondedores) y ODM08 (Multiplexores pasivos).
4. **Mapeo de Canales:**
   * Configurar el puerto de cliente (ej. 10GE LAN/WAN PHY o 1GE).
   * Asignar y habilitar el láser de línea en el canal ITU correspondiente:
     * Para sentido UP: Canales C21 (192.1 THz) a C24 (192.4 THz).
     * Para sentido DOWN: Canales C25 (192.5 THz) a C28 (192.8 THz).
   * Validar que la potencia de salida del transponder se encuentre dentro del rango operativo nominal (+0 dBm a +4 dBm).

---

## 3. Procedimientos de Prueba y Medición

### 3.1 Análisis Espectral con OSA RXT4510
1. Limpiar el jumper monomodo (LC/UPC a SC/UPC según corresponda).
2. Conectar el puerto de entrada del OSA al puerto de monitoreo del ODF (`MON DWDM 1` en el puerto 1-A o `MON DWDM 2` en el puerto 1-B). Este puerto desacopla una muestra atenuada (usualmente el 1% o -20 dB) del enlace total sin cortar el servicio.
3. Encender el OSA RXT4510 (credenciales `pass1`), seleccionar la aplicación **OSA DWDM**.
4. Ajustar el barrido (*Sweep*) a la **Banda C**, definiendo la grilla a **100 GHz**.
5. Presionar **Single Sweep** o **Repeat Sweep**.
6. Analizar la tabla de resultados:
   * **Peak Power:** Potencia pico de cada portadora óptica.
   * **Wavelength / Frequency:** Longitud de onda central real y desviación respecto al grid ITU nominal.
   * **OSNR:** Relación señal a ruido óptica (verificar que cumpla con el umbral $> 25\text{ dB}$).

### 3.2 Inserción del Atenuador Óptico Variable (OVA)
1. Para realizar curvas de estrés y evaluar el margen del receptor, intercalar el atenuador JW3303 entre la salida de línea del ODF (`LIN DWDM 1` o `2`) y la entrada a la bobina de fibra de 25 km (`FIBRA 1` o `2`).
2. Configurar la longitud de onda de calibración del OVA en **1550 nm**.
3. Iniciar con la atenuación mínima de inserción (ej. 1.25 dB) e incrementar gradualmente en pasos de 1 dB o 2 dB, registrando el comportamiento del tráfico.

### 3.3 Medición de Tráfico Ethernet con MTX150x (RFC 2544 / Y.1564)
1. Conectar el Puerto 1 del MTX150x al puerto `CLI 1 DWDM 1` (ODF puerto 3) y el Puerto 2 al puerto `CLI 1 DWDM 2` (ODF puerto 4).
2. Configurar el MTX150x en modo **Dual Port** o **Loopback** según la topología.
3. **Prueba RFC 2544:**
   * Ejecutar la prueba evaluando tramas de 64, 128, 256, 512, 1024, 1280 y 1518 bytes.
   * Registrar: Rendimiento (*Throughput* al 100% de la tasa de línea), Latencia promedio (ida y vuelta), Pérdida de tramas (*Frame Loss Rate*) y Ráfagas continuas (*Back-to-Back*).
4. **Prueba ITU-T Y.1564 (EtherSAM):**
   * Realizar la prueba de configuración de servicio (Service Configuration Test) validando CIR (Committed Information Rate) y EIR (Excess Information Rate).
   * Realizar la prueba de rendimiento de servicio (Service Performance Test) durante una ventana de tiempo prolongada, verificando que no existan errores de bit (BER = 0).
