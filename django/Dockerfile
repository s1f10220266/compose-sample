#使用するイメージを指定
FROM python:3.11-slim

#環境変数の指定 pythonの出力をバッファリングしないようにする
ENV PYTHONUNBUFFERED 1

#コンテナ内の/codeを作業ディレクトリとする
WORKDIR /code

#requirements.txtをコンテナ内の/code下にコピー
COPY ./requirements.txt /code/

#イメージをビルドする際にrequirements.txtに記載されている依存パッケージをインストール
RUN pip install -r requirements.txt

#カレントディレクトリ下にある全ての内容をコンテナ内の/code下にコピー
COPY . /code/