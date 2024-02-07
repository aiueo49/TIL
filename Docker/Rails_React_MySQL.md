# Docker環境構築（まるパクリ）

[Dockerを使ってRails7, React18の開発環境を構築します](https://zenn.dev/925rycki/articles/655462e9c76906)

### 構成

```bash
Rails
React
MySQL
```

```ruby
zenn-rails-react
├backend
│├Gemfile
│├Gemfile.lock
│├entrypoint.sh
│└Dockerfile
└docker-compose.yml
```

### backend
Gemfile

```ruby
source 'https://rubygems.org'
gem 'rails', '~>7.0.4'
```

### entrypoint.sh

```ruby
#!/bin/bash
set -e

rm -f /api/tmp/pids/server.pid

exec "$@"
```

### Dockerfile

```ruby
FROM ruby:latest

ARG RUBYGEMS_VERSION=3.4.6

RUN mkdir /api

WORKDIR /api

COPY Gemfile /api/Gemfile

COPY Gemfile.lock /api/Gemfile.lock

RUN gem update --system ${RUBYGEMS_VERSION} && \
    bundle install

COPY . /api

COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]

CMD ["rails", "server", "-b", "0.0.0.0"]
```

### docker-compose.yml

```ruby
version: '3'
services:
  api:
    build: ./backend/
    command: /bin/sh -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - ./backend:/api
    ports:
      - 3001:3000
    depends_on:
      - db
    tty: true
    stdin_open: true
  db:
    image: mysql:latest
    command: mysqld --character-set-server=utf8 --collation-server=utf8_unicode_ci
    volumes:
      - db-volume:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: password
      TZ: "Asia/Tokyo"
    ports:
      - "3306:3306"
  
volumes:
  db-volume:
```

### rails new

```ruby
docker compose run api rails new . --force --database=mysql --api
```

### エラー発生

```ruby
~/mkapp/zenn-rails-react/backend docker compose run api rails new . --force --databese=mysql --api
[+] Running 11/11
 ✔ db 10 layers [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                                                                46.0s
   ✔ 0647c03756d6 Pull complete                                                                                    25.8s
   ✔ 7c571053c52e Pull complete                                                                                     0.7s
   ✔ d6453741958d Pull complete                                                                                     1.1s
   ✔ 03e97ad7011a Pull complete                                                                                     3.6s
   ✔ 65f90da87686 Pull complete                                                                                     2.8s
   ✔ 389c2a8fa113 Pull complete                                                                                     4.6s
   ✔ c9ec19ee1f6a Pull complete                                                                                    27.3s
   ✔ fe4ac7a81f6f Pull complete                                                                                     6.4s
   ✔ 425626fff053 Pull complete                                                                                    39.2s
   ✔ b24585fbe705 Pull complete                                                                                    27.2s
[+] Creating 3/3
 ✔ Network zenn-rails-react_default     Created                                                                     0.0s
 ✔ Volume "zenn-rails-react_db-volume"  Created                                                                     0.0s
 ✔ Container zenn-rails-react-db-1      Created                                                                     0.3s
[+] Running 1/1
 ✔ Container zenn-rails-react-db-1  Started                                                                         0.2s
[+] Building 18.0s (15/15) FINISHED                                                                 docker:desktop-linux
 => [api internal] load build definition from Dockerfile                                                            0.0s
 => => transferring dockerfile: 435B                                                                                0.0s
 => [api internal] load metadata for docker.io/library/ruby:latest                                                  1.7s
 => [api auth] library/ruby:pull token for registry-1.docker.io                                                     0.0s
 => [api internal] load .dockerignore                                                                               0.0s
 => => transferring context: 2B                                                                                     0.0s
 => CACHED [api 1/9] FROM docker.io/library/ruby:latest@sha256:79fd4a27fc343abc7372e9082fcfae3750e24cdb519c1cfd89c  0.0s
 => [api internal] load build context                                                                               0.0s
 => => transferring context: 774B                                                                                   0.0s
 => [api 2/9] RUN mkdir /api                                                                                        0.6s
 => [api 3/9] WORKDIR /api                                                                                          0.1s
 => [api 4/9] COPY Gemfile /api/Gemfile                                                                             0.0s
 => [api 5/9] COPY Gemfile.lock /api/Gemfile.lock                                                                   0.0s
 => [api 6/9] RUN gem update --system 3.4.6 &&     bundle install                                                  14.6s
 => [api 7/9] COPY . /api                                                                                           0.1s
 => [api 8/9] COPY entrypoint.sh /usr/bin/                                                                          0.0s
 => [api 9/9] RUN chmod +x /usr/bin/entrypoint.sh                                                                   0.1s
 => [api] exporting to image                                                                                        0.4s
 => => exporting layers                                                                                             0.4s
 => => writing image sha256:c32bf0342c04ecfc64fefcfa6ce9191471ab6815298d0f6647b9c6544bd77bf1                        0.0s
 => => naming to docker.io/library/zenn-rails-react-api                                                             0.0s
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:14: [BUG] Segmentation fault at 0x0016ffff7f9606c0
ruby 3.3.0 (2023-12-25 revision 5124f9ac75) [aarch64-linux]

-- Control frame information -----------------------------------------------
c:0039 p:---- s:0287 e:000286 CFUNC  :resume
c:0038 p:0007 s:0283 E:0003d0 BLOCK  /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:14 [FINISH]
c:0037 p:---- s:0280 e:000279 CFUNC  :synchronize
c:0036 p:0026 s:0276 E:002058 METHOD /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:12
c:0035 p:0007 s:0271 e:000270 CLASS  /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:18
c:0034 p:0025 s:0268 e:000267 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:5 [FINISH]
c:0033 p:---- s:0265 e:000264 CFUNC  :require
c:0032 p:0121 s:0260 e:000259 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0031 p:0041 s:0243 e:000242 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/reentrant_read_write_lock.rb:7 [FINISH]
c:0030 p:---- s:0240 e:000239 CFUNC  :require
c:0029 p:0121 s:0235 e:000234 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0028 p:0047 s:0218 e:000217 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomics.rb:8 [FINISH]
c:0027 p:---- s:0215 e:000214 CFUNC  :require
c:0026 p:0121 s:0210 e:000209 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0025 p:0029 s:0193 e:000192 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent.rb:6 [FINISH]
c:0024 p:---- s:0190 e:000189 CFUNC  :require
c:0023 p:0121 s:0185 e:000184 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0022 p:0017 s:0168 e:000167 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_thread_safe_level.rb:5 [FINISH]
c:0021 p:---- s:0165 e:000164 CFUNC  :require
c:0020 p:0121 s:0160 e:000159 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0019 p:0017 s:0143 e:000142 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_silence.rb:5 [FINISH]
c:0018 p:---- s:0140 e:000139 CFUNC  :require
c:0017 p:0121 s:0135 e:000134 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0016 p:0005 s:0118 e:000117 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger.rb:3 [FINISH]
c:0015 p:---- s:0115 e:000114 CFUNC  :require
c:0014 p:0121 s:0110 e:000109 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0013 p:0023 s:0093 e:000092 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support.rb:29 [FINISH]
c:0012 p:---- s:0090 e:000089 CFUNC  :require
c:0011 p:0121 s:0085 e:000084 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0010 p:0005 s:0068 e:000067 TOP    /usr/local/bundle/gems/railties-7.0.8/lib/rails/command.rb:3 [FINISH]
c:0009 p:---- s:0065 e:000064 CFUNC  :require
c:0008 p:0121 s:0060 e:000059 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0007 p:0030 s:0043 E:000000 TOP    /usr/local/bundle/gems/railties-7.0.8/lib/rails/cli.rb:12 [FINISH]
c:0006 p:---- s:0040 e:000039 CFUNC  :require
c:0005 p:0121 s:0035 e:000034 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88
c:0004 p:0042 s:0018 e:000017 TOP    /usr/local/bundle/gems/railties-7.0.8/exe/rails:10 [FINISH]
c:0003 p:---- s:0013 e:000012 CFUNC  :load
c:0002 p:0078 s:0008 E:0004c0 EVAL   /usr/local/bundle/bin/rails:25 [FINISH]
c:0001 p:0000 s:0003 E:001cd0 DUMMY  [FINISH]

-- Ruby level backtrace information ----------------------------------------
/usr/local/bundle/bin/rails:25:in `<main>'
/usr/local/bundle/bin/rails:25:in `load'
/usr/local/bundle/gems/railties-7.0.8/exe/rails:10:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/railties-7.0.8/lib/rails/cli.rb:12:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/railties-7.0.8/lib/rails/command.rb:3:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support.rb:29:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger.rb:3:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_silence.rb:5:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_thread_safe_level.rb:5:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent.rb:6:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomics.rb:8:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/reentrant_read_write_lock.rb:7:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:88:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:5:in `<top (required)>'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:18:in `<module:Concurrent>'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:12:in `mutex_owned_per_thread?'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:12:in `synchronize'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:14:in `block in mutex_owned_per_thread?'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:14:in `resume'

-- Threading information ---------------------------------------------------
Total ractor count: 1
Ruby thread count for this ractor: 1

-- Machine register context ------------------------------------------------
  x0: 0x0000aaab19bd9b50  x1: 0x0000aaab1a04b600  x2: 0x0000ffffd9cb73e0
  x3: 0x0000ffff6565ff60  x4: 0x0000ffff65660018  x5: 0x0000ffff65680000
  x6: 0x0000ffff7feb2b88  x7: 0x0000000000000000 x18: 0x0000000000000169
 x19: 0x0000000000000000 x20: 0x0000000000000000 x21: 0x0000000000000000
 x22: 0x0000000000000000 x23: 0x0000000000000000 x24: 0x0000000000000000
 x25: 0x0000000000000000 x26: 0x0000000000000000 x27: 0x0000000000000000
 x28: 0x0000000000000000 x29: 0x0000000000000000  sp: 0x0000ffff65660000
 fau: 0x0016ffff7f9606c0

-- C level backtrace information -------------------------------------------
~/mkapp/zenn-rails-react/backend
```

### 対処1

GitHub Copilot: Rubyの最新バージョンを使用しているとのことですが、`Gemfile.lock`の一部を見ると、`concurrent-ruby`のバージョンが1.2.3であることがわかります。このバージョンは古い可能性があり、問題の原因となっているかもしれません。

以下の手順で`concurrent-ruby`のバージョンを更新してみてください。

1. `Gemfile`に以下の行を追加します：
    
    ```ruby
    gem 'concurrent-ruby', '1.1.9'
    ```
    
2. `bundle update concurrent-ruby`を実行します。
3. Dockerコンテナを再構築します。

これにより、`concurrent-ruby`のバージョンが1.1.9にダウングレードされ、問題が解決する可能性があります。

### 対処2

GitHub Copilot: `concurrent-ruby`のバージョンが正常にダウングレードされたようです。次に、Dockerコンテナを再構築してみてください。以下のコマンドを実行します：

```bash
docker-compose build --no-cache

```

これにより、新しい依存関係を反映した状態でDockerコンテナが再構築されます。その後、再度`rails new`コマンドを実行してみてください。問題が解決していることを確認してみてください。

### 対処3

GitHub Copilot: Dockerコンテナの再構築が成功したようです。次に、再度`rails new`コマンドを実行してみてください。以下のコマンドを実行します：

```bash
docker compose run api rails new . --force --database=mysql --api

```

これにより、新しいRailsアプリケーションが作成されます。問題が解決していることを確認してみてください。

### 解消

### config/database.yml

```ruby
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password
  host: db
```

```bash
docker compose up
```

### エラー

```bash
~/mkapp/zenn-rails-react/backend docker compose up
[+] Running 2/2
 ✔ Container zenn-rails-react-db-1   Running                                                                        0.0s
 ✔ Container zenn-rails-react-api-1  Created                                                                        0.1s
Attaching to api-1, db-1
api-1  | Could not find mysql2-0.5.5, puma-5.6.8, bootsnap-1.18.3, msgpack-1.7.2, irb-1.11.1, reline-0.4.2, io-console-0.7.2 in locally installed gems
api-1  | Run `bundle install` to install missing gems.
api-1 exited with code 7
^CGracefully stopping... (press Ctrl+C again to force)
[+] Stopping 2/2
 ✔ Container zenn-rails-react-api-1  Stopped                                                                        0.0s
 ✔ Container zenn-rails-react-db-1   Stopped                                                                        1.9s
canceled
```

### 対処

GitHub Copilot: エラーメッセージから、一部のgemが見つからないという問題が発生していることがわかります。これは、Dockerコンテナ内で`bundle install`が正しく実行されていない可能性があります。

以下の手順で問題を解決してみてください。

1. Dockerfile内で`bundle install`を実行する行があることを確認します。なければ追加してください。
    
    ```
    RUN bundle install
    
    ```
    
2. Dockerコンテナを再構築します。
    
    ```bash
    docker-compose build --no-cache
    
    ```
    
3. 再度`docker compose up`を実行します。

これにより、Dockerコンテナ内で必要なgemがインストールされ、問題が解決するはずです。

### 解消

```bash
docker compose exec api rails db:create
```

### http://localhost:3001/にアクセス

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/d49fece9-3bba-41f7-951e-0ba790b59646/cd5f500b-4c16-4c32-bf05-1886554f3575/Untitled.png)

