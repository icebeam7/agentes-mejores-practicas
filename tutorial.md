## Paso 1. Creación y configuración de recursos:

- Crea un recurso de Foundry en el Portal de Azure

![Foundry en Azure](https://github.com/user-attachments/assets/f1a8a33d-4562-4d4b-b97b-4f8aa00129da)

![Datos del recurso](https://github.com/user-attachments/assets/ead1a891-f68f-44b0-99c2-4d41000b9b61)

- Da clic en Ir al Portal de Foundry y elige la experiencia anterior

- Copia el valor de endpoint (punto de conexion) en Azure OpenAI

![Endpoint](https://github.com/user-attachments/assets/867f25fb-76b5-4696-8d77-3ae2b1c33173)

- Implementa un modelo (gpt-4.1) 

## Paso 2. Código

- Crea un nuevo repositorio (con el nombre que desees). De preferencia, incluye un archivo .gitignore de Dotnet.

Crea un archivo llamado `.devcontainer/devcontainer.json` con el siguiente contenido:

```
{
	"name": "C# (.NET)",
	"image": "mcr.microsoft.com/devcontainers/dotnet:10.0",
	"features": {
		"ghcr.io/devcontainers/features/azure-cli:1": {
			"version": "latest",
			"bicepVersion": "latest"
		}
	}
}
```

Luego, crea un Codespace a partir de él. Ejecuta el siguiente comando en la terminal para iniciar sesión en Azure:

```
az login
```

Ahora crea un proyecto de .NET que se usará para crear un Agente Navideño.

En la Terminal:

```
dotnet new console -n AgenteApp -o .
```

Agrega paquetes en la Terminal:

```
dotnet add package Azure.AI.OpenAI --version 2.7.0-beta.2
dotnet add package Azure.Identity --version 1.17.1
dotnet add package Microsoft.Agents.AI --version 1.0.0-preview.251204.1
dotnet add package Microsoft.Agents.AI.OpenAI --version 1.0.0-preview.251204.1
```

Crea los siguientes archivos (con sus carpetas respectivas):

* assets/datos.json
* tools/Navidad.cs
* Program.cs

Obtén el código de los archivos desde este repositorio: [Agente Navideño](https://github.com/icebeam7/agente-navideno).

Consideraciones:

### Diseño de Prompts y Herramientas

Un buen agente no es solo un prompt largo, es una combinación de:

- Instrucciones claras
- Herramientas bien definidas
- Límites explícitos
- Guardrails semánticos

Ejemplo:

```
Rol:
Eres un agente de apoyo a decisiones para [dominio].

Objetivo:
Ayudar al usuario a entender opciones y recomendar acciones basadas en datos disponibles.

Límites:
- No inventes información
- Si faltan datos, solicita aclaraciones
- No tomes decisiones finales
- Si la solicitud del usuario es de otro dominio/tema, declina comentar al respecto

Formato de respuesta:
- Resumen corto
- Opciones
- Recomendación
```

### Herramientas (Tools / Actions) 

Las herramientas reducen alucinaciones:

- Base de conocimientos (documentos o base interna)
- Funciones (tus propias funciones, APIs externas)

Un agente decide cuándo usar una herramienta. 

Prompts + tools = comportamiento confiable

### Evaluación y Métricas

Si no lo evalúas, no es un agente. Un agente no se evalua únicamente con prompts:

- Dataset de preguntas y respuestas esperadas (Ground Truth Dataset)
- Respuestas del agente
- Evaluadores automáticos

Métricas que se pueden evaluar:

- Groundedness (¿usa la info correcta?)
- Relevance
- Coherence
- Safety
- ¡Y más!

### Seguridad

La seguridad no es una característica extra, **es parte del diseño del agente**.

Ejemplos de reglas: 

- “El agente no responde temas fuera del dominio”
- “No genera contenido sensible”

Considera:

- Content filters
- Instrucciones del sistema como guardrails
- Uso de data boundaries

### Costos

Un agente eficiente es mejor que uno muy inteligente pero caro

Lo siguiente tiene impacto en el costo de la solución:

- Prompts largos
- Respuestas extensas
- Uso excesivo de tools

Buenas prácticas

- Prompts concisos
- Respuestas con formato
- Evaluar costo vs valor del agente
