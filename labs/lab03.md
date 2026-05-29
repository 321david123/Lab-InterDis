# Laboratorio 3 — RETO: Franquicia en el campus

| | |
|---|---|
| **Alumno** | David Martinez Rodriguez · A01832275 |
| **UF** | Interconexión de Dispositivos |
| **Campus** | ITESM Querétaro · 28 mayo 2026 |
| **Formato** | Reporte |

---

## Qué hicimos en el laboratorio

Ese día el reto sonaba a escenario real: una franquicia nueva en el campus y hay que dejar la red lista antes de que abran. Escaneé el QR del monitor y me tocó el **Router J**, con el bloque **192.168.110.0/25**; el tercer octeto es **X = 110** y con eso armé todo lo demás.

### Conectar la estación

Lo primero fue lo de siempre en la mesa: Ethernet a la toma y el rollover a la consola para poder entrar al router sin adivinar.

<img src="lab03/img/01-interconexion-admin01-ethernet.png" alt="Ethernet en Admin01" width="420" />

<img src="lab03/img/02-cable-rollover-y-toma-red.png" alt="Rollover y toma de red" width="420" />

### Buscar nuestro router en el rack

En el rack hay cables por todos lados; sin las etiquetas **J-Con**, **J-FE0/0** y **J-FE0/1** hubiera sido un ratón de cables. También ubiqué el switch **ISP PRIMARIO**, que es por donde sale Internet hacia **G0/0/0**.

<img src="lab03/img/03-rack-patch-panel-routers.png" alt="Patch panel" width="420" />

<img src="lab03/img/04-rack-routers-etiquetados.png" alt="Etiquetas por router" width="420" />

<img src="lab03/img/05-isp-primario-y-routers.png" alt="ISP Primario" width="420" />

<img src="lab03/img/06-estacion-rollover-ethernet.png" alt="Estación cableada" width="420" />

### Preparar y encender el Router J

Para no atorarme en consola, primero escribí los comandos en `Configuracion.txt` y después los fui pegando.

<img src="lab03/img/07-configuracion-txt-parte1.png" alt="Borrador de configuración" width="420" />

Prendimos el router y conectamos **GE 0/0/0** al ISP y **GE 0/0/1** a la LAN.

<img src="lab03/img/08-router-j-encendido.png" alt="Encendido Router J" width="420" />

<img src="lab03/img/09-router-j-cables.png" alt="Interfaces GE" width="420" />

En ese punto Windows seguía sin Internet, y tiene sentido: todavía no había IPs ni DHCP funcionando.

<img src="lab03/img/10-estado-inicial-sin-red.png" alt="Sin red al inicio" width="420" />

### Configurar el router

Entré por **Tera Term**, puse el hostname **RF-SucursalHmo**, aseguré la consola con `login local`, di de alta a CEO y CIT, configuré dominio y RSA, las dos interfaces, el pool **Pool-NFHmo** y la ruta default por **G0/0/0**. Me equivoqué una vez al tipear `network`, lo corregí, vi que SSH quedó activo y guardé la configuración.

| Parte | Cómo quedó |
|-------|------------|
| Salida a Internet | G0/0/0 → 192.168.110.254/30 |
| Red local | G0/0/1 → 192.168.110.126/25 |
| DHCP | 192.168.110.0/25, DNS 8.8.8.8, sin asignar .126 |

<img src="lab03/img/12-teraterm-configuracion.png" alt="Consola Tera Term" width="420" />

<img src="lab03/img/13-configuracion-completa-txt.png" alt="Script final" width="420" />

Ahí entendí mejor el cableado: el de la mesa no llega directo al router, pasa por el stack de switches del rack.

<img src="lab03/img/14-rack-switches.png" alt="Rack de switches" width="420" />

### Probar que ya jala

Activé DHCP en Admin01, hice `ping` a **192.168.110.126**, abrí **tec.mx** y al final entré a **10.25.20.241**; salió la página de *Arena Borregos*.

<img src="lab03/img/11-navegacion-tec-mx.png" alt="tec.mx" width="420" />

<img src="lab03/img/15-validacion-ping-navegador.png" alt="Ping y Arena Borregos" width="420" />

---

## Validación

Lo que pedía la Fase 9 lo fui comprobando así:

| Prueba | Qué pasó |
|--------|----------|
| Ping a **192.168.110.126** (LAN) | Respondió bien, TTL 255 |
| Ping a **192.168.110.254** (ISP) | También respondió |
| Sitios en Internet | **tec.mx** abrió sin problema |
| **10.25.20.241** | Cargó la web y el ping contestó (TTL 61) |

En resumen: **X = 110** salió del QR; **.126** no va al DHCP porque es la IP de la LAN en el router; con `ip route 0.0.0.0 0.0.0.0 G0/0/0` el tráfico hacia fuera sale por el ISP; y `login local` en consola deja pasar solo a CEO o CIT.

---

## Reflexión — accesibilidad e inclusión

Me gustó que no fuera solo teoría: conectar consola, buscar el router en un rack lleno de etiquetas y al final comprobar con ping y navegador se siente como trabajo de verdad. La estación tenía espacio y las tomas a la mano, eso ayuda en equipo. El QR y las etiquetas de colores en los puertos facilitan el lab, sobre todo si te cuesta seguir solo con instrucciones habladas. El aula se siente pensada para que más gente pueda hacer la práctica sin quedarse atrás.
