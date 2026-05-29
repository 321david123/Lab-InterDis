# Laboratorio 3 — RETO: Franquicia en el campus

| | |
|---|---|
| **Alumno** | [David Martinez Rodriguez] · [A01832275] |
| **UF** | Interconexión de Dispositivos |
| **Campus** | ITESM Querétaro · 28 mayo 2026 |
| **Formato** | Reporte |

<!-- En la vista previa (Cmd+Shift+V) las fotos usan ancho fijo; ajusta el número si las quieres más chicas -->
<style>
  img.evidencia { width: 420px; max-width: 100%; height: auto; display: block; margin: 0.6em 0; }
</style>

---

## Qué vivimos en el laboratorio

Llegamos con un reto concreto: habilitar la red de una franquicia nueva antes de la inauguración. El QR del monitor me asignó el **Router J** y el bloque **`192.168.110.0/25`** — de ahí **X = 110**.

### 1. Conectar la estación

Enchufamos Ethernet a la toma de la mesa y el rollover a la consola del router.

<img class="evidencia" src="lab03/img/01-interconexion-admin01-ethernet.png" alt="Ethernet en Admin01" width="420" />

<img class="evidencia" src="lab03/img/02-cable-rollover-y-toma-red.png" alt="Rollover y toma de red" width="420" />

### 2. Encontrar nuestro equipo en el rack

Entre el patch panel localizamos **J-Con**, **J-FE0/0** y **J-FE0/1**; un piso arriba, el switch **ISP PRIMARIO** es el camino de Internet hacia **G0/0/0**.

<img class="evidencia" src="lab03/img/03-rack-patch-panel-routers.png" alt="Patch panel" width="420" />

<img class="evidencia" src="lab03/img/04-rack-routers-etiquetados.png" alt="Etiquetas por router" width="420" />

<img class="evidencia" src="lab03/img/05-isp-primario-y-routers.png" alt="ISP Primario" width="420" />

<img class="evidencia" src="lab03/img/06-estacion-rollover-ethernet.png" alt="Estación cableada" width="420" />

### 3. Preparar y encender el Router J

Armamos `Configuracion.txt` en Admin01 para pegar comandos más rápido en consola.

<img class="evidencia" src="lab03/img/07-configuracion-txt-parte1.png" alt="Borrador de configuración" width="420" />

Encendimos el router y cableamos **GE 0/0/0** (ISP) y **GE 0/0/1** (LAN).

<img class="evidencia" src="lab03/img/08-router-j-encendido.png" alt="Encendido Router J" width="420" />

<img class="evidencia" src="lab03/img/09-router-j-cables.png" alt="Interfaces GE" width="420" />

Todavía sin configurar, Windows seguía sin Internet — esperable hasta tener IPs y DHCP.

<img class="evidencia" src="lab03/img/10-estado-inicial-sin-red.png" alt="Sin red al inicio" width="420" />

### 4. Configurar el router

Por **Tera Term** dejamos el equipo como **RF-SucursalHmo**: consola con `login local`, usuarios CEO/CIT, dominio y RSA, interfaces **.254** (ISP) y **.126** (LAN), pool **Pool-NFHmo** y ruta default por **G0/0/0**. Corregimos un `network` mal tipeado; al terminar SSH quedó activo y guardamos la config.

| Qué | Valor |
|-----|--------|
| ISP | G0/0/0 → 192.168.110.254/30 |
| LAN + gateway | G0/0/1 → 192.168.110.126/25 |
| DHCP | 192.168.110.0/25, DNS 8.8.8.8, excluye .126 |

<img class="evidencia" src="lab03/img/12-teraterm-configuracion.png" alt="Consola Tera Term" width="420" />

<img class="evidencia" src="lab03/img/13-configuracion-completa-txt.png" alt="Script final" width="420" />

El cable de mesa no va directo al router: pasa por el stack de switches del rack.

<img class="evidencia" src="lab03/img/14-rack-switches.png" alt="Rack de switches" width="420" />

### 5. Comprobar que todo funciona

DHCP en Admin01, `ping` a **192.168.110.126**, navegador en **tec.mx** y en **10.25.20.241** (*Arena Borregos*).

<img class="evidencia" src="lab03/img/11-navegacion-tec-mx.png" alt="tec.mx" width="420" />

<img class="evidencia" src="lab03/img/15-validacion-ping-navegador.png" alt="Ping y Arena Borregos" width="420" />

---

## Validación (Fase 9)

| Prueba | Resultado |
|--------|-----------|
| Ping **192.168.110.126** (G0/0/1) | OK — TTL 255, &lt;1 ms |
| Ping **192.168.110.254** (G0/0/0) | OK |
| Internet (mitec, cnn, cisco) | OK en **tec.mx**; [cnn / cisco si aplica] |
| **10.25.20.241** | OK — web y ping, TTL 61 |

**X = 110** (QR). **.126** excluida del DHCP (es G0/0/1). Ruta `0.0.0.0 → G0/0/0` sale a Internet. `login local` restringe la consola a CEO/CIT.

---

## Reflexión — accesibilidad e inclusión (487 caracteres)

El laboratorio de redes nos permitió pasar del diagrama a una PoC real: consola, rack etiquetado y validación con ping y navegador. La estación Admin01, con mobiliario accesible y tomas visibles, facilitó el trabajo en equipo. El QR para asignar router y direccionamiento redujo errores; las etiquetas J-Con, GE 0/0/0 y colores de cable ayudaron a quienes identifican puertos mejor de forma visual. El entorno “aula sin barreras” hizo más ágil documentar y repetir pasos sin depender solo de memoria oral del profesor.
