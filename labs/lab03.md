# Laboratorio 3 — RETO: Franquicia en el campus

| | |
|---|---|
| **Alumno** | David Martinez Rodriguez · A01832275 |
| **UF** | Interconexión de Dispositivos |
| **Campus** | ITESM Querétaro · 28 mayo 2026 |
| **Formato** | Reporte |

---

## Qué hicimos en el laboratorio

Ese día el reto sonaba a escenario real: una franquicia nueva en el campus y hay que dejar la red lista antes de que abran. 

### Conectar la estación

Lo primero fue lo de siempre en la mesa: Ethernet a la toma y el rollover a la consola para poder entrar al router sin adivinar.

<img src="lab03/img/01-interconexion-admin01-ethernet.png" alt="Ethernet en Admin01" width="420" />

<img src="lab03/img/02-cable-rollover-y-toma-red.png" alt="Rollover y toma de red" width="420" />

### Buscar nuestro router en el rack

Busqué los puertos que se necesitaban conectar (con un poco de dificultad)  

<img src="lab03/img/03-rack-patch-panel-routers.png" alt="Patch panel" width="420" />

<img src="lab03/img/04-rack-routers-etiquetados.png" alt="Etiquetas por router" width="420" />

<img src="lab03/img/05-isp-primario-y-routers.png" alt="ISP Primario" width="420" />

<img src="lab03/img/06-estacion-rollover-ethernet.png" alt="Estación cableada" width="420" />

### Preparar y encender el Router J

Para hacerlo todo de una, primero escribí los comandos en un archivo de texto aunque no tuviera acceso aun bien a la consola.

<img src="lab03/img/07-configuracion-txt-parte1.png" alt="Borrador de configuración" width="420" />

Prendimos el router y conectamos **GE 0/0/0** al ISP y **GE 0/0/1** a la LAN.

<img src="lab03/img/08-router-j-encendido.png" alt="Encendido Router J" width="420" />

<img src="lab03/img/09-router-j-cables.png" alt="Interfaces GE" width="420" />

En ese punto Windows seguía sin Internet

<img src="lab03/img/10-estado-inicial-sin-red.png" alt="Sin red al inicio" width="420" />

### Configurar el router

Entré por **Tera Term**, (finalmente pude, se conectaba automaticamente esa compu) y pegue la configuración


<img src="lab03/img/12-teraterm-configuracion.png" alt="Consola Tera Term" width="420" />

<img src="lab03/img/13-configuracion-completa-txt.png" alt="Script final" width="420" />

Ahí entendí mejor el cableado al revisar las conexiones de los demás

<img src="lab03/img/14-rack-switches.png" alt="Rack de switches" width="420" />

### Probar que ya jala

Logré hacer el `ping` a **192.168.110.126**, abrí **tec.mx** y al final entré a **10.25.20.241**; salió la página de Arena Borregos.

<img src="lab03/img/11-navegacion-tec-mx.png" alt="tec.mx" width="420" />

<img src="lab03/img/15-validacion-ping-navegador.png" alt="Ping y Arena Borregos" width="420" />

---

## Validación

Lo que pedía la Fase 9 lo fui comprobando así:

| Prueba | Qué pasó |
|--------|----------|
| Ping a **192.168.110.126** | Respondió bien|
| Ping a **192.168.110.254** | También respondió |
| Sitios en Internet | **tec.mx** abrió sin problema |
| **10.25.20.241** | Cargó la web |

---

## Reflexión — accesibilidad e inclusión

Estuvo bien la práctica aunque tardé en encontrar el error de la conexión. 
