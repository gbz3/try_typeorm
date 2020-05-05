# try_typeorm

## 環境設定

### Node.jsバージョン指定

```bash
$ nodenv local 14.1.0
$ node -v
v14.1.0
$ npm -v
6.14.4
```

### npmモジュール指定( webpack+TypeScriptの最小構成 )

- [最新版TypeScript+webpack 4の環境構築まとめ(React, Vue.js, Three.jsのサンプル付き)](https://ics.media/entry/16329/)

```bash
$ npm init
$ npm install --save-dev webpack webpack-cli typescript ts-loader
$ vi package.json
$ cat package.json
...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "watch": "webpack -w"
  },
...
  "devDependencies": {
    "ts-loader": "^7.0.2",
    "typescript": "^3.8.3",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.11"
  },
  "private": true
}
```

### tsconfig.json

```json
{
  "compilerOptions": {
    "sourceMap": true,
    "target": "es5",
    "module": "es2015"
  }
}
```

### webpack.config.js

```javascript
module.exports = {
  mode: "development",
  entry: "./src/main.ts",
  module: {
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',
      }
    ]
  },
  resolve: {
    extensions: [
      '.ts', '.js',
    ]
  }
};
```

### ビルド

```bash
$ npm run build
...
Insufficient number of arguments or no entry found.
Alternatively, run 'webpack(-cli) --help' for usage info.
```

### 開発ツールインストール( sqlite3のビルドが必要なので )

```bash
$ sudo apt-get update
$ sudo apt install build-essential
$ gcc --version
gcc (Ubuntu 7.5.0-3ubuntu1~18.04) 7.5.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

### TypeORMの設定

- [# Instration](https://typeorm.io/#/undefined/installation)

```bash
$ sudo ln -s /usr/bin/python3 /usr/bin/python  # sqlite3ビルド時に /usr/bin/python の存在が前提のため
$ npm install --save typeorm reflect-metadata @types/node sqlite3
$ cat package.json
  "devDependencies": {
    "ts-loader": "^7.0.2",
    "typescript": "^3.8.3",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.11"
  },
  "private": true,
  "dependencies": {
    "@types/node": "^13.13.4",
    "reflect-metadata": "^0.1.13",
    "sqlite3": "^4.2.0",
    "typeorm": "^0.2.24"
  }
}
```

### tsconfig.json 追記

```json
{
  "compilerOptions": {
    "sourceMap": true,
    "target": "es5",
    "module": "es2015",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true
  }
}
```

### TypeORM初期化

```bash
$ ./node_modules/.bin/typeorm init --database sqlite
Project created inside current directory.
$ cat ormconfig.json
{
   "type": "sqlite",
   "database": "database.sqlite",
   "synchronize": true,
   "logging": false,
   "entities": [
      "src/entity/**/*.ts"
   ],
   "migrations": [
      "src/migration/**/*.ts"
   ],
   "subscribers": [
      "src/subscriber/**/*.ts"
   ],
   "cli": {
      "entitiesDir": "src/entity",
      "migrationsDir": "src/migration",
      "subscribersDir": "src/subscriber"
   }
}
$ rm -rf node_modules
$ npm install
$ npm start
...
Inserting a new user into the database...
Saved a new user with id: 1
Loading users from the database...
Loaded users:  [ User { id: 1, firstName: 'Timber', lastName: 'Saw', age: 25 } ]
Here you can setup and run express/koa/any other framework.```
