# ==========================
# Étape 1 : Build du frontend
# ==========================
FROM node:22-alpine AS frontend-builder

WORKDIR /app
COPY . .

# Installe et build le frontend
RUN cd open_webui/frontend && npm ci && npm run build

# ==========================
# Étape 2 : Build du backend Python
# ==========================
FROM python:3.11-slim

WORKDIR /app

# Copie le backend et les dépendances
COPY backend ./backend
COPY requirements.txt .

# Copie le build frontend depuis la première étape
COPY --from=frontend-builder /app/open_webui/frontend/dist ./build

# Installe dépendances
RUN apt-get update && apt-get install -y --no-install-recommends gcc ffmpeg && \
    pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    apt-get remove -y gcc && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

# Variables d'environnement
ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"
ENV PYTHONPATH=/app/backend
ENV PORT=8080

EXPOSE 8080

# Crée le dossier data et changelog
RUN mkdir -p backend/data && chmod 777 backend/data && \
    touch backend/open_webui/CHANGELOG.md

# Démarrage du serveur FastAPI
CMD ["python", "backend/open_webui/main.py"]
