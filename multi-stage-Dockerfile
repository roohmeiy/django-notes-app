# Stage 1: Build dependencies
FROM python:3.9-slim AS builder

WORKDIR /app

COPY requirements.txt .
RUN pip install --upgrade pip

# This command downloads and prepares all your Python packages listed in requirements.txt.
# It creates ready-to-install wheel files (.whl) inside a folder called /wheels.
RUN pip wheel --no-cache-dir --wheel-dir /wheels -r requirements.txt

# Stage 2: Final runtime image
FROM python:3.9-slim

WORKDIR /app

# Environment settings
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

COPY --from=builder /wheels /wheels
COPY requirements.txt .

# pip install uses those wheels to install packages fast without rebuilding or downloading anything.
RUN pip install --no-cache-dir --no-index --find-links=/wheels -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
