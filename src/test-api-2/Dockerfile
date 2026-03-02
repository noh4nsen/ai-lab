FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1

WORKDIR /app

# Create non-root user
RUN addgroup --system app && adduser --system --ingroup app app

# Install dependencies first for better layer caching
COPY requirements.txt .
RUN pip install -r requirements.txt

# Copy application as non-root owner
COPY --chown=app:app app.py ./app.py

USER app

EXPOSE 8080

CMD ["gunicorn", "--bind", "0.0.0.0:8080", "--workers", "2", "--threads", "4", "--timeout", "60", "app:app"]