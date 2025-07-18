# Automatización de Facturación con IA, Power Automate y Power BI

Este proyecto automatiza el procesamiento de correos electrónicos con facturas adjuntas utilizando:

- Power Automate para detectar correos y mover archivos
- FastAPI en Python para analizar si un archivo PDF contiene una factura, dentro de un flujo de Power Automate utilizando HTTP de tipo POST creada por mi
- IA Builder para extraer datos de facturación y completar una grilla sencilla de excel
- Power BI para visualizar los gastos mensuales (de manera local)

## 🧩 Flujo de trabajo

1. 📧 Llega un correo a una casilla monitoreada
2. 📎 Si **no** tiene adjuntos → se guarda en carpeta "No es factura"
3. 📄 Si **tiene adjuntos** → se envían vía POST a una API
4. 🧠 La API detecta si puede ser una factura (por keywords y estructura)
5. ✅ Si es factura → se extraen datos con IA Builder y se cargan a un Excel
6. ❓ Si es dudoso → va a "Tal vez es factura"
7. 📊 Se visualiza todo en Power BI como egresos frente a ingresos fijos

## ⚙️ Herramientas usadas

- Power Automate
- Python (FastAPI, PyPDF2, regex)
- Power BI
- Excel
- IA Builder de Microsoft Power Platform

## 🎯 Objetivo



Reducir el tiempo de control de facturas recibidas por correo y tener una visión clara de su impacto financiero.

## 📷 Capturas

<img width="650" height="400" alt="Screenshot 2025-07-18 151500" src="https://github.com/user-attachments/assets/77deb492-6f81-44b3-8e1d-6e3640bf78f1" />
<img width="650" height="600" alt="Screenshot 2025-07-18 151514" src="https://github.com/user-attachments/assets/9f5a4d17-8a47-4424-bfde-588a98e0083d" />

<img width="800" height="400" alt="Screenshot 2025-07-18 150755" src="https://github.com/user-attachments/assets/eb8066a4-10af-4f29-a138-4f9991c9ec4e" />

