# ===============================
# ÉTAPE 1 : Build du frontend
# ===============================
FROM node:22-alpine AS frontend-builder

WORKDIR /app/openwebui

# Copier les fichiers nécessaires du frontend
COPY ./openwebui/package*.json ./
RUN npm ci --legacy-peer-deps

COPY ./openwebui ./
RUN npm run build


# ===============================
# ÉTAPE 2 : Backend Python (FastAPI)
# ===============================
FROM python:3.11-slim

WORKDIR /app

# Copier le backend et requirements
COPY ./backend ./backend
COPY ./requirements.txt .

# Copier le build du frontend
COPY --from=frontend-builder /app/openwebui/build ./build

# Installer les dépendances système et Python
RUN apt-get update && apt-get install -y --no-install-recommends gcc ffmpeg && \
    pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    apt-get remove -y gcc && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

# Variables d’environnement
ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"
ENV PYTHONPATH=/app/backend
ENV PORT=8080

# Préparer la base de données
RUN mkdir -p backend/data && chmod 777 backend/data
RUN touch backend/open_webui/CHANGELOG.md

EXPOSE 8080

# Commande de démarrage
CMD ["python", "backend/open_webui/main.py"]
