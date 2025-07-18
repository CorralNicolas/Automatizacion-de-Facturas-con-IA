# 🐍 API Python - Clasificación de archivos PDF como factura o no

Este archivo contiene una API desarrollada en **FastAPI**, que recibe archivos PDF y determina si tienen características que podrían indicar que son una factura. Fue diseñada para integrarse con un flujo de **Power Automate** mediante una solicitud HTTP tipo `POST`.

---

## 🚀 Tecnologías y librerías utilizadas

- **FastAPI**: framework para crear la API.
- **Uvicorn**: servidor ASGI para ejecutar FastAPI.
- **PyMuPDF (fitz)**: para leer el contenido textual de PDFs.
- **re (expresiones regulares)**: para buscar patrones típicos de facturas (como CUIT, montos, palabras clave, etc).
- **Render.com**: para el despliegue online de la API.
---
## ⚙️ Cómo se usa

## 🌐 API desplegada en producción

La API está disponible públicamente en:

📍 **`https://factura-detector.onrender.com/es_factura/`**

- Se puede acceder a la documentación interactiva en:  
  👉 [https://factura-detector.onrender.com/docs](https://factura-detector.onrender.com/docs)
## 📄 Endpoint disponible

### `POST /es_factura/`

Este es el endpoint principal que recibe un archivo PDF y responde si se trata o no de una factura.

- **Entrada**: archivo PDF (como `application/pdf`)
- **Salida**: JSON con esta estructura:
  ```json
  {
    "es_factura": true,
    "detalles": {
      "palabras_clave_encontradas": [...],
      "tiene_monto": true,
      "tiene_cuit": true
    }
  }

## Api con FastApi
- Ubicado en main.py, y esto utiliza un regimen de puntuacion, que si pasa un Umbral de puntaje, se lo considera una factura
- Tambien requiere un archivo .txt para funcionar
