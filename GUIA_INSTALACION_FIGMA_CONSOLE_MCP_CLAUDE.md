# Guia paso a paso para instalar Figma Console MCP usando Claude

Esta guia esta pensada para personas no tecnicas y usa **Claude Desktop** como aplicacion principal.

## 1. Que vas a instalar

Para que Claude pueda trabajar con Figma necesitas estas piezas:

- **Claude Desktop**: la app de Claude en tu ordenador
- **Figma Desktop**: la app de Figma para escritorio
- **Node.js**: lo que permite ejecutar el servidor MCP
- **Figma Console MCP**: el conector que une Claude con Figma
- **Figma Access Token**: la clave privada de acceso a tu cuenta de Figma
- **Figma Desktop Bridge**: un plugin dentro de Figma que permite la conexion local y las acciones avanzadas

Resumen simple:

`Claude Desktop -> Figma Console MCP -> Figma Desktop Bridge -> tu archivo de Figma`

## 2. Para que sirve todo esto

Cuando este montado, Claude podra:

- leer componentes y estilos de Figma
- inspeccionar selecciones
- sacar capturas
- crear y editar elementos en Figma
- ayudarte con tokens, variables y componentes

## 3. Lo que necesitas antes de empezar

Prepara esto:

- un Mac
- una cuenta de Claude
- una cuenta de Figma
- conexion a internet

Y estas apps instaladas:

- **Claude Desktop**
- **Figma Desktop**

Tiempo estimado: **10 a 20 minutos**

## 4. Instalar Node.js

### Que hace

Node.js permite arrancar el paquete `figma-console-mcp`. Sin esto, Claude no puede lanzar el conector.

### Pasos

