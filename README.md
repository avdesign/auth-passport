<p align="center"><img src="https://painel.avdesign.com.br/img/logo/login-title.png"></p>

# Laravel Passport / Vue.js
### Instalação do Laravel
````
$ mkdir auth-passport
$ cd auth-passport
$ composer create-project --prefer-dist laravel/laravel back-end
$ cp .env.example .env
$ php artisan key:generate
````
### Istalação Laravel/Passport 
Tutorial: **[SPA Authentication Laravel Passport](https://laravel.com/docs/7.x/passport)**.
````
$ composer require laravel/passport
````
Adicione suas credenciais de banco de dados no arquivo `.env` e execute o camando: 
````
$ php artisan migrate
````
Em seguida, você deve executar o comando. Este comando criará as chaves de criptografia necessárias para gerar tokens de acesso seguro
````
php artisan passport:install
````
No model `User` adicione a trait `HasApiTokens`
```
use Laravel\Passport\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;
}
````
Essa trait fornecerá alguns métodos auxiliares ao seu modelo, que permitem inspecionar o token e os escopos do usuário autenticado.

Em `Providers/AuthServiceProvider` adicione `Passport::routes();` Este método registrará as rotas necessárias para emitir tokens de acesso e revogar tokens de acesso, clientes e tokens de acesso pessoais
````
use Laravel\Passport\Passport;

public function boot()
{
    $this->registerPolicies();
    Passport::routes();
}
````


Altere no  `config/auth.php` o driver da api para passport.

````
'api' => [
    'driver' => 'passport',
    _ _ _ _ _ _ _ _ _ _ _
````
### Personal Access Tokens
Coloque o ID do cliente e o `plain-text secret` no arquivo`.env` do seu aplicativo :
- **[OBS:](#)** Se você não executou o comando `php artisan passport:install`, você precisa executar este `php artisan passport:client --personal`

````
PASSPORT_PERSONAL_ACCESS_CLIENT_ID=client-id-value
PASSPORT_PERSONAL_ACCESS_CLIENT_SECRET=unhashed-client-secret-value
````
Em seguida, você deve registrar esses valores, colocando as seguintes chamadas dentro do  método `boot` em `Providers/AuthServiceProvider`

````
public function boot()
{
    $this->registerPolicies();

    Passport::routes();

    Passport::personalAccessClientId(
        config('passport.personal_access_client.id')
    );

    Passport::personalAccessClientSecret(
        config('passport.personal_access_client.secret')
    );
}

````

Se você está executando a API do Laravel em um caminho de URL diferente da `http://127.0.0.1:8000`, então você precisa atualizar o caminho do URL no `src/apis/Api.js` do Vue.js app.

# Vue.js Setup

```
cd front-end-vue
npm install
```

### Compilar e recarregar em desenvolvimento

```
npm run serve
```
### Compilar em produção
```
npm run build
```
### Verificar erros no código
```
npm run lint
```