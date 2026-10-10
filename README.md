# 🌡️ ThermoNet — Conversor Celsius a Fahrenheit con IA

Conversor de temperatura cuya conversión la realiza una **red neuronal** entrenada con TensorFlow/Keras. El modelo se expone mediante una **API REST con FastAPI** y una página web permite deslizar una barra o escribir un valor en °C para ver la predicción en °F en tiempo real.

---

## ✨ Características

- **Modelo Keras** que aprende la conversión °C → °F.
- **API REST** con FastAPI (endpoints `POST` y `GET`) y documentación interactiva con Swagger.
- **Modo de respaldo:** si el modelo no puede cargarse, la API responde con la fórmula matemática e indica la fuente (`model` o `formula`).
- **Interfaz web** con barra deslizante (de -100 °C a 100 °C) y campo numérico, con predicción en tiempo real.
- **Contenedor Docker** para levantar la API sin instalar dependencias.

---

## 🧠 El modelo

| Característica | Detalle |
| -------------- | ------- |
| Entrada | 1 valor (grados Celsius) |
| Arquitectura | `Dense(3)` → `Dense(3)` → `Dense(1)`, todas con activación lineal |
| Parámetros entrenables | 22 |
| Optimizador / pérdida | Adam / error cuadrático medio (MSE) |

Como la relación entre las escalas es lineal (°F = °C × 9/5 + 32), el modelo usa capas lineales.

### 📊 Resultados

Se comparó la salida del modelo contra la fórmula exacta en 401 valores entre **-50 °C y 150 °C**:

| Métrica | Valor |
| ------- | ----- |
| Error medio | < 0.0001 °F |
| Error máximo | 0.0001 °F |

Ejemplos:

| °C | Modelo (°F) | Fórmula (°F) |
| -- | ----------- | ------------ |
| -40 | -40.0000 | -40.0 |
| 0 | 32.0000 | 32.0 |
| 21 | 69.8000 | 69.8 |
| 37 | 98.6000 | 98.6 |
| 100 | 212.0000 | 212.0 |

---

## 🔌 API

| Método | Ruta | Descripción |
| ------ | ---- | ----------- |
| `GET` | `/` | Estado del servicio y del modelo |
| `POST` | `/predict` | Recibe `{"celsius": 21}` y devuelve la predicción |
| `GET` | `/predict/{celsius}` | Lo mismo, pasando el valor en la ruta |

**Ejemplo de respuesta:**

```json
{
  "celsius": 21.0,
  "fahrenheit": 69.8,
  "source": "model"
}
```

La documentación interactiva (Swagger UI) queda disponible en `http://localhost:8000/docs`.

---

## 📸 Capturas

### Interfaz
![Interfaz con 0 °C](docs/interfaz-0c.png)

### Predicciones del modelo
![37 °C a °F](docs/cuerpo-37c.png)
![-40 °C a °F](docs/menos-40.png)

### Documentación de la API
![Swagger UI](docs/swagger.png)

### Prueba de la API
![Prueba de la API](docs/pruebaAPI.png)

---

## 🚀 Cómo ejecutarlo

### 1. Clonar el repositorio

```bash
git clone https://github.com/KPVE2004/thermonet.git
cd thermonet
```

### 2. Levantar la API (desde la carpeta `api`)

**Opción A — Local:**

```bash
cd api
python -m venv venv
venv\Scripts\activate          # En macOS/Linux: source venv/bin/activate
pip install -r requirements.txt
python -m uvicorn main:app --reload
```

**Opción B — Docker:**

```bash
cd api
docker build -t thermonet-api .
docker run -p 8000:8000 thermonet-api
```

La API quedará en `http://localhost:8000`.

### 3. Abrir la interfaz

Con la API corriendo, abre `index.html` en el navegador (doble clic, o con la extensión *Live Server* de VS Code).

---

## 🗂️ Estructura del proyecto

```
├── api/
│   ├── main.py                    # API con FastAPI
│   ├── Dockerfile
│   ├── requirements.txt
│   └── modelo_GCtoGFV2.keras      # Modelo entrenado
├── docs/                          # Capturas de la aplicación
└── index.html                     # Interfaz web
```

---

## 🛠️ Tecnologías

Python · TensorFlow / Keras · FastAPI · Uvicorn · NumPy · Docker · HTML5 · CSS3 · JavaScript

---

## 👩‍💻 Autora

**Katia Paola Victoria Escalera**
