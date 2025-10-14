# ---- 1. 全ステージで共通のベース ----
FROM python:3.12-slim as base
WORKDIR /workspace/backend
ENV PYTHONPATH "${PYTHONPATH}:/workspace/backend"

# wait-for-it.shをコピーして実行権限を付与
COPY wait-for-it.sh /usr/local/bin/wait-for-it.sh
RUN chmod +x /usr/local/bin/wait-for-it.sh

# 依存関係をインストール
COPY backend/requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt


# ---- 2. 開発用ステージ ----
FROM base as development
# 開発中はソースコードをマウントするので、COPYは不要
# CMDで--reloadを有効にする
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]


# ---- 3. 本番用ステージ ----
FROM base as production
# 非rootユーザーを作成
RUN addgroup --system app && adduser --system --group app
USER app

# ソースコードをイメージ内にコピー
COPY ./backend .

# CMDは--reloadなし
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
