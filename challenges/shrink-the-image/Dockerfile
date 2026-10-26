# Use a lightweight Python runtime image
FROM python:3.13-slim

# Python runtime optimizations
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1

# Set working directory
WORKDIR /app

# Install only production dependencies
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

# Create a non-root user
RUN groupadd --gid 10001 appgroup \
    && useradd --uid 10001 --gid 10001 \
       --create-home appuser

# Copy only the application source
COPY --chown=10001:10001 app.py .

# Switch to non-root user
USER 10001:10001

# Flask/Gunicorn port
EXPOSE 8000

# Start the real application using Gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--workers", "2", "app:app"]

