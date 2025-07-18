# 📊 Power BI - Visualización de gastos por facturas

Este tablero de Power BI forma parte del proyecto de clasificación automática de facturas con Power Automate y una API en Python. El objetivo es mostrar una visualización básica pero funcional de los **gastos por facturas detectadas**, en relación con ingresos fijos estimados.

---

## 🧩 ¿Qué hace este dashboard?

- Lee un **archivo Excel** con las facturas reconocidas por la IA en el flujo de Power Automate.
- Muestra:
  - 🧾 **Listado de facturas registradas**
  - 💰 **Monto total en facturas**
  - 📈 **Diferencia entre ingresos fijos y gastos**
  - 📆 Posible evolución por fechas

<img width="1297" height="713" alt="image" src="https://github.com/user-attachments/assets/b6039fda-7e7f-41ce-b45b-c67a1372801e" />


## 📁 Fuente de datos

- Archivo Excel ubicado localmente: (se puede agregar un flujo mas despues de cada iteracion en PowerAutomate, para actualizar los datos de manera Online)   
- Este Excel se alimenta automáticamente desde Power Automate cuando una factura es reconocida.

<img width="1015" height="218" alt="image" src="https://github.com/user-attachments/assets/cb263de4-40c5-4106-8a9e-aba95002b6ef" />

---

## 📈 Visualizaciones incluidas

- **Tabla simple de facturas** con columnas como:  
  - Numero de factura 
  - Fecha
  - Monto  
  - Emisor

- **Indicadores clave**:
  - Total facturado
  - Ingresos estimados fijos (ej.: $3.000.000)

- Gráfico de evolución mensual si hay campo de fecha

<img width="604" height="302" alt="image" src="https://github.com/user-attachments/assets/9640fe42-2ce1-4d06-80c6-67342022bab8" />


---

## 🧠 Es sencillo pero claro

Aunque el tablero es sencillo, demuestra:

- Capacidad de conectar Power BI con archivos **actualizados automáticamente**
- Buen manejo de visualizaciones básicas y limpieza de datos en Power BI Desktop
