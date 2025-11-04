FROM python:3.11-slim

# Dossier de travail dans le conteneur
WORKDIR /app

# Copier le backend complet et le requirements.txt
COPY backend ./backend
COPY requirements.txt .

# Installer les dépendances système et Python
RUN apt-get update && apt-get install -y --no-install-recommends gcc && \
    pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    apt-get remove -y gcc && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

# Variables d'environnement
ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"

# Pour que Python trouve le module open_webui
ENV PYTHONPATH=/app/backend

# Exposer le port du backend
EXPOSE 8080

# Lancer le vrai module Python (le package open_webui)
CMD ["python", "-m", "open_webui"]
