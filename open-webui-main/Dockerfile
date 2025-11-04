# ===============================
# Dockerfile pour Open WebUI (Frontend + Backend UI)
# ===============================

FROM python:3.11-slim

WORKDIR /app

# Copier tout le code source d'Open WebUI
COPY . .

# Installer les dépendances
RUN pip install --no-cache-dir -r requirements.txt

# Config par défaut : Open WebUI utilise LiteLLM comme backend IA
ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"

EXPOSE 8080

# Lancer le backend FastAPI de l'UI
CMD ["python", "-m", "backend.app"]
