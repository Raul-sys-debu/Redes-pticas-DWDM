# 03 - Manual de Configuración

## Medidas de Seguridad Óptica
*   **Radiación Láser (Clase 1M/3R):** Los láseres DWDM operan en la Banda C (invisible) y pueden causar daño retiniano permanente. Nunca inspeccionar visualmente una fibra encendida. Utilizar siempre un medidor de potencia óptica (OPM) para verificar estado.
*   **Manejo de Conectores:** Es crítico no mezclar conectores UPC (pulido recto, conector azul) con APC (pulido angular, conector verde). Conectarlos entre sí causará alta reflexión (Return Loss) y daño físico irreversible a la férula.
*   **Limpieza:** Inspeccionar las férulas con microscopio y limpiar siempre con alcohol isopropílico o cintas de limpieza antes de realizar la inserción en el ODF o módulos SFP.

## Configuración del Chasis HT6000
1.  **Gestión de Red:** Conectar el PC a la interfaz de gestión (NMS) de los equipos. Las direcciones IP asignadas en la maqueta son `192.168.1.101` para el DWDM 1 y `192.168.1.102` para el DWDM 2.
2.  **Autenticación:** Ingresar mediante el software de gestión utilizando las credenciales predeterminadas (Usuario: `admin`, Contraseña: `admin`).
3.  **Aprovisionamiento de Servicios:** En la tarjeta OTU, configurar los puertos de cliente (Client) con la tasa correspondiente al tráfico a inyectar (ej. 10G LAN).
4.  **Mapeo de Puertos:** Establecer la conexión lógica cruzada asignando el tráfico del puerto cliente al puerto de línea sintonizado en el canal WDM correspondiente (ej. C21 o C22).

## Procedimientos de Prueba y Medición
1.  **Inserción del OVA (JW3303):** Conectar el atenuador óptico variable en serie con los carretes de 25 KM a través del ODF[cite: 2, 6]. Ajustar la atenuación simulando fallas en la planta externa para medir sensibilidad de recepción óptica.
2.  **Medición Espectral:** Conectar el Analizador de Espectro Óptico (RXT4510, IP: `192.168.1.202`) al puerto `MON` del módulo DWDM. Escanear la Banda C para validar las potencias pico (dBm), longitudes de onda (nm) y OSNR.
3.  **Pruebas Ethernet:** Con el MTX150x conectado en los puertos cliente, inyectar tráfico L2/L3 y ejecutar pruebas estandarizadas como RFC 2544. Comprobar *Throughput*, pérdida de tramas y medir el Bit Error Rate (BER) conforme se eleva la atenuación en el OVA.
