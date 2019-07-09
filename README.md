# docker-php-develop

自分の開発用に用意した `nginx` + `php-fpm` + `mysql` のコンテナ定義。
`Visual Studio Code` の `Remote - Containers` に対応しており、コンテナ上での開発が可能。

実環境で利用する際はCentOSベースが多いため、差異を極力少なくする意味を込めてDebianベースの一般的なイメージを使用せず、CentOSイメージに環境を構築している。

xdebugへのアクセスは `9001` ポートを使用。

## lauch.json の設定例

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for XDebug",
            "type": "php",
            "request": "launch",
            "port": 9001,
            "stopOnEntry": true,
            "pathMappings": {
                "/var/www/html": "${workspaceRoot}"
            }
        },
        {
            "name": "Launch currently open script",
            "type": "php",
            "request": "launch",
            "program": "${file}",
            "cwd": "${fileDirname}",
            "port": 9001
        }
    ]
}
```
