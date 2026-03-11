# README

Este repositorio recoge material de apoyo sobre **Figma Console MCP** y su uso con asistentes como Codex o Claude. Antes de entrar en la instalacion, conviene tener claros algunos conceptos base que suelen aparecer juntos en este tipo de flujos.

## Vision general

En un flujo moderno con IA, la relacion entre conceptos suele ser esta:

`LLM -> Agente -> MCP -> Skills -> Herramientas y acciones reales`

Eso significa:

- el **LLM** aporta la capacidad de entender y generar lenguaje
- el **agente** organiza esa capacidad para ejecutar una tarea con pasos y herramientas
- **MCP** define como ese agente se conecta a sistemas externos
- las **skills** encapsulan instrucciones o procedimientos especializados para resolver tareas concretas

## LLM

Un **LLM** (`Large Language Model`) es un modelo de lenguaje entrenado con grandes volumenes de texto para comprender instrucciones y generar respuestas. Es la base "inteligente" del sistema.

En la practica, un LLM puede:

- responder preguntas
- resumir informacion
- generar codigo o texto
- razonar sobre una tarea

Pero por si solo, un LLM normalmente no actua sobre aplicaciones reales ni mantiene un flujo de trabajo completo. Para eso entra la idea de agente.

## Agente

Un **agente** es una capa que usa un LLM para resolver objetivos mas complejos. No solo responde, sino que puede:

- interpretar una meta
- dividirla en pasos
- usar herramientas
- leer archivos
- ejecutar acciones
- verificar resultados

La diferencia importante es esta:

- un **LLM** genera lenguaje
- un **agente** usa ese lenguaje para operar dentro de un proceso

Por ejemplo, un agente puede leer un repositorio, editar un fichero, consultar una herramienta de Figma y luego producir una respuesta final con cambios reales.

## MCP

**MCP** (`Model Context Protocol`) es un protocolo que permite conectar asistentes o agentes con herramientas externas de forma estructurada.

Su objetivo es que el asistente no solo "sepa cosas", sino que tambien pueda interactuar con sistemas reales como:

- Figma
- bases de datos
- APIs
- sistemas de archivos
- herramientas internas

En este proyecto, **Figma Console MCP** actua como puente entre el asistente y Figma. Gracias a MCP, el agente puede pedir informacion del diseno, inspeccionar componentes o ejecutar ciertas acciones sin depender solo del texto de la conversacion.

## Skills

Las **skills** son paquetes de instrucciones especializadas que le dicen al agente como abordar una tarea concreta.

Una skill puede incluir:

- reglas de trabajo
- pasos recomendados
- referencias
- scripts auxiliares
- convenciones para un dominio concreto

La idea es que el agente no tenga que improvisar siempre desde cero. Si existe una skill adecuada, puede seguir un flujo mas consistente, reutilizable y preciso.

Ejemplos de uso de skills:

- implementar una interfaz desde Figma
- auditar un sistema de diseno
- trabajar con documentacion de OpenAI
- automatizar despliegues o validaciones

## MCP oficial de Figma vs Figma Console MCP

Cuando se habla de "el MCP de Figma", en realidad conviene distinguir dos enfoques distintos:

- el **MCP oficial de Figma** esta orientado sobre todo a **Dev Mode y generacion de codigo**
- **Figma Console MCP** esta orientado sobre todo a **extraccion de datos, depuracion e interaccion operativa con Figma**

### MCP oficial de Figma

El MCP oficial de Figma esta pensado principalmente para transformar disenos en artefactos utiles para desarrollo. Segun la comparacion publicada en el repositorio de `figma-console-mcp`, sus puntos fuertes son:

- generacion de codigo React o HTML a partir de disenos
- generacion de clases Tailwind o CSS
- scaffolding o base inicial de componentes

En otras palabras, su foco principal es ayudar a pasar del diseno a una primera implementacion de codigo.

### Figma Console MCP

`Figma Console MCP` cubre otra capa del trabajo con Figma. Su foco esta en inspeccionar, depurar y extraer informacion mas rica del entorno de diseno. En esa misma comparacion se destacan capacidades como:

- logs en tiempo real desde plugins de Figma
- capturas y depuracion visual
- trazas de errores y monitorizacion en tiempo de ejecucion
- extraccion de datos de diseno en JSON
- funcionamiento tanto en remoto como en local

Ademas, en este repositorio trabajamos precisamente con esta opcion porque resulta especialmente util cuando el asistente necesita leer estructuras de Figma, inspeccionar componentes, revisar variables, o interactuar con el archivo mas alla de la simple generacion de codigo.

### Diferencia practica

La diferencia mas util de recordar es esta:

- si tu objetivo principal es **generar codigo desde un diseno**, el MCP oficial de Figma encaja mejor
- si tu objetivo principal es **depurar, inspeccionar, extraer datos o operar sobre Figma con mas contexto**, `Figma Console MCP` suele ser mas adecuado

Tambien pueden usarse de forma complementaria:

- MCP oficial de Figma para generar una primera base de codigo
- Figma Console MCP para inspeccionar el diseno real, depurar, validar y extraer informacion adicional

Fuente de referencia:

- [Comparacion "vs. Figma Official MCP" en el repositorio de figma-console-mcp](https://github.com/southleft/figma-console-mcp?tab=readme-ov-file#-vs-figma-official-mcp)

## Como se conecta todo

Una forma simple de entenderlo es esta:

1. El **LLM** aporta la capacidad de comprension y generacion.
2. El **agente** convierte esa capacidad en un flujo de trabajo.
3. **MCP** conecta el agente con herramientas externas.
4. Las **skills** le dan procedimientos especializados para resolver mejor cada tipo de tarea.

Juntos, estos elementos permiten pasar de una simple conversacion a una colaboracion real con herramientas, archivos y aplicaciones.

## En el contexto de este repositorio

Este repositorio esta orientado a entender y configurar un entorno donde un asistente pueda trabajar con **Figma** mediante **MCP**. Si estas empezando, las guias de instalacion son el siguiente paso natural:

- [Guia de instalacion de Figma Console MCP para Codex](./GUIA_INSTALACION_FIGMA_CONSOLE_MCP.md)
- [Guia de instalacion de Figma Console MCP para Claude](./GUIA_INSTALACION_FIGMA_CONSOLE_MCP_CLAUDE.md)
