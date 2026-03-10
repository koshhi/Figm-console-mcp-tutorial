# Guia paso a paso para instalar Figma Console MCP

Esta guia esta pensada para personas no tecnicas y para un flujo en macOS usando **Codex** como cliente MCP.

## 1. Que vas a instalar exactamente

Antes de empezar, conviene entender las piezas:

- **Figma Desktop**: la aplicacion de Figma para ordenador. Hace falta para que el puente local funcione.
- **Codex**: la app donde el asistente usa herramientas MCP.
- **Node.js**: un componente que permite ejecutar paquetes como `figma-console-mcp`.
- **Figma Console MCP**: el servidor MCP que conecta el asistente con Figma.
- **Figma Access Token**: una clave privada de tu cuenta de Figma para autorizar el acceso.
- **Figma Desktop Bridge**: un plugin que se instala dentro de Figma Desktop para que el servidor pueda leer y modificar cosas en tiempo real.

Resumen simple:

`Codex -> Figma Console MCP -> Figma Desktop Bridge -> tu archivo de Figma`

## 2. Que puedes hacer cuando este todo instalado

Con la instalacion completa, el asistente puede:

- leer componentes, estilos y variables de Figma
- sacar capturas de nodos o pantallas
- inspeccionar selecciones
- crear y editar elementos en Figma
- trabajar con tokens y componentes del sistema de diseño

## 3. Lo que necesitas antes de empezar

Prepara esto:

- un Mac
- una cuenta de Figma
- la app **Figma Desktop** instalada
- la app **Codex** instalada
- conexion a internet para descargar Node.js y el paquete MCP

Tiempo estimado: **10 a 20 minutos**

## 4. Instalar Node.js

### Que hace

Node.js permite ejecutar el paquete `figma-console-mcp`. Sin esto, Codex no puede arrancar el servidor.

### Pasos

