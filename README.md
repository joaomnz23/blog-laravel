# Blog com Autenticação em Laravel

Blog em Laravel 12 com autenticação de usuários, CRUD de posts e categorias e upload de imagens.

[![PHP](https://img.shields.io/badge/PHP-^8.2-777BB4?logo=php)](https://www.php.net/)
[![Laravel](https://img.shields.io/badge/Laravel-^12.0-FF2D20?logo=laravel)](https://laravel.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-^3.1.0-06B6D4?logo=tailwindcss)](https://tailwindcss.com/)
[![Alpine.js](https://img.shields.io/badge/Alpine.js-^3.4.2-8BC0D0?logo=alpinedotjs)](https://alpinejs.dev/)
[![Vite](https://img.shields.io/badge/Vite-^7.0.7-646CFF?logo=vite)](https://vite.dev/)

<!-- Adicione capturas de tela em docs/screenshots/ e descomente a linha abaixo. -->
<!-- ![Listagem de posts](docs/screenshots/posts.png) -->

## Sobre

O projeto é uma aplicação web de blog com área administrativa protegida por login. A autenticação usa o Laravel Breeze (cadastro, login, logout e edição de perfil), e somente usuários autenticados acessam o gerenciamento de posts e categorias. Cada post pertence a uma categoria e pode ter uma imagem de capa.

## Funcionalidades

- Cadastro, login e logout de usuários
- Edição de perfil e alteração de senha
- CRUD de categorias
- CRUD de posts com título, texto, categoria e imagem opcional
- Upload de imagem nos formatos JPEG, PNG ou GIF, com até 2 MB, substituída ou removida junto com o post
- Exclusão em cascata: ao excluir uma categoria, os posts vinculados a ela também são excluídos
- Interface responsiva com Tailwind CSS

## Tecnologias

| Camada | Tecnologia |
|---|---|
| Linguagem | PHP ^8.2 |
| Framework | Laravel ^12.0, Laravel Breeze ^2.4 |
| Interface | Blade, Tailwind CSS ^3.1.0, Alpine.js ^3.4.2 |
| Build de assets | Vite ^7.0.7 |
| Banco de dados | SQLite (padrão), MySQL ou PostgreSQL |
| Testes | PHPUnit ^11.5 |

## Modelo de dados

| Entidade | Campos principais | Relacionamentos |
|---|---|---|
| `users` | name, email, password | Usuários com acesso à área administrativa |
| `categorias` | id, name | Uma categoria possui vários posts |
| `posts` | id, title, text, image (opcional), categorias_id | Pertence a uma categoria (`categorias_id`, exclusão em cascata) |

## Como executar

### Pré-requisitos

- PHP 8.2 ou superior
- Composer 2
- Node.js 20.19 ou superior
- Git

### Instalação

```bash
git clone https://github.com/joaomnz23/auth-app.git
cd auth-app

composer install
npm install

cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan storage:link

npm run build
php artisan serve
```

Acesse `http://localhost:8000` no navegador.

No Windows, use `copy .env.example .env` no lugar de `cp`.

<details>
<summary>Passo a passo detalhado</summary>

1. **Arquivo de ambiente.** O `.env` guarda as configurações locais (banco de dados, chave da aplicação). Ele é criado a partir do `.env.example`.

2. **Chave da aplicação.** `php artisan key:generate` preenche a variável `APP_KEY`, usada para assinar sessões e cookies.

3. **Migrations.** `php artisan migrate` cria as tabelas. Com SQLite, se o Laravel perguntar se deseja criar o arquivo `database/database.sqlite`, confirme com `yes`.

4. **Link do storage.** `php artisan storage:link` expõe a pasta `storage/app/public` em `public/storage`, necessário para exibir as imagens enviadas nos posts.

5. **Assets.** `npm run build` gera a versão final dos estilos e scripts. Para desenvolvimento com recarga automática, use `npm run dev` em um terminal separado, mantendo o `php artisan serve` rodando em outro.

6. **Permissões (Linux e macOS).** Se houver erro de permissão em uploads ou cache, execute `chmod -R 775 storage bootstrap/cache`.

</details>

### Banco de dados

O projeto usa SQLite por padrão, e o arquivo é criado pelas migrations. Para usar MySQL, crie o banco e ajuste o `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=auth_app
DB_USERNAME=seu_usuario
DB_PASSWORD=sua_senha
```

A extensão `pdo_mysql` precisa estar habilitada no `php.ini`. Para PostgreSQL, use `DB_CONNECTION=pgsql`, porta `5432` e a extensão `pdo_pgsql`.

### Primeiro acesso

Acesse `/register` para criar um usuário. Depois do cadastro, o sistema redireciona para o painel. Crie ao menos uma categoria em **Categorias** antes de cadastrar o primeiro post, pois todo post exige uma categoria.

## Testes

```bash
php artisan test
```

A suíte inclui os testes de autenticação e de perfil que acompanham o Laravel Breeze.

## Licença

Distribuído sob a licença MIT. Consulte o arquivo `LICENSE`.

## Autor

[joaomnz23](https://github.com/joaomnz23)