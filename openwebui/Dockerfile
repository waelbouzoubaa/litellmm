# ==========================
# Étape 1 : Build du frontend
# ==========================
FROM node:22-alpine AS frontend-builder

WORKDIR /app

# 👇 Copie le dossier openwebui (ton frontend réel)
COPY ./openwebui /app/openwebui

RUN cd /app/openwebui && npm ci && npm run build

# ==========================
# Étape 2 : Backend Python
# ==========================
FROM python:3.11-slim

WORKDIR /app

COPY ./backend ./backend
COPY ./requirements.txt .

# 👇 Copie le build du frontend dans le dossier /app/build
COPY --from=frontend-builder /app/openwebui/build ./build

RUN apt-get update && apt-get install -y --no-install-recommends gcc ffmpeg && \
    pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    apt-get remove -y gcc && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"
ENV PYTHONPATH=/app/backend
ENV PORT=8080

EXPOSE 8080

RUN mkdir -p backend/data && chmod 777 backend/data
RUN touch backend/open_webui/CHANGELOG.md

CMD ["python", "backend/open_webui/main.py"]
