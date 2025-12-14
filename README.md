# Dataset Agent 🤖📊

https://vimeo.com/1146260053?share=copy&fl=sv&fe=ci

Flujo automatizado de n8n que analiza datasets (CSV o Google Sheets) utilizando IA, generando informes completos con recomendaciones de limpieza, feature engineering y modelos de ML.

## 📁 Estructura de Archivos
```
/
├── Dataset Agent.json          # Flujo de n8n para importar
├── Informe.pdf                 # Informe completo del proyecto
├── correo_de_ejemplo.html      # Ejemplo del correo HTML generado
│
└── Google Drive/
    ├── Datasets de ejemplo/    # 📂 Carpeta INPUT - CSV a analizar
    ├── Docs/                   # 📂 Google Docs con el análisis
    └── Logs/                   # 📂 Google Sheet con historial de ejecuciones
```

### Descripción de Directorios

- **Datasets de ejemplo**: Carpeta monitoreada por el trigger. Al subir un archivo CSV o Google Sheet aquí, el flujo se ejecuta automáticamente (se incluye de base un archivo CSV de prueba con datos climáticos).
- **Docs**: Carpeta donde se guardan los Google Docs con el análisis completo generado por el agente de IA.
- **Logs**: Contiene un Google Sheet que registra todas las ejecuciones del flujo (ID, estado, fecha de inicio/fin, etc.).

## 🗂️ Configuración de Carpetas de Google Drive

Este proyecto incluye la estructura completa de carpetas con archivos de ejemplo. Para que el flujo funcione correctamente, el usuario debe subirlas a su Google Drive y actualizar los IDs en el workflow.

**Pasos para configurar:**

1. **Descarga** la carpeta `Google Drive/` completa de este repositorio
2. **Sube toda la estructura** a tu Google Drive (puedes crear una carpeta raíz llamada "Dataset Agent" y subir todo ahí)
3. **Importa el archivo XLSX a Google Sheets**:
   - Abre el archivo `Dataset Agent - Logs.xlsx` en Google Drive
   - Click derecho → "Abrir con" → "Google Sheets"
   - Esto creará el Google Sheet necesario para los logs

### 2. Obtener los IDs de las Carpetas y del Sheet

Para cada carpeta y el Google Sheet, necesitas obtener su ID único:

**Cómo obtener un ID:**
1. Abre la carpeta o archivo en Google Drive
2. Copia la URL de la barra de direcciones
3. El ID es la parte que aparece después de `/folders/` o `/spreadsheets/d/`

**Ejemplo de URL de carpeta:**
```
https://drive.google.com/drive/folders/1a2b3c4d5e6f7g8h9i0j
                                      └─────────┬─────────┘
                                              ID de la carpeta
```

**Ejemplo de URL de Google Sheet:**
```
https://docs.google.com/spreadsheets/d/1xyz789abc456def/edit
                                       └──────┬──────┘
                                            ID del Sheet
```

### 3. Actualizar los Nodos del Flujo

Una vez importado `Dataset Agent.json` en n8n, debes actualizar los siguientes nodos con tus IDs:

#### 🔹 Nodo: "Cuando se carga un nuevo archivo" (Google Drive Trigger)
- **Campo a modificar**: `Folder`
- **Valor a ingresar**: ID de tu carpeta **"Datasets de ejemplo"**

#### 🔹 Nodo: "Crear Docs"
- **Campo a modificar**: `Folder Name or ID`
- **Valor a ingresar**: ID de tu carpeta **"Docs"**

#### 🔹 Nodo: "Logs" (Google Sheets)
- **Campo a modificar**: `Document by ID`
- **Valor a ingresar**: ID del **Google Sheet de logs** (el archivo XLSX importado)

### 📋 Tabla de Referencia Rápida

| Nodo                             | Campo             | Carpeta/Archivo a Usar       |
| -------------------------------- | ----------------- | ---------------------------- |
| Cuando se carga un nuevo archivo | Folder            | ID de "Datasets de ejemplo/" |
| Crear Docs                       | Folder Name or ID | ID de "Docs/"                |
| Logs                             | Document ID       | ID del Google Sheet de logs  |

## ⚙️ Configuración Requerida

### 1. Credenciales de n8n

Este flujo requiere configurar las siguientes credenciales en su instancia de n8n:

#### 🔹 Google Service Account
**Nodos que la usan:**
- "Cuando se carga un nuevo archivo" (Google Drive Trigger)
- "Descarga del Archivo" (Google Drive)

#### 🔹 Google Sheets OAuth2
**Nodos que la usan:**
- "Extraer Google Sheets"
- "Logs"

#### 🔹 Google Docs OAuth2
**Nodos que la usan:**
- "Crear Docs"
- "Insertar análisis en Docs"

#### 🔹 Gmail OAuth2
**Nodos que la usan:**
- "Enviar por correo"
- "Enviar por correo - Error"

⚠️ **ACCIÓN CRÍTICA:** Cambiar el campo `sendTo` en ambos nodos con su dirección de correo de preferencia.

#### 🔹 OpenAI API
**Nodo que la usa:**
- "OpenAI Chat Model"

**Modelo usado:** `gpt-4.1-mini`

### 2. ⚠️ CORREOS - CONFIGURACIÓN OBLIGATORIA

**Nodos a modificar:**

1. **"Enviar por correo"**
   - Campo: `sendTo`
   - Valor actual: np-1999@hotmail.com
   - **Acción:** Cambiar por su correo de preferencia

2. **"Enviar por correo - Error"**
   - Campo: `sendTo`
   - Valor actual: np-1999@hotmail.com
   - **Acción:** Cambiar por su correo de preferencia

## 📧 Vista Previa del Output

El flujo genera un correo HTML profesional con análisis detallado.

**Ver ejemplo:** Abrir `correo_de_ejemplo.html` en el navegador para ver el formato exacto del correo.

**Características del correo:**
- 🎨 Diseño responsive con gradientes
- 📊 Información estructurada en secciones
- 📋 Análisis completo del dataset
- 🔍 Recomendaciones de limpieza y feature engineering
- 🤖 Modelos de ML sugeridos
- ✨ Footer con branding del proyecto

## 📝 Notas Adicionales

- El análisis se basa en el dataset **completo**, no en muestras
- El agente utiliza GPT-4.1-mini para generar recomendaciones técnicas
- Los outliers se reportan pero se mantienen por defecto (representan eventos reales)
- El flujo registra automáticamente cada ejecución en el Sheet de logs