### CORSの設定

### Gemfile

```bash
gem "rack-cors"
```

```bash
docker compose exec api bundle install
```

### backend/config/initializers/cors.rb

```ruby
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "localhost:3000"

    resource "*",
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

ここまででRailsとMySQLの開発環境が用意できました

## Reactの開発環境を構築

### frontendディレクトリを作成し、その下にはDockerfileを作成

```ruby
zenn-rails-react
├backend
│├Gemfile
│├Gemfile.lock
│├entrypoint.sh
│└Dockerfile
├frontend
│└Dockerfile
└docker-compose.yml
```

### Dockerfile

```ruby
FROM node:lts-slim

WORKDIR /front

ENV CI=true
```

### docker-compose.yml

```ruby
version: '3'
services:
  api:
    build: ./backend/
    command: /bin/sh -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - ./backend:/api
    ports:
      - 3001:3000
    depends_on:
      - db
    tty: true
    stdin_open: true
  db:
    image: mysql:latest
    command: mysqld --character-set-server=utf8 --collation-server=utf8_unicode_ci
    volumes:
      - db-volume:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: password
      TZ: "Asia/Tokyo"
    ports:
      - "3306:3306"
  front:
    build: ./frontend/
    volumes:
      - ./frontend/app:/front
    ports:
      - 3000:3000
    tty: true
    stdin_open: true
    environment:
      - CHOKIDAR_USEPOLLING=true
  
