FROM python:3.11-slim

WORKDIR /app

# Copier uniquement le code backend (car c’est lui qu’on exécute)
COPY backend ./backend
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

ENV OPENAI_API_BASE_URL=http://litellm:4000
ENV OPENAI_API_KEY="fake-key"

EXPOSE 8080

# Lancer le backend principal
CMD ["python", "-m", "backend.app"]
