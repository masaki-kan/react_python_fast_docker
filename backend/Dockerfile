FROM python:3.12-slim

WORKDIR /app

# Prisma が実行する Node バイナリは libatomic.so.1 を必要とするため
# slim イメージに不足しているライブラリを追加する
RUN apt-get update \
    && apt-get install -y --no-install-recommends libatomic1 \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

RUN prisma generate

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
