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

![Flujo Power Automate](<img width="475" height="665" alt="Screenshot 2025-07-18 150643" src="https://github.com/user-attachments/assets/c44e1adf-82e1-49aa-a991-98fbc26f3b64" />
)
![Power BI](<img width="1296" height="725" alt="Screenshot 2025-07-18 150755" src="https://github.com/user-attachments/assets/5d42513c-c60e-42c7-84d2-22b29a4a821b" />
)
