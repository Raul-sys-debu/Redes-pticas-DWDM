La maqueta de laboratorio DWDM está compuesta por instrumentación de medición avanzada y equipos de transmisión óptica de transporte. A continuación, se detalla la función técnica, el tipo de interfaces y el rol dentro del rack de cada uno de los 7 componentes principales.

<img width="1351" height="757" alt="Maqueta" src="https://github.com/user-attachments/assets/24248ac2-1715-4524-bdc7-a8c87ec800b7" />


### 1. Analizador Ethernet (VeEX MTX150x)
*   **Función Técnica:** Equipo de prueba multifunción para generación de tráfico de red en capas L2/L3 y validación de rendimiento mediante pruebas estandarizadas (RFC 2544 y Y.1564).
*   **Tipo de Interfaz:** Dispone de puertos RJ45 para cobre y puertos ópticos SFP/SFP+ para enlaces de hasta 10 Gbps.
*   **Rol Técnico:** Actúa como el equipo cliente extremo (origen y destino de los datos). Permite inyectar tráfico a la red de transporte y medir métricas de calidad de servicio como el *Bit Error Rate* (BER), pérdida de tramas y latencia.

### 2. Analizador de Espectro Óptico - OSA (VeEX RXT-4510)
*   **Función Técnica:** Análisis detallado del espectro óptico de la señal DWDM multiplexada, operando típicamente en la Banda C.
*   **Tipo de Interfaz:** Puertos ópticos de entrada con conectores intercambiables para acoplarse a los puertos de monitoreo (MON) del sistema DWDM.
*   **Rol Técnico:** Es el instrumento crítico de diagnóstico de capa física. Se utiliza para comprobar que los canales de la grilla (ej. C21 a C28) estén operando correctamente, midiendo la potencia por canal (dBm), la longitud de onda central exacta (nm) y la Relación Señal a Ruido Óptica (OSNR).

### 3. Switch Ethernet (MikroTik CSS610-8G-2S+IN)
*   **Función Técnica:** Conmutación L2 (Switching) para la agregación de clientes Ethernet hacia la red óptica DWDM.
*   **Tipo de Interfaz:** 8 puertos Gigabit Ethernet (RJ45) y 2 puertos SFP+ (10 Gbps).
*   **Rol Técnico:** Funciona como un switch de acceso. Recibe el tráfico de datos (ej. desde PCs o el analizador) y lo encamina hacia los puertos de cliente (Client Ports) de las tarjetas transpondedoras del chasis WDM.

### 4. ODF (Optical Distribution Frame)
*   **Función Técnica:** Panel de parcheo pasivo diseñado para la organización, terminación y administración física del cableado de fibra óptica.
*   **Tipo de Interfaz:** Acopladores ópticos dúplex pasantes en el panel frontal.
*   **Rol Técnico:** Proporciona un punto de interconexión (Cross-Connect) flexible. Protege los puertos reales de los equipos activos, permitiendo realizar todos los puentes (*patching*) hacia los carretes de fibra o la instrumentación de forma ordenada y segura para los conectores.

### 5. Chasis DWDM 1 y DWDM 2 (HTF HT6000-1U)
*   **Función Técnica:** Plataforma WDM activa que integra la tarjeta de gestión de red (NMS), la conversión electro-óptica (O-E-O) en las tarjetas OTU y la multiplexación pasiva.
*   **Tipo de Interfaz:**
    *   **Módulo MUX/DEMUX (ODM08):** Puertos LC dúplex por canal (C21 a C28), puerto de línea multiplexada (Out/In) y puerto de monitoreo (MON).
    *   **Tarjetas Transpondedoras (OTU):** Puertos SFP/SFP+ "Client" para recibir la señal gris del switch, y puertos SFP/SFP+ "Line" sintonizados en colores específicos DWDM para conectar al MUX.
*   **Rol Técnico:** Son los nodos terminales del enlace de transporte. Las OTU convierten las señales cliente en longitudes de onda coloreadas específicas. El módulo MUX combina ópticamente estas longitudes en un solo hilo de fibra para su transmisión hacia la planta externa, mientras el DEMUX realiza la separación en el extremo remoto.

### 6. Carretes de Fibra Óptica (AB - 25 KM y BA - 25 KM)
*   **Función Técnica:** Bobinas de prueba de fibra monomodo que introducen atenuación física real por distancia.
*   **Tipo de Interfaz:** Cables de fibra terminados en conectores ópticos estándar.
*   **Rol Técnico:** Simulan físicamente el medio de transmisión de la planta externa (OSP). En esta maqueta representan un enlace bidireccional punto a punto de 25 km por sentido. Considerando las propiedades de la fibra, introducen una atenuación natural aproximada de 5 dB al enlace.

### 7. Atenuador Óptico Variable - OVA (Joinwit JW3303)
*   **Función Técnica:** Control manual y dinámico de la pérdida de potencia óptica en el enlace.
*   **Tipo de Interfaz:** Conectores ópticos de entrada y salida para inserción en serie.
*   **Rol Técnico:** Se intercala con los carretes de fibra para añadir atenuación artificial. Permite simular degradaciones en la planta externa (como empalmes deficientes o macrocurvaturas) para probar la sensibilidad de los receptores ópticos de línea, comprobar el margen de potencia del enlace y observar cómo impacta la caída de potencia en el BER.
