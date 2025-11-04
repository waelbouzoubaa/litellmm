# ===============================
# Dockerfile pour LiteLLM (Backend IA)
# ===============================

FROM python:3.11-slim

WORKDIR /app

# Copier tout le code source
COPY . .

# Installer LiteLLM et dépendances FastAPI
RUN pip install --no-cache-dir -e . &&     pip install --no-cache-dir "litellm[proxy]" fastapi uvicorn

EXPOSE 4000

# Lancer le serveur API
CMD ["uvicorn", "litellm.proxy.proxy_server:app", "--host", "0.0.0.0", "--port", "4000"]
