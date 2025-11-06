# ---------- STAGE 1 : Build du frontend ----------
FROM node:18 AS frontend
WORKDIR /app
COPY package*.json ./
COPY vite.config.ts svelte.config.js tailwind.config.js postcss.config.js ./
COPY src ./src
COPY static ./static
RUN npm install && npm run build

# ---------- STAGE 2 : Backend ----------
FROM python:3.11-slim

WORKDIR /app
COPY backend ./backend
COPY requirements.txt ./
RUN apt-get update && apt-get install -y --no-install-recommends gcc && \
    pip install --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt && \
    apt-get remove -y gcc && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

# Copier le build frontend compilé
COPY --from=frontend /app/build ./build

# Variables d'environnement
ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"
ENV PYTHONPATH=/app/backend

EXPOSE 8080

RUN mkdir -p backend/data && chmod 777 backend/data
RUN touch backend/open_webui/CHANGELOG.md

CMD ["python", "backend/open_webui/main.py"]
