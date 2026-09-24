## Diagrama de Conexiones Físicas

imagen drawio

El flujo físico general de la maqueta interconecta los equipos activos (Switch, MTX150x, HT6000) de forma centralizada mediante el panel de parcheo (ODF), aislando la manipulación física de la "planta externa" (Carretes + OVA) y facilitando el análisis mediante los puertos `MON`

## Mapeo de Puertos en ODF
De acuerdo con la etiqueta de conexionado frontal del rack, el ruteo de cables en el panel ODF es el siguiente

| Puerto ODF | Nombre de Conexión | Descripción del Enlace Físico |
| :--- | :--- | :--- |
| **1-A** | MON DWDM 1 | Salida del Puerto de Monitoreo DWDM 1 hacia el OSA |
| **1-B** | MON DWDM 2 | Salida del Puerto de Monitoreo DWDM 2 hacia el OSA |
| **3** | CLI 1 DWDM 1 | Inyección cliente al Tx/Rx DWDM 1 (OTU) |
| **4** | CLI 1 DWDM 2 | Inyección cliente al Tx/Rx DWDM 2 (OTU) |
| **6** | FIBRA 1 | Conexión al Carrete de Fibra Óptica 1 (25 KM) |
| **7** | FIBRA 2 | Conexión al Carrete de Fibra Óptica 2 (25 KM) |
| **9** | LIN DWDM 1 | Conector de Línea MUX/DEMUX DWDM 1 |
| **10** | LIN DWDM 2 | Conector de Línea MUX/DEMUX DWDM 2 |

## Flujo de la Señal End-to-End
La trayectoria que recorre un flujo de datos (ej. Puerto 1 a Puerto 2 del analizador) es la siguiente.

1. **Generación (Cliente TX):** El Analizador VeEX MTX150x transmite las tramas Ethernet L2/L3 desde su Puerto Tx P1.
2. **Conversión O-E-O:** La señal ingresa al puerto `CLI` de la tarjeta OTU en el nodo HT6000 (DWDM 1), se regenera y se modula sobre una portadora láser específica (ej. Canal C21).
3. **Multiplexación:** La señal en el canal WDM ingresa al MUX pasivo, donde se combina con otros posibles canales activos hacia un único hilo por el puerto `LIN DWDM 1`.
4. **Tránsito (OSP):** La señal viaja a través del ODF, atraviesa el Carrete de Fibra 1 (simulando 25 km, con caída inherente de ~5 dB) y pasa por el Atenuador Variable (OVA) si está insertado en el lazo.
5. **Demultiplexación:** La señal atenuada ingresa por `LIN DWDM 2` al nodo remoto HT6000. El DEMUX filtra ópticamente la longitud de onda (C21) y la envía al puerto receptor de su respectiva tarjeta OTU.
6. **Recepción (Cliente RX):** La OTU demodula el canal WDM y lo retorna como señal cliente hacia el Puerto Rx P2 del MTX150x, donde se analizan errores o caídas del flujo.
