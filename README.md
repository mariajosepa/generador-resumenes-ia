# 📄 Generador de Resúmenes Ejecutivos (IA + n8n + Docker)

Proyecto final del curso **Tecnologías Emergentes – IA para Desarrolladores**.  
Esta solución integra:

- 🤖 **GitHub Models (gpt-4o-mini)**
- ⚙️ **n8n (automatización de flujos)**
- 🐳 **Docker / docker-compose**
- 🔗 **API externa (Gmail / Slack / Telegram)**

El sistema recibe un **archivo PDF**, extrae su contenido y genera automáticamente un **resumen ejecutivo en máximo 10 puntos**, listo para ser usado o enviado vía API externa.

---

## 🚀 Funcionalidad Principal

1. Subes un PDF al endpoint `/resumen`.
2. n8n extrae el texto del PDF.
3. El texto se envía al modelo **GitHub Models (gpt-4o-mini)**.
4. La IA genera un **resumen ejecutivo** breve y claro.
5. El sistema devuelve la respuesta por:
   - Webhook response  
   - o por correo (si se configura Gmail API)

---

## 🛠 Tecnologías Usadas

- **Docker + docker-compose**
- **n8n** para automatización
- **GitHub Models** como motor de IA
- **API Externa** (Gmail / Slack / Telegram)
- Lectura y extracción de **PDF**

---

## 📂 Estructura del Proyecto

```text
generador-resumenes-ia/
├── README.md
├── n8n/
│   ├── docker-compose.yml
│   ├── workflows/
│   │   └── resumen_pdf.json
│   ├── credentials/
│   │   └── github_models.json.example
│   └── n8n_data/             # generado automáticamente
├── docs/
│   ├── arquitectura.md
│   ├── instalacion.md
│   └── ejemplos/
├── data/
│   └── ejemplo.pdf
├── src/
└── tests/
```

---

## ▶️ Cómo ejecutar el proyecto

### 1️⃣ Levantar n8n y PostgreSQL

Desde la carpeta `n8n/`:

```bash
docker-compose up -d
```

### 2️⃣ Abrir la interfaz de n8n

- URL: http://localhost:5678  
- Usuario: `admin`  
- Contraseña: `admin123`

### 3️⃣ Importar el workflow

- Archivo: `n8n/workflows/resumen_pdf.json`

### 🧪 Probar el endpoint

Envía un PDF al webhook usando `curl`:

```bash
curl -X POST "http://localhost:5678/webhook/resumen" \
  -F "file=@data/ejemplo.pdf"
```

---

## 📊 Arquitectura

Documentación completa en [`docs/arquitectura.md`](docs/arquitectura.md), que incluye:

- Diagrama de flujo
- Componentes
- Secuencia de procesamiento
- Módulos del workflow

---

## 🔐 Credenciales

- Ejemplo: `n8n/credentials/github_models.json.example`
- Define la estructura para configurar GitHub Models en n8n.
- No compartas credenciales reales en el repositorio.

---

## 📦 Integraciones con APIs

Puedes conectar el resumen a servicios como `Gmail`, `Slack` o `Telegram` para enviarlo como:

- Email automático
- Mensaje en un canal
- Notificación instantánea

---

## 🧰 Mejoras Futuras

- Añadir opción de “resumen analítico”.
- Exportar los resúmenes como PDF o Markdown.
- Crear un dashboard simple para cargar documentos.

---

## 👨‍💻 Autor

Proyecto desarrollado para el curso Tecnologías Emergentes – IA para Desarrolladores  
Universidad Javeriana – 2025.
