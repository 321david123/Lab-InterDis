# Laboratorio 5 — Red Tec Store (VLSM, router y switch)

| | |
|---|---|
| **Alumno** | David Martinez Rodriguez · A01832275 |
| **UF** | Interconexión de Dispositivos |
| **Campus** | ITESM Querétaro · 5 junio 2026 |
| **Formato** | Reporte |

---

## Qué hicimos en el laboratorio

Tocaba dejar lista la red de una **TEC Store**: VLSM en papel, router, switch y probar que jalara desde Admin01.

Del QR salió **172.19.120.0/21**. Con eso llené la Tabla 01 y los scripts del **RF-TecStore-CHI** y **S-TecStore-CHI**.

### VLSM y tabla de direcciones

Partí el bloque en tres pedazos: **Tienda 802** (/24), **Gerencia 302** (/27) y **Gestión** (/28).

| Segmento | Prefijo | Bloque | Router |
|----------|---------|--------|--------|
| Tienda 802 | /24 | 172.19.120.0 | G0/0/1.802 → .254 |
| Gerencia 302 | /27 | 172.19.121.0 | G0/0/1.302 → .30 |
| Gestión | /28 | 172.19.121.32 | G0/0/1.911 → .46 |

<img src="lab05/img/01-vlsm-tabla-direcciones.png" alt="Hoja VLSM y Tabla 01" width="420" />

### Cablear la estación

Bloc con la tabla a la vista, rollover a consola y Ethernet a la toma. Lo de siempre, pero con IPs de tienda.

<img src="lab05/img/02-mesa-consola-tabla.png" alt="Mesa con consola y tabla de direcciones" width="420" />

### Router y switch

Router: banner, usuario **CIT**, SSH, **G0/0/0** al ISP y subinterfaces dot1Q en **G0/0/1**.

Switch: VLANs, **G0/1 en trunk**, access en F0/8-12 y F0/14-20, SVI en **911** con gateway **172.19.121.30**.

<img src="lab05/img/03-router-script-inicio.png" alt="Script del router" width="420" />

<img src="lab05/img/04-switch-script-inicio.png" alt="Script del switch" width="420" />

<img src="lab05/img/05-switch-vlans-trunk.png" alt="VLANs y trunk" width="420" />

| Parte | Cómo quedó |
|-------|------------|
| Trunk | G0/1 → `switchport mode trunk` |
| Tienda | F0/8-12 → VLAN 802 |
| Gerencia | F0/14-20 → VLAN 302 |
| Gestión | VLAN 911 → .29 / gw .30 |

### Probar que ya jala

Ping a **172.19.121.30** (gw) respondió al 100 %. A **172.19.121.29** (switch) al principio fallaba la mitad; ya estabilizó al terminar la config.

<img src="lab05/img/06-ping-gateway-switch.png" alt="Ping gateway y switch" width="420" />

Después abrí **mitec**, **EL NORTE**, **tecstore.mx** y desde el cel Netflix e Instagram. Todo cargó.

<img src="lab05/img/07-mitec-login.png" alt="mitec" width="420" />

<img src="lab05/img/08-elnorte-web.png" alt="EL NORTE" width="420" />

<img src="lab05/img/09-tecstore-web.png" alt="TEC Store" width="420" />

<img src="lab05/img/10-netflix-busqueda.png" alt="Netflix en Google" width="420" />

<img src="lab05/img/11-instagram-netflix.png" alt="Instagram Netflix" width="420" />

<img src="lab05/img/12-transito-lab.png" alt="Recorrido lab" width="420" />

---

## Validación

| Prueba | Qué pasó |
|--------|----------|
| Ping **172.19.121.30** | 0 % pérdida, ~1 ms |
| Ping **172.19.121.29** | Raro al inicio; luego bien |
| **mitec** / **elnorte** / **tecstore** | Abrían |
| Celular (Netflix, IG) | Cargó normal |

En corto: VLSM reparte las subredes, el router las enruta y el trunk las sube hasia el RF. Si el cálculo sale mal, las IPs se pisan.

---

## Reflexión — accesibilidad e inclusión

Tener la Tabla 01 en papel al lado de la consola me ahorró regresar al PDF cada vez que dudaba de una máscara. El VLSM suena fácil en clase; aquí se nota cuando Gerencia es /27 y Gestión /28 en el mismo bloque. Probar con paginas reales (mitec, tecstore) confirma que no solo hay ping, sino DNS y salida a Internet.
