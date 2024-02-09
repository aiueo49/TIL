# [完]Docker環境構築（オリジナル）

### ベースはこれで

[Dockerを使ってRails7, React18の開発環境を構築します](https://zenn.dev/925rycki/articles/655462e9c76906)

### 構成を変更する

```ruby
Rails
React
PostgreSQL
Redis
```

```ruby
# プロジェクトルート
Original_sample
├backend
│├Gemfile
│├Gemfile.lock
│├entrypoint.sh
│└Dockerfile
├frontend
└docker-compose.yml
```

### Gemfile

```ruby
FROM ruby:latest

ARG RUBYGEMS_VERSION=3.5.6

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

```yaml
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
      - redis
    tty: true
    stdin_open: true
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: username
      POSTGRES_PASSWORD: password
      POSTGRES_DB: dbname
      TZ: "Asia/Tokyo"
    volumes:
      - db-volume:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:latest
    ports:
      - "6379:6379"

volumes:
  db-volume:
```

```bash
docker compose run api rails new . --force --database=postgresql --api
```

```yaml
~/mkapp/Original_sample/backend docker compose run api rails new . --force --database=postgresql --api
[+] Running 9/9
 ✔ redis 8 layers [⣿⣿⣿⣿⣿⣿⣿⣿]      0B/0B      Pulled                                                                 7.3s
   ✔ 25d3892798f8 Already exists                                                                                    0.0s
   ✔ e5d458cf0bea Pull complete                                                                                     0.7s
   ✔ 5f6ae8126b8f Pull complete                                                                                     0.7s
   ✔ c1b1c9c7fe0c Pull complete                                                                                     0.8s
   ✔ 3b0bca0ecfe5 Pull complete                                                                                     3.2s
   ✔ 7d0a39b44797 Pull complete                                                                                     1.4s
   ✔ 4f4fb700ef54 Pull complete                                                                                     1.7s
   ✔ 8249a387d011 Pull complete                                                                                     2.6s
[+] Creating 5/4
 ✔ Network original_sample_default                                                                                                                   Created0.0s reated                                                                      0.0s
 ✔ Volume "original_sample_db-volume"                                                                                                                Created0.0s reating                                                                     0.1s
 ✔ Container original_sample-db-1                                                                                                                    Created0.2s
 ✔ Container original_sample-redis-1                                                                                                                 Created0.1s
 ! db The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested 0.0s
[+] Running 2/2
 ✔ Container original_sample-redis-1  Started                                                                       0.1s
 ✔ Container original_sample-db-1     Started                                                                       0.1s
[+] Building 14.3s (15/15) FINISHED                                                                 docker:desktop-linux
 => [api internal] load build definition from Dockerfile                                                            0.0s
 => => transferring dockerfile: 435B                                                                                0.0s
 => [api internal] load metadata for docker.io/library/ruby:latest                                                  2.3s
 => [api auth] library/ruby:pull token for registry-1.docker.io                                                     0.0s
 => [api internal] load .dockerignore                                                                               0.0s
 => => transferring context: 2B                                                                                     0.0s
 => [api 1/9] FROM docker.io/library/ruby:latest@sha256:6936c28e1221d653c0d9af9d2b6a9e27ba0350cd10c9cc57464c62743d  3.7s
 => => resolve docker.io/library/ruby:latest@sha256:6936c28e1221d653c0d9af9d2b6a9e27ba0350cd10c9cc57464c62743d723b  0.0s
 => => sha256:35b802cf7c78b6d5768c67353203595e6643da357385f72d22b3446a0d5642e3 2.32kB / 2.32kB                      0.0s
 => => sha256:9ed21283c0da995898d2f72391e1c390841aeba1cf35e5e5fb921ade04570bd9 36.45MB / 36.45MB                    3.1s
 => => sha256:c30abfeef8e75f3d0386e972bb0ac9a12307553b8e67957c094619f899ff5d20 143B / 143B                          0.7s
 => => sha256:6936c28e1221d653c0d9af9d2b6a9e27ba0350cd10c9cc57464c62743d723b76 9.89kB / 9.89kB                      0.0s
 => => sha256:2ad457130a9ccd3bd1f6e4830ded06ffcd51719e68224f701e13ede83c12835f 7.07kB / 7.07kB                      0.0s
 => => extracting sha256:9ed21283c0da995898d2f72391e1c390841aeba1cf35e5e5fb921ade04570bd9                           0.6s
 => => extracting sha256:c30abfeef8e75f3d0386e972bb0ac9a12307553b8e67957c094619f899ff5d20                           0.0s
 => [api internal] load build context                                                                               0.1s
 => => transferring context: 774B                                                                                   0.1s
 => [api 2/9] RUN mkdir /api                                                                                        0.2s
 => [api 3/9] WORKDIR /api                                                                                          0.0s
 => [api 4/9] COPY Gemfile /api/Gemfile                                                                             0.0s
 => [api 5/9] COPY Gemfile.lock /api/Gemfile.lock                                                                   0.0s
 => [api 6/9] RUN gem update --system 3.5.6 &&     bundle install                                                   7.6s
 => [api 7/9] COPY . /api                                                                                           0.0s
 => [api 8/9] COPY entrypoint.sh /usr/bin/                                                                          0.0s
 => [api 9/9] RUN chmod +x /usr/bin/entrypoint.sh                                                                   0.1s
 => [api] exporting to image                                                                                        0.2s
 => => exporting layers                                                                                             0.2s
 => => writing image sha256:760836fb3454933dbdc99192ac20440534be8c99737eaaaed04266f27b1967ab                        0.0s
 => => naming to docker.io/library/original_sample-api                                                              0.0s
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:14: [BUG] Segmentation fault at 0x0077ffffba2a06c0
ruby 3.3.0 (2023-12-25 revision 5124f9ac75) [aarch64-linux]

-- Control frame information -----------------------------------------------
c:0039 p:---- s:0177 e:000176 CFUNC  :resume
c:0038 p:0007 s:0173 E:000380 BLOCK  /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:14 [FINISH]
c:0037 p:---- s:0170 e:000169 CFUNC  :synchronize
c:0036 p:0026 s:0166 E:002118 METHOD /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:12
c:0035 p:0007 s:0161 e:000160 CLASS  /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:18
c:0034 p:0025 s:0158 e:000157 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:5 [FINISH]
c:0033 p:---- s:0155 e:000154 CFUNC  :require
c:0032 p:0023 s:0150 e:000149 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0031 p:0041 s:0144 e:000143 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/reentrant_read_write_lock.rb:7 [FINISH]
c:0030 p:---- s:0141 e:000140 CFUNC  :require
c:0029 p:0023 s:0136 e:000135 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0028 p:0047 s:0130 e:000129 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomics.rb:8 [FINISH]
c:0027 p:---- s:0127 e:000126 CFUNC  :require
c:0026 p:0023 s:0122 e:000121 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0025 p:0029 s:0116 e:000115 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent.rb:6 [FINISH]
c:0024 p:---- s:0113 e:000112 CFUNC  :require
c:0023 p:0023 s:0108 e:000107 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0022 p:0017 s:0102 e:000101 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_thread_safe_level.rb:5 [FINISH]
c:0021 p:---- s:0099 e:000098 CFUNC  :require
c:0020 p:0023 s:0094 e:000093 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0019 p:0017 s:0088 e:000087 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_silence.rb:5 [FINISH]
c:0018 p:---- s:0085 e:000084 CFUNC  :require
c:0017 p:0023 s:0080 e:000079 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0016 p:0005 s:0074 e:000073 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger.rb:3 [FINISH]
c:0015 p:---- s:0071 e:000070 CFUNC  :require
c:0014 p:0023 s:0066 e:000065 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0013 p:0023 s:0060 e:000059 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support.rb:29 [FINISH]
c:0012 p:---- s:0057 e:000056 CFUNC  :require
c:0011 p:0023 s:0052 e:000051 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0010 p:0005 s:0046 e:000045 TOP    /usr/local/bundle/gems/railties-7.0.8/lib/rails/command.rb:3 [FINISH]
c:0009 p:---- s:0043 e:000042 CFUNC  :require
c:0008 p:0023 s:0038 e:000037 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0007 p:0030 s:0032 E:001f50 TOP    /usr/local/bundle/gems/railties-7.0.8/lib/rails/cli.rb:12 [FINISH]
c:0006 p:---- s:0029 e:000028 CFUNC  :require
c:0005 p:0023 s:0024 e:000023 METHOD <internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136
c:0004 p:0042 s:0018 e:000017 TOP    /usr/local/bundle/gems/railties-7.0.8/exe/rails:10 [FINISH]
c:0003 p:---- s:0013 e:000012 CFUNC  :load
c:0002 p:0078 s:0008 E:0021f0 EVAL   /usr/local/bundle/bin/rails:25 [FINISH]
c:0001 p:0000 s:0003 E:000c90 DUMMY  [FINISH]

-- Ruby level backtrace information ----------------------------------------
/usr/local/bundle/bin/rails:25:in `<main>'
/usr/local/bundle/bin/rails:25:in `load'
/usr/local/bundle/gems/railties-7.0.8/exe/rails:10:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/railties-7.0.8/lib/rails/cli.rb:12:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/railties-7.0.8/lib/rails/command.rb:3:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support.rb:29:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger.rb:3:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_silence.rb:5:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_thread_safe_level.rb:5:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent.rb:6:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomics.rb:8:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/reentrant_read_write_lock.rb:7:in `<top (required)>'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
<internal:/usr/local/lib/ruby/site_ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:136:in `require'
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
  x0: 0x0000aaaad6082b50  x1: 0x0000aaaad64d0a30  x2: 0x0000ffffc71b1a20
  x3: 0x0000ffff9ff9ff60  x4: 0x0000ffff9ffa0018  x5: 0x0000ffff9ffc0000
  x6: 0x0000ffffba7f6b88  x7: 0x0000000000000000 x18: 0x0000000000000056
 x19: 0x0000000000000000 x20: 0x0000000000000000 x21: 0x0000000000000000
 x22: 0x0000000000000000 x23: 0x0000000000000000 x24: 0x0000000000000000
 x25: 0x0000000000000000 x26: 0x0000000000000000 x27: 0x0000000000000000
 x28: 0x0000000000000000 x29: 0x0000000000000000  sp: 0x0000ffff9ffa0000
 fau: 0x0077ffffba2a06c0

-- C level backtrace information -------------------------------------------
~/mkapp/Original_sample/backend
```

### 以下ファイル、対処を参照

[[完]Docker環境構築（まるパクリ）](https://www.notion.so/Docker-6bd34613ec8042b9a32b5f51031f7704?pvs=21)

GitHub Copilot: `concurrent-ruby` gemの最新の安定版は現在 `1.1.9` です。これは広く使用されており、多くの場合において安定して動作します。

Gemfileに以下の行を追加または変更して、特定のバージョンを指定することができます：

```ruby
gem 'concurrent-ruby', '1.1.9'
```

その後、`bundle update concurrent-ruby`を実行して、指定したバージョンに更新します。

ただし、この変更が問題を解決するかどうかは、他の依存関係や環境によります。問題が解決しない場合は、さらに詳細な情報が必要になるかもしれません。

```bash
bundle install
```

```bash
~/mkapp/Original_sample/backend bundle install
Fetching gem metadata from https://rubygems.org/...........
Resolving dependencies...
Fetching minitest 5.22.2
Installing minitest 5.22.2
Bundle complete! 2 Gemfile dependencies, 45 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
```

```bash
~/mkapp/Original_sample/backend docker-compose build --no-cache

[+] Building 11.3s (15/15) FINISHED                                                                 docker:desktop-linux
 => [api internal] load build definition from Dockerfile                                                            0.0s
 => => transferring dockerfile: 435B                                                                                0.0s
 => [api internal] load metadata for docker.io/library/ruby:latest                                                  1.8s
 => [api auth] library/ruby:pull token for registry-1.docker.io                                                     0.0s
 => [api internal] load .dockerignore                                                                               0.0s
 => => transferring context: 2B                                                                                     0.0s
 => CACHED [api 1/9] FROM docker.io/library/ruby:latest@sha256:6936c28e1221d653c0d9af9d2b6a9e27ba0350cd10c9cc57464  0.0s
 => [api internal] load build context                                                                               0.0s
 => => transferring context: 3.99kB                                                                                 0.0s
 => [api 2/9] RUN mkdir /api                                                                                        0.1s
 => [api 3/9] WORKDIR /api                                                                                          0.0s
 => [api 4/9] COPY Gemfile /api/Gemfile                                                                             0.0s
 => [api 5/9] COPY Gemfile.lock /api/Gemfile.lock                                                                   0.0s
 => [api 6/9] RUN gem update --system 3.5.6 &&     bundle install                                                   8.8s
 => [api 7/9] COPY . /api                                                                                           0.0s
 => [api 8/9] COPY entrypoint.sh /usr/bin/                                                                          0.0s
 => [api 9/9] RUN chmod +x /usr/bin/entrypoint.sh                                                                   0.1s
 => [api] exporting to image                                                                                        0.3s
 => => exporting layers                                                                                             0.3s
 => => writing image sha256:a0ae175336e7a08a0481019f64bdfa5fdcbdab02c5b17a0cfe1f3a7ae9ad4860                        0.0s
 => => naming to docker.io/library/original_sample-api                                                              0.0s
~/mkapp/Original_sample/backend
```

```bash
~/mkapp/Original_sample/backend docker compose run api rails new . --force --database=postgresql --api
[+] Creating 4/3
 ✔ Network original_sample_default                                                                                                                   Created0.0s eating                                                                      0.0s
 ✔ Container original_sample-db-1                                                                                                                    Created0.1s
 ✔ Container original_sample-redis-1                                                                                                                 Created0.1s
 ! db The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested 0.0s
[+] Running 2/2
 ✔ Container original_sample-db-1     Started                                                                       0.1s
 ✔ Container original_sample-redis-1  Started                                                                       0.1s
       exist
      create  README.md
      create  Rakefile
      create  .ruby-version
      create  config.ru
      create  .gitignore
      create  .gitattributes
       force  Gemfile
         run  git init from "."
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint: 	git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint: 	git branch -m <name>
Initialized empty Git repository in /api/.git/
      create  app
      create  app/assets/config/manifest.js
      create  app/assets/stylesheets/application.css
      create  app/channels/application_cable/channel.rb
      create  app/channels/application_cable/connection.rb
      create  app/controllers/application_controller.rb
      create  app/helpers/application_helper.rb
      create  app/jobs/application_job.rb
      create  app/mailers/application_mailer.rb
      create  app/models/application_record.rb
      create  app/views/layouts/application.html.erb
      create  app/views/layouts/mailer.html.erb
      create  app/views/layouts/mailer.text.erb
      create  app/assets/images
      create  app/assets/images/.keep
      create  app/controllers/concerns/.keep
      create  app/models/concerns/.keep
      create  bin
      create  bin/rails
      create  bin/rake
      create  bin/setup
      create  config
      create  config/routes.rb
      create  config/application.rb
      create  config/environment.rb
      create  config/cable.yml
      create  config/puma.rb
      create  config/storage.yml
      create  config/environments
      create  config/environments/development.rb
      create  config/environments/production.rb
      create  config/environments/test.rb
      create  config/initializers
      create  config/initializers/assets.rb
      create  config/initializers/content_security_policy.rb
      create  config/initializers/cors.rb
      create  config/initializers/filter_parameter_logging.rb
      create  config/initializers/inflections.rb
      create  config/initializers/new_framework_defaults_7_0.rb
      create  config/initializers/permissions_policy.rb
      create  config/locales
      create  config/locales/en.yml
      create  config/master.key
      append  .gitignore
      create  config/boot.rb
      create  config/database.yml
      create  db
      create  db/seeds.rb
      create  lib
      create  lib/tasks
      create  lib/tasks/.keep
      create  lib/assets
      create  lib/assets/.keep
      create  log
      create  log/.keep
      create  public
      create  public/404.html
      create  public/422.html
      create  public/500.html
      create  public/apple-touch-icon-precomposed.png
      create  public/apple-touch-icon.png
      create  public/favicon.ico
      create  public/robots.txt
      create  tmp
      create  tmp/.keep
      create  tmp/pids
      create  tmp/pids/.keep
      create  tmp/cache
      create  tmp/cache/assets
      create  vendor
      create  vendor/.keep
      create  test/fixtures/files
      create  test/fixtures/files/.keep
      create  test/controllers
      create  test/controllers/.keep
      create  test/mailers
      create  test/mailers/.keep
      create  test/models
      create  test/models/.keep
      create  test/helpers
      create  test/helpers/.keep
      create  test/integration
      create  test/integration/.keep
      create  test/channels/application_cable/connection_test.rb
      create  test/test_helper.rb
      create  storage
      create  storage/.keep
      create  tmp/storage
      create  tmp/storage/.keep
      remove  app/assets
      remove  lib/assets
      remove  tmp/cache/assets
      remove  app/helpers
      remove  test/helpers
      remove  app/views/layouts/application.html.erb
      remove  public/404.html
      remove  public/422.html
      remove  public/500.html
      remove  public/apple-touch-icon-precomposed.png
      remove  public/apple-touch-icon.png
      remove  public/favicon.ico
      remove  config/initializers/assets.rb
      remove  app/assets/config/manifest.js
      remove  app/assets/config
      remove  app/assets/stylesheets/application.css
      remove  config/initializers/content_security_policy.rb
      remove  config/initializers/permissions_policy.rb
      remove  config/initializers/new_framework_defaults_7_0.rb
         run  bundle install
Fetching gem metadata from https://rubygems.org/..........
Resolving dependencies...
Fetching msgpack 1.7.2
Fetching puma 5.6.8
Fetching pg 1.5.4
Fetching io-console 0.7.2
Installing msgpack 1.7.2 with native extensions
Installing puma 5.6.8 with native extensions
Installing pg 1.5.4 with native extensions
Installing io-console 0.7.2 with native extensions
Fetching reline 0.4.2
Installing reline 0.4.2
Fetching irb 1.11.2
Installing irb 1.11.2
Fetching bootsnap 1.18.3
Installing bootsnap 1.18.3 with native extensions
Bundle complete! 6 Gemfile dependencies, 56 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
         run  bundle binstubs bundler
~/mkapp/Original_sample/backend
```

PostgreSQLをデータベースとして使用するRailsアプリケーションを作成しました。`docker compose run api rails new . --force --database=postgresql --api`コマンドを実行した後、新しいRailsアプリケーションが正常に作成されました。

このアプリケーションは、PostgreSQLデータベースを使用しています。また、GemfileにはPostgreSQL用のgemである `pg` も自動的に追加されています。

新しいアプリケーションは正常にセットアップされ、依存関係のgemもインストールされました。必要に応じて、`bundle install`コマンドでインストールされたgemの詳細を確認できます。

これで、新しいRailsアプリケーションをPostgreSQLとRedisを使用するようにセットアップしました。

### config/database.yml

```bash
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV["POSTGRES_USER"] %>
  password: <%= ENV["POSTGRES_PASSWORD"] %>
  host: db
```

### Redisは特に追記しないみたい

はい、正しいです。RedisをDocker Composeファイルに記述することで、Railsアプリケーションが正しくRedisに接続できるようになります。通常、Docker ComposeファイルでRedisのサービスを定義し、そのサービスの名前をRailsアプリケーションの設定ファイルで使用します。

例えば、以下のようなDocker ComposeファイルでRedisのサービスを定義できます：

```yaml
version: '3.8'

services:
  redis:
    image: redis:latest
    ports:
      - "6379:6379"

```

このようにして、`docker-compose.yml`ファイル内でRedisのサービスを定義し、ポート番号などの必要な設定を行います。その後、Railsアプリケーションの設定ファイルでRedisのホスト名やポート番号を正しく指定することで、RailsアプリケーションがDockerコンテナ内で実行されているRedisに正しく接続できるようになります。

つまり、Redisの設定を変更する場合、通常はDocker Composeファイル内で設定を行います。Railsアプリケーションの設定ファイルでは、Redisのホスト名やポート番号などが適切に指定されていることを確認してください。

```bash
docker compose up
```

```bash
ActiveRecord::ConnectionNotEstablished
connection to server at "172.29.0.3", port 5432 failed: fe_sendauth: no password supplied
Extracted source (around line #87):
85
86
87
88
89
90
              
            raise ActiveRecord::DatabaseConnectionError.hostname_error(conn_params[:hostname])
          else
            raise ActiveRecord::ConnectionNotEstablished, error.message
          end
        end
      end

Rails.root: /api

Application Trace | Framework Trace | Full Trace
activerecord (7.0.8) lib/active_record/connection_adapters/postgresql_adapter.rb:87:in `rescue in new_client'
activerecord (7.0.8) lib/active_record/connection_adapters/postgresql_adapter.rb:77:in `new_client'
activerecord (7.0.8) lib/active_record/connection_adapters/postgresql_adapter.rb:37:in `postgresql_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:656:in `public_send'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:656:in `new_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:700:in `checkout_new_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:679:in `try_to_checkout_new_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:640:in `acquire_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:341:in `checkout'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:181:in `connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_handler.rb:211:in `retrieve_connection'
activerecord (7.0.8) lib/active_record/connection_handling.rb:313:in `retrieve_connection'
activerecord (7.0.8) lib/active_record/connection_handling.rb:280:in `connection'
activerecord (7.0.8) lib/active_record/migration.rb:648:in `connection'
activerecord (7.0.8) lib/active_record/migration.rb:643:in `build_watcher'
activerecord (7.0.8) lib/active_record/migration.rb:625:in `block in call'
activerecord (7.0.8) lib/active_record/migration.rb:624:in `synchronize'
activerecord (7.0.8) lib/active_record/migration.rb:624:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/callbacks.rb:27:in `block in call'
activesupport (7.0.8) lib/active_support/callbacks.rb:99:in `run_callbacks'
actionpack (7.0.8) lib/action_dispatch/middleware/callbacks.rb:26:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/executor.rb:14:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/actionable_exceptions.rb:17:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/debug_exceptions.rb:28:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/show_exceptions.rb:29:in `call'
railties (7.0.8) lib/rails/rack/logger.rb:40:in `call_app'
railties (7.0.8) lib/rails/rack/logger.rb:25:in `block in call'
activesupport (7.0.8) lib/active_support/tagged_logging.rb:99:in `block in tagged'
activesupport (7.0.8) lib/active_support/tagged_logging.rb:37:in `tagged'
activesupport (7.0.8) lib/active_support/tagged_logging.rb:99:in `tagged'
railties (7.0.8) lib/rails/rack/logger.rb:25:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/remote_ip.rb:93:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/request_id.rb:26:in `call'
rack (2.2.8) lib/rack/runtime.rb:22:in `call'
activesupport (7.0.8) lib/active_support/cache/strategy/local_cache_middleware.rb:29:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/server_timing.rb:61:in `block in call'
actionpack (7.0.8) lib/action_dispatch/middleware/server_timing.rb:26:in `collect_events'
actionpack (7.0.8) lib/action_dispatch/middleware/server_timing.rb:60:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/executor.rb:14:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/static.rb:23:in `call'
rack (2.2.8) lib/rack/sendfile.rb:110:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/host_authorization.rb:138:in `call'
railties (7.0.8) lib/rails/engine.rb:530:in `call'
puma (5.6.8) lib/puma/configuration.rb:252:in `call'
puma (5.6.8) lib/puma/request.rb:77:in `block in handle_request'
puma (5.6.8) lib/puma/thread_pool.rb:340:in `with_force_shutdown'
puma (5.6.8) lib/puma/request.rb:76:in `handle_request'
puma (5.6.8) lib/puma/server.rb:443:in `process_client'
puma (5.6.8) lib/puma/thread_pool.rb:147:in `block in spawn_thread'
Exception Causes
PG::ConnectionBad: connection to server at "172.29.0.3", port 5432 failed: fe_sendauth: no password supplied
Request
Parameters:

None
Toggle session dump
Toggle env dump
Response
Headers:

None
```

GitHub Copilot: エラーメッセージから、PostgreSQLデータベースへの接続に問題があることがわかります。具体的には、データベースへの接続にパスワードが提供されていないようです。

この問題を解決するためには、データベースへの接続情報を含む設定ファイル（通常は`config/database.yml`）を確認し、正しい接続情報が設定されていることを確認する必要があります。

`config/database.yml`の内容は以下のようになるはずです：

```yaml
default: &default
  adapter: postgresql
  encoding: unicode
  username: your_username
  password: your_password
  host: your_host
  port: your_port
  database: your_database

development:
  <<: *default

test:
  <<: *default

production:
  <<: *default

```

このファイルには、データベースへの接続に必要な情報（ユーザー名、パスワード、ホスト、ポート、データベース名）が含まれています。これらの情報が正しく設定されていることを確認してください。

また、セキュリティ上の理由から、パスワードなどの機密情報は直接このファイルに記述するのではなく、環境変数を通じて提供することが推奨されます。そのため、`config/database.yml`の`password`行は以下のようになるはずです：

```yaml
password: <%= ENV['DATABASE_PASSWORD'] %>
```

そして、環境変数`DATABASE_PASSWORD`には、データベースのパスワードが設定されている必要があります。

### `config/database.yml` を変更

```bash
username: postgres 
password: password
```

```bash
ActiveRecord::DatabaseConnectionError
There is an issue connecting to your database with your username/password, username: postgres.
Please check your database configuration to ensure the username/password are valid.
Extracted source (around line #83):
81
82
83
84
85
86
              
            raise ActiveRecord::NoDatabaseError.db_error(conn_params[:dbname])
          elsif conn_params && conn_params[:user] && error.message.include?(conn_params[:user])
            raise ActiveRecord::DatabaseConnectionError.username_error(conn_params[:user])
          elsif conn_params && conn_params[:hostname] && error.message.include?(conn_params[:hostname])
            raise ActiveRecord::DatabaseConnectionError.hostname_error(conn_params[:hostname])
          else

Rails.root: /api

Application Trace | Framework Trace | Full Trace
activerecord (7.0.8) lib/active_record/connection_adapters/postgresql_adapter.rb:83:in `rescue in new_client'
activerecord (7.0.8) lib/active_record/connection_adapters/postgresql_adapter.rb:77:in `new_client'
activerecord (7.0.8) lib/active_record/connection_adapters/postgresql_adapter.rb:37:in `postgresql_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:656:in `public_send'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:656:in `new_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:700:in `checkout_new_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:679:in `try_to_checkout_new_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:640:in `acquire_connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:341:in `checkout'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_pool.rb:181:in `connection'
activerecord (7.0.8) lib/active_record/connection_adapters/abstract/connection_handler.rb:211:in `retrieve_connection'
activerecord (7.0.8) lib/active_record/connection_handling.rb:313:in `retrieve_connection'
activerecord (7.0.8) lib/active_record/connection_handling.rb:280:in `connection'
activerecord (7.0.8) lib/active_record/migration.rb:648:in `connection'
activerecord (7.0.8) lib/active_record/migration.rb:643:in `build_watcher'
activerecord (7.0.8) lib/active_record/migration.rb:625:in `block in call'
activerecord (7.0.8) lib/active_record/migration.rb:624:in `synchronize'
activerecord (7.0.8) lib/active_record/migration.rb:624:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/callbacks.rb:27:in `block in call'
activesupport (7.0.8) lib/active_support/callbacks.rb:99:in `run_callbacks'
actionpack (7.0.8) lib/action_dispatch/middleware/callbacks.rb:26:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/executor.rb:14:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/actionable_exceptions.rb:17:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/debug_exceptions.rb:28:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/show_exceptions.rb:29:in `call'
railties (7.0.8) lib/rails/rack/logger.rb:40:in `call_app'
railties (7.0.8) lib/rails/rack/logger.rb:25:in `block in call'
activesupport (7.0.8) lib/active_support/tagged_logging.rb:99:in `block in tagged'
activesupport (7.0.8) lib/active_support/tagged_logging.rb:37:in `tagged'
activesupport (7.0.8) lib/active_support/tagged_logging.rb:99:in `tagged'
railties (7.0.8) lib/rails/rack/logger.rb:25:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/remote_ip.rb:93:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/request_id.rb:26:in `call'
rack (2.2.8) lib/rack/runtime.rb:22:in `call'
activesupport (7.0.8) lib/active_support/cache/strategy/local_cache_middleware.rb:29:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/server_timing.rb:61:in `block in call'
actionpack (7.0.8) lib/action_dispatch/middleware/server_timing.rb:26:in `collect_events'
actionpack (7.0.8) lib/action_dispatch/middleware/server_timing.rb:60:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/executor.rb:14:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/static.rb:23:in `call'
rack (2.2.8) lib/rack/sendfile.rb:110:in `call'
actionpack (7.0.8) lib/action_dispatch/middleware/host_authorization.rb:138:in `call'
railties (7.0.8) lib/rails/engine.rb:530:in `call'
puma (5.6.8) lib/puma/configuration.rb:252:in `call'
puma (5.6.8) lib/puma/request.rb:77:in `block in handle_request'
puma (5.6.8) lib/puma/thread_pool.rb:340:in `with_force_shutdown'
puma (5.6.8) lib/puma/request.rb:76:in `handle_request'
puma (5.6.8) lib/puma/server.rb:443:in `process_client'
puma (5.6.8) lib/puma/thread_pool.rb:147:in `block in spawn_thread'
Exception Causes
PG::ConnectionBad: connection to server at "172.29.0.3", port 5432 failed: FATAL: password authentication failed for user "postgres"
Request
Parameters:

None
Toggle session dump
Toggle env dump
Response
Headers:

None
```

## 解決までの道のり

- 「そんなユーザーはいない」的なエラーを直す
docker-compose.ymlとdatabase.ymlとでユーザーとパスワードが一致していないことが原因ではないかと考えて修正を行なった。
    
    提供された設定は、Railsアプリケーションがデータベースに接続するためのデフォルトの設定を定義しています。しかし、デフォルトの設定が正しく機能するためには、PostgreSQLコンテナが同じユーザー名とパスワードで動作している必要があります。
    
    提供されたDocker Composeファイルでは、PostgreSQLコンテナに対して環境変数を設定していますが、その中には `POSTGRES_USER` と `POSTGRES_PASSWORD` が含まれていません。そのため、PostgreSQLコンテナはデフォルトのユーザー名 `postgres` として動作し、パスワードが設定されていますが、指定されていません。
    
    この場合、正しく設定するためには、Docker Composeファイルで `POSTGRES_USER` と `POSTGRES_PASSWORD` 環境変数を指定する必要があります。例えば以下のように設定します。
    
    ```yaml
    db:
      image: postgres:latest
      environment:
        POSTGRES_USER: postgres
        POSTGRES_PASSWORD: password
        TZ: "Asia/Tokyo"
      volumes:
        - db-volume:/var/lib/postgresql/data
      ports:
        - "5432:5432"
    
    ```
    
    上記の設定により、PostgreSQLコンテナが正しく起動し、Railsアプリケーションがデフォルトのユーザー名とパスワードでデータベースに接続できるようになります。
    

### docker-compose.yml

```bash
db:
    image: postgres:latest
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      TZ: "Asia/Tokyo"
    volumes:
      - db-volume:/var/lib/postgresql/data
    ports:
      - "5432:5432"
```

### backend/config/database.yml

```bash
default: &default
  adapter: postgresql
  encoding: unicode
  # For details on connection pooling, see Rails configuration guide
  # https://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: db
  username: postgres
  password: password
```

その後、下記サイトを参考に、volumeを一旦削除することにした。

[Rails+Postgresql+Dockerで発生する「There is an issue connecting to your database with your username/password, username xxx」でハマった件 - 行動すれば次の現実](https://blog.furu07yu.com/entry/docker-postgresql-rails-connecting-error)

GitHub Copilot: ボリュームが使用中であるため、削除できないというエラーメッセージが表示されています。まず、Docker Composeを停止してからボリュームを削除してみてください。

以下のコマンドを実行してみてください。

```bash
# Docker Composeを停止
docker-compose down

# Dockerのボリュームを削除
docker volume rm original_sample_db-volume

# Docker Composeを再起動
docker-compose up

```

これにより、Postgresのデータがリセットされ、新たにユーザー"postgres"が作成され、パスワードが設定されます。

エラーメッセージが変わった。

既視感あるやつ、多分データベースを作ってないから発生してるのではと推測。

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/d49fece9-3bba-41f7-951e-0ba790b59646/b780f8a4-0d62-4013-91f5-4c5145fcdece/Untitled.png)

```bash
docker compose exec api bin/rails db:create
```

```bash
~/mkapp/Original_sample docker compose exec api bin/rails db:create
Created database 'api_development'
Created database 'api_test'
~/mkapp/Original_sample
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/d49fece9-3bba-41f7-951e-0ba790b59646/3ada9f2d-390d-4a2b-b980-dbe9df29a210/Untitled.png)

成功。

```bash
~/mkapp/Original_sample docker compose up
[+] Running 4/3
 ✔ Network original_sample_default                                                                                                                   Created0.0s eating                                                                      0.0s
 ✔ Container original_sample-redis-1                                                                                                                 Created0.1s
[+] Running 5/5iginal_sample-db-1                                                                                         ✔ Network original_sample_default                                                                                                                   Created0.0s nux/amd64) does not match the detected host platform (linux/arm64/v8) and no spec ✔ Container original_sample-redis-1                                                                                                                 Created0.1s                                                                                   ✔ Container original_sample-db-1                                                                                                                    Created0.1s
 ! db The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested 0.0s
 ✔ Container original_sample-api-1                                                                                                                   Created0.1s
Attaching to api-1, db-1, redis-1
redis-1  | 1:C 08 Feb 2024 23:44:50.120 * oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis-1  | 1:C 08 Feb 2024 23:44:50.120 * Redis version=7.2.4, bits=64, commit=00000000, modified=0, pid=1, just started
redis-1  | 1:C 08 Feb 2024 23:44:50.120 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis-1  | 1:M 08 Feb 2024 23:44:50.120 * monotonic clock: POSIX clock_gettime
redis-1  | 1:M 08 Feb 2024 23:44:50.120 * Running mode=standalone, port=6379.
redis-1  | 1:M 08 Feb 2024 23:44:50.121 * Server initialized
redis-1  | 1:M 08 Feb 2024 23:44:50.121 * Ready to accept connections tcp
db-1     |
db-1     | PostgreSQL Database directory appears to contain a database; Skipping initialization
db-1     |
db-1     | 2024-02-09 08:44:50.711 JST [1] LOG:  starting PostgreSQL 16.1 (Debian 16.1-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
db-1     | 2024-02-09 08:44:50.715 JST [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db-1     | 2024-02-09 08:44:50.715 JST [1] LOG:  listening on IPv6 address "::", port 5432
db-1     | 2024-02-09 08:44:50.717 JST [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db-1     | 2024-02-09 08:44:50.759 JST [67] LOG:  database system was shut down at 2024-02-09 08:43:47 JST
db-1     | 2024-02-09 08:44:50.867 JST [1] LOG:  database system is ready to accept connections
api-1    | => Booting Puma
api-1    | => Rails 7.0.8 application starting in development
api-1    | => Run `bin/rails server --help` for more startup options
api-1    | Puma starting in single mode...
api-1    | * Puma version: 5.6.8 (ruby 3.3.0-p0) ("Birdie's Version")
api-1    | *  Min threads: 5
api-1    | *  Max threads: 5
api-1    | *  Environment: development
api-1    | *          PID: 8
api-1    | * Listening on http://0.0.0.0:3000
api-1    | Use Ctrl-C to stop
^CGracefully stopping... (press Ctrl+C again to force)
[+] Stopping 3/3
 ✔ Container original_sample-api-1    Stopped                                                                      10.2s
 ✔ Container original_sample-db-1     Stopped                                                                       0.2s
 ✔ Container original_sample-redis-1  Stopped                                                                       0.3s
canceled
~/mkapp/Original_sample docker compose down
[+] Running 4/4
 ✔ Container original_sample-api-1    Removed                                                                       0.0s
 ✔ Container original_sample-redis-1  Removed                                                                       0.0s
 ✔ Container original_sample-db-1     Removed                                                                       0.0s
 ✔ Network original_sample_default    Removed                                                                       0.1s
~/mkapp/Original_sample
```

GitHub Copilot: いいえ、エラーはありません。Docker Composeが正常に起動し、アプリケーション（api）、データベース（db）、およびRedis（redis）の各サービスが正常に動作しています。

アプリケーションはポート3000でリッスンしているため、ブラウザから`http://localhost:3000`にアクセスすることでアプリケーションに接続できるはずです。

もしアプリケーションに接続できない、またはアプリケーションでエラーが発生する場合は、その詳細を教えていただけますと幸いです。

### CORSの設定

```bash
docker compose up した状態で
```

```ruby
gem "rack-cors"
```

```bash
docker compose exec api bundle install
```

### backend/config/initializers/cors.rb

```bash
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "localhost:3000"

    resource "*",
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

# フロントエンドの構築

```bash
Original_sample
├backend
│├Gemfile
│├Gemfile.lock
│├entrypoint.sh
│└Dockerfile
├frontend
│└Dockerfile
└docker-compose.yml
```

### frontend/Dockerfile

```bash
FROM node:latest

WORKDIR /front

ENV CI=true
```

### docker-compose.yml 追記

```bash
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
```

### 一旦Docker compose downしたのち、

```bash
docker compose up --build
```

```bash
~/mkapp/Original_sample docker compose up --build
[+] Building 36.5s (22/22) FINISHED                                                                 docker:desktop-linux
 => [front internal] load build definition from Dockerfile                                                          0.0s
 => => transferring dockerfile: 121B                                                                                0.0s
 => [api internal] load build definition from Dockerfile                                                            0.0s
 => => transferring dockerfile: 435B                                                                                0.0s
 => [front internal] load metadata for docker.io/library/node:latest                                                2.9s
 => [api internal] load metadata for docker.io/library/ruby:latest                                                  2.1s
 => [front auth] library/node:pull token for registry-1.docker.io                                                   0.0s
 => [api auth] library/ruby:pull token for registry-1.docker.io                                                     0.0s
 => [api internal] load .dockerignore                                                                               0.0s
 => => transferring context: 2B                                                                                     0.0s
 => [api 1/9] FROM docker.io/library/ruby:latest@sha256:fb9d97f39ada3ff3ed0f1d04fc94efb59490c6c519f468b54a91e48de9  0.0s
 => => resolve docker.io/library/ruby:latest@sha256:fb9d97f39ada3ff3ed0f1d04fc94efb59490c6c519f468b54a91e48de9ea0e  0.0s
 => [api internal] load build context                                                                               0.1s
 => => transferring context: 1.17MB                                                                                 0.1s
 => CACHED [api 2/9] RUN mkdir /api                                                                                 0.0s
 => CACHED [api 3/9] WORKDIR /api                                                                                   0.0s
 => [api 4/9] COPY Gemfile /api/Gemfile                                                                             0.0s
 => [api 5/9] COPY Gemfile.lock /api/Gemfile.lock                                                                   0.0s
 => [api 6/9] RUN gem update --system 3.5.6 &&     bundle install                                                  29.0s
 => [front internal] load .dockerignore                                                                             0.0s
 => => transferring context: 2B                                                                                     0.0s
 => [front 1/2] FROM docker.io/library/node:latest@sha256:3af9f785cb8fc1a9c60a77c7b31b1ba7f5c74a066d142c996fbce61d  8.1s
 => => resolve docker.io/library/node:latest@sha256:3af9f785cb8fc1a9c60a77c7b31b1ba7f5c74a066d142c996fbce61d2420dd  0.0s
 => => sha256:d107a28dd93944540862052480444c9c247f1e6ae47afcf478d239ad97fea3e3 49.17MB / 49.17MB                    6.5s
 => => sha256:0a7b221eeb44c5e9694a009ad42dac109a419152eefdb6996230b05de1499ebd 2.23MB / 2.23MB                      2.0s
 => => sha256:cb7a03a532775c938026778d5433751da3b3c9c49c6e4e91224f9b4697de1f05 3.38kB / 3.38kB                      0.5s
 => => sha256:3af9f785cb8fc1a9c60a77c7b31b1ba7f5c74a066d142c996fbce61d2420dd8c 1.21kB / 1.21kB                      0.0s
 => => sha256:dff8495ffe77d4ad6ad6f86202778c7584ca235e54ba8f9b887830f22ac3af80 2.00kB / 2.00kB                      0.0s
 => => sha256:1ca4ed5f1542127bc4b0b94213daafabc887b73dc49e7116d59f3122c45460f5 7.35kB / 7.35kB                      0.0s
 => => extracting sha256:cb7a03a532775c938026778d5433751da3b3c9c49c6e4e91224f9b4697de1f05                           0.0s
 => => sha256:693d843fe0d88f1881130adaf2fbe451f7050fa8cb177dd44d576089acce49ac 451B / 451B                          1.3s
 => => extracting sha256:d107a28dd93944540862052480444c9c247f1e6ae47afcf478d239ad97fea3e3                           1.4s
 => => extracting sha256:0a7b221eeb44c5e9694a009ad42dac109a419152eefdb6996230b05de1499ebd                           0.0s
 => => extracting sha256:693d843fe0d88f1881130adaf2fbe451f7050fa8cb177dd44d576089acce49ac                           0.0s
 => [front 2/2] WORKDIR /front                                                                                      0.2s
 => [front] exporting to image                                                                                      0.0s
 => => exporting layers                                                                                             0.0s
 => => writing image sha256:5137636ace1db2a7c30f56b9ff851cd1f6cced382db9c403f499a72c9d2bef49                        0.0s
 => => naming to docker.io/library/original_sample-front                                                            0.0s
 => [api 7/9] COPY . /api                                                                                           2.0s
 => [api 8/9] COPY entrypoint.sh /usr/bin/                                                                          0.1s
 => [api 9/9] RUN chmod +x /usr/bin/entrypoint.sh                                                                   1.3s
 => [api] exporting to image                                                                                        1.5s
 => => exporting layers                                                                                             1.5s
 => => writing image sha256:a71a6ace64337fbc76af78a074cbfc664d0b75bf2cd638a8fbdd1bf8f8c65f54                        0.0s
 => => naming to docker.io/library/original_sample-api                                                              0.0s
[+] Running 4/4
 ✔ Container original_sample-front-1  Created                                                                       0.2s
 ✔ Container original_sample-redis-1  Created                                                                       0.0s
 ✔ Container original_sample-db-1     Created                                                                       0.0s
 ✔ Container original_sample-api-1    Recreated                                                                     0.2s
Attaching to api-1, db-1, front-1, redis-1
redis-1  | 1:C 09 Feb 2024 01:38:28.522 * oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis-1  | 1:C 09 Feb 2024 01:38:28.522 * Redis version=7.2.4, bits=64, commit=00000000, modified=0, pid=1, just started
redis-1  | 1:C 09 Feb 2024 01:38:28.522 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis-1  | 1:M 09 Feb 2024 01:38:28.522 * monotonic clock: POSIX clock_gettime
redis-1  | 1:M 09 Feb 2024 01:38:28.523 * Running mode=standalone, port=6379.
redis-1  | 1:M 09 Feb 2024 01:38:28.524 * Server initialized
redis-1  | 1:M 09 Feb 2024 01:38:28.531 * Loading RDB produced by version 7.2.4
redis-1  | 1:M 09 Feb 2024 01:38:28.531 * RDB age 52 seconds
redis-1  | 1:M 09 Feb 2024 01:38:28.531 * RDB memory usage when created 0.86 Mb
redis-1  | 1:M 09 Feb 2024 01:38:28.531 * Done loading RDB, keys loaded: 0, keys expired: 0.
redis-1  | 1:M 09 Feb 2024 01:38:28.531 * DB loaded from disk: 0.007 seconds
redis-1  | 1:M 09 Feb 2024 01:38:28.531 * Ready to accept connections tcp
front-1  | Welcome to Node.js v21.6.1.
front-1  | Type ".help" for more information.
db-1     |
db-1     | PostgreSQL Database directory appears to contain a database; Skipping initialization
db-1     |
db-1     | 2024-02-09 10:38:30.366 JST [1] LOG:  starting PostgreSQL 16.1 (Debian 16.1-1.pgdg120+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 12.2.0-14) 12.2.0, 64-bit
db-1     | 2024-02-09 10:38:30.372 JST [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
db-1     | 2024-02-09 10:38:30.372 JST [1] LOG:  listening on IPv6 address "::", port 5432
db-1     | 2024-02-09 10:38:30.817 JST [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
db-1     | 2024-02-09 10:38:30.837 JST [68] LOG:  database system was shut down at 2024-02-09 10:37:36 JST
db-1     | 2024-02-09 10:38:30.888 JST [1] LOG:  database system is ready to accept connections
api-1    | => Booting Puma
api-1    | => Rails 7.0.8 application starting in development
api-1    | => Run `bin/rails server --help` for more startup options
api-1    | Puma starting in single mode...
api-1    | * Puma version: 5.6.8 (ruby 3.3.0-p0) ("Birdie's Version")
api-1    | *  Min threads: 5
api-1    | *  Max threads: 5
api-1    | *  Environment: development
api-1    | *          PID: 8
api-1    | * Listening on http://0.0.0.0:3000
api-1    | Use Ctrl-C to stop
```

提供された情報を見る限り、コンテナが正常に起動し、エラーが発生していないように見えます。Redis、PostgreSQL、フロントエンド、およびバックエンドのすべてのコンテナが起動し、APIサーバーも正常に起動しています。

フロントエンドのコンテナでは、Node.jsのバージョンが表示され、PostgreSQLのコンテナはデータベースの初期化をスキップしていることが表示されています。バックエンドのAPIサーバーも正常に起動し、ポート3000でリクエストを待機しています。

したがって、提供されたDocker Composeファイルでのコンテナの起動は成功しています。

```bash
~/mkapp/Original_sample docker compose ps
NAME                      IMAGE                   COMMAND                   SERVICE   CREATED              STATUS              PORTS
original_sample-api-1     original_sample-api     "entrypoint.sh /bin/…"   api       About a minute ago   Up About a minute   0.0.0.0:3001->3000/tcp
original_sample-db-1      postgres:latest         "docker-entrypoint.s…"   db        18 minutes ago       Up About a minute   0.0.0.0:5432->5432/tcp
original_sample-front-1   original_sample-front   "docker-entrypoint.s…"   front     About a minute ago   Up About a minute   0.0.0.0:3000->3000/tcp
original_sample-redis-1   redis:latest            "docker-entrypoint.s…"   redis     18 minutes ago       Up About a minute   0.0.0.0:6379->6379/tcp
~/mkapp/Original_sample
```

成功してそうです。

```bash
docker compose exec front bash
```

```bash
npx create-react-app .
```

```bash
root@a94c6310b7f6:/front# npx create-react-app .
npm WARN exec The following package was not found and will be installed: create-react-app@5.0.1
npm WARN deprecated tar@2.2.2: This version of tar is no longer supported, and will not receive security updates. Please upgrade asap.

Creating a new React app in /front.

Installing packages. This might take a couple of minutes.
Installing react, react-dom, and react-scripts with cra-template...

added 1486 packages in 56s

251 packages are looking for funding
  run `npm fund` for details

Initialized a git repository.

Installing template dependencies using npm...

added 69 packages, and changed 1 package in 6s

255 packages are looking for funding
  run `npm fund` for details
Removing template package using npm...

removed 1 package, and audited 1555 packages in 1s

255 packages are looking for funding
  run `npm fund` for details

8 vulnerabilities (2 moderate, 6 high)

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
Git commit not created Error: Command failed: git commit -m "Initialize project using Create React App"
    at genericNodeError (node:internal/errors:984:15)
    at wrappedFn (node:internal/errors:538:14)
    at checkExecSyncError (node:child_process:890:11)
    at execSync (node:child_process:962:15)
    at tryGitCommit (/front/node_modules/react-scripts/scripts/init.js:62:5)
    at module.exports (/front/node_modules/react-scripts/scripts/init.js:350:25)
    at [eval]:3:14
    at runScriptInThisContext (node:internal/vm:144:10)
    at node:internal/process/execution:109:14
    at [eval]-wrapper:6:24 {
  status: 128,
  signal: null,
  output: [ null, null, null ],
  pid: 332,
  stdout: null,
  stderr: null
}
Removing .git directory...

Success! Created front at /front
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd /front
  npm start

Happy hacking!
root@a94c6310b7f6:/front#
```

`npx create-react-app .` コマンドを実行した結果、Reactアプリケーションが `/front` ディレクトリに作成されました。しかし、`git commit`コマンドの実行中にエラーが発生しました。

このエラーは、`git commit`が失敗したことを示しています。このエラーは、プロジェクトの初期化時にgitの設定が不足しているか、または環境によって引き起こされた可能性があります。

しかし、Reactアプリは正常に作成されましたので、以下のコマンドを使用してReactアプリを開始することができます。

```
cd /front
npm start

```

これにより、開発サーバーが起動し、Reactアプリケーションがブラウザで表示されます。

GitHub Copilot: Reactアプリケーションの作成が成功しました。次に進む前に、いくつかの警告と脆弱性が報告されていることに注意してください。これらは、使用しているパッケージが古いか、セキュリティ上の問題がある可能性があることを示しています。

これらの問題を解決するために、次のコマンドを実行することをお勧めします：

```bash
npm audit fix

```

このコマンドは、可能な限り自動的に問題を修正します。ただし、一部の問題は手動で解決する必要があるかもしれません。

その後、Reactアプリケーションを起動するには、次のコマンドを実行します：

```bash
npm start

```

これにより、開発サーバーが起動し、ブラウザでアプリケーションを表示できます。

GitHub Copilot: Reactアプリケーションを作成する際に表示されたメッセージによると、以下のような警告と脆弱性があります：

1. `npm WARN deprecated tar@2.2.2: This version of tar is no longer supported, and will not receive security updates. Please upgrade asap.`
    
    これは、使用している`tar`パッケージが古く、サポートが終了し、セキュリティ更新が提供されないことを示しています。早急にアップグレードすることが推奨されています。
    
2. `8 vulnerabilities (2 moderate, 6 high)`
    
    これは、インストールされたパッケージに8つの脆弱性（2つは中程度、6つは高度）が存在することを示しています。これらの脆弱性は、アプリケーションが攻撃に対して脆弱になる可能性がある問題です。
    

これらの問題を解決するためには、`npm audit fix`コマンドを実行することが推奨されています。このコマンドは、可能な限り自動的に問題を修正します。ただし、一部の問題は手動で解決する必要があるかもしれません。

### 脆弱性への対処

```bash
npm audit fix
```

```bash
root@a94c6310b7f6:/front# npm audit fix

up to date, audited 1555 packages in 4s

255 packages are looking for funding
  run `npm fund` for details

# npm audit report

nth-check  <2.0.1
Severity: high
Inefficient Regular Expression Complexity in nth-check - https://github.com/advisories/GHSA-rp65-9cf3-cjxr
fix available via `npm audit fix --force`
Will install react-scripts@3.0.1, which is a breaking change
node_modules/svgo/node_modules/nth-check
  css-select  <=3.1.0
  Depends on vulnerable versions of nth-check
  node_modules/svgo/node_modules/css-select
    svgo  1.0.0 - 1.3.2
    Depends on vulnerable versions of css-select
    node_modules/svgo
      @svgr/plugin-svgo  <=5.5.0
      Depends on vulnerable versions of svgo
      node_modules/@svgr/plugin-svgo
        @svgr/webpack  4.0.0 - 5.5.0
        Depends on vulnerable versions of @svgr/plugin-svgo
        node_modules/@svgr/webpack
          react-scripts  >=2.1.4
          Depends on vulnerable versions of @svgr/webpack
          Depends on vulnerable versions of resolve-url-loader
          node_modules/react-scripts

postcss  <8.4.31
Severity: moderate
PostCSS line return parsing error - https://github.com/advisories/GHSA-7fh5-64p2-3v2j
fix available via `npm audit fix --force`
Will install react-scripts@3.0.1, which is a breaking change
node_modules/resolve-url-loader/node_modules/postcss
  resolve-url-loader  0.0.1-experiment-postcss || 3.0.0-alpha.1 - 4.0.0
  Depends on vulnerable versions of postcss
  node_modules/resolve-url-loader

8 vulnerabilities (2 moderate, 6 high)

To address all issues (including breaking changes), run:
  npm audit fix --force
root@a94c6310b7f6:/front#
```

GitHub Copilot: `npm audit fix`コマンドを実行した結果、いくつかの脆弱性が依然として存在しているようです。これらの脆弱性は、`nth-check`と`postcss`パッケージに関連しています。

これらの問題を解決するためには、`npm audit fix --force`コマンドを実行することが推奨されています。ただし、このコマンドは破壊的な変更を含む可能性があります。つまり、一部のパッケージが大幅にダウングレードされ、アプリケーションの動作に影響を与える可能性があります。

したがって、このコマンドを実行する前に、アプリケーションのバックアップを取るか、ソースコードのバージョン管理システム（例えばGit）を使用していることを確認してください。

### 破壊的な変更にビビってググる

[create-react-appでは脆弱性の警告が出るが無視して良い](https://zenn.dev/appare45/articles/7f667031aab94b)

よくわからないけど、出るものらしい。

無視することを決め込むぜ！

```bash
npm start
```

```bash
root@a94c6310b7f6:/front# npm start

> front@0.1.0 start
> react-scripts start

(node:407) [DEP_WEBPACK_DEV_SERVER_ON_AFTER_SETUP_MIDDLEWARE] DeprecationWarning: 'onAfterSetupMiddleware' option is deprecated. Please use the 'setupMiddlewares' option.
(Use `node --trace-deprecation ...` to show where the warning was created)
(node:407) [DEP_WEBPACK_DEV_SERVER_ON_BEFORE_SETUP_MIDDLEWARE] DeprecationWarning: 'onBeforeSetupMiddleware' option is deprecated. Please use the 'setupMiddlewares' option.
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
  On Your Network:  http://192.168.80.3:3000

Note that the development build is not optimized.
To create a production build, use npm run build.

webpack compiled successfully
```

GitHub Copilot: Reactアプリケーションの起動に成功しました。ただし、いくつかの警告が表示されています。

1. `DeprecationWarning: 'onAfterSetupMiddleware' option is deprecated. Please use the 'setupMiddlewares' option.` と `DeprecationWarning: 'onBeforeSetupMiddleware' option is deprecated. Please use the 'setupMiddlewares' option.`
    
    これらの警告は、Webpack Dev Serverの設定オプションが非推奨になったことを示しています。これらは開発サーバーの設定に関連しており、アプリケーションの動作には影響しません。
    
2. `One of your dependencies, babel-preset-react-app, is importing the "@babel/plugin-proposal-private-property-in-object" package without declaring it in its dependencies.`
    
    これは、`babel-preset-react-app`が依存関係を正しく宣言していないために発生する警告です。この問題を解決するためには、`@babel/plugin-proposal-private-property-in-object`をdevDependenciesに追加することが推奨されています。
    

これらの警告は、開発中のアプリケーションの動作には影響しませんが、将来的に問題が発生する可能性があります。したがって、これらの警告を解決することをお勧めします。

### ・・・アプリの動作に影響しないなら無視を決め込むぜ！

### localhost:3000にアクセス

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/d49fece9-3bba-41f7-951e-0ba790b59646/8938b346-7513-4966-b1f8-28714b16204c/Untitled.png)

成功。
