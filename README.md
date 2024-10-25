# react-template

React × Typescript on Docker環境を手早く作成するためのhow to(Macユーザ向け)

## 前提条件

* Node.jsの実行環境(v22)
* Dockerの実行環境

## 使い方

1. リポジトリをクローン
2. プロジェクトのルートディレクトリを任意の名前に変更

    ```bash
    mv ./react-tamplate ./my-project
    ```

3. プロジェクトルート階層から下記コマンドを打ち込み、React × Typescriptの新規アプリを作成

    ```bash
    # react-appの部分をご自身のプロジェクトの名前に置き換えてください
    docker-compose run --rm app sh -c 'npx create-react-app react-app --template typescript'
    ```

4. Dockerfileのコメントアウトを外す

   ```text
    # 依存パッケージをコピーしてインストール
    COPY package.json package-lock.json ./
    RUN npm install

    # アプリケーションのソースコードをコピー
    COPY . .

    # アプリケーションを起動
    CMD ["npm", "start"]
    ```

5. Dockerfile、compose.ymlを新規プロジェクトに移動

   ```bash
   mv compose.yml Dockerfile ./react-app/
   ```

6. 新規に作成したディレクトリ内でdockerコンテナを立ち上げる

    ```bash
    docker-compose up
    ```

    ※もし下記のエラーが発生した場合

    ```bash
    docker-compose up

    [+] Running 1/0
    ✔ Container react-app-app-1  Created                                                                                0.0s
    Attaching to app-1
    app-1  |
    app-1  | > react-app@0.1.0 start
    app-1  | > react-scripts start
    app-1  |
    app-1  | sh: react-scripts: not found
    app-1 exited with code 127
    ```

    以下のコマンドでキャッシュをクリアし、再ビルドを行ってみてください。

    ```text
    docker-compose down --rmi all --volumes --remove-orphans
    docker-compose up --build
    ```
