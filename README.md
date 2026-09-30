<p align="center">
  <img src="TRENDY.png" alt="Logo do Trendy" width="260">
</p>

<h1 align="center">Trendy</h1>

<p align="center">
  Rede social de microblog inspirada no Twitter, desenvolvida como projeto final da disciplina WDI
  na Universidade Federal de Rondônia (UNIR).
</p>

<p align="center">
  <img src="https://img.shields.io/badge/PHP-8.2%2B-777BB4?logo=php&logoColor=white" alt="PHP 8.2+">
  <img src="https://img.shields.io/badge/MySQL-MariaDB-4479A1?logo=mysql&logoColor=white" alt="MySQL">
  <img src="https://img.shields.io/badge/Eloquent-ORM-FF2D20?logo=laravel&logoColor=white" alt="Eloquent ORM">
  <img src="https://img.shields.io/badge/HTML5-CSS3-E34F26?logo=html5&logoColor=white" alt="HTML e CSS">
</p>

> **Projeto encerrado.** Este é um trabalho acadêmico de 2024 que não recebe mais atualizações. O código fica
> disponível como registro e portfólio.

## Sobre o projeto

O Trendy é uma aplicação web em que usuários criam uma conta, publicam mensagens curtas de até 280 caracteres
(os "trends"), curtem publicações de outras pessoas e gerenciam o próprio perfil. O projeto usa PHP puro no
back end, com o Eloquent ORM (componente de banco de dados do Laravel) rodando de forma independente através do
`illuminate/database`, e MySQL como banco de dados.

## Funcionalidades

**Contas e autenticação**
- Cadastro de usuário com verificação de nome de usuário já existente
- Senhas armazenadas com hash bcrypt (`password_hash` e `password_verify`)
- Login e logout com controle de sessão em PHP
- Páginas protegidas: o feed e o perfil só abrem para usuários logados

**Feed**
- Publicação de mensagens com limite de 280 caracteres
- Linha do tempo ordenada da mais recente para a mais antiga
- Curtir e descurtir publicações, com contador de curtidas
- Edição das próprias publicações, que passam a exibir a marcação "(editado)"
- Exclusão das próprias publicações
- Suporte a emojis (conexão configurada em `utf8mb4`)

**Perfil**
- Alteração do nome de usuário, com checagem de disponibilidade
- Troca de senha com campo de confirmação

**Cargos**
- Cada usuário possui um cargo (`user` por padrão ou `admin`)
- Administradores podem excluir publicações de qualquer usuário
- Para administradores, a barra de navegação aparece em vermelho, indicando o modo de moderação

## Tecnologias

| Camada | Tecnologia |
| --- | --- |
| Back end | PHP 8.2 ou superior |
| Acesso a dados | Eloquent ORM (`illuminate/database` 11.x) via Capsule |
| Banco de dados | MySQL ou MariaDB |
| Front end | HTML5 e CSS3, sem frameworks |
| Dependências | Composer |

## Estrutura de arquivos

```
.
├── index.html          # Tela inicial de login
├── login.php           # Autenticação do usuário
├── register.html       # Formulário de cadastro
├── register.php        # Criação da conta
├── feed.php            # Linha do tempo: publicar, curtir, editar, excluir e sair
├── profile.php         # Edição de nome de usuário e senha
├── database.php        # Configuração da conexão com o banco (Capsule)
├── User.php            # Model de usuários
├── Tweet.php           # Model de publicações
├── like.php            # Model de curtidas
├── criatabelas.sql     # Script de criação das tabelas
├── style.css           # Estilos das telas de login, cadastro e perfil
├── TRENDY.png          # Logo do projeto
├── composer.json
└── composer.lock
```

## Banco de dados

O script `criatabelas.sql` cria três tabelas no banco `idw`:

- **users**: `id`, `username` (único), `password` (hash) e `cargo`
- **tweets**: `id`, `user_id`, `username`, `content`, `is_edited`, `created_at` e `updated_at`
- **likes**: relação entre `tweet_id` e `user_id`; as curtidas são apagadas junto com a publicação (`ON DELETE CASCADE`)

## Como executar

### Pré requisitos

- PHP 8.2 ou superior com a extensão `pdo_mysql`
- MySQL ou MariaDB
- Composer

Uma opção prática é usar o XAMPP ou o Laragon, que já trazem PHP e MySQL.

### Passo a passo

1. Clone o repositório:
   ```bash
   git clone https://github.com/Wyllgner/trendy.git
   cd trendy
   ```

2. Instale as dependências (a pasta `vendor` já está no repositório, mas este passo garante que ela esteja atualizada):
   ```bash
   composer install
   ```

3. Crie o banco de dados e as tabelas:
   ```bash
   mysql -u root -p -e "CREATE DATABASE idw CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
   mysql -u root -p idw < criatabelas.sql
   ```

4. Ajuste as credenciais em `database.php` caso o seu MySQL use outro usuário, senha ou porta:
   ```php
   'host'     => 'localhost:3306',
   'database' => 'idw',
   'username' => 'root',
   'password' => '',
   ```

5. Inicie o servidor embutido do PHP:
   ```bash
   php -S localhost:8000
   ```

6. Acesse `http://localhost:8000` no navegador, crie uma conta e comece a publicar.

### Tornando um usuário administrador

Não existe tela para promover usuários. Para isso, altere o cargo diretamente no banco:

```sql
UPDATE users SET cargo = 'admin' WHERE username = 'seu_usuario';
```

Depois, faça logout e login novamente para que a sessão carregue o novo cargo.

## Pontos de melhoria

Como o projeto está encerrado, estes ajustes conhecidos ficaram registrados, mas não serão feitos:

- A coluna `id` da tabela `likes` não tem `AUTO_INCREMENT` nem chave primária. Em servidores MySQL com modo
  estrito ativo, a curtida pode falhar; basta alterar a coluna para `AUTO_INCREMENT PRIMARY KEY`
- A exclusão de publicações só é restrita na interface; o servidor ainda não confere se quem pediu a exclusão é
  o autor ou um administrador
- Há um `var_dump($_SESSION)` esquecido no envio de novas publicações em `feed.php`
- Os formulários não usam token CSRF
- As credenciais do banco ficam fixas em `database.php`; o ideal seria lê-las de variáveis de ambiente
- A pasta `vendor` está versionada; poderia ser ignorada pelo Git e gerada com `composer install`

## Autores

Projeto desenvolvido por:

- **Samih Santos**
- **Wyllgner França**

Universidade Federal de Rondônia (UNIR), 2024.