1. Abre esta pagina: [https://nodejs.org](https://nodejs.org)
2. Descarga la version **LTS**
3. Ejecuta el instalador y pulsa siempre en continuar
4. Cuando termine, cierra el instalador

### Como comprobarlo

1. Abre la app **Terminal**
2. Escribe este comando:

```bash
node --version
```

3. Si aparece algo como `v20.x.x` o `v22.x.x`, ya esta bien instalado

## 5. Instalar Figma Desktop

### Que hace

La version web de Figma no sirve para esta parte. El plugin puente necesita la app de escritorio.

### Pasos

1. Ve a [https://www.figma.com/downloads/](https://www.figma.com/downloads/)
2. Descarga **Figma for macOS**
3. Instala la aplicacion como cualquier otra app
4. Inicia sesion en Figma Desktop

## 6. Crear tu token personal de Figma

### Que hace

Este token es la llave que permite que el servidor MCP acceda a tu cuenta de Figma con tu permiso.

### Importante

- empieza normalmente por `figd_`
- copialo y guardalo temporalmente
- no lo compartas con nadie

### Pasos

1. Abre la ayuda oficial de Figma sobre tokens personales:
   [https://help.figma.com/hc/en-us/articles/8085703771159-Manage-personal-access-tokens](https://help.figma.com/hc/en-us/articles/8085703771159-Manage-personal-access-tokens)
2. Crea un nuevo token personal
3. Ponle un nombre facil, por ejemplo: `Figma Console MCP`
4. Copia el token cuando Figma lo muestre

## 7. Configurar Codex para que use Figma Console MCP

### Que hace

Aqui le dices a Codex:

- que arranque el servidor `figma-console-mcp`
- con que comando debe hacerlo
- que token debe usar

### Archivo que hay que editar

En macOS, el archivo suele estar en:

```text
~/.codex/config.toml
```

En lenguaje normal, eso significa:

```text
/Users/TU_USUARIO/.codex/config.toml
```

### Si el archivo no existe

Puedes crearlo con un editor de texto plano.

### Que debes pegar

Abre `~/.codex/config.toml` y añade esto:

```toml
[mcp_servers.figma-console]
command = "npx"
args = ["-y", "figma-console-mcp@latest"]
enabled = true

[mcp_servers.figma-console.env]
ENABLE_MCP_APPS = "true"
FIGMA_ACCESS_TOKEN = "PEGA_AQUI_TU_TOKEN"
```

### Que significa cada linea

- `[mcp_servers.figma-console]`: crea la configuracion del servidor MCP
- `command = "npx"`: le dice a Codex que lo ejecute con `npx`
- `args = ["-y", "figma-console-mcp@latest"]`: descarga o usa la ultima version del paquete y la arranca
- `enabled = true`: deja el servidor activado
- `[mcp_servers.figma-console.env]`: define variables privadas para ese servidor
- `ENABLE_MCP_APPS = "true"`: activa interfaces auxiliares del servidor
- `FIGMA_ACCESS_TOKEN = "PEGA_AQUI_TU_TOKEN"`: guarda el token de Figma que has creado

## 8. Reiniciar Codex

### Que hace

Codex solo carga la configuracion MCP al arrancar. Si no reinicias la app, no vera el nuevo servidor.

### Pasos

1. Cierra Codex por completo
2. Abre Codex otra vez

## 9. Instalar el plugin Figma Desktop Bridge dentro de Figma

### Que hace

Este plugin es el puente local entre Figma Desktop y el servidor MCP. Sin el, muchas funciones avanzadas no funcionaran.

### Paso previo: localizar la carpeta del paquete

Abre **Terminal** y ejecuta:

```bash
npx figma-console-mcp@latest --print-path
```

### Que hace ese comando

Muestra en pantalla la carpeta donde `npx` ha guardado el paquete `figma-console-mcp`.

Dentro de esa carpeta encontraras este archivo:

```text
figma-desktop-bridge/manifest.json
```

### Importar el plugin en Figma

1. Abre **Figma Desktop**
2. Ve al menu **Plugins**
3. Entra en **Development**
4. Haz clic en **Import plugin from manifest...**
5. Busca la carpeta que te devolvio el comando anterior
6. Entra en `figma-desktop-bridge`
7. Selecciona `manifest.json`
8. Pulsa en **Open**

### Como sabras que ha ido bien

El plugin aparecera en la lista de plugins de desarrollo con el nombre:

```text
Figma Desktop Bridge
```

## 10. Abrir el plugin en tu archivo de Figma

### Que hace

Al ejecutarlo, el plugin abre una conexion local por WebSocket con el servidor MCP. Dicho de forma simple: ambas partes "se encuentran" y empiezan a hablar entre si.

### Pasos

1. Abre en Figma el archivo con el que quieras trabajar
2. Haz clic derecho dentro del lienzo
3. Ve a **Plugins -> Development -> Figma Desktop Bridge**
4. Deja la ventanita del plugin abierta

### Resultado esperado

Deberias ver una confirmacion de que el bridge esta activo.

## 11. Probar que todo funciona

Haz una prueba simple dentro de Codex con mensajes como estos:

```text
Check Figma status
```

o

```text
Lista mi seleccion actual en Figma
```

o

```text
Crea un frame simple con fondo azul
```

Si responde sin errores y detecta Figma, la instalacion esta correcta.

## 12. Problemas habituales y como resolverlos

### Problema: Codex no detecta el servidor

Que suele pasar:

- el archivo `config.toml` tiene un error
- no has reiniciado Codex
- Node.js no esta bien instalado

Que hacer:

1. revisa que el bloque TOML este bien copiado
2. revisa que el token este pegado entre comillas
3. cierra y abre Codex otra vez
4. prueba `node --version` en Terminal

### Problema: Figma no conecta con el bridge

Que suele pasar:

- el plugin no esta abierto
- se importo una version antigua del plugin
- se cerro la ventana del plugin

Que hacer:

1. abre de nuevo **Figma Desktop Bridge**
2. si sigue fallando, vuelve a importar `manifest.json`
3. deja la ventana del plugin abierta mientras usas Codex

### Problema: el plugin no aparece en Figma

Que suele pasar:

- se eligio mal el archivo
- no se importo `manifest.json`

Que hacer:

1. vuelve a **Plugins -> Development -> Import plugin from manifest...**
2. selecciona exactamente `figma-desktop-bridge/manifest.json`

### Problema: el servidor usa otro puerto y el plugin no engancha

Esto puede ocurrir si ya habia otras instancias abiertas. La version moderna del plugin escanea varios puertos locales automaticamente, pero si usabas una importacion antigua puede que no lo haga.

Que hacer:

1. reimporta el plugin desde `manifest.json`
2. vuelve a abrir **Figma Desktop Bridge**

## 13. Mantenimiento basico

### Actualizar el servidor

No tienes que hacer casi nada. Como se usa:

```text
figma-console-mcp@latest
```

Codex intentara usar la version mas reciente.

### Cuando debes reimportar el plugin

Reimporta `manifest.json` si:

- el plugin deja de conectar
- has cambiado de maquina
- sospechas que estas usando una version antigua del bridge

## 14. Configuracion opcional: MCP oficial de Figma

Esto **no es obligatorio** para usar Figma Console MCP.

Solo tiene sentido si quieres tener tambien el conector oficial de Figma en Codex, por separado.

### Que hace

El MCP oficial de Figma usa otra conexion distinta, basada en OAuth y enlaces de Figma.

### Configuracion opcional en `~/.codex/config.toml`

```toml
[mcp_servers.figma]
url = "https://mcp.figma.com/mcp"
bearer_token_env_var = "FIGMA_OAUTH_TOKEN"
http_headers = { "X-Figma-Region" = "us-east-1" }
```

### Lo importante

- `figma-console` y `figma` son conectores distintos
- para instalar **Figma Console MCP**, lo esencial es todo lo anterior de esta guia
- añade el MCP oficial solo si sabes que lo necesitas

## 15. Checklist final

Antes de darlo por terminado, confirma esto:

- Node.js instalado
- Figma Desktop instalado
- token personal de Figma creado
- `~/.codex/config.toml` configurado
- Codex reiniciado
- plugin **Figma Desktop Bridge** importado
- plugin abierto dentro del archivo de Figma
- prueba simple funcionando desde Codex

## 16. Resumen en una frase

Si quieres la version corta: instala Node.js, crea tu token de Figma, configura `figma-console-mcp` en `~/.codex/config.toml`, importa `figma-desktop-bridge/manifest.json` en Figma Desktop y deja el plugin abierto mientras usas Codex.