1. Abre [https://nodejs.org](https://nodejs.org)
2. Descarga la version **LTS**
3. Ejecuta el instalador
4. Pulsa continuar hasta terminar

### Como comprobarlo

1. Abre **Terminal**
2. Ejecuta:

```bash
node --version
```

3. Si ves algo como `v20.x.x` o `v22.x.x`, ya esta correcto

## 5. Instalar Claude Desktop

### Que hace

Es la aplicacion desde la que vas a hablar con Claude y donde se cargan los conectores MCP.

### Pasos

1. Descarga Claude Desktop desde la web oficial de Anthropic
2. Instalala en tu Mac
3. Abrela e inicia sesion

## 6. Instalar Figma Desktop

### Que hace

Necesitas la app de escritorio. La version web no es suficiente para usar el plugin puente.

### Pasos

1. Abre [https://www.figma.com/downloads/](https://www.figma.com/downloads/)
2. Descarga **Figma for macOS**
3. Instala la app
4. Inicia sesion en Figma Desktop

## 7. Crear tu token personal de Figma

### Que hace

Es la clave que autoriza a Figma Console MCP a acceder a tu cuenta.

### Importante

- suele empezar por `figd_`
- copialo cuando aparezca
- guardalo temporalmente
- no lo compartas

### Pasos

1. Abre esta ayuda oficial:
   [https://help.figma.com/hc/en-us/articles/8085703771159-Manage-personal-access-tokens](https://help.figma.com/hc/en-us/articles/8085703771159-Manage-personal-access-tokens)
2. Crea un token personal nuevo
3. Pon un nombre facil, por ejemplo: `Figma Console MCP`
4. Copia el token

## 8. Configurar Claude Desktop

### Que hace

Aqui le dices a Claude Desktop que, cuando arranque, lance el servidor `figma-console-mcp` usando tu token.

### Archivo que hay que editar

En macOS, el archivo suele estar aqui:

```text
~/Library/Application Support/Claude/claude_desktop_config.json
```

En lenguaje normal, sera algo parecido a esto:

```text
/Users/TU_USUARIO/Library/Application Support/Claude/claude_desktop_config.json
```

### Si el archivo no existe

Crealo con un editor de texto plano.

### Que debes pegar

Abre ese archivo y pega este contenido:

```json
{
  "mcpServers": {
    "figma-console": {
      "command": "npx",
      "args": ["-y", "figma-console-mcp@latest"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "PEGA_AQUI_TU_TOKEN",
        "ENABLE_MCP_APPS": "true"
      }
    }
  }
}
```

### Que significa cada parte

- `"mcpServers"`: es la lista de conectores MCP
- `"figma-console"`: el nombre que tendra este conector
- `"command": "npx"`: le dice a Claude que lo ejecute con `npx`
- `"args": ["-y", "figma-console-mcp@latest"]`: descarga o usa la ultima version del paquete y la arranca
- `"FIGMA_ACCESS_TOKEN"`: tu token personal de Figma
- `"ENABLE_MCP_APPS": "true"`: activa funciones adicionales del servidor

## 9. Reiniciar Claude Desktop

### Que hace

Claude Desktop solo lee su configuracion al arrancar. Si no lo reinicias, no detectara el nuevo conector.

### Pasos

1. Cierra Claude Desktop por completo
2. Vuelve a abrirlo

## 10. Localizar el plugin Figma Desktop Bridge

### Que hace

Ahora necesitas encontrar la carpeta del paquete para poder importar el plugin dentro de Figma.

### Pasos

1. Abre **Terminal**
2. Ejecuta:

```bash
npx figma-console-mcp@latest --print-path
```

### Que hace ese comando

Te devuelve la ruta donde esta guardado el paquete.

Dentro de esa ruta veras esta carpeta:

```text
figma-desktop-bridge
```

Y dentro de ella este archivo:

```text
manifest.json
```

## 11. Importar Figma Desktop Bridge en Figma

### Que hace

Este plugin crea el puente local entre Figma Desktop y el servidor MCP que esta usando Claude.

### Pasos

1. Abre **Figma Desktop**
2. Ve a **Plugins**
3. Entra en **Development**
4. Pulsa **Import plugin from manifest...**
5. Ve a la carpeta que obtuviste con el comando anterior
6. Entra en `figma-desktop-bridge`
7. Selecciona `manifest.json`
8. Pulsa **Open**

### Resultado esperado

En la lista de plugins de desarrollo aparecera:

```text
Figma Desktop Bridge
```

## 12. Abrir el plugin dentro de tu archivo de Figma

### Que hace

Cuando lo abres, el plugin se conecta por local al servidor MCP. Ese es el paso que realmente une Figma con Claude.

### Pasos

1. Abre en Figma el archivo con el que quieras trabajar
2. Haz clic derecho en el lienzo
3. Ve a **Plugins -> Development -> Figma Desktop Bridge**
4. Deja la ventana del plugin abierta

### Resultado esperado

Deberias ver que el bridge esta activo.

## 13. Probar que funciona con Claude

Abre Claude Desktop y prueba mensajes simples como:

```text
Check Figma status
```

o

```text
Show my current Figma selection
```

o

```text
Create a simple blue frame in Figma
```

Si Claude responde usando Figma sin error, ya lo tienes listo.

## 14. Problemas habituales

### Problema: Claude no ve el conector

Que suele pasar:

- el archivo JSON tiene un error
- no has reiniciado Claude Desktop
- Node.js no esta bien instalado

Que hacer:

1. revisa que el JSON este bien copiado
2. revisa que no falten llaves o comas
3. revisa que el token este bien pegado
4. cierra y abre Claude Desktop
5. prueba `node --version`

### Problema: Figma no conecta

Que suele pasar:

- el plugin no esta abierto
- la ventana del plugin se ha cerrado
- el plugin se importo hace tiempo y esta desactualizado

Que hacer:

1. vuelve a abrir **Figma Desktop Bridge**
2. deja la ventana abierta
3. si sigue fallando, reimporta `manifest.json`

### Problema: el plugin no aparece en Figma

Que hacer:

1. vuelve a **Plugins -> Development -> Import plugin from manifest...**
2. selecciona exactamente `figma-desktop-bridge/manifest.json`

### Problema: el bridge no engancha porque hay varios puertos

La version actual del plugin busca varios puertos locales automaticamente. Si alguna vez importaste una version antigua, puede fallar.

Que hacer:

1. vuelve a importar `manifest.json`
2. abre otra vez el plugin

## 15. Mantenimiento basico

### Actualizaciones

Como la configuracion usa:

```text
figma-console-mcp@latest
```

Claude intentara usar la version mas reciente del paquete.

### Cuando conviene reimportar el plugin

Hazlo si:

- deja de conectar
- cambias de ordenador
- sospechas que Figma esta usando una version antigua del plugin

## 16. Opcion simple pero limitada: conector remoto de solo lectura

Si solo quieres explorar datos y no crear ni editar diseños, existe una opcion mas simple de solo lectura.

En Claude Desktop:

1. Abre **Settings**
2. Ve a **Connectors**
3. Pulsa **Add Custom Connector**
4. Usa estos datos:
   - **Name:** `Figma Console (Read-Only)`
   - **URL:** `https://figma-console-mcp.southleft.com/sse`

### Que hace esta opcion

Permite leer datos de diseño, pero **no** crear ni modificar cosas en Figma.

### Recomendacion

Si quieres la experiencia completa, usa la instalacion principal de esta guia, no esta opcion reducida.

## 17. Nota opcional: Claude Code

Si algun dia quieres hacerlo con **Claude Code** en vez de Claude Desktop, la configuracion puede añadirse con un comando parecido a este:

```bash
claude mcp add figma-console -s user -e FIGMA_ACCESS_TOKEN=TU_TOKEN -e ENABLE_MCP_APPS=true -- npx -y figma-console-mcp@latest
```

Pero para personas no tecnicas, **Claude Desktop** suele ser la via mas sencilla.

## 18. Checklist final

Confirma esto:

- Node.js instalado
- Claude Desktop instalado
- Figma Desktop instalado
- token personal de Figma creado
- `claude_desktop_config.json` configurado
- Claude Desktop reiniciado
- plugin **Figma Desktop Bridge** importado
- plugin abierto dentro del archivo de Figma
- prueba simple funcionando en Claude

## 19. Resumen en una frase

Instala Node.js, crea tu token de Figma, añade `figma-console-mcp` al archivo `claude_desktop_config.json`, importa `figma-desktop-bridge/manifest.json` en Figma Desktop y deja el plugin abierto mientras usas Claude.
