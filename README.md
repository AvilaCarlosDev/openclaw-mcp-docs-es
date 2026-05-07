# 📚 Documentación MCP Servers para OpenClaw

[![Estado](https://img.shields.io/badge/estado-activo-success)](https://github.com/AvilaCarlosDev/openclaw-mcp-docs-es)
[![Licencia](https://img.shields.io/badge/licencia-MIT-blue)](LICENSE)
[![Español](https://img.shields.io/badge/idioma-español-yellow)](README.md)

Documentación completa en español sobre **MCP Servers** para OpenClaw. Aprende cómo configurar, usar y extender las capacidades de tu asistente AI con el protocolo MCP (Model Context Protocol).

---

## 📖 Tabla de Contenidos

- [Introducción](#introducción)
- [¿Qué es MCP?](#qué-es-mcp)
- [Instalación y Configuración](#instalación-y-configuración)
- [Servidores MCP Disponibles](#servidores-mcp-disponibles)
- [Guía Completa](docs/mcp-servers-guia-completa.md)
- [Contribuir](#contribuir)
- [Licencia](#licencia)

---

## Introducción

**MCP (Model Context Protocol)** es un protocolo abierto que permite a los asistentes AI como OpenClaw conectarse con fuentes de datos externas, herramientas y servicios. Con MCP, puedes extender las capacidades de tu asistente más allá del chat básico.

Este repositorio contiene documentación en español para ayudarte a:

- ✅ Entender qué es MCP y cómo funciona
- ✅ Configurar servidores MCP en tu sistema
- ✅ Conectar OpenClaw con herramientas externas
- ✅ Crear tus propios servidores MCP personalizados

---

## ¿Qué es MCP?

MCP es un estándar abierto desarrollado para permitir la comunicación entre asistentes AI y recursos externos. Piensa en ello como un "puente" que conecta tu asistente con:

- 📁 Sistemas de archivos locales
- 🌐 APIs y servicios web
- 💾 Bases de datos
- 🔧 Herramientas de desarrollo
- 📊 Fuentes de datos en tiempo real

---

## Instalación y Configuración

Para comenzar a usar MCP con OpenClaw:

1. **Instala OpenClaw** siguiendo la documentación oficial
2. **Configura tus servidores MCP** en el archivo de configuración
3. **Reinicia el gateway** para aplicar los cambios
4. **Verifica la conexión** con `openclaw gateway status`

📖 Para instrucciones detalladas, consulta la [Guía Completa](docs/mcp-servers-guia-completa.md).

---

## Servidores MCP Disponibles

Algunos de los servidores MCP más populares que puedes usar:

| Servidor | Descripción | Dificultad |
|----------|-------------|------------|
| `filesystem` | Acceso a archivos locales | ⭐ Fácil |
| `github` | Integración con GitHub API | ⭐⭐ Media |
| `postgres` | Conexión a bases de datos PostgreSQL | ⭐⭐ Media |
| `puppeteer` | Automatización de navegador | ⭐⭐⭐ Avanzada |
| `slack` | Integración con Slack | ⭐⭐ Media |

---

## Contribuir

¡Las contribuciones son bienvenidas! Si quieres mejorar esta documentación:

1. Haz un fork del repositorio
2. Crea una rama para tu cambio (`git checkout -b feature/mejora-docs`)
3. Realiza tus cambios y haz commit
4. Push a tu rama y abre un Pull Request

---

## Licencia

Este proyecto está bajo la licencia **MIT**. Consulta el archivo [LICENSE](LICENSE) para más detalles.

---

**¿Tienes preguntas?** Abre un issue en GitHub o consulta la [documentación oficial de OpenClaw](https://github.com/openclaw/openclaw).
