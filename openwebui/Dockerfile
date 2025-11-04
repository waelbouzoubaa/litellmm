FROM python:3.11-slim

# Dossier de travail
WORKDIR /app

# Copier le backend et les dépendances
COPY backend ./backend
COPY requirements.txt .

# Installer dépendances système + Python
RUN apt-get update && apt-get install -y --no-install-recommends gcc && \
    pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    apt-get remove -y gcc && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

# Variables d'environnement
ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"
ENV PYTHONPATH=/app/backend

EXPOSE 8080

# ✅ Créer le dossier base de données et donner les droits
RUN mkdir -p backend/data && chmod 777 backend/data

# ✅ Créer un changelog vide pour éviter l’erreur du fichier manquant
RUN touch backend/open_webui/CHANGELOG.md

# ✅ Démarre l'app FastAPI
CMD ["python", "backend/open_webui/main.py"]