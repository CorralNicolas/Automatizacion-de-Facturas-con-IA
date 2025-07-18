# 📧 Flujo de Power Automate - Clasificación y Análisis de Correos con Facturas

Este flujo automatiza la gestión de correos entrantes para clasificar si contienen facturas o no, y extraer datos de aquellas que sí lo son.

---

## 🎯 Objetivo

1. Detectar correos entrantes.
2. Verificar si contienen archivos adjuntos.
3. Clasificar los correos en:
   - ❌ Sin adjuntos → carpeta "No es factura"
   - ✅ Con adjuntos → se envían a una API para determinar si el archivo es una factura
     - 📄 Si lo es → se extraen datos con IA Builder y se agregan a un Excel
     - ❓ Si no lo es claramente → se mueve a carpeta "Tal vez es factura"

---

## 🔄 Flujo General

```text
[📥 Nuevo correo recibido]
       ↓
[📎 ¿Tiene adjuntos?]
 ┌────────────┬─────────────┐
 ↓            ↓
❌ NO         ✅ SÍ
↓             ↓
Guardar en    Para cada adjunto:
"No es        → Obtener contenido
factura"      → Llamar a API
              → ¿Es factura?
                 ┌──────┬─────────────┐
                 ↓      ↓
              ✅ SÍ    ❌ NO
              ↓         ↓
Extraer info  Guardar en
con IA        "Tal vez es factura"
y agregar a Excel
```
## 🧠 EXPLICACIÓN PASO A PASO

---

### 1️⃣ Disparador: Nuevo correo recibido

- **Acción**: `Cuando llega un nuevo correo (V3)`
- **Configuración**:
  - Carpeta: Bandeja de entrada  
  - Incluir adjuntos: ✅
  - Aca pedimos que tenga datos adjuntos, para poder avanzar, sino, el bucle no se ejecuta, lo cual no rompe nada

<img width="200" height="120" alt="Screenshot 2025-07-18 152818" src="https://github.com/user-attachments/assets/54671a22-77fd-4a71-8dbf-3ff717e73275" />


---

### 2️⃣ Control: Aplicar a cada uno

- **Acción**: `Control`
- **Expresión**:
  - Utilizamos esto por si un mail, contiene mas de un archivo adjunto, y realizar el flujo tantas veces como ajuntos tengamos
 
  <img width="200" height="120" alt="Screenshot 2025-07-18 153317" src="https://github.com/user-attachments/assets/4dd53c98-9aab-4994-a5b8-5c7b45b59b59" />

### 3️⃣ Control: Condicion

- **Acción**: `Control`
- **Expresión**:
  - Si este archivo adjunto, es del tipo .pdf, entra al bucle del true, sino al False. Esto para evitar consumir datos en cosas que sabemos que no son facturas
 
    <img width="230" height="200" alt="image" src="https://github.com/user-attachments/assets/789cd1c6-3d3d-4bac-982a-d35e11585a36" />

### 4️⃣ FALSE Accion: Crear un archivo
- **Acción**: `Crear archivo`
- **Expresión**:
  - Aca en caso de que no sea un pdf, se lo mueve a una carpeta llamada "NO ES FACTURA", con utilizando el nombre su fecha, el nombre de quien lo mando y al final NO ES FACTURA
  - Aclaracion: importante que aca el formato del pdf no se lo guarda como tal, para eso es importante utlizar el base64() pero porque estoy utilizando onedrive. Capaz con otras plataformas no es importante

  <img width="900" height="350" alt="image" src="https://github.com/user-attachments/assets/941d8ed9-6d65-4cda-a242-46c86f2f9696" />

### 4️⃣ TRUE Accion: HTTP
- **Acción**: `Devolver un Bool`
- **Expresión**:
  - Aca se utiliza una Api en Python hecha por mi y la IA de OpenAi, que lo que hace es leer un pdf, y decidir si es factura por palabras clave que dice "Monto", "Fecha", "Total", etc
  - La api esta hecha con la libreriria FastApi de python, y ES IMPORTANTE que se la detecta perfectamente si no es una imagen, sino que tiene que estar en el formato multipart/form-data .
  - Si no, habria que decodificarla, y con imagenes no funciona, solo con formato .pdf
  - Por eso si cumple la condicion, se lo envia a la carpeta "ES FACTURA", y si no, a la carpeta "TAL VEZ ES FACTURA".
  - La api esta ya desplegada en https://factura-detector.onrender.com/es_factura/.
  - IMPORTANTE!!! El body para que lea esta api es complejo, pero funciona, que es exactamente asi:
    {
  "$content-type": "multipart/form-data",
  "$multipart": [
    {
      "headers": {
        "Content-Disposition": "form-data; name=\"file\"; filename=\"@{items('Aplicar_a_cada_uno')?['Name']}\"",
        "Content-Type": "application/pdf"
      },
      "body": @{base64ToBinary(items('Aplicar_a_cada_uno')?['ContentBytes'])}
    }
  ]
}
  - 
  <img width="1606" height="361" alt="image" src="https://github.com/user-attachments/assets/a196d84d-5438-4739-9591-32fb46541bd5" />

### 5️⃣ Control: Condicion 
- **Acción**: `Condicion`
- **Expresión**: Si la api da true, abri el flujo para ver si es factura o si tal vez lo sea

 <img width="1578" height="234" alt="image" src="https://github.com/user-attachments/assets/3d870d50-5b68-4a31-844c-1d8369997778" />

### 6️⃣ Accion: Crear Archivo 
- **Acción**: `Crear`
- **Expresión**: Si es true, crear archivo en carpeta "ES FACTURA", sino, en carpeta "TAL VEZ ES FACTURA"
  <img width="640" height="265" alt="image" src="https://github.com/user-attachments/assets/5c36042b-1115-4334-9200-cb582eb4c65b" />

### 7️⃣ Accion: IA Builder de Power Automate
 - **Acción**: `Extraer informacion`
- **Expresión**: Ahora que sabemos que es factura, sacamos la informacion importante con la IA de PowerAutomate, para saber Monto, Fecha, Cliente y Numero de Factura.
- Importante! Extraer la informacion tiene que estar en base64ToBinaru(), sino puede tener errores 
<img width="1233" height="197" alt="image" src="https://github.com/user-attachments/assets/78dab543-d31a-498c-a0ca-82bc4bce7f4a" />

### 8️⃣ Accion: Agregar fila a Excel
- **Acción**: `Agregar fila a Excel`
- **Expresión**: Con la informacion que sacamos, lo agregamos automaticamente a una Tabla, de un archivo ya subido Online en un Onedrive, y se actualizara cada vez que llegue un nuevo correo
- Aca se puede poner con que confianza se extraen los datos, para que sea mas facil corroborar que este bien, ya que la IA es muy eficiente, pero puede tener errores

<img width="1236" height="350" alt="image" src="https://github.com/user-attachments/assets/b06945fe-69b0-4d05-a1cb-45003cf7ef52" />

###  LISTO!!!
Este proceso tarda aproximadamente 20 segundos por archivo adjunto, y no consume tanto (para ser mas robustas, necesitan si o si la version paga)
  
