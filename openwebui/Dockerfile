FROM python:3.11-slim

WORKDIR /app

# Copier le code complet du backend
COPY backend ./backend

# Copier les dépendances
COPY backend/requirements.txt .

# Installer les dépendances système et Python
RUN apt-get update && apt-get install -y --no-install-recommends gcc && \
    pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    apt-get remove -y gcc && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

# Config d'environnement
ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"
ENV PYTHONPATH=/app/backend

EXPOSE 8080

# Lancer le vrai module Python
CMD ["python", "-m", "open_webui"]
