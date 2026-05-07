# 📘 Guía Completa de MCP Servers para OpenClaw

> **Última actualización:** Mayo 2026  
> **Versión:** 1.0.0  
> **Idioma:** Español (España/Latinoamérica)

---

## 📋 Índice

1. [Introducción a MCP](#1-introducción-a-mcp)
2. [Arquitectura del Protocolo](#2-arquitectura-del-protocolo)
3. [Instalación y Configuración](#3-instalación-y-configuración)
4. [Servidores MCP Esenciales](#4-servidores-mcp-esenciales)
5. [Configuración Detallada por Servidor](#5-configuración-detallada-por-servidor)
6. [Creación de Servidores Personalizados](#6-creación-de-servidores-personalizados)
7. [Solución de Problemas](#7-solución-de-problemas)
8. [Mejores Prácticas y Seguridad](#8-mejores-prácticas-y-seguridad)
9. [Recursos Adicionales](#9-recursos-adicionales)

---

## 1. Introducción a MCP

### ¿Qué es MCP?

**MCP (Model Context Protocol)** es un protocolo de comunicación estandarizado que permite a los modelos de lenguaje (LLMs) y asistentes AI como OpenClaw interactuar con sistemas externos de manera segura y controlada.

### ¿Por qué usar MCP?

| Ventaja | Descripción |
|---------|-------------|
| 🔌 **Modularidad** | Conecta y desconecta herramientas según necesites |
| 🔒 **Seguridad** | Cada servidor tiene permisos delimitados |
| 🚀 **Extensibilidad** | Crea tus propios servidores personalizados |
| 📦 **Estandarización** | Un protocolo único para todas las integraciones |
| 🌍 **Comunidad** | Ecosistema creciente de servidores open-source |

### Casos de Uso Comunes

- 📂 **Gestión de archivos**: Leer, escribir y organizar documentos
- 💻 **Desarrollo**: Integración con Git, IDEs, y herramientas de CI/CD
- 📊 **Datos**: Consultas a bases de datos y APIs
- 🤖 **Automatización**: Ejecución de scripts y comandos
- 💬 **Comunicación**: Integración con Slack, Discord, Email

---

## 2. Arquitectura del Protocolo

### Componentes Principales

```
┌─────────────────┐         ┌──────────────────┐         ┌─────────────────┐
│   OpenClaw      │ ◄─────► │   MCP Host       │ ◄─────► │   MCP Server    │
│   (Cliente AI)  │         │   (Gateway)      │         │   (Recurso)     │
└─────────────────┘         └──────────────────┘         └─────────────────┘
```

### Flujo de Comunicación

1. **OpenClaw** solicita acceso a un recurso
2. **MCP Host** valida permisos y enruta la solicitud
3. **MCP Server** ejecuta la operación y devuelve resultados
4. **OpenClaw** procesa la respuesta y genera salida

### Tipos de Operaciones MCP

| Operación | Descripción | Ejemplo |
|-----------|-------------|---------|
| `resources/list` | Listar recursos disponibles | Archivos en un directorio |
| `resources/read` | Leer contenido de un recurso | Leer un archivo .md |
| `tools/list` | Listar herramientas ejecutables | Comandos disponibles |
| `tools/call` | Ejecutar una herramienta | Ejecutar script Python |
| `prompts/list` | Listar prompts predefinidos | Templates de respuestas |

---

## 3. Instalación y Configuración

### Requisitos Previos

- ✅ OpenClaw instalado y configurado
- ✅ Node.js v18+ (para servidores basados en Node)
- ✅ Python 3.9+ (para servidores basados en Python)
- ✅ Acceso a internet para descargar paquetes

### Paso 1: Verificar Instalación de OpenClaw

```bash
openclaw gateway status
```

Debe mostrar el gateway en estado `running`.

### Paso 2: Configurar Servidores MCP

Edita el archivo de configuración de OpenClaw (generalmente en `~/.openclaw/config.json`):

```json
{
  "mcp": {
    "servers": {
      "filesystem": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-filesystem", "/home/carlosdev/docs"],
        "enabled": true
      },
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_TOKEN": "tu_token_aqui"
        },
        "enabled": true
      }
    }
  }
}
```

### Paso 3: Reiniciar Gateway

```bash
openclaw gateway restart
```

### Paso 4: Verificar Conexión

```bash
openclaw mcp status
```

Debe listar los servidores activos.

---

## 4. Servidores MCP Esenciales

### 4.1 Filesystem Server

**Propósito**: Acceso seguro al sistema de archivos local.

**Instalación**:
```bash
npm install -g @modelcontextprotocol/server-filesystem
```

**Configuración**:
```json
{
  "filesystem": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-filesystem", "/ruta/permitida"],
    "enabled": true
  }
}
```

**Operaciones disponibles**:
- `list_directory`: Listar archivos en un directorio
- `read_file`: Leer contenido de archivo
- `write_file`: Escribir en archivo
- `search_files`: Buscar archivos por patrón

### 4.2 GitHub Server

**Propósito**: Integración completa con GitHub API.

**Instalación**:
```bash
npm install -g @modelcontextprotocol/server-github
```

**Configuración**:
```json
{
  "github": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-github"],
    "env": {
      "GITHUB_TOKEN": "ghp_tu_token_personal"
    },
    "enabled": true
  }
}
```

**Operaciones disponibles**:
- `list_repositories`: Listar repositorios del usuario
- `create_repository`: Crear nuevo repositorio
- `read_file`: Leer archivo de un repo
- `write_file`: Crear/actualizar archivo
- `list_issues`: Listar issues de un repo
- `create_issue`: Crear nuevo issue
- `create_pull_request`: Abrir PR

### 4.3 PostgreSQL Server

**Propósito**: Conexión a bases de datos PostgreSQL.

**Instalación**:
```bash
npm install -g @modelcontextprotocol/server-postgres
```

**Configuración**:
```json
{
  "postgres": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-postgres"],
    "env": {
      "DATABASE_URL": "postgresql://usuario:password@localhost:5432/mibase"
    },
    "enabled": true
  }
}
```

**Operaciones disponibles**:
- `query`: Ejecutar consulta SQL de solo lectura
- `list_tables`: Listar tablas disponibles
- `describe_table`: Mostrar estructura de tabla

### 4.4 Puppeteer Server

**Propósito**: Automatización de navegador Chromium.

**Instalación**:
```bash
npm install -g @modelcontextprotocol/server-puppeteer
```

**Configuración**:
```json
{
  "puppeteer": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-puppeteer"],
    "enabled": true
  }
}
```

**Operaciones disponibles**:
- `navigate`: Ir a una URL
- `screenshot`: Capturar pantalla
- `click`: Hacer clic en elemento
- `type`: Escribir texto en input
- `evaluate`: Ejecutar JavaScript en página

### 4.5 Slack Server

**Propósito**: Integración con Slack para mensajería.

**Instalación**:
```bash
npm install -g @modelcontextprotocol/server-slack
```

**Configuración**:
```json
{
  "slack": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-slack"],
    "env": {
      "SLACK_BOT_TOKEN": "xoxb-tu-bot-token",
      "SLACK_TEAM_ID": "T00000000"
    },
    "enabled": true
  }
}
```

**Operaciones disponibles**:
- `list_channels`: Listar canales disponibles
- `post_message`: Enviar mensaje a canal
- `get_thread`: Obtener hilos de conversación
- `add_reaction`: Reaccionar a mensaje

---

## 5. Configuración Detallada por Servidor

### Variables de Entorno Comunes

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `GITHUB_TOKEN` | Token de acceso a GitHub | `ghp_xxxxxxxxxxxx` |
| `DATABASE_URL` | URL de conexión a BD | `postgresql://user:pass@host:5432/db` |
| `SLACK_BOT_TOKEN` | Token de bot de Slack | `xoxb-xxxxxxxxxxxx` |
| `OPENAI_API_KEY` | API key de OpenAI | `sk-xxxxxxxxxxxx` |

### Configuración Avanzada: Timeouts y Reintentos

```json
{
  "mcp": {
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "timeout": 30000,
        "retries": 3,
        "env": {
          "GITHUB_TOKEN": "ghp_xxx"
        }
      }
    },
    "globalTimeout": 60000,
    "maxConcurrentServers": 5
  }
}
```

### Configuración de Permisos

```json
{
  "mcp": {
    "permissions": {
      "filesystem": {
        "allowedPaths": ["/home/carlosdev/docs", "/home/carlosdev/projects"],
        "readOnly": false
      },
      "github": {
        "allowedRepos": ["AvilaCarlosDev/*"],
        "writeAccess": true
      }
    }
  }
}
```

---

## 6. Creación de Servidores Personalizados

### Estructura Básica de un Servidor MCP

```
mi-servidor-mcp/
├── package.json
├── src/
│   └── index.js
└── README.md
```

### package.json de Ejemplo

```json
{
  "name": "@tu-usuario/mi-servidor-mcp",
  "version": "1.0.0",
  "type": "module",
  "bin": {
    "mi-servidor": "./src/index.js"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^1.0.0"
  }
}
```

### index.js de Ejemplo

```javascript
#!/usr/bin/env node

import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';
import {
  ListResourcesRequestSchema,
  ReadResourceRequestSchema,
} from '@modelcontextprotocol/sdk/types.js';

const server = new Server(
  {
    name: 'mi-servidor-personalizado',
    version: '1.0.0',
  },
  {
    capabilities: {
      resources: {},
    },
  }
);

// Manejar solicitud de listar recursos
server.setRequestHandler(ListResourcesRequestSchema, async () => {
  return {
    resources: [
      {
        uri: 'mi-recurso://ejemplo',
        name: 'Ejemplo de Recurso',
        description: 'Un recurso de ejemplo',
        mimeType: 'text/plain',
      },
    ],
  };
});

// Manejar solicitud de leer recurso
server.setRequestHandler(ReadResourceRequestSchema, async (request) => {
  const { uri } = request.params;
  
  if (uri === 'mi-recurso://ejemplo') {
    return {
      contents: [
        {
          uri,
          mimeType: 'text/plain',
          text: 'Contenido de ejemplo del recurso',
        },
      ],
    };
  }
  
  throw new Error(`Recurso no encontrado: ${uri}`);
});

// Iniciar servidor
const transport = new StdioServerTransport();
await server.connect(transport);

console.error('Servidor MCP iniciado correctamente');
```

### Probar tu Servidor

```bash
# Instalar dependencias
npm install

# Hacer ejecutable
chmod +x src/index.js

# Probar localmente
node src/index.js
```

### Publicar en npm (Opcional)

```bash
npm login
npm publish --access public
```

---

## 7. Solución de Problemas

### Problema: Servidor no inicia

**Síntomas**: Error `Connection refused` o timeout.

**Soluciones**:
1. Verifica que el comando sea correcto en la configuración
2. Asegúrate de que el paquete esté instalado globalmente
3. Revisa los logs del gateway: `openclaw gateway logs`
4. Prueba ejecutar el comando manualmente en terminal

### Problema: Permisos denegados

**Síntomas**: Error `Permission denied` al acceder a recursos.

**Soluciones**:
1. Verifica la configuración de `allowedPaths` en filesystem
2. Asegúrate de que los tokens de API sean válidos
3. Revisa los permisos del archivo/directorio en el sistema operativo

### Problema: Timeout en operaciones

**Síntomas**: Operaciones que tardan mucho y fallan.

**Soluciones**:
1. Aumenta el `timeout` en la configuración del servidor
2. Optimiza la operación (ej: usa filtros en queries de BD)
3. Verifica la conectividad de red

### Problema: Variables de entorno no se cargan

**Síntomas**: Error de autenticación a pesar de tener tokens configurados.

**Soluciones**:
1. Reinicia el gateway después de cambiar la configuración
2. Verifica que las variables estén en el formato correcto
3. Usa comillas para valores con caracteres especiales

### Comandos de Diagnóstico

```bash
# Ver estado de servidores MCP
openclaw mcp status

# Ver logs del gateway
openclaw gateway logs --follow

# Probar conexión a servidor específico
openclaw mcp test <nombre-servidor>

# Reiniciar solo el subsistema MCP
openclaw mcp restart
```

---

## 8. Mejores Prácticas y Seguridad

### 🔒 Seguridad

| Práctica | Descripción |
|----------|-------------|
| **Tokens rotativos** | Regenera tokens API cada 90 días |
| **Principio de mínimo privilegio** | Otorga solo los permisos necesarios |
| **Paths delimitados** | Restringe acceso a directorios específicos |
| **Logs de auditoría** | Habilita logging de operaciones sensibles |
| **Actualizaciones** | Mantén servidores MCP actualizados |

### 📝 Configuración Recomendada

```json
{
  "mcp": {
    "servers": {
      "filesystem": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-filesystem"],
        "enabled": true,
        "readOnly": true,
        "allowedPaths": ["/home/carlosdev/docs"]
      }
    },
    "auditLog": "/var/log/openclaw/mcp-audit.log",
    "maxRequestSize": 1048576,
    "rateLimit": {
      "requestsPerMinute": 60,
      "burstLimit": 10
    }
  }
}
```

### ⚡ Rendimiento

- **Cachea resultados** de operaciones frecuentes
- **Usa filtros** en lugar de traer todos los datos
- **Limita concurrent requests** según capacidad del sistema
- **Monitorea uso de memoria** de servidores

### 🧪 Testing

```bash
# Pruebas unitarias de tu servidor
npm test

# Pruebas de integración con OpenClaw
openclaw mcp test-all

# Benchmark de rendimiento
openclaw mcp benchmark --server filesystem
```

---

## 9. Recursos Adicionales

### Documentación Oficial

- 📖 [Especificación MCP](https://modelcontextprotocol.io/specification)
- 📖 [SDK de MCP](https://github.com/modelcontextprotocol/sdk)
- 📖 [OpenClaw Docs](https://github.com/openclaw/openclaw)

### Servidores Oficiales

| Nombre | npm | GitHub |
|--------|-----|--------|
| Filesystem | `@modelcontextprotocol/server-filesystem` | [repo](https://github.com/modelcontextprotocol/servers) |
| GitHub | `@modelcontextprotocol/server-github` | [repo](https://github.com/modelcontextprotocol/servers) |
| PostgreSQL | `@modelcontextprotocol/server-postgres` | [repo](https://github.com/modelcontextprotocol/servers) |
| Puppeteer | `@modelcontextprotocol/server-puppeteer` | [repo](https://github.com/modelcontextprotocol/servers) |
| Slack | `@modelcontextprotocol/server-slack` | [repo](https://github.com/modelcontextprotocol/servers) |

### Comunidad

- 💬 [Discord de MCP](https://discord.gg/modelcontextprotocol)
- 🐦 [Twitter @ModelContext](https://twitter.com/ModelContext)
- 📚 [Awesome MCP Servers](https://github.com/modelcontextprotocol/awesome-mcp-servers)

### Contribuir a esta Guía

¿Encontraste un error o quieres agregar información?

1. Fork este repositorio
2. Crea una rama: `git checkout -b docs/mejora-seccion-X`
3. Edita este archivo
4. Commit: `docs: mejora en sección X`
5. Push y abre Pull Request

---

## 📄 Licencia

Esta guía está bajo licencia **MIT**. Siéntete libre de usar, modificar y distribuir.

**Autor**: Comunidad OpenClaw en Español  
**Mantenimiento**: AvilaCarlosDev  
**Última actualización**: Mayo 2026

---

<div align="center">

**¿Te fue útil esta guía?** ⭐ Dale una estrella al repositorio

[⬆️ Volver al inicio](#-guía-completa-de-mcp-servers-para-openclaw)

</div>
