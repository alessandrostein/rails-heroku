## Criando e fazendo deploy de uma aplicação Ruby on Rails no Heroku.

Este manual foi escrito com base no seguinte ambiente:

- Ubuntu 14.04
- Ruby 2.1.2
- Rails 4.1.4
- PostgreSQL 9.1
- Git 1.9.1

Primeiramente cadastre-se no **Heroku** (https://id.heroku.com/signup) e faça o login na plataforma, após este passo baixe o **Heroku Toolbelt** e instale (https://devcenter.heroku.com/toolbelt-downloads/debian).

No terminal (**CRTL+ALT+T**) faça a autenticação no heroku.

```
$ heroku login
```

Será necessário informar o seu **email** e **senha** cadastrados previamente.

Para criar uma aplicação em Rails utilize o seguinte comando no terminal.

```
$ rails new rails-heroku --database=postgresql
```

Neste caso precisamos especificar qual base de dados será utilizado, o Rails possui a base de dados padrão **SQLite**, este **não aceito pelo Heroku** até o momento.

Acesse a pasta da sua aplicação.

```
$ cd rails-heroku
```

Utilize o **generate** do Rails para criar um **Controller**

```
$ rails generate controller welcome
```

Na pasta **rails-heroku/app/views/welcome/** adicione um arquivo com o seguinte nome **index.html.erb**
Abra este arquivo e adicione o código desejado.

```html
<h2>Hello Heroku</h2>
<p>
  The time is now: <%= Time.now %>
</p>
```

É necessário adicionarmos as **Rotas** da aplicação qual será a página inicial da nossa aplicação, no caso o arquivo recém criado. Abra o seguinte arquivo **rails-heroku/config/routes.rb** e adicione na segunda linha o seguinte código.

```ruby
root 'welcome#index'
```

Este arquivo ficará parecido com este código (os comentários foram removidos).

```ruby
Rails.application.routes.draw do
root 'welcome#index'
end
```

Para realizar a integração com o Heroku adicione as **gem rails_12factor, sqlite3 e pg, além da versão ruby utilizada nesta aplicação**, no arquivo **Gemfile**, o qual se encontra na pasta raiz da aplicação (**rails-heroku/Gemfile**).

```ruby
gem 'rails_12factor', group: :production
gem 'sqlite3'
gem 'pg'
ruby "2.1.2"
```
Instale todas as **Gem** da sua aplicação utilizando o terminal.

```
$ bundle install
```

Agora abra o arquivo **database.yml**, ele esta neste caminho **rails-heroku/config/**, e **remova o username** da sua configuração. Seu código deverá ficar parecido com este (os comentários foram removidos).

```ruby
default: &default
adapter: postgresql
encoding: unicode
pool: 5
production:
<default
database: rails-heroku_production
password: <%= ENV['RAILS-HEROKU_DATABASE_PASSWORD'] %&gt;
```

Crie uma **repositório** da sua aplicação.

```
$ git init
$ git add .
$ git commit -m "init"
```

Agora, para **criar uma aplicação no heroku**, digite no terminal o seguinte comando.

```
$ heroku create
```

E para fazer o **deploy da aplicação**, simplesmente faça o seguinte comando.

```
$ git push heroku master
```

Pronto, a sua aplicação deve estar rodando, confira o link de acesso no seu terminal.

```
http://safe-brook-6072.herokuapp.com/  deployed to Heroku
```
### Este manual foi baseado na documentação oficial do Heroku: https://devcenter.heroku.com/articles/getting-started-with-rails4