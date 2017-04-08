# Configurações 

## Configuração do projeto TypeScript

As seguintes configurações no **tsconfig.json** são necessárias para o
funcionamento.

```json
{
    "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true
    }
}
```

## Configurações da Aplicação

```json
{
  "port": "3000",
  "defaultRoutes": true,
  "database": {
    "name": "proton-example",
    "username": "",
    "password": "",
    "options": {
      "dialect": "sqlite",
      "storage": "./src/test/proton.sqlite",
      "define": {
        "underscored": "true"
      },
      "logging": false
    }
  },
  "cors": {
    "origin": "*",
    "methods": ["GET", "POST", "OPTIONS", "PUT", "PATCH", "DELETE"],
    "allowedHeaders": ["Content-Type", "Authorization"]
  },
  "jwtSecret": "secret",
  "jwtSession": {
    "session": false
  },
  "logger": {
    "enabled": false,
    "transports": [
      {
        "type": "file",
        "options": {
          "filename": "./test/logs.log"
        }
      },
      {
        "type": "console"
      }
    ]
  },
  "https": {
    "enabled": false,
    "key": "./src/test/cert/cert.key",
    "cert": "./src/test/cert/cert.cert"
  }
}
```