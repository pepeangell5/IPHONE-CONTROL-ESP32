# IPHONE-CONTROL-ESP32 / CYBERDECK S3 APPS

[![Instagram](https://img.shields.io/badge/Instagram-@pepeangell-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/pepeangell)
[![Facebook](https://img.shields.io/badge/Facebook-ESP32--Tools-1877F2?style=for-the-badge&logo=facebook&logoColor=white)](https://facebook.com/esp32-tools)
[![GitHub](https://img.shields.io/badge/GitHub-pepeangell5-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/pepeangell5)
[![Web Flasher](https://img.shields.io/badge/Web_Flasher-Instalar-00C853?style=for-the-badge&logo=googlechrome&logoColor=white)](https://pepeangell5.github.io/IPHONE-CONTROL-ESP32/flasher/)

Firmware para ESP32-S3 creado para el CYBERDECK MINI de PepeAngell. El proyecto esta pensado para videos cortos de ciberseguridad, electronica y hacking etico, con pantallas llamativas y funciones reales dentro de un marco legal y responsable.

El firmware combina un launcher visual, herramientas de diagnostico, control HID USB para PC, control BLE para iPhone, GPS, microSD, generador QR, radio scope pasivo con nRF24 y utilidades de bateria.

> Estado actual: release funcional con codigo fuente, capturas, binarios y web flasher.

## Web flasher

Instalacion desde navegador compatible con Web Serial:

[Abrir CYBERDECK S3 APPS Web Flasher](https://pepeangell5.github.io/IPHONE-CONTROL-ESP32/flasher/)

Tambien puedes usar los archivos de la carpeta `binarios/` con herramientas externas como `esptool.py`, ESP Flash Download Tool o flasheadores compatibles.

## Principios del proyecto

- Usar solamente dispositivos propios o autorizados.
- Priorizar demos visuales, diagnostico y aprendizaje.
- Evitar acciones ocultas, persistencia, robo de datos o automatizacion no consentida.
- Requerir accion fisica del usuario para las funciones HID/BLE.
- Mantener cada app clara, grabable y facil de explicar en video.

## Hardware usado

| Periferico | Funcion | GPIO |
| --- | --- | --- |
| TFT ST7789 240x320 SPI | SCK / MOSI / MISO | 12 / 11 / 13 |
| TFT ST7789 240x320 SPI | CS / DC / RST | 10 / 21 / 14 |
| nRF24 #1 | CE / CSN | 4 / 5 |
| nRF24 #2 | CE / CSN | 6 / 7 |
| microSD SPI dedicado | SCK / MOSI / MISO / CS | 36 / 35 / 37 / 16 |
| GPS NEO-6M UART1 | ESP RX / ESP TX | 18 / 17 |
| Botones | UP / DOWN / OK / BACK | 1 / 2 / 42 / 41 |
| Encoder | CLK / DT / SW | 40 / 39 / 38 |
| Buzzer | Signal | 15 |
| Bateria ADC | VBAT | 9 |

## Controles

- `UP` y `DOWN`: mover seleccion.
- Encoder: mover seleccion.
- `OK`: abrir app o ejecutar accion.
- Switch del encoder: tambien funciona como seleccion.
- `BACK`: regresar.
- `OK` mantenido: regreso alternativo desde menus y apps.

## Galeria

| Inicio | Menu | System Pulse | GPS Rescue |
| --- | --- | --- | --- |
| <img src="img/cyberdeck.JPG" width="180"> | <img src="img/menu.JPG" width="180"> | <img src="img/system_pulse.JPG" width="180"> | <img src="img/gps_rescue.JPG" width="180"> |

| WiFi Radar | WiFi Channels | BLE Pulse | GPS SOS |
| --- | --- | --- | --- |
| <img src="img/wifi_radar.JPG" width="180"> | <img src="img/wifi_chanel.JPG" width="180"> | <img src="img/ble_pulse.JPG" width="180"> | <img src="img/gps_sos.JPG" width="180"> |

| Cyber Demo | SD Explorer | QR Text | Radio Scope |
| --- | --- | --- | --- |
| <img src="img/cyber_demo.JPG" width="180"> | <img src="img/sd_explorer.JPG" width="180"> | <img src="img/qr_text.JPG" width="180"> | <img src="img/radio_scope.JPG" width="180"> |

| Passcode Sim | HID Pad | iPhone Remote | Battery |
| --- | --- | --- | --- |
| <img src="img/passcode.JPG" width="180"> | <img src="img/hid_pad.JPG" width="180"> | <img src="img/iphone_remote.JPG" width="180"> | <img src="img/battery.JPG" width="180"> |

| About |
| --- |
| <img src="img/about_template.JPG" width="180"> |

## Apps incluidas

### SYSTEM PULSE

Dashboard de estado del dispositivo.

- Muestra bateria y porcentaje estimado.
- Muestra actividad GPS por caracteres procesados.
- Muestra satelites detectados por el GPS.
- Sirve como pantalla rapida para confirmar que el hardware esta vivo.

### GPS RADAR / GPS RESCUE

Vista GPS redisenada para hiking, emergencia y ubicacion real cuando el NEO-6M consigue fix.

- Usa UART1 en `RX18 / TX17` a `9600 baud`.
- Pausa los radios nRF24 al entrar para reducir consumo y ruido.
- Muestra estado `sin rx`, `nmea activo` o `coord real`.
- Muestra latitud, longitud, altitud, hora UTC, satelites, HDOP y rumbo.
- Usa coordenadas firmadas en grados decimales para que sean faciles de dictar en una emergencia.
- Incluye brujula visual para rumbo cuando el GPS reporta movimiento.
- Calcula ciudad, municipio, estado y pais aproximado usando una base local offline.
- Puede leer una base de lugares desde microSD en `/APPS/GPS/places.csv`.
- Guarda snapshot en microSD con `OK` en `/APPS/GPS_SNAPSHOT.txt` y `/APPS/GPS/GPS_SNAPSHOT.txt`.

Notas sobre GPS:

- El GPS no entrega nombres de ciudad; solo coordenadas. Los nombres salen de una tabla local editable en el firmware.
- En interior es normal ver `Sats: 0`. Para fix real se recomienda ventana o cielo abierto.
- Si `Chars` no sube rapido, revisar TX del GPS a GPIO18, GND comun, alimentacion y baudrate.
- Para emergencia real, la coordenada decimal `LAT` y `LON` es mas importante que el nombre de ciudad.

#### Base offline de ciudades en microSD

Para que el firmware funcione en America, Espana u otros lugares sin recompilar, crea este archivo en la microSD:

```text
/APPS/GPS/places.csv
```

Formato:

```csv
lat,lng,city,municipality,state,country,radius_km
28.6353,-106.0889,Chihuahua,Chihuahua,Chihuahua,Mexico,65
31.6904,-106.4245,Ciudad Juarez,Juarez,Chihuahua,Mexico,70
40.4168,-3.7038,Madrid,Madrid,Comunidad de Madrid,Espana,45
41.3874,2.1686,Barcelona,Barcelona,Catalunya,Espana,35
```

El firmware busca el punto mas cercano y muestra la ciudad/municipio estimado. `radius_km` define cuando se considera que estas dentro de la zona aproximada.

Recomendacion para publicar el firmware:

- Mantener una base compacta con ciudades principales.
- Para hiking, agregar pueblos, parques, refugios o puntos de referencia cercanos a la ruta.
- Para una base grande, usar datos tipo GeoNames y generar un CSV compacto solo con columnas necesarias.
- Si no existe `places.csv`, el firmware crea una muestra pequena automaticamente.

### WIFI LOCATOR

Escaner pasivo de redes WiFi con radar relativo de intensidad.

- Escanea redes WiFi cercanas en modo STA.
- Muestra lista de SSID, BSSID, canal, cifrado y RSSI.
- Permite seleccionar una red/AP especifico por BSSID.
- Abre un radar visual donde el punto se acerca al centro cuando sube la intensidad.
- Usa animacion fluida con sweep continuo, glow de objetivo, anillos y barra de intensidad.
- Muestra RSSI actual, porcentaje relativo, pico de senal y tendencia.
- Muestra distancia aproximada en metros calculada desde RSSI.
- Indica `ACERCANDOTE`, `ALEJANDOTE`, `ESTABLE` o `BUSCANDO`.
- Guarda historial visual de intensidad para caminar y comparar.
- En el radar, `DOWN` abre `DIR SCAN`, un barrido por sectores.
- `DIR SCAN` mide `FRENTE`, `DERECHA`, `ATRAS` e `IZQUIERDA`, compara dBm y muestra el sector con mayor senal.
- Muestra confianza `ALTA`, `MEDIA`, `BAJA` o `INCIERTA` segun diferencia en dB.
- No se conecta a la red.
- No intenta contrasenas.
- No transmite paquetes de ataque.

Limitacion importante:

El ESP32-S3 tiene una sola antena WiFi, asi que no puede saber la direccion real exacta de una red como una brujula. El radar, los metros aproximados y `DIR SCAN` son relativos: sirven para caminar, girar, comparar intensidad y buscar el punto de mayor senal. Para direccion real o distancia exacta se necesitaria hardware con varias antenas, antena direccional o mediciones con GPS/triangulacion mas avanzada.

### WIFI CHANNELS

Analizador pasivo de saturacion WiFi por canal.

- Escanea redes WiFi cercanas y cuenta cuantas hay en cada canal del 1 al 14.
- Muestra una lista con barras por canal, conteo de redes y mejor RSSI detectado.
- `UP/DOWN` selecciona canal.
- `OK` abre el detalle del canal seleccionado.
- En el detalle muestra SSID, BSSID, RSSI y cifrado de las redes guardadas en ese canal.
- `OK` dentro del detalle re-escanea todos los canales.
- No se conecta a ninguna red.
- No intenta contrasenas.
- Sirve para diagnosticar saturacion y elegir canales menos congestionados.

### BLE DEVICE RADAR

Radar pasivo de anuncios Bluetooth Low Energy cercanos.

- Escanea dispositivos BLE cercanos.
- Muestra nombre cuando el dispositivo lo anuncia; si viene oculto, crea una etiqueta por fabricante, servicio BLE, apariencia o final de MAC.
- Muestra direccion MAC BLE, RSSI, servicios anunciados y potencia TX cuando esta disponible.
- Permite seleccionar un dispositivo por direccion.
- Abre una vista `BLE PULSE` con estado dinamico `CERCA/MEDIA/LEJOS`, medidor vertical, ondas de proximidad limpias, historial RSSI y distancia aproximada.
- Muestra tendencia `ACERCANDOTE`, `ALEJANDOTE`, `ESTABLE` o `BUSCANDO`.
- No se conecta al dispositivo.
- No empareja.
- No lee servicios privados.
- No envia paquetes de ataque.

Limitacion importante:

BLE DEVICE RADAR mide anuncios BLE visibles. Algunos telefonos y accesorios ocultan su nombre o cambian de MAC por privacidad, asi que el firmware usa etiquetas como `Apple BLE`, `HID Device`, `Beacon BLE` o `BLE AA:BB:CC` cuando no hay nombre publico.

### GPS SOS MODE

Pantalla de emergencia pensada para hiking o situaciones donde necesitas dictar ubicacion rapido.

- Muestra `SOS 112/911`, estado `FIX OK/BUSCANDO/SIN RX`, latitud y longitud en grande.
- Muestra altitud, hora UTC, satelites, HDOP, bateria y velocidad RX NMEA.
- Muestra lugar aproximado si la base offline esta disponible.
- `OK` guarda snapshot SOS en `/APPS/SOS_LAST.txt` y `/APPS/GPS/SOS_LAST.txt`.
- Tambien agrega historial en `/APPS/GPS/SOS_LOG.txt`.
- El snapshot incluye mensaje rapido para compartir, coordenadas, DMS, altitud, bateria, satelites, HDOP, lugar aproximado y enlace de Google Maps.

Para emergencia real, las coordenadas decimales `LAT` y `LON` son el dato principal.

### CYBER DEMO LAUNCHER

Modo rapido para grabar reels.

- Lista demos principales con tarjeta destacada, etiqueta de categoria y modo etico visible.
- Al seleccionar una demo muestra cuenta regresiva 3, 2, 1 con pantalla `EN VIVO`.
- `BACK` permite cancelar durante la cuenta regresiva antes de lanzar la demo.
- Lanza WiFi Locator, WiFi Channels, BLE Radar, GPS SOS, Passcode Sim, HID Pad, iPhone Remote, Radio Scope o QR Text.
- Muestra `@esp32_tools` como referencia visual dentro del launcher.

### SD VAULT

Explorador completo para la tarjeta microSD.

- Usa bus SPI dedicado.
- Navega carpetas desde la raiz con `UP/DOWN`, `OK` y `BACK`.
- Muestra carpetas, archivos, tamanos y ruta actual.
- Permite crear carpetas y archivos `.txt` con teclado virtual en pantalla.
- Permite ver archivos de texto con scroll.
- Permite eliminar archivos y carpetas con confirmacion.
- Las carpetas se eliminan de forma recursiva, incluyendo su contenido.
- Si no hay tarjeta o falla el montaje, `OK` reintenta detectar la microSD.

### QR TEXT

Generador QR offline para texto corto.

- Abre un teclado virtual en pantalla.
- Permite escribir letras, numeros, espacios y caracteres utiles para URLs cortas.
- `OK` genera un codigo QR version 4-L directamente en el ESP32.
- El QR codifica texto plano; en iPhone una palabra o frase puede abrirse como busqueda web.
- El QR se muestra con zona blanca de lectura para escanearlo con la camara del telefono.
- Soporta hasta 78 caracteres ASCII para mantenerlo legible en la TFT 240x320.
- No usa WiFi, backend ni microSD.
- `BACK` edita el texto y `OK` en la pantalla QR empieza uno nuevo.

### RADIO SCOPE

Monitor pasivo de actividad en 2.4 GHz usando los nRF24.

- Escanea canales de forma visual.
- Muestra barras de actividad por canal.
- Indica canal pico y energia aproximada.
- Permite pausar y reanudar con `OK`.
- Es una herramienta de visualizacion/diagnostico, no transmite ni interfiere.

### PASSCODE SIM

Simulacion cinematica para videos.

- Muestra intentos visuales durante 15 segundos.
- Termina mostrando el PIN demo fijo `9764`.
- No usa HID.
- No escribe en telefonos.
- No intenta desbloquear dispositivos reales.

Esta app es solo una animacion en pantalla para explicar conceptos de forma visual y segura.

### HID PAD

Modo USB HID para controlar una PC propia con acciones locales y confirmadas desde el CYBERDECK.

El ESP32-S3 se presenta como teclado USB y control multimedia. No ejecuta nada al conectarse; todo requiere seleccionar una opcion y presionar `OK`.

Menus incluidos:

- `ABRIR APPS`: abre CMD, PowerShell, Opera GX, Paint, Bloc de notas y Calculadora.
- `CMD TOOLS`: comandos locales seguros para Windows.
- `POWERSHELL`: comandos locales seguros para PowerShell.
- `MULTIMEDIA`: play/pausa, siguiente, anterior, stop, mute y volumen.
- `DEMO GUIADO`: abre Bloc de notas, escribe un texto en espanol y lanza comandos inofensivos para video.

Comandos CMD incluidos:

- `DETENER` envia Ctrl+C.
- `cls`
- `echo CYBERDECK S3`
- `whoami`
- `hostname`
- `ver`
- `echo %COMPUTERNAME%`
- `echo %PROCESSOR_ARCHITECTURE%`
- `echo %NUMBER_OF_PROCESSORS%`
- `ipconfig`
- `netstat`
- `ping 8.8.8.8`
- `tasklist`
- `driverquery`
- `route print`
- `systeminfo`

Comandos PowerShell incluidos:

- `DETENER` envia Ctrl+C.
- `cls`
- `echo CYBERDECK S3`
- `whoami`
- `hostname`
- `ver`
- `ipconfig`
- `netstat`
- `tasklist`
- `ping 8.8.8.8`
- `driverquery`
- `systeminfo`
- `date`

Multimedia USB:

- Play / pausa.
- Siguiente pista o video.
- Pista o video anterior.
- Stop.
- Mute.
- Volumen live para subir o bajar volumen varias veces sin salir del menu.

### IPHONE REMOTE

Control BLE HID para iPhone. El ESP32-S3 aparece como `CYBERDECK-REMOTE` y debe emparejarse manualmente desde `Configuracion > Bluetooth` en el iPhone.

Funciones principales:

- Pantalla de emparejamiento BLE.
- Apertura de apps usando Spotlight.
- Control multimedia.
- Control de volumen live.
- Acciones de camara.
- Escritura de texto demo en Notas.
- Busqueda web segura para video.

Apps que puede abrir por Spotlight:

- Safari.
- Notas.
- YouTube.
- Spotify.
- WhatsApp.
- Instagram.
- Facebook.
- Configuracion.
- Galeria / Fotos.

Acciones iPhone:

- `HOME` por atajo compatible.
- `APP SWITCH` por atajo compatible.
- `TEXTO DEMO`: abre Notas y escribe una demostracion segura.
- `BUSCAR WEB`: abre Safari y envia una busqueda/URL segura.
- `MULTIMEDIA`: play/pausa, siguiente, anterior, stop, mute y volumen.

Camara iPhone:

- Abrir Camara.
- Disparo remoto de foto usando evento de volumen.
- Start/stop de video usando evento de volumen.
- Pausa de video si iOS lo permite.
- Burst de 3 fotos.
- Timer de 3 segundos.
- Atajos `Cyber Foto` y `Cyber Video` por Spotlight para cambiar de modo de forma confiable usando Shortcuts de iOS.

Limites intencionales:

- No desbloquea iPhones.
- No prueba PINs.
- No salta permisos.
- Requiere emparejamiento aceptado por el usuario.

### BATTERY METER

Medidor visual de bateria Li-ion 1S.

- Lee `VBAT` por ADC en GPIO9.
- Usa divisor resistivo 2.2k / 1k.
- Muestra voltaje aproximado.
- Calcula porcentaje estimado entre 3.25 V y 4.20 V.

### ABOUT

Pantalla final de creditos y redes.

- Instagram: `@pepeangell`.
- Facebook: `ESP32-Tools`.
- GitHub: `pepeangell5`.
- Muestra la mascota: el ajolote de la pantalla de inicio.

## Estructura del proyecto

```text
include/CyberdeckPins.h  Pines del hardware
include/AppInput.h       API comun de botones y encoder
src/AppInput.cpp         Lectura de botones, encoder, debounce y long press
src/main.cpp             Launcher, apps visuales, GPS, SD, HID USB y BLE
platformio.ini           Configuracion ESP32-S3, TFT_eSPI, librerias y USB HID
```

## Dependencias

El proyecto usa PlatformIO con Arduino framework para ESP32-S3.

Librerias principales:

- `TFT_eSPI`
- `RF24`
- `TinyGPSPlus`
- `USB`
- `ESP32 BLE Arduino`
- `SD`
- `SPI`

## Compilar

```powershell
pio run
```

## Subir con PlatformIO

```powershell
pio run -t upload
```

## Metodos de flasheo

### Web flasher

Metodo recomendado para usuarios finales:

[https://pepeangell5.github.io/IPHONE-CONTROL-ESP32/flasher/](https://pepeangell5.github.io/IPHONE-CONTROL-ESP32/flasher/)

Usa Chrome o Edge, conecta el ESP32-S3 por USB, presiona `Install`, selecciona el puerto serial y espera a que termine. La pagina usa `binarios/CYBERDECK-S3-APPS-full.bin`.

### Binarios incluidos

La carpeta `binarios/` contiene:

- `CYBERDECK-S3-APPS-full.bin`: binario final combinado para flasheo desde `0x0`.
- `firmware.bin`: aplicacion principal para flasheo desde `0x10000`.
- `bootloader.bin`: bootloader para `0x0`.
- `partitions.bin`: tabla de particiones para `0x8000`.
- `boot_app0.bin`: binario OTA auxiliar para `0xE000`.

### Flasheo manual con archivos separados

```powershell
esptool.py --chip esp32s3 --baud 460800 write_flash -z 0x0 binarios/bootloader.bin 0x8000 binarios/partitions.bin 0xE000 binarios/boot_app0.bin 0x10000 binarios/firmware.bin
```

### Flasheo manual con binario combinado

```powershell
esptool.py --chip esp32s3 --baud 460800 write_flash -z 0x0 binarios/CYBERDECK-S3-APPS-full.bin
```

## Configuracion de pantalla

La pantalla usada es una ST7789 SPI de 2.8 pulgadas con resolucion 240x320. El firmware dibuja en orientacion horizontal con un canvas de 320x240.

Parametros importantes en `platformio.ini`:

- `ST7789_DRIVER`
- `TFT_WIDTH=240`
- `TFT_HEIGHT=320`
- `TFT_MISO=13`
- `TFT_MOSI=11`
- `TFT_SCLK=12`
- `TFT_CS=10`
- `TFT_DC=21`
- `TFT_RST=14`
- `TFT_INVERSION_OFF=1`

## Release

Este release deja el proyecto listo para publicacion:

- Firmware funcional para ESP32-S3.
- Capturas en `img/`.
- Binarios en `binarios/`.
- Web flasher en `flasher/`.
- README con funciones, hardware, controles, imagenes y modos de flasheo.

## Nota etica

Este firmware esta disenado para aprendizaje, diagnostico, electronica, accesibilidad, control local y demostraciones de ciberseguridad defensiva. Usalo solamente en tus propios equipos, redes y laboratorios, o con autorizacion explicita.

No esta pensado para acceso no autorizado, robo de informacion, evasion de seguridad, persistencia, interferencia de radio o automatizacion contra terceros.
