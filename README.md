# wiremock-extension-maven
## 詰まったところ
- wiremockのバージョンをextensionとstandalone側で合わせないとエラー
- dockerのwiremock/wiremockイメージは最新で2.27.0
- wiremock:2.27.0はJava Runtimeの52までしか対応していない
- Java Runtimeの52に対応しているJDK8で実装する必要がある

## jar生成
```shell
mvn package
```

## wiremockへの追加方法
docker-composeの場合（extensions内にjarを配置）
```dockerfile
version: "3"
services:
  wiremock:
    image: "wiremock/wiremock:2.27.2"
    container_name: wiremock
    ports:
      - "8080:8080"
    volumes:
      - ./wiremock:/home/wiremock
      - ./extensions:/var/wiremock/extensions
    command: --global-response-templating --disable-gzip --verbose --extensions="wiremock.extensions.transformer.ExampleTransformer"
```