# ── Stage: runtime ────────────────────────────────────────────────────────────
FROM python:3.11-slim

# Metadatos
LABEL maintainer="Katia Paola Victoria Escalera"
LABEL description="API REST FastAPI – Celsius a Fahrenheit con modelo Keras"

# Variables de entorno
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    MODEL_PATH=/app/modelo_GCtoGFV2.keras

WORKDIR /app

# ── Dependencias del sistema (mínimas para TensorFlow slim) ───────────────────
RUN apt-get update && apt-get install -y --no-install-recommends \
        libgomp1 \
    && rm -rf /var/lib/apt/lists/*

# ── Dependencias Python ───────────────────────────────────────────────────────
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# ── Código fuente ─────────────────────────────────────────────────────────────
COPY main.py .

# ── Modelo (cópialo junto al Dockerfile antes de hacer build) ─────────────────
COPY modelo_GCtoGFV2.keras /app/modelo_GCtoGFV2.keras

# Puerto expuesto
EXPOSE 8000

# ── Comando de arranque ───────────────────────────────────────────────────────
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]