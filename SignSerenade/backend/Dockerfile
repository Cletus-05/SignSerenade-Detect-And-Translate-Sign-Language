FROM python:3.10-slim

WORKDIR /app

# System dependencies
RUN apt-get update && apt-get install -y \
    libglib2.0-0 \
    libgl1 \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN pip install --upgrade pip

# Copy requirements first (cache-friendly)
COPY requirements.txt .

# Install PyTorch CPU FIRST (key optimization)
RUN pip install --no-cache-dir \
    torch==2.0.1+cpu \
    -f https://download.pytorch.org/whl/torch_stable.html

# Install remaining dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy app code
COPY . .

ENV MODEL_PATH=best(4).pt
ENV PORT=3000

EXPOSE 3000

CMD ["gunicorn", "-b", "0.0.0.0:3000", "app:app"]