volumes:
  db-volume:
```

```bash
docker compose up -d --build
```

```bash
docker compose exec front bash
```

```bash
root@8e780c25c632:/front# npx create-react-app . --tailwind
```

```bash
root@39c4c8e1ee98:/front# npm start
```

### エラー発生

```bash
root@39c4c8e1ee98:/front# npm start

> front@0.1.0 start
> react-scripts start

Cannot find module './codegen'
Require stack:
- /front/node_modules/eslint-webpack-plugin/node_modules/ajv/dist/compile/errors.js
- /front/node_modules/eslint-webpack-plugin/node_modules/ajv/dist/compile/validate/boolSchema.js
- /front/node_modules/eslint-webpack-plugin/node_modules/ajv/dist/compile/validate/index.js
- /front/node_modules/eslint-webpack-plugin/node_modules/ajv/dist/core.js
- /front/node_modules/eslint-webpack-plugin/node_modules/ajv/dist/ajv.js
- /front/node_modules/eslint-webpack-plugin/node_modules/schema-utils/dist/validate.js
- /front/node_modules/eslint-webpack-plugin/node_modules/schema-utils/dist/index.js
- /front/node_modules/eslint-webpack-plugin/dist/options.js
- /front/node_modules/eslint-webpack-plugin/dist/index.js
- /front/node_modules/react-scripts/config/webpack.config.js
- /front/node_modules/react-scripts/scripts/start.js
root@39c4c8e1ee98:/front#
```

### 対処

GitHub Copilot: このエラーは、`eslint-webpack-plugin`が依存している`ajv`モジュールの一部が見つからないことを示しています。`node_modules`ディレクトリの問題が原因である可能性が高いです。

以下の手順で問題を解決できるか試してみてください。

1. `node_modules`ディレクトリと`package-lock.json`ファイルを削除します。

```bash
rm -rf node_modules package-lock.json

