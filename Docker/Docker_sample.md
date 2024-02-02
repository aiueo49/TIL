# Dockerの環境構築を試してみた

# 技術面談

# Docker環境を構築できない。

## ゴール

Dockerの環境を構築する。

## 状況

[https://school.runteq.jp/v2/mypage/helps/articles/build_docker_environment_rails#rails new](https://school.runteq.jp/v2/mypage/helps/articles/build_docker_environment_rails#rails%20new)

を参考に進めていたところ、

`docker compose run --rm web rails new . -d=postgresql -j=esbuild -c=sass`

投入後のエラーを解消できない。

## エラーメッセージ

```bash
~/mkapp/Docker_sample docker compose run --rm web rails new . -d=postgresql -j=esbuild -c=sass
[+] Creating 1/0
 ✔ Container docker_sample-db-1  Created                                                                            0.0s
[+] Running 1/1
 ✔ Container docker_sample-db-1  Started                                                                            0.4s
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:14: [BUG] Segmentation fault at 0x004cffff93c106c0
ruby 3.3.0 (2023-12-25 revision 5124f9ac75) [aarch64-linux]

-- Control frame information -----------------------------------------------
c:0039 p:---- s:0177 e:000176 CFUNC  :resume
c:0038 p:0007 s:0173 E:001b90 BLOCK  /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:14 [FINISH]
c:0037 p:---- s:0170 e:000169 CFUNC  :synchronize
c:0036 p:0026 s:0166 E:000fe8 METHOD /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:12
c:0035 p:0007 s:0161 e:000160 CLASS  /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:18
c:0034 p:0025 s:0158 e:000157 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/lock_local_var.rb:5 [FINISH]
c:0033 p:---- s:0155 e:000154 CFUNC  :require
c:0032 p:0023 s:0150 e:000149 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0031 p:0041 s:0144 e:000143 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/reentrant_read_write_lock.rb:7 [FINISH]
c:0030 p:---- s:0141 e:000140 CFUNC  :require
c:0029 p:0023 s:0136 e:000135 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0028 p:0047 s:0130 e:000129 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomics.rb:8 [FINISH]
c:0027 p:---- s:0127 e:000126 CFUNC  :require
c:0026 p:0023 s:0122 e:000121 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0025 p:0029 s:0116 e:000115 TOP    /usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent.rb:6 [FINISH]
c:0024 p:---- s:0113 e:000112 CFUNC  :require
c:0023 p:0023 s:0108 e:000107 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0022 p:0017 s:0102 e:000101 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_thread_safe_level.rb:5 [FINISH]
c:0021 p:---- s:0099 e:000098 CFUNC  :require
c:0020 p:0023 s:0094 e:000093 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0019 p:0017 s:0088 e:000087 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_silence.rb:5 [FINISH]
c:0018 p:---- s:0085 e:000084 CFUNC  :require
c:0017 p:0023 s:0080 e:000079 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0016 p:0005 s:0074 e:000073 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger.rb:3 [FINISH]
c:0015 p:---- s:0071 e:000070 CFUNC  :require
c:0014 p:0023 s:0066 e:000065 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0013 p:0023 s:0060 e:000059 TOP    /usr/local/bundle/gems/activesupport-7.0.8/lib/active_support.rb:29 [FINISH]
c:0012 p:---- s:0057 e:000056 CFUNC  :require
c:0011 p:0023 s:0052 e:000051 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0010 p:0005 s:0046 e:000045 TOP    /usr/local/bundle/gems/railties-7.0.8/lib/rails/command.rb:3 [FINISH]
c:0009 p:---- s:0043 e:000042 CFUNC  :require
c:0008 p:0023 s:0038 e:000037 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0007 p:0030 s:0032 E:002540 TOP    /usr/local/bundle/gems/railties-7.0.8/lib/rails/cli.rb:12 [FINISH]
c:0006 p:---- s:0029 e:000028 CFUNC  :require
c:0005 p:0023 s:0024 e:000023 METHOD <internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127
c:0004 p:0042 s:0018 e:000017 TOP    /usr/local/bundle/gems/railties-7.0.8/exe/rails:10 [FINISH]
c:0003 p:---- s:0013 e:000012 CFUNC  :load
c:0002 p:0078 s:0008 E:000d80 EVAL   /usr/local/bundle/bin/rails:25 [FINISH]
c:0001 p:0000 s:0003 E:000750 DUMMY  [FINISH]

-- Ruby level backtrace information ----------------------------------------
/usr/local/bundle/bin/rails:25:in `<main>'
/usr/local/bundle/bin/rails:25:in `load'
/usr/local/bundle/gems/railties-7.0.8/exe/rails:10:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/railties-7.0.8/lib/rails/cli.rb:12:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/railties-7.0.8/lib/rails/command.rb:3:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support.rb:29:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger.rb:3:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_silence.rb:5:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/activesupport-7.0.8/lib/active_support/logger_thread_safe_level.rb:5:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent.rb:6:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomics.rb:8:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
/usr/local/bundle/gems/concurrent-ruby-1.2.3/lib/concurrent-ruby/concurrent/atomic/reentrant_read_write_lock.rb:7:in `<top (required)>'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
<internal:/usr/local/lib/ruby/3.3.0/rubygems/core_ext/kernel_require.rb>:127:in `require'
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
  x0: 0x0000aaaae6c08b50  x1: 0x0000aaaae715a060  x2: 0x0000ffffdb8e1610
  x3: 0x0000ffff7990ff60  x4: 0x0000ffff79910018  x5: 0x0000ffff79930000
  x6: 0x0000ffff9415db88  x7: 0x0000000000000000 x18: 0x0000000000000025
 x19: 0x0000000000000000 x20: 0x0000000000000000 x21: 0x0000000000000000
 x22: 0x0000000000000000 x23: 0x0000000000000000 x24: 0x0000000000000000
 x25: 0x0000000000000000 x26: 0x0000000000000000 x27: 0x0000000000000000
 x28: 0x0000000000000000 x29: 0x0000000000000000  sp: 0x0000ffff79910000
 fau: 0x004cffff93c106c0

-- C level backtrace information -------------------------------------------
~/mkapp/Docker_sample
```

## GPT

提供されたエラーメッセージを見る限り、`concurrent-ruby` gemのバージョン1.2.3が、Ruby 3.3.0との互換性のないセグメンテーションフォルト（Segmentation fault）を引き起こしているようです。これは一般的に、gemやライブラリが特定のRubyバージョンと互換性がない場合に発生します。

問題を解決するためには、いくつかのアプローチが考えられます。

1. **`concurrent-ruby` gemのバージョンの変更**：別のバージョンの`concurrent-ruby` gemを試してみてください。Gemfileにてバージョンを指定するか、最新の安定版を使用してみてください。例えば、Gemfileに以下のように記述します。
    
    ```ruby
    gem 'concurrent-ruby', '1.1.8'
    
    ```
    
    そして、`bundle install`を実行します。
    
2. **Rubyのバージョンの変更**：Rubyのバージョンを変更してみることも検討できます。Ruby 3.2.0など、互換性のあるバージョンを試してみてください。Dockerfile内のRubyのバージョンを変更して再度ビルドします。
    
    ```
    # Dockerfile内のFROM行を変更
    FROM ruby:3.2.0
    
    ```
    
3. **デバッグツールの利用**：デバッグツールを使用して、詳細なエラーメッセージやスタックトレースを取得し、問題の特定を試みることもできます。しかし、これにはRubyやgemの開発者向けのスキルが必要です。

上記のアプローチのいずれかが問題を解決するか、少なくとも原因を特定する手助けとなるでしょう。問題が解決しない場合や追加の支援が必要な場合は、具体的なgemのバージョンやDockerfileの内容を教えていただくと、より具体的なアドバイスができるかもしれません。

## Dockerfile内、Rubyのバージョンをruby:3.2.0に変更

## docker-compose buildを実行

```bash
~/mkapp/Docker_sample docker-compose build

[+] Building 63.3s (17/17) FINISHED                                                                 docker:desktop-linux
 => [web internal] load build definition from Dockerfile                                                            0.0s
 => => transferring dockerfile: 1.03kB                                                                              0.0s
 => [web internal] load metadata for docker.io/library/ruby:3.2.2                                                   2.6s
 => [web auth] library/ruby:pull token for registry-1.docker.io                                                     0.0s
 => [web internal] load .dockerignore                                                                               0.0s
 => => transferring context: 2B                                                                                     0.0s
 => [web  1/11] FROM docker.io/library/ruby:3.2.2@sha256:bc2b1b2c5cf0423b9ca12f2c2196c51efa80c8d3d4d0cf01858e67dc2  7.3s
 => => resolve docker.io/library/ruby:3.2.2@sha256:bc2b1b2c5cf0423b9ca12f2c2196c51efa80c8d3d4d0cf01858e67dc2c7136e  0.0s
 => => sha256:bc2b1b2c5cf0423b9ca12f2c2196c51efa80c8d3d4d0cf01858e67dc2c7136e4 7.60kB / 7.60kB                      0.0s
 => => sha256:384a6c3f9103bba8c9812e5427721b62f17ff446dfee7eebbbc7dbd4cb597922 2.00kB / 2.00kB                      0.0s
 => => sha256:4060a4e2dc3b039355b5c8f3a5eaaeea4057267372de1aaa8c3a3d8f41f04983 34.58MB / 34.58MB                    4.6s
 => => sha256:45b47e100c5ba13451d58940a01d5b04024ef56a0c6379ddde735edf1797dcdf 6.97kB / 6.97kB                      0.0s
 => => sha256:f9ce1f44eaed88bed59c21480fd169f9d2687115eea462c9d694fc85f535b959 144B / 144B                          0.8s
 => => extracting sha256:4060a4e2dc3b039355b5c8f3a5eaaeea4057267372de1aaa8c3a3d8f41f04983                           2.6s
 => => extracting sha256:f9ce1f44eaed88bed59c21480fd169f9d2687115eea462c9d694fc85f535b959                           0.0s
 => [web internal] load build context                                                                               0.0s
 => => transferring context: 4.92kB                                                                                 0.0s
 => [web  2/11] RUN curl -sL https://deb.nodesource.com/setup_20.x | bash - && wget --quiet -O - /tmp/pubkey.gpg   33.1s
 => [web  3/11] RUN mkdir /app                                                                                      0.9s
 => [web  4/11] WORKDIR /app                                                                                        0.1s
 => [web  5/11] RUN gem install bundler                                                                             1.9s
 => [web  6/11] COPY Gemfile /app/Gemfile                                                                           0.0s
 => [web  7/11] COPY Gemfile.lock /app/Gemfile.lock                                                                 0.0s
 => [web  8/11] COPY yarn.lock /app/yarn.lock                                                                       0.0s
 => [web  9/11] RUN bundle install                                                                                 15.2s
 => [web 10/11] RUN yarn install                                                                                    0.9s
 => [web 11/11] COPY . /app                                                                                         0.0s
 => [web] exporting to image                                                                                        0.9s
 => => exporting layers                                                                                             0.9s
 => => writing image sha256:f19aa19cda7468f268a7e9cd43f3fdc9aaf527ddbda79f9e94ca1229981cbb5b                        0.0s
 => => naming to docker.io/library/docker_sample-web                                                                0.0s
```

## docker compose run --rm web rails new . -d=postgresql -j=esbuild -c=sass実行

```bash
~/mkapp/Docker_sample docker compose run --rm web rails new . -d=postgresql -j=esbuild -c=sass
[+] Creating 1/0
 ✔ Container docker_sample-db-1  Running                                                                            0.0s
Ignoring nio4r-2.7.0 because its extensions are not built. Try: gem pristine nio4r --version 2.7.0
Ignoring websocket-driver-0.7.6 because its extensions are not built. Try: gem pristine websocket-driver --version 0.7.6
       exist
      create  README.md
      create  Rakefile
      create  .ruby-version
      create  config.ru
      create  .gitignore
      create  .gitattributes
    conflict  Gemfile
Overwrite /app/Gemfile? (enter "h" for help) [Ynaqdhm]
```

おや？何やら今まで見たことのない画面に。

```bash
Overwrite /app/Gemfile? (enter "h" for help) [Ynaqdhm] y
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
Initialized empty Git repository in /app/.git/
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
      create  test/system
      create  test/system/.keep
      create  test/application_system_test_case.rb
      create  storage
      create  storage/.keep
      create  tmp/storage
      create  tmp/storage/.keep
      remove  config/initializers/cors.rb
      remove  config/initializers/new_framework_defaults_7_0.rb
         run  bundle install
Ignoring nio4r-2.7.0 because its extensions are not built. Try: gem pristine nio4r --version 2.7.0
Ignoring websocket-driver-0.7.6 because its extensions are not built. Try: gem pristine websocket-driver --version 0.7.6
Fetching gem metadata from https://rubygems.org/..........
Resolving dependencies...
Fetching rake 13.1.0
Installing rake 13.1.0
Fetching timeout 0.4.1
Fetching public_suffix 5.0.4
Fetching racc 1.7.3
Fetching base64 0.2.0
Fetching bindex 0.8.1
Fetching msgpack 1.7.2
Fetching date 3.3.4
Installing timeout 0.4.1
Installing nio4r 2.7.0 with native extensions
Fetching regexp_parser 2.9.0
Installing racc 1.7.3 with native extensions
Installing public_suffix 5.0.4
Installing msgpack 1.7.2 with native extensions
Installing base64 0.2.0
Fetching stringio 3.1.0
Installing bindex 0.8.1 with native extensions
Fetching io-console 0.7.2
Installing date 3.3.4 with native extensions
Installing stringio 3.1.0 with native extensions
Installing io-console 0.7.2 with native extensions
Installing regexp_parser 2.9.0
Fetching pg 1.5.4
Installing pg 1.5.4 with native extensions
Fetching rexml 3.2.6
Installing rexml 3.2.6
Fetching rubyzip 2.3.2
Installing rubyzip 2.3.2
Fetching websocket 1.2.10
Installing websocket 1.2.10
Fetching sprockets 4.2.1
Installing sprockets 4.2.1
Installing websocket-driver 0.7.6 with native extensions
Fetching net-protocol 0.2.2
Installing net-protocol 0.2.2
Fetching addressable 2.8.6
Installing addressable 2.8.6
Fetching selenium-webdriver 4.17.0
Fetching xpath 3.2.0
Fetching psych 5.1.2
Installing psych 5.1.2 with native extensions
Installing xpath 3.2.0
Fetching capybara 3.40.0
Installing capybara 3.40.0
Installing selenium-webdriver 4.17.0
Fetching jbuilder 2.11.5
Installing jbuilder 2.11.5
Fetching sprockets-rails 3.4.2
Installing sprockets-rails 3.4.2
Fetching cssbundling-rails 1.4.0
Installing cssbundling-rails 1.4.0
Fetching jsbundling-rails 1.3.0
Installing jsbundling-rails 1.3.0
Fetching stimulus-rails 1.3.3
Installing stimulus-rails 1.3.3
Fetching turbo-rails 1.5.0
Fetching web-console 4.2.1
Installing web-console 4.2.1
Installing turbo-rails 1.5.0
Fetching reline 0.4.2
Fetching rdoc 6.6.2
Installing reline 0.4.2
Installing rdoc 6.6.2
Fetching irb 1.11.1
Installing irb 1.11.1
Fetching debug 1.9.1
Installing debug 1.9.1 with native extensions
Fetching puma 5.6.8
Installing puma 5.6.8 with native extensions
Fetching bootsnap 1.18.3
Installing bootsnap 1.18.3 with native extensions
Bundle complete! 15 Gemfile dependencies, 76 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
         run  bundle binstubs bundler
       rails  javascript:install:esbuild
       apply  /usr/local/bundle/gems/jsbundling-rails-1.3.0/lib/install/install.rb
  Compile into app/assets/builds
      create    app/assets/builds
      create    app/assets/builds/.keep
      append    app/assets/config/manifest.js
      append    .gitignore
      append    .gitignore
  Add JavaScript include tag in application layout
      insert    app/views/layouts/application.html.erb
  Create default entrypoint in app/javascript/application.js
      create    app/javascript
      create    app/javascript/application.js
  Add default package.json
      create    package.json
  Add bin/dev to start foreman
      create    bin/dev
       apply  /usr/local/bundle/gems/jsbundling-rails-1.3.0/lib/install/install_procfile.rb
  Add default Procfile.dev
      create    Procfile.dev
  Ensure foreman is installed
         run    gem install foreman from "."
Fetching foreman-0.87.2.gem
Successfully installed foreman-0.87.2
1 gem installed

A new release of RubyGems is available: 3.4.10 → 3.5.5!
Run `gem update --system 3.5.5` to update your installation.

Install esbuild
         run  yarn add esbuild from "."
yarn add v1.22.19
error An unexpected error occurred: "ENOSYS: function not implemented, fstat".
info If you think this is a bug, please open a bug report with the information provided in "/app/yarn-error.log".
info Visit https://yarnpkg.com/en/docs/cli/add for documentation about this command.
Add build script
         run  npm pkg set scripts.build="esbuild app/javascript/*.* --bundle --sourcemap --format=esm --outdir=app/assets/builds --public-path=/assets" from "."
         run  yarn build from "."
yarn run v1.22.19
$ esbuild app/javascript/*.* --bundle --sourcemap --format=esm --outdir=app/assets/builds --public-path=/assets
/bin/sh: 1: esbuild: not found
error Command failed with exit code 127.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
       rails  turbo:install stimulus:install
Import Turbo
      append  app/javascript/application.js
Install Turbo
         run  yarn add @hotwired/turbo-rails from "."
yarn add v1.22.19
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 3 new dependencies.
info Direct dependencies
└─ @hotwired/turbo-rails@7.3.0
info All dependencies
├─ @hotwired/turbo-rails@7.3.0
├─ @hotwired/turbo@7.3.0
└─ @rails/actioncable@7.1.3
Done in 17.54s.
Run turbo:install:redis to switch on Redis and use it in development for turbo streams
Create controllers directory
      create  app/javascript/controllers
      create  app/javascript/controllers/index.js
      create  app/javascript/controllers/application.js
      create  app/javascript/controllers/hello_controller.js
Import Stimulus controllers
      append  app/javascript/application.js
Install Stimulus
         run  yarn add @hotwired/stimulus from "."
yarn add v1.22.19
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 1 new dependency.
info Direct dependencies
└─ @hotwired/stimulus@3.2.2
info All dependencies
└─ @hotwired/stimulus@3.2.2
Done in 2.05s.
       rails  css:install:sass
       apply  /usr/local/bundle/gems/cssbundling-rails-1.4.0/lib/install/install.rb
  Build into app/assets/builds
       exist    app/assets/builds
   identical    app/assets/builds/.keep
   unchanged    app/assets/config/manifest.js
  Stop linking stylesheets automatically
        gsub    app/assets/config/manifest.js
   unchanged    .gitignore
   unchanged    .gitignore
  Remove app/assets/stylesheets/application.css so build output can take over
      remove    app/assets/stylesheets/application.css
  Add stylesheet link tag in application layout
   unchanged    app/views/layouts/application.html.erb
      append    Procfile.dev
  Add bin/dev to start foreman
   identical    bin/dev
Install Sass
      create  app/assets/stylesheets/application.sass.scss
         run  yarn add sass from "."
yarn add v1.22.19
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 17 new dependencies.
info Direct dependencies
└─ sass@1.70.0
info All dependencies
├─ anymatch@3.1.3
├─ binary-extensions@2.2.0
├─ braces@3.0.2
├─ chokidar@3.5.3
├─ fill-range@7.0.1
├─ glob-parent@5.1.2
├─ immutable@4.3.5
├─ is-binary-path@2.1.0
├─ is-extglob@2.1.1
├─ is-glob@4.0.3
├─ is-number@7.0.0
├─ normalize-path@3.0.0
├─ picomatch@2.3.1
├─ readdirp@3.6.0
├─ sass@1.70.0
├─ source-map-js@1.0.2
└─ to-regex-range@5.0.1
Done in 2.68s.
Add build:css script
Add build:css script
         run  npm pkg set scripts.build:css="sass ./app/assets/stylesheets/application.sass.scss:./app/assets/builds/application.css --no-source-map --load-path=node_modules" from "."
         run  yarn build:css from "."
yarn run v1.22.19
$ sass ./app/assets/stylesheets/application.sass.scss:./app/assets/builds/application.css --no-source-map --load-path=node_modules
Done in 1.13s.
~/mkapp/Docker_sample
```

いけてそう。

## GPT

新しいRuby on RailsアプリケーションをDockerで初期化し、プロジェクトのセットアップを開始したようですね。ただし、いくつかの問題があるようです：

1. **esbuildが見つかりません:**
    
    ```
    /bin/sh: 1: esbuild: not found
    
    ```
    
    `esbuild`がインストールされていないか、プロジェクトで利用できないようです。以下のコマンドで`esbuild`をインストールしてみてください：
    
    ```bash
    yarn add -D esbuild
    
    ```
    
2. **Foremanのインストール:**
    
    ```
    A new release of RubyGems is available: 3.4.10 → 3.5.5!
    Run `gem update --system 3.5.5` to update your installation.
    
    ```
    
    提案されたコマンドを実行してRubyGemsをアップデートできます：
    
    ```bash
    gem update --system 3.5.5
    
    ```
    
3. **Turbo Streams Redis:**
    
    ```
    Run turbo:install:redis to switch on Redis and use it in development for turbo streams
    
    ```
    
    Turbo Streamsを使用し、開発でRedisを活用したい場合は、次のコマンドを実行できます：
    
    ```bash
    rails turbo:install:redis
    
    ```
    
4. **Sassの設定:**
    
    ```
    error An unexpected error occurred: "ENOSYS: function not implemented, fstat".
    
    ```
    
    このエラーは、ファイルシステム操作に関連する問題がある可能性があります。Dockerのセットアップが正しくファイル操作を許可しているか確認してください。
    
5. **Sassのコンパイルエラー:**
    
    ```
    error Command failed with exit code 127.
    
    ```
    
    このエラーは、Sassのコンパイルスクリプトに問題がある可能性があります。`package.json`とSassに関連するスクリプトが正しく設定されているか確認してください。
    

これらの問題に対処した後、Ruby on Railsプロジェクトのセットアップを続行できるはずです。特定のエラーが発生した場合やさらなる質問がある場合は、お気軽に質問してください。

## 上記エラー全無視でデータベースを整備

## **config/database.ymlのdefaultに追記**

```bash
host: db
username: postgres
password: password
```

## コンテナの中でデータベースをcreateしてmigrate

```bash
~/mkapp/Docker_sample docker-compose run web bash

[+] Creating 1/0
 ✔ Container docker_sample-db-1  Created                                                                            0.0s
[+] Running 1/1
 ✔ Container docker_sample-db-1  Started                                                                            0.2s
root@60f7326d3f27:/app# bin/rails db:create
Created database 'app_development'
Created database 'app_test'
root@60f7326d3f27:/app# bin/rails db:migrate
root@60f7326d3f27:/app#
root@60f7326d3f27:/app#
root@60f7326d3f27:/app# exit
exit
```

## 起動する

```bash
~/mkapp/Docker_sample docker compose up
[+] Running 2/0
 ✔ Container docker_sample-db-1   Running                                                                           0.0s
 ✔ Container docker_sample-web-1  Created                                                                           0.0s
Attaching to db-1, web-1
web-1  | => Booting Puma
web-1  | => Rails 7.0.8 application starting in development
web-1  | => Run `bin/rails server --help` for more startup options
web-1  | Puma starting in single mode...
web-1  | * Puma version: 5.6.8 (ruby 3.2.2-p53) ("Birdie's Version")
web-1  | *  Min threads: 5
web-1  | *  Max threads: 5
web-1  | *  Environment: development
web-1  | *          PID: 1
web-1  | * Listening on http://0.0.0.0:3000
web-1  | Use Ctrl-C to stop
web-1  | Started GET "/" for 192.168.65.1 at 2024-02-02 12:11:33 +0900
web-1  | Cannot render console from 192.168.65.1! Allowed networks: 127.0.0.0/127.255.255.255, ::1
web-1  |   ActiveRecord::SchemaMigration Pluck (2.4ms)  SELECT "schema_migrations"."version" FROM "schema_migrations" ORDER BY "schema_migrations"."version" ASC
web-1  | Processing by Rails::WelcomeController#index as HTML
web-1  |   Rendering /usr/local/bundle/gems/railties-7.0.8/lib/rails/templates/rails/welcome/index.html.erb
web-1  |   Rendered /usr/local/bundle/gems/railties-7.0.8/lib/rails/templates/rails/welcome/index.html.erb (Duration: 2.0ms | Allocations: 489)
web-1  | Completed 200 OK in 18ms (Views: 4.5ms | ActiveRecord: 0.0ms | Allocations: 4739)
web-1  |
web-1  |
db-1   | 2024-02-02 12:15:58.680 JST [63] LOG:  checkpoint starting: time
db-1   | 2024-02-02 12:19:13.247 JST [63] LOG:  checkpoint complete: wrote 1856 buffers (11.3%); 1 WAL file(s) added, 0 removed, 0 recycled; write=194.408 s, sync=0.068 s, total=194.560 s; sync files=605, longest=0.003 s, average=0.001 s; distance=8612 kB, estimate=8612 kB; lsn=0/1D7C638, redo lsn=0/1D7C600
```

[http://localhost:3000](http://localhost:3000/)にアクセスできた。成功。
