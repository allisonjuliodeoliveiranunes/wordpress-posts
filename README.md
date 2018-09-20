# Aplicativo de posts Wordpress

Este projeto foi criado para o teste de Front End na iMedicina, ele faz a integração do [Angular v6]() com o [Wordpress](https://wordpress.org). 

#### Este app consegue fazer:
- Listagem de posts
- Editar um post
- Criar um post 
- Excluir um post

### Iniciando as configurações necessárias para rodar o App
#### Iniciando o Wordpress 
  * Este App foi testado com uma versão local instalada do Wordpress, que pode ser baixada [aqui](https://wordpress.org/download/).´

- Extraia o arquivo zipado para a raiz do seu servidor local apache, seja, [XAMPP](https://www.apachefriends.org/pt_br/download.html) ou [WAMPP](http://www.wampserver.com/en/#download-wrapper)

- Crie um banco para seu blog e faça as configurações normais para que eles esteja em funcionamento em (http://localhost/), documentação explicando como fazer esse [processo](http://www.adamsilva.com.br/programacao/como-instalar-o-wordpress-localhost/).
#### Após instalado, precisando adcionar dois plugins ao Wordpress.
 
- [WP REST API](https://br.wordpress.org/plugins/rest-api/) (Que irá dar acesso a Api do Wordpress, que nos retornará os post em formato .JSON)
- [JWT Authentication](https://wordpress.org/plugins/jwt-authentication-for-wp-rest-api/) (Vamos usá-la para autenticar nosso usuário e dar permissão para acessar os endpoints da Api.
  
 Agora precisamos fazer umasconfigurações nos arquivos do seu blog para continuar, primeiro habilique os permantelinks no painel de administração do seu Wordpress, após isso terá na raiz do seu blog um arquivo ``.htaccess``, que deverá ter esse mesmo escopo, copie o código abaixo e cole no seu arquivo, não se esqueça de alterar o domínio do seu site.
 
```sh
  # BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /substitua-pelo-seu-dominio-local/
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /substitua-pelo-seu-dominio-local/index.php [L]
RewriteCond %{HTTP:Authorization} ^(.*)
RewriteRule ^(.*) - [E=HTTP_AUTHORIZATION:%1]
SetEnvIf Authorization "(.*)" HTTP_AUTHORIZATION=$1
</IfModule>

# END WordPress
``` 
Após essa etapa, precisamos alaterar o arquivo ``wp-config.php``

```sh
define('DB_NAME', 'sua-base-de-dados');

define('DB_USER', 'root'); <-- Usuário do mySql

define('DB_PASSWORD', '');  <--- Sua senha

define('DB_HOST', 'localhost');
```
Logo abaixo você deverá adicionar essas linhas no php

```sh
define('JWT_AUTH_SECRET_KEY', 'senha-secreta-gerada-pelo-jwt-para-cada-site');
define('JWT_AUTH_CORS_ENABLE', true);
```
Para gerar 0 key de acesso, vá nesse [link](https://api.wordpress.org/secret-key/1.1/salt/) , e copie a string da segunda linha 
você verá algo assim:

define('AUTH_KEY',         't;TbCr+<0m5}x&<$]5<Ce/RCG3mg5{f.GvS @XV|nWCq=f?Bm@G6r4-N_JPCOz(x');
define('SECURE_AUTH_KEY',  '``gG2t{>j+bA|1+kn|>-`4h&f-I=,WuHaPs)](f@l?4`9kv+DP6q-{G.3dJ[A6*a]l``'); 
copie somente o conteúdo destacado como no <strong>EXEMPLO</strong> acima