```

1. 依存関係を再インストールします。

```bash
npm install

```

1. アプリケーションを再起動します。

```bash
npm start

```

これにより、依存関係が正しくインストールされ、エラーが解消されるはずです。

### 未対処

```bash
root@39c4c8e1ee98:/front# npm install
npm WARN deprecated rollup-plugin-terser@7.0.2: This package has been deprecated and is no longer maintained. Please use @rollup/plugin-terser
npm WARN deprecated sourcemap-codec@1.4.8: Please use @jridgewell/sourcemap-codec instead
npm WARN deprecated workbox-cacheable-response@6.6.0: workbox-background-sync@6.6.0
npm WARN deprecated stable@0.1.8: Modern JS already guarantees Array#sort() is a stable sort, so this library is deprecated. See the compatibility table on MDN: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort#browser_compatibility
npm WARN deprecated domexception@2.0.1: Use your platform's native DOMException instead
npm WARN deprecated abab@2.0.6: Use your platform's native atob() and btoa() methods instead
npm WARN deprecated w3c-hr-time@1.0.2: Use your platform's native performance.now() and performance.timeOrigin.
npm WARN deprecated @babel/plugin-proposal-numeric-separator@7.18.6: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-numeric-separator instead.
npm WARN deprecated @babel/plugin-proposal-nullish-coalescing-operator@7.18.6: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-nullish-coalescing-operator instead.
npm WARN deprecated @babel/plugin-proposal-class-properties@7.18.6: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-class-properties instead.
npm WARN deprecated @babel/plugin-proposal-private-methods@7.18.6: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-private-methods instead.
npm WARN deprecated @babel/plugin-proposal-optional-chaining@7.21.0: This proposal has been merged to the ECMAScript standard and thus this plugin is no longer maintained. Please use @babel/plugin-transform-optional-chaining instead.
npm WARN deprecated svgo@1.3.2: This SVGO version is no longer supported. Upgrade to v2.x.x.

