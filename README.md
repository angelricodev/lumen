# LUMEN

Asistente de voz para el hogar tipo Jarvis. Detecta mi llegada y me saluda con el clima y las noticias, con conversación por voz. Local-first y 100% gratis.

## Puesta en marcha

### Requisitos

- Docker
- Git

### 1. Levantar los servicios

```bash
cd homeassistant
docker compose up -d
```

Home Assistant queda en `http://localhost:8123`.

### 2. Configuración manual en Home Assistant

Estos pasos no se pueden versionar (viven en la base de datos interna de HA), así que hay que hacerlos a mano.

**a. Instalar Browser Mod**

Browser Mod convierte el navegador en un `media_player`, para poder oír a LUMEN sin un parlante físico.

1. Descarga el ZIP de [hass-browser_mod](https://github.com/thomasloven/hass-browser_mod) (botón **Code** → **Download ZIP**).
2. Copia `custom_components/browser_mod/` del ZIP a `homeassistant/config/custom_components/browser_mod/`.
3. Reinicia el contenedor:

```bash
   docker compose restart homeassistant
```

4. En HA: **Configuración → Dispositivos y servicios → Agregar integración → Browser Mod**.

**b. Registrar el navegador**

En el panel **Browser Mod** de la barra lateral, activa **Register** y pon como Browser ID exactamente:

`browser_speaker`

Esto crea la entidad `media_player.browser_speaker`, que la automatización usa como parlante.

**c. Crear el interruptor de presencia**

**Configuración → Dispositivos y servicios → Auxiliares → Crear auxiliar → Alternar**, con el nombre exacto:

`Ángel en casa`

Esto crea `input_boolean.angel_en_casa`, que es el disparador de la automatización.

**d. Configurar la URL interna**

**Configuración → Sistema → Red → Red local**: desactiva **Automático** y escribe:

`http://localhost:8123`

Sin esto, HA le entrega al navegador su IP interna de Docker (`172.x.x.x`), que es inalcanzable desde el host, y el audio nunca carga.

### Probar

Prende `input_boolean.angel_en_casa` desde HA. LUMEN debe saludarte por voz.