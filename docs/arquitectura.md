# 🏗️ Arquitectura del Sistema – Generador de Resúmenes Ejecutivos

Este documento describe la arquitectura técnica del sistema encargado de recibir un archivo PDF, extraer su contenido y generar automáticamente un resumen ejecutivo usando GitHub Models. Toda la solución se construye con n8n y se ejecuta dentro de contenedores Docker para simplificar la instalación y la portabilidad.

---

## 1. Componentes Principales

### 1.1 n8n (Motor de Automatización)
Herramienta central del sistema. Recibe el PDF, lo procesa como binary data, extrae el texto, realiza la llamada a GitHub Models y devuelve el resultado al usuario. Toda la lógica del workflow reside aquí.

### 1.2 GitHub Models (Servicio de IA)
Se utiliza el modelo `gpt-4o-mini` para generar resúmenes ejecutivos de máximo 10 puntos. n8n envía al modelo el texto extraído del PDF y un prompt que describe el formato deseado.

### 1.3 API Externa (ej. Gmail)
Permite enviar el resumen al usuario mediante Gmail API (o servicios equivalentes como Slack o Telegram), cumpliendo con los requisitos del curso respecto a la integración con una API externa.

### 1.4 PostgreSQL
Base de datos utilizada por n8n para almacenar configuraciones internas, datos de ejecución y credenciales encriptadas. No se expone directamente al usuario final, pero es necesaria para el correcto funcionamiento de n8n.

### 1.5 Docker
Toda la infraestructura se levanta mediante `docker-compose.yml`, lo que garantiza entornos consistentes, reproducibles y fáciles de desplegar.

---

## 2. Flujo General del Sistema

1. El usuario envía un archivo PDF a n8n vía webhook.
2. n8n recibe el archivo como binary data.
3. Se extrae el texto completo del PDF con un nodo de procesamiento.
4. El texto se envía a GitHub Models junto con el prompt de resumen.
5. El modelo devuelve el resumen ejecutivo.
6. n8n entrega el resultado al usuario por el webhook o lo reenvía mediante la API externa configurada.

---

## 3. Arquitectura de Contenedores

El sistema se ejecuta en dos contenedores principales:

- **Contenedor n8n:** aloja la interfaz web, los workflows y la lógica del sistema.
- **Contenedor PostgreSQL:** almacena los datos internos de n8n.

Ambos servicios se levantan con un solo comando desde la carpeta `n8n/`:

```bash
docker-compose up -d
```

---

## 4. Seguridad de Credenciales

- Las credenciales reales (tokens de GitHub Models, claves de Gmail API, etc.) nunca se almacenan en el repositorio.
- Se incluye una plantilla en `n8n/credentials/github_models.json.example` que documenta la estructura necesaria sin exponer datos sensibles.
- Cada entorno debe copiar la plantilla y completar sus propios valores seguros antes de ejecutar los workflows.

---

## 5. Razón del Diseño

La arquitectura se diseñó para:

- Cumplir con los requisitos del curso (Docker, n8n, GitHub Models, API externa).
- Ser simple de ejecutar y mantener en diferentes máquinas.
- Facilitar una demostración clara del flujo de punta a punta.
- Permitir extensiones futuras (nuevos tipos de resumen, más integraciones o diferentes canales de entrega).

