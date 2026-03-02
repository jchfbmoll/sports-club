# docker/app/Dockerfile
FROM python:3.13-slim-trixie
EXPOSE 8080

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Set work directory
RUN useradd sportsclubuser
RUN groupadd sportclubgroup

RUN usermod -aG sportclubgroup sportsclubuser

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
RUN pip install --no-cache-dir uvicorn

# Copy project
COPY sportsclub/ .

RUN chown -R sportsclubuser:sportclubgroup /app
# Collect static files (needed when serving through a reverse proxy)
USER sportsclubuser

RUN python manage.py collectstatic --noinput

# Run uvicorn
CMD ["uvicorn", "sportsclub.asgi:application", "--host", "0.0.0.0", "--port", "8080"]