added 1487 packages, and audited 1488 packages in 2m

255 packages are looking for funding
  run `npm fund` for details

8 vulnerabilities (2 moderate, 6 high)

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
root@39c4c8e1ee98:/front#
```

GitHub Copilot: `npm install`の結果を見ると、いくつかのパッケージが非推奨になっていることがわかります。これらのパッケージは、新しいバージョンに更新するか、代替のパッケージを使用することが推奨されています。

また、8つの脆弱性（2つは中程度、6つは高度）が報告されています。これらの問題を解決するには、`npm audit fix`を実行することが推奨されています。ただし、これにより一部のパッケージが大幅に更新され、互換性の問題が発生する可能性があるため、注意が必要です。

以下のコマンドを実行して、脆弱性を修正してみてください。

```bash
npm audit fix

```

それでも問題が解決しない場合、または互換性の問題が発生した場合は、問題のあるパッケージを個別に更新するか、代替のパッケージを探す必要があります。

### localhost:3000にアクセス

```bash
root@39c4c8e1ee98:/front# npm start

> front@0.1.0 start
> react-scripts start

(node:240) [DEP_WEBPACK_DEV_SERVER_ON_AFTER_SETUP_MIDDLEWARE] DeprecationWarning: 'onAfterSetupMiddleware' option is deprecated. Please use the 'setupMiddlewares' option.
(Use `node --trace-deprecation ...` to show where the warning was created)
(node:240) [DEP_WEBPACK_DEV_SERVER_ON_BEFORE_SETUP_MIDDLEWARE] DeprecationWarning: 'onBeforeSetupMiddleware' option is deprecated. Please use the 'setupMiddlewares' option.
Starting the development server...

One of your dependencies, babel-preset-react-app, is importing the
"@babel/plugin-proposal-private-property-in-object" package without
declaring it in its dependencies. This is currently working because
"@babel/plugin-proposal-private-property-in-object" is already in your
node_modules folder for unrelated reasons, but it may break at any time.

babel-preset-react-app is part of the create-react-app project, which
is not maintianed anymore. It is thus unlikely that this bug will
ever be fixed. Add "@babel/plugin-proposal-private-property-in-object" to
your devDependencies to work around this error. This will make this message
go away.
Compiled successfully!

You can now view front in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://172.22.0.3:3000

Note that the development build is not optimized.
To create a production build, use npm run build.

webpack compiled successfully
```

成功
