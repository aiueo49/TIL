# Rails APIモードを構築する試み

[[Rails]APIモード＋Reactでrails newする](https://zenn.dev/redheadchloe/articles/03fe77e5a438d3)

**rails new**

```bash
rails new . --api
```

**gemをインストール**

```ruby
# Use Rack CORS for handling Cross-Origin Resource Sharing (CORS), making cross-origin AJAX possible
gem "rack-cors"
```

```bash
bundle install
```

**CORS対策**

```ruby
# config/initializers/cors.rb
****
# Be sure to restart your server when you modify this file.

# Avoid CORS issues when API is called from the frontend app.
# Handle Cross-Origin Resource Sharing (CORS) in order to accept cross-origin Ajax requests.

# Read more: https://github.com/cyu/rack-cors

 Rails.application.config.middleware.insert_before 0, Rack::Cors do
   allow do
     origins "http://localhost:3001" # This is the URL of the frontend app

     resource "*",
       headers: :any,
       methods: [:get, :post, :put, :patch, :delete, :options, :head]
   end
 end
```

<aside>
💡 railsを3000ポートで起動すると仮定した場合、ポート重複を避けて
reactは3001ポートで起動するため、origins "http://localhost:3001"となる。
ポート番号を間違えるとデータを取得できなくなるので注意。
データを取得できない場合はブラウザの（検証）のconsoleにエラーメッセージが表示されている可能性があるのでそこをチェック。

</aside>

**scaffoldでPostモデルを作成**

```bash
rails g scaffold Post title body:text
```

```bash
rails db:migrate
```

`**Faker`を使ってダミー投稿を作成**

```ruby
# Gemfile

gem 'faker'
```

```bash
bundle install
```

```ruby
# db/seeds.rb

require 'faker'

20.times do
  Post.create(
    title: Faker::Lorem.sentence(word_count: 3),
    body: Faker::Lorem.paragraphs(number: 3).join("\n")  # sentencesをnumberに変更
  )
end
```

- 解説
    
    
このコードは、Railsアプリケーションの`db/seeds.rb`ファイルに含まれているもので、データベースにダミーデータ（仮データ）を挿入するためのものです。具体的には、`Post`モデルに20個のダミーポストを作成しています。
    
    ここで行っていることは以下の通りです：
    
    1. `require 'faker'`: `faker` gemを使うために、`Faker`ライブラリを読み込んでいます。
    2. `20.times do ... end`: 
20回繰り返すループを作成しています。これにより、20個のダミーポストを作成する処理が20回実行されます。
    3. `Post.create(...)`: 
`Post`モデルに対して新しいレコードを作成します。各レコードにはランダムなタイトルと本文が含まれます。
    4. `title: Faker::Lorem.sentence(word_count: 3)`: 
`Faker::Lorem.sentence`メソッドを使用して、3つの単語からなるランダムな文を生成し、それを`title`として設定します。
    5. `body: Faker::Lorem.paragraphs(number: 3).join("\\n")`: 
`Faker::Lorem.paragraphs`メソッドを使用して、3つの段落からなるランダムな本文を生成し、それを`body`として設定します。`number`引数は段落の数を指定します。そして、それらの段落を改行で結合しています。
    
    
これにより、データベースに20個のダミーポストが挿入され、開発中にアプリケーションの機能をテストしたり、UIを開発したりする際に使用できるようになります。
    

```bash
rails db:seed
```

`**http://localhost:3000/posts`にアクセス**

```ruby
[{"id":1,"title":"Quae quia sit.","body":"Et iure iusto. Est harum expedita. Et a qui.\nNatus rerum odio. 
Voluptatem reiciendis saepe. Ut occaecati saepe.\nIste aut necessitatibus. Ut aperiam est. Expedita quis 
qui.","created_at":"2024-02-05T22:34:25.794Z","updated_at":"2024-02-05T22:34:25.794Z"},{"id":2,"title":"Est tempora 
exercitationem.","body":"Quasi voluptates sit. Velit porro explicabo. Eos qui nisi.\nQuaerat error ut. Doloremque 
at quas. Rerum deserunt qui.\nMinus dolore omnis. Consequatur nemo recusandae. Accusamus qui 
ducimus.","created_at":"2024-02-05T22:34:25.796Z","updated_at":"2024-02-05T22:34:25.796Z"},{"id":3,"title":"Autem 
in suscipit.","body":"Expedita quo sunt. Est dignissimos quos. Occaecati saepe ab.\nUt quaerat iste. Qui dolor 
minus. Repellendus eum corporis.\nEt sed autem. Itaque sunt molestiae. Ipsa eos 
sunt.","created_at":"2024-02-05T22:34:25.798Z","updated_at":"2024-02-05T22:34:25.798Z"},{"id":4,"title":"Aut 
tempora assumenda.","body":"Pariatur totam assumenda. Tempore in numquam. Consequuntur aut et.\nRepellendus eos 
quo. Expedita sit ab. Ut quo quasi.\nVelit unde ex. Rerum exercitationem dolorum. Nostrum rerum 
adipisci.","created_at":"2024-02-05T22:34:25.799Z","updated_at":"2024-02-05T22:34:25.799Z"},{"id":5,"title":"Ipsum 
ab quam.","body":"Nihil saepe sunt. Sunt incidunt omnis. Nobis voluptatum neque.\nDolorem minus est. Sapiente 
magnam iste. Molestias quo consequatur.\nQuisquam eaque nobis. Ex nihil sunt. Voluptas rerum 
ullam.","created_at":"2024-02-05T22:34:25.801Z","updated_at":"2024-02-05T22:34:25.801Z"},{"id":6,"title":"Et eaque 
rerum.","body":"Possimus beatae commodi. Ut ipsam qui. Quibusdam et omnis.\nUt praesentium est. Dolorem blanditiis 
omnis. Odio in aperiam.\nRerum voluptate itaque. Eum facere ipsam. Repellendus numquam 
eligendi.","created_at":"2024-02-05T22:34:25.802Z","updated_at":"2024-02-05T22:34:25.802Z"},{"id":7,"title":"Cum ab 
delectus.","body":"Reprehenderit ab dolores. Occaecati ex sequi. Non aliquid unde.\nVero sit sint. Et illum iusto. 
Necessitatibus doloribus laudantium.\nMagnam aut rerum. Voluptas dolor omnis. Neque placeat 
et.","created_at":"2024-02-05T22:34:25.803Z","updated_at":"2024-02-05T22:34:25.803Z"},{"id":8,"title":"Nesciunt qui 
minima.","body":"Nisi suscipit tempore. Non ad quo. Autem voluptas quidem.\nOmnis aut aliquam. Sint molestiae 
nostrum. Numquam aut culpa.\nNemo vitae amet. Beatae odio laudantium. Nulla dolorem 
illo.","created_at":"2024-02-05T22:34:25.804Z","updated_at":"2024-02-05T22:34:25.804Z"},{"id":9,"title":"Aut 
voluptas laboriosam.","body":"Quas accusamus aut. Itaque omnis nostrum. Enim perspiciatis aut.\nSaepe est 
laudantium. Inventore quae neque. Et laboriosam quod.\nVitae est dolores. Inventore molestiae dolore. Et fugit 
repellendus.","created_at":"2024-02-05T22:34:25.805Z","updated_at":"2024-02-05T22:34:25.805Z"},{"id":10,"title":"Officiis 
magnam ut.","body":"Hic molestiae delectus. Placeat saepe voluptatem. Libero dolores animi.\nHarum pariatur 
architecto. Odio sint doloribus. Expedita et labore.\nIusto eius quo. Magni omnis delectus. Assumenda modi 
qui.","created_at":"2024-02-05T22:34:25.807Z","updated_at":"2024-02-05T22:34:25.807Z"},{"id":11,"title":"Repudiandae 
aliquid dolorem.","body":"Est quae quia. Provident voluptatum optio. Magnam error nesciunt.\nQuibusdam et possimus. 
Rerum pariatur minus. Tempora fugit recusandae.\nEst dolorem non. Sequi saepe expedita. Facere maxime 
necessitatibus.","created_at":"2024-02-05T22:34:25.808Z","updated_at":"2024-02-05T22:34:25.808Z"},{"id":12,"title":"Omnis 
qui praesentium.","body":"Ex molestiae ut. Fuga vel sunt. Ab sit atque.\nQuia natus optio. Voluptates non fugiat. 
Fuga amet quis.\nMinima dolores maxime. Dolore ut quod. Consequatur culpa 
autem.","created_at":"2024-02-05T22:34:25.809Z","updated_at":"2024-02-05T22:34:25.809Z"},{"id":13,"title":"Repellendus 
vel voluptatem.","body":"Delectus est ut. Officia assumenda voluptatem. Tenetur fugiat incidunt.\nSunt ad voluptas. 
Et eos minima. Nobis voluptatem quos.\nOccaecati dolor quasi. Nisi non architecto. Officia nulla 
tempore.","created_at":"2024-02-05T22:34:25.810Z","updated_at":"2024-02-05T22:34:25.810Z"},{"id":14,"title":"Sed 
aut eveniet.","body":"Omnis culpa eos. Sequi nisi et. Est voluptatem fugiat.\nDelectus vero consequatur. Ab illum 
quia. A corporis suscipit.\nCorporis id aliquid. Omnis et nobis. Adipisci est 
facilis.","created_at":"2024-02-05T22:34:25.811Z","updated_at":"2024-02-05T22:34:25.811Z"},{"id":15,"title":"Nihil 
saepe libero.","body":"Temporibus pariatur nostrum. Consequatur nobis quia. Dicta sint magni.\nIpsam perferendis 
atque. Sint perspiciatis vel. Necessitatibus ut et.\nIpsa dolores voluptatem. Laborum veniam pariatur. Ut 
architecto 
alias.","created_at":"2024-02-05T22:34:25.817Z","updated_at":"2024-02-05T22:34:25.817Z"},{"id":16,"title":"Adipisci 
a provident.","body":"Sequi maxime ut. Molestiae vero veniam. Voluptas itaque voluptatibus.\nTotam veritatis non. 
Assumenda corrupti ullam. Doloremque rerum quod.\nFuga non non. Ex et nostrum. Nihil vel 
fuga.","created_at":"2024-02-05T22:34:25.819Z","updated_at":"2024-02-05T22:34:25.819Z"},{"id":17,"title":"Laudantium 
soluta aliquid.","body":"Numquam dicta eveniet. Aperiam voluptatem delectus. Vel est harum.\nVel quidem voluptatum. 
Et maxime voluptate. Molestias eos cupiditate.\nNam voluptas accusamus. Officia iure fugit. Molestiae totam 
quis.","created_at":"2024-02-05T22:34:25.820Z","updated_at":"2024-02-05T22:34:25.820Z"},{"id":18,"title":"Dignissimos 
porro sint.","body":"Odio incidunt explicabo. Quia quis sit. Non cumque excepturi.\nVoluptas ad cumque. Et quia 
error. Adipisci eaque magni.\nCulpa accusamus ut. Qui aliquid velit. Molestias temporibus 
voluptas.","created_at":"2024-02-05T22:34:25.821Z","updated_at":"2024-02-05T22:34:25.821Z"},{"id":19,"title":"Praesentium 
enim dolores.","body":"Voluptates aperiam aut. Enim ut quas. Sed enim labore.\nQuis nisi aspernatur. Porro 
praesentium omnis. Laudantium consequatur enim.\nConsequatur ea libero. Voluptas iste ut. Natus amet 
et.","created_at":"2024-02-05T22:34:25.822Z","updated_at":"2024-02-05T22:34:25.822Z"},{"id":20,"title":"Labore 
sapiente ut.","body":"Illo quo corporis. Aut similique fuga. Reiciendis consequatur non.\nAb doloribus fuga. Vero 
est non. Minima tempora delectus.\nConsequuntur dolorum amet. Consequatur animi accusantium. Enim harum 
laboriosam.","created_at":"2024-02-05T22:34:25.823Z","updated_at":"2024-02-05T22:34:25.823Z"}]
```

配列[]にデータが入っていれば成功。

**APIエンドポイントを指定**

**`routes.rb`にAPI用ルートを追加**

```ruby
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :posts
    end
  end
end
```

- 解説
    
    `/api/v1/posts`エンドポイントに対するAPIエンドポイントを設定します。
    
    `namespace :api`: API関連のエンドポイントが`/api`の下にまとめられます。
    
    `namespace :v1`: 
APIのバージョン1（`v1`）のエンドポイントが`/api/v1`の下にまとめられます。バージョン管理を行う際に、新しいバージョンを追加することもできます（例：`namespace 
:v2`）。
    
    - 解説
        
        `namespace :api`内に更に`namespace 
:v1`を追加しているコードは、バージョン管理を考慮したAPIエンドポイントの設計を行っています。これにより、将来的なAPIの変更やアップデートが容易になります。
        
        具体的な違いと利点は以下の通りです：
        
        1. **バージョン管理:**
            - `namespace 
:api`のみを使う場合は、APIエンドポイントが単一の名前空間になります。しかし、将来的な変更やアップデートがある場合、新しいバージョンを追加するのが難しくなります。
            - `namespace :api`内に更に`namespace 
:v1`を追加すると、複数のバージョンを管理できます。例えば、`/api/v1/posts`といった形でバージョン1のAPIエンドポイントが定義されます。
        2. **変更の影響を抑える:**
            - 
バージョン管理を行うことで、APIの変更が既存のクライアントに与える影響を抑えることができます。新しいバージョンが追加された場合、既存のクライアントは引き続き古いバージョンを使用できます。
        3. **将来の拡張:**
            - `namespace 
:v1`のようなバージョンを指定することで、将来的なバージョンの追加や変更に対応しやすくなります。バージョンを切り替えることで、新しい機能や修正を追加しやすくなります。
        
        
総じて、APIエンドポイントにバージョン管理を追加することで、柔軟性が向上し、将来の変更に対する拡張性が高まります。したがって、バージョン管理を考慮することは一般的に良い実践とされています。
        
    
    こちらの設定によって、次のようなAPIエンドポイントが作成されます：
    
    - `GET /api/v1/posts`: すべての投稿を取得するためのエンドポイント。
    - `GET /api/v1/posts/:id`: 特定の投稿を取得するためのエンドポイント。
    - `POST /api/v1/posts`: 新しい投稿を作成するためのエンドポイント。
    - `PUT /api/v1/posts/:id`: 特定の投稿を更新するためのエンドポイント。
    - `DELETE /api/v1/posts/:id`: 特定の投稿を削除するためのエンドポイント。
    

**コントローラーにネームスペースを設定**

ルーティングとコントローラーのネームスペースを一致させることが推奨されるのでコントローラーもネームスペースを設定します。

1.　`controllers/`のディレクトリ内に、`api/`フォルダを作成し、更にその中に`v1/`フォルダーを作成します。

`posts_controller.rb`を`controllers/api/v1/`内に移動します。

2.　Postsコントローラーにネームスペースを追加します。

```ruby
# app/controllers/api/v1/posts_controller.rb

class Api::V1::PostsController < ApplicationController
...
end`
```

!https://static.zenn.studio/images/copy-icon.svg

!https://static.zenn.studio/images/wrap-icon.svg

`http://localhost:3000/api/v1/posts`にアクセスし、投稿がJSON形式で表示されていることを確認します。

```ruby
# http://localhost:3000/api/v1/posts

[{"id":1,"title":"Quae quia sit.","body":"Et iure iusto. Est harum expedita. Et a qui.\nNatus rerum odio. 
Voluptatem reiciendis saepe. Ut occaecati saepe.\nIste aut necessitatibus. Ut aperiam est. Expedita quis 
qui.","created_at":"2024-02-05T22:34:25.794Z","updated_at":"2024-02-05T22:34:25.794Z"},{"id":2,"title":"Est tempora 
exercitationem.","body":"Quasi voluptates sit. Velit porro explicabo. Eos qui nisi.\nQuaerat error ut. Doloremque 
at quas. Rerum deserunt qui.\nMinus dolore omnis. Consequatur nemo recusandae. Accusamus qui 
ducimus.","created_at":"2024-02-05T22:34:25.796Z","updated_at":"2024-02-05T22:34:25.796Z"},{"id":3,"title":"Autem 
in suscipit.","body":"Expedita quo sunt. Est dignissimos quos. Occaecati saepe ab.\nUt quaerat iste. Qui dolor 
minus. Repellendus eum corporis.\nEt sed autem. Itaque sunt molestiae. Ipsa eos 
sunt.","created_at":"2024-02-05T22:34:25.798Z","updated_at":"2024-02-05T22:34:25.798Z"},{"id":4,"title":"Aut 
tempora assumenda.","body":"Pariatur totam assumenda. Tempore in numquam. Consequuntur aut et.\nRepellendus eos 
quo. Expedita sit ab. Ut quo quasi.\nVelit unde ex. Rerum exercitationem dolorum. Nostrum rerum 
adipisci.","created_at":"2024-02-05T22:34:25.799Z","updated_at":"2024-02-05T22:34:25.799Z"},{"id":5,"title":"Ipsum 
ab quam.","body":"Nihil saepe sunt. Sunt incidunt omnis. Nobis voluptatum neque.\nDolorem minus est. Sapiente 
magnam iste. Molestias quo consequatur.\nQuisquam eaque nobis. Ex nihil sunt. Voluptas rerum 
ullam.","created_at":"2024-02-05T22:34:25.801Z","updated_at":"2024-02-05T22:34:25.801Z"},{"id":6,"title":"Et eaque 
rerum.","body":"Possimus beatae commodi. Ut ipsam qui. Quibusdam et omnis.\nUt praesentium est. Dolorem blanditiis 
omnis. Odio in aperiam.\nRerum voluptate itaque. Eum facere ipsam. Repellendus numquam 
eligendi.","created_at":"2024-02-05T22:34:25.802Z","updated_at":"2024-02-05T22:34:25.802Z"},{"id":7,"title":"Cum ab 
delectus.","body":"Reprehenderit ab dolores. Occaecati ex sequi. Non aliquid unde.\nVero sit sint. Et illum iusto. 
Necessitatibus doloribus laudantium.\nMagnam aut rerum. Voluptas dolor omnis. Neque placeat 
et.","created_at":"2024-02-05T22:34:25.803Z","updated_at":"2024-02-05T22:34:25.803Z"},{"id":8,"title":"Nesciunt qui 
minima.","body":"Nisi suscipit tempore. Non ad quo. Autem voluptas quidem.\nOmnis aut aliquam. Sint molestiae 
nostrum. Numquam aut culpa.\nNemo vitae amet. Beatae odio laudantium. Nulla dolorem 
illo.","created_at":"2024-02-05T22:34:25.804Z","updated_at":"2024-02-05T22:34:25.804Z"},{"id":9,"title":"Aut 
voluptas laboriosam.","body":"Quas accusamus aut. Itaque omnis nostrum. Enim perspiciatis aut.\nSaepe est 
laudantium. Inventore quae neque. Et laboriosam quod.\nVitae est dolores. Inventore molestiae dolore. Et fugit 
repellendus.","created_at":"2024-02-05T22:34:25.805Z","updated_at":"2024-02-05T22:34:25.805Z"},{"id":10,"title":"Officiis 
magnam ut.","body":"Hic molestiae delectus. Placeat saepe voluptatem. Libero dolores animi.\nHarum pariatur 
architecto. Odio sint doloribus. Expedita et labore.\nIusto eius quo. Magni omnis delectus. Assumenda modi 
qui.","created_at":"2024-02-05T22:34:25.807Z","updated_at":"2024-02-05T22:34:25.807Z"},{"id":11,"title":"Repudiandae 
aliquid dolorem.","body":"Est quae quia. Provident voluptatum optio. Magnam error nesciunt.\nQuibusdam et possimus. 
Rerum pariatur minus. Tempora fugit recusandae.\nEst dolorem non. Sequi saepe expedita. Facere maxime 
necessitatibus.","created_at":"2024-02-05T22:34:25.808Z","updated_at":"2024-02-05T22:34:25.808Z"},{"id":12,"title":"Omnis 
qui praesentium.","body":"Ex molestiae ut. Fuga vel sunt. Ab sit atque.\nQuia natus optio. Voluptates non fugiat. 
Fuga amet quis.\nMinima dolores maxime. Dolore ut quod. Consequatur culpa 
autem.","created_at":"2024-02-05T22:34:25.809Z","updated_at":"2024-02-05T22:34:25.809Z"},{"id":13,"title":"Repellendus 
vel voluptatem.","body":"Delectus est ut. Officia assumenda voluptatem. Tenetur fugiat incidunt.\nSunt ad voluptas. 
Et eos minima. Nobis voluptatem quos.\nOccaecati dolor quasi. Nisi non architecto. Officia nulla 
tempore.","created_at":"2024-02-05T22:34:25.810Z","updated_at":"2024-02-05T22:34:25.810Z"},{"id":14,"title":"Sed 
aut eveniet.","body":"Omnis culpa eos. Sequi nisi et. Est voluptatem fugiat.\nDelectus vero consequatur. Ab illum 
quia. A corporis suscipit.\nCorporis id aliquid. Omnis et nobis. Adipisci est 
facilis.","created_at":"2024-02-05T22:34:25.811Z","updated_at":"2024-02-05T22:34:25.811Z"},{"id":15,"title":"Nihil 
saepe libero.","body":"Temporibus pariatur nostrum. Consequatur nobis quia. Dicta sint magni.\nIpsam perferendis 
atque. Sint perspiciatis vel. Necessitatibus ut et.\nIpsa dolores voluptatem. Laborum veniam pariatur. Ut 
architecto 
alias.","created_at":"2024-02-05T22:34:25.817Z","updated_at":"2024-02-05T22:34:25.817Z"},{"id":16,"title":"Adipisci 
a provident.","body":"Sequi maxime ut. Molestiae vero veniam. Voluptas itaque voluptatibus.\nTotam veritatis non. 
Assumenda corrupti ullam. Doloremque rerum quod.\nFuga non non. Ex et nostrum. Nihil vel 
fuga.","created_at":"2024-02-05T22:34:25.819Z","updated_at":"2024-02-05T22:34:25.819Z"},{"id":17,"title":"Laudantium 
soluta aliquid.","body":"Numquam dicta eveniet. Aperiam voluptatem delectus. Vel est harum.\nVel quidem voluptatum. 
Et maxime voluptate. Molestias eos cupiditate.\nNam voluptas accusamus. Officia iure fugit. Molestiae totam 
quis.","created_at":"2024-02-05T22:34:25.820Z","updated_at":"2024-02-05T22:34:25.820Z"},{"id":18,"title":"Dignissimos 
porro sint.","body":"Odio incidunt explicabo. Quia quis sit. Non cumque excepturi.\nVoluptas ad cumque. Et quia 
error. Adipisci eaque magni.\nCulpa accusamus ut. Qui aliquid velit. Molestias temporibus 
voluptas.","created_at":"2024-02-05T22:34:25.821Z","updated_at":"2024-02-05T22:34:25.821Z"},{"id":19,"title":"Praesentium 
enim dolores.","body":"Voluptates aperiam aut. Enim ut quas. Sed enim labore.\nQuis nisi aspernatur. Porro 
praesentium omnis. Laudantium consequatur enim.\nConsequatur ea libero. Voluptas iste ut. Natus amet 
et.","created_at":"2024-02-05T22:34:25.822Z","updated_at":"2024-02-05T22:34:25.822Z"},{"id":20,"title":"Labore 
sapiente ut.","body":"Illo quo corporis. Aut similique fuga. Reiciendis consequatur non.\nAb doloribus fuga. Vero 
est non. Minima tempora delectus.\nConsequuntur dolorum amet. Consequatur animi accusantium. Enim harum 
laboriosam.","created_at":"2024-02-05T22:34:25.823Z","updated_at":"2024-02-05T22:34:25.823Z"}]
```

成功です。

**Reactアプリケーションの作成**

```bash
npx create-react-app react_app
```

現在のディレクトリにreact_appディレクトリが作成される。

```ruby
my-project/
|-- rails-api/
|   |-- app/
|   |-- config/
|   |-- ...
|-- react-app/
|   |-- src/
|   |-- public/
|   |-- ...
```

階層構造はこんな感じがわかりやすくて良いかも。

**Reactアプリケーション内のHTTPクライアントの導入**

Reactアプリケーション内でHTTPクライアントを使用してRails APIと通信するため、**`axios`**を導入します。

```bash
npm install axios
```

- なぜaxiosを使うのか
    
    もちろんです！ReactとRails APIを繋げるために`axios`を使う理由を、中学生にもわかりやすく説明します。
    
    **1. ウェブブラウザとサーバーのやり取り:**
    
    - イメージとして、ウェブブラウザ（例: Google 
Chrome）があなたのコンピューターで動いていて、ウェブサイトを見ているとします。このウェブサイトは、遠くのサーバーにあるコンピューターから情報を取得しています。この情報のやり取りを「HTTP通信」と呼びます。
    
    **2. HTTP通信の手段:**
    
    - `axios`は、このHTTP通信を簡単に行うための手段です。ReactアプリケーションからRails 
APIに「お願い事（リクエスト）」をして、その結果として「お返し（レスポンス）」をもらうことができます。
    
    **3. データのやり取り:**
    
    - 
例えば、ウェブサイトで新しいメッセージを表示したり、お気に入りの動画を取得したりするとき、その情報はサーバーから取得されます。`axios`は、このデータを取得する手助けをしてくれます。
    
    **4. サーバーにお願いする方法:**
    
    - 
Reactアプリケーションが「サーバーさん、新しいメッセージくれる？」と頼む時、`axios`はその頼み事を効率的に伝えて、サーバーからの返事を待ってくれます。
    
    **5. 使いやすさと可読性:**
    
    - 
`axios`を使うことで、コードがきれいになります。これにより、Reactアプリケーションの中でサーバーとのやり取りがどのように行われているか、中学生でもわかりやすくなります。
    
    **簡単に言えば:**
    
    - 
`axios`は、Reactアプリケーションがサーバーとコミュニケーションをとるための道具であり、お願いや返事のやりとりを手助けしてくれる友達のような存在です。それによって、ウェブサイトがスムーズに動作し、楽しく使えるようになるんだよ！

**`src/App.js` にaxiosの設定を追加**

```bash
import logo from './logo.svg';
import './App.css';
// 追加
import axios from 'axios';

// 追加
// axiosの設定
axios.defaults.baseURL = 'http://localhost:3000/api/v1';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
```

**連携を確かめるためにお試しのコードを書く**

**Reactコンポーネントの作成**

Reactアプリケーション内でRails 
APIからデータを取得して表示するための新しいコンポーネントを作成します。例として、**`ApiDataDisplay.js`**という名前のファイルを作成します。

```ruby
// src/components/ApiDataDisplay.js
import React, { useState, useEffect } from 'react';
import axios from 'axios';

const ApiDataDisplay = () => {
  const [apiData, setApiData] = useState([]);

  useEffect(() => {
    // Rails APIからデータを取得
    axios.get('posts')  // Rails APIのエンドポイントに合わせて変更
      .then(response => {
        // 取得したデータをセット
        setApiData(response.data);
      })
      .catch(error => {
        console.error('Error fetching data from Rails API:', error);
      });
  }, []); // コンポーネントがマウントされたときに1回だけ実行

  return (
    <div>
      <h2>Data from Rails API:</h2>
      <ul>
        {apiData.map(item => (
          <li key={item.id}>
           <strong>{item.title}</strong>
           <p>{item.body}</p>
          </li>
```

- axios.get(’post’)の解説
    
    ```ruby
    axios.defaults.baseURL = 'http://localhost:3000/api/v1';
    ```
    
    **`src/App.js` で基本のURLを設定しているため、省略可能。
    posts = http://localhost:3000/api/v1/posts**
    

**作成したコンポーネントを表示する**

作成した**`ApiDataDisplay`**コンポーネントを**`App.js`**で表示します。

```ruby
// src/App.js
import React from 'react';
import './App.css';
import ApiDataDisplay from './components/ApiDataDisplay';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        {/* 既存のコード */}
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>

        {/* 追加: Rails APIからデータを取得して表示するコンポーネント */}
        <ApiDataDisplay />
      </header>
    </div>
  );
}

export default App;
```

[**http://localhost:3000/api/v1/posts](http://localhost:3000/api/v1/posts)　にアクセス**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/d49fece9-3bba-41f7-951e-0ba790b59646/2a2d87fc-9ea6-4ea8-84ec-e0da83ed28a6/Untitled.png)

rails側で用意したデータが表示されれば成功。

[**http://localhost:3001/](http://localhost:3001/) にアクセス**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/d49fece9-3bba-41f7-951e-0ba790b59646/9a681d84-ef67-4e64-868b-5cae19379774/Untitled.png)

データが表示されていれば成功。

このデータはhttp://localhost:3000/api/v1/posts　にアクセスして取得されたもの。



