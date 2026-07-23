# 📚 Tutoriais e Comandos MySQL

Repositório dedicado ao armazenamento de guias, documentações e cheat-sheets de comandos SQL e administração de bancos de dados MySQL/MariaDB.

## 🎯 Objetivo
Registrar procedimentos práticos de configuração de ambiente local, gerenciamento de usuários, permissões e boas práticas de banco de dados.

## 📂 Conteúdo do Repositório

| Arquivo / Guia | Descrição |
| :--- | :--- |
| `configuracao-root-wamp.md` | Redefinição de senha e recuperação de privilégios do usuário `root` no MySQL 8.x (WampServer). |
| `gerenciamento-usuarios.md` | Comandos para criação de usuários, atribuição de privilégios com o Princípio do Menor Privilégio e remoção de acessos. |

---

## 🛠️ Tecnologias & Ferramentas
* **Engine:** MySQL 8.x / MariaDB
* **Ambiente Local:** WampServer / Command Line (CMD/Terminal)
* **Linguagem:** SQL (DCL, DDL, DML)

---
*Mantido como material de apoio e estudo contínuo.*




# 📖 Tutorial: Configuração e Redefinição de Senha do `root` no MySQL (WampServer)

### 🎯 Objetivo

Este guia orienta o processo de redefinição de senha e atualização dos privilégios do usuário administrador (`root`) do MySQL em um ambiente local WampServer (versão MySQL 8.x).

---

### 📋 Pré-requisitos

* WampServer instalado e com o serviço do MySQL iniciado.
* **Prompt de Comando (CMD)** do Windows aberto na pasta `bin` do seu MySQL.
* *Exemplo de caminho:* `C:\wamp64\bin\mysql\mysql8.x.x\bin\`



---

## 🛠️ Passo a Passo

### 1. Autenticar no Cliente do MySQL

No Prompt de Comando, inicie a sessão interativa com o usuário `root`.

```cmd
mysql.exe -u root -p

```

> **Explicação:**
> * `mysql.exe`: Executável do cliente de linha de comando do MySQL.
> * `-u root`: Especifica o usuário de acesso (`root`).
> * `-p`: Solicita a senha do usuário. *(Caso o root esteja sem senha no momento, basta pressionar **Enter**).*
> 
> 

---

### 2. Alterar a Senha do Usuário `root`

Assim que o terminal exibir o prompt `mysql>`, execute a instrução de alteração de credencial usando a sintaxe moderna do MySQL 8.x:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '123@Mudar';

```

> **Explicação:**
> * `ALTER USER`: Comando DCL para alterar definições de uma conta existente.
> * `'root'@'localhost'`: Identifica a conta e o host de conexão.
> * `IDENTIFIED BY '123@Mudar'`: Aplica o novo hash de senha de forma segura ao usuário.
> 
> 

---

### 3. Garantir Privilégios Totais ao `root` *(Caso necessário)*

Para garantir que a conta `root` mantenha controle irrestrito sobre o servidor e permissão de gerenciar outros usuários:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;

```

> **Explicação:**
> * `ALL PRIVILEGES`: Concede todos os tipos de acesso e operações no servidor.
> * `ON *.*`: Aplica as permissões globalmente (em **todos** os bancos e **todas** as tabelas).
> * `WITH GRANT OPTION`: Permite que o `root` gerencie os privilégios de outros usuários.
> 
> 

---

### 4. Recarregar as Tabelas de Privilégios

Para que o MySQL aplique imediatamente as alterações de permissões na memória sem precisar reiniciar o serviço:

```sql
FLUSH PRIVILEGES;

```

> **Explicação:**
> * `FLUSH PRIVILEGES`: Força o servidor a recarregar as tabelas de acesso (`mysql.user`, etc.) a partir do disco para a memória RAM.
> 
> 

---

### 5. Sair do MySQL

Para encerrar a sessão no terminal do MySQL e retornar ao Prompt de Comando do Windows:

```sql
EXIT;

```




# Criando Banco de Dados e Usuário Dedicado no MySQL

Este tutorial prático mostra como configurar o ambiente do **MySQL 8.4** via linha de comando no Windows, criando um banco de dados isolado e um usuário com permissões específicas para desenvolvimento. 🛠️

---

## 🚀 Passo a Passo de Configuração

### 1️⃣ Acessar o MySQL como Administrador (`root`)

Abra o **Prompt de Comando (CMD)** do Windows, navegue até a pasta de executáveis do seu servidor MySQL e conecte-se como `root`:

```cmd
c:\wamp64\bin\mysql\mysql8.4.7\bin>mysql.exe -u root -p

```

O terminal solicitará a senha. Digite a credencial do administrador e pressione **Enter**:

```cmd
Enter password: *********

```

---

### 2️⃣ Criar a Base de Dados

No terminal do MySQL (`mysql>`), execute o comando para criar o banco de dados do sistema:

```sql
CREATE DATABASE projeto;

```

---

### 3️⃣ Criar o Usuário Dedicado

Crie a conta de acesso `projeto` vinculada ao ambiente local (`localhost`) com a credencial definida:

```sql
CREATE USER 'projeto'@'localhost' IDENTIFIED BY '123@mudar';

```

> ⚠️ **Nota Didática:** A senha `123@mudar` é utilizada exclusivamente para testes e aprendizado em ambiente local de desenvolvimento. Em servidores de produção, utilize senhas fortes e complexas.

---

### 4️⃣ Atribuir Privilégios ao Usuário

Conceda acesso total e direitos de gerenciamento exclusivamente sobre o banco `projeto`:

```sql
GRANT ALL PRIVILEGES ON projeto.* TO 'projeto'@'localhost' WITH GRANT OPTION;

```

---

### 5️⃣ Recarregar as Permissões

Atualize o cache de privilégios em memória para aplicar as regras imediatamente no servidor:

```sql
FLUSH PRIVILEGES;

```

---

### 6️⃣ Encerrar a Sessão de Administrador

Desconecte a conta `root` para testar o acesso restrito:

```sql
EXIT;

```

---

### 7️⃣ Testar a Conexão com o Novo Usuário

Conecte-se ao terminal utilizando o usuário recém-criado:

```cmd
c:\wamp64\bin\mysql\mysql8.4.7\bin>mysql.exe -u projeto -p

```

Quando solicitado, informe a senha `123@mudar`.

---

### 8️⃣ Validar o Isolamento de Acesso

Execute a listagem para confirmar o princípio do menor privilégio (o usuário deve enxergar apenas o banco `projeto` e as tabelas padrão do sistema):

```sql
SHOW DATABASES;

```

---

### 9️⃣ Selecionar o Banco para Operação

Ative a base de dados para iniciar a criação das tabelas e manipulação de dados:

```sql
USE projeto;

```
---



# Criando Conexão Segura com Banco de Dados no PHP (PDO)

Neste tutorial prático, você aprenderá a construir o arquivo `conexao.php` utilizando **PDO** (*PHP Data Objects*).

Cobriremos boas práticas de segurança, configuração do tratamento de erros, suporte completo a caracteres e os principais cuidados de sintaxe para evitar falhas de execução no PHP. 🛠️

---

## 🛠️ O Código Completo (`conexao.php`)

```php
<?php

$servidor = "localhost";
$banco    = "projeto";
$usuario  = "projeto";
$senha    = "123@mudar";

try {
    $pdo = new PDO("mysql:host=$servidor;dbname=$banco;charset=utf8mb4", $usuario, $senha);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("Erro de conexão: " . $e->getMessage());
}

```

> ⚠️ **Nota Didática:** A senha `123@mudar` e os dados de acesso são utilizados neste exemplo apenas para fins didáticos e de desenvolvimento local. Em ambientes de produção, utilize sempre credenciais fortes e armazenadas com segurança.

---

## 📖 Passo a Passo e Explicação Detalhada

### 1️⃣ Parâmetros de Acesso

Definimos as variáveis com as credenciais da base de dados MySQL:

* `$servidor`: Endereço do servidor MySQL (ex: `localhost`).
* `$banco`: Nome exato do banco de dados ao qual queremos conectar.
* `$usuario` e `$senha`: Credenciais de acesso configuradas no servidor de banco de dados.

---

### 2️⃣ O Bloco `try` e a Instância da Conexão

Dentro do bloco `try`, tentamos estabelecer a comunicação com a base de dados:

```php
$pdo = new PDO("mysql:host=$servidor;dbname=$banco;charset=utf8mb4", $usuario, $senha);

```

> 💡 **Pontos de Atenção no DSN:**
> * **Atribuição do Banco:** A string de conexão (*Data Source Name*) precisa receber a variável `$banco` no parâmetro `dbname=$banco`.
> * **Suporte UTF-8 (`charset=utf8mb4`):** Define a codificação para 4 bytes, permitindo o armazenamento correto de acentuação, caracteres especiais e emojis sem corromper os dados no MySQL.
> 
> 

---

### 3️⃣ Configuração do Modo de Erros

```php
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

```

Por padrão, o PDO pode falhar silenciosamente. Essa instrução altera o comportamento para que qualquer falha (senha incorreta, banco inexistente ou consulta SQL inválida) interrompa o fluxo no `try` e lance uma exceção capturada imediatamente pelo `catch`.

* `PDO::ATTR_ERRMODE`: A regra de configuração a ser alterada (Modo de Erro).
* `PDO::ERRMODE_EXCEPTION`: O comportamento desejado (Disparar Exceção/Exceção PDO).

---

## ⚠️ Dúvidas Frequentes e Armadilhas de Sintaxe

Pequenos detalhes de sintaxe podem interromper a execução do script. Veja os pontos críticos para evitar erros comuns:

### 1️⃣ Como Funciona o `catch (PDOException $e)`?

O bloco `catch` atua como uma barreira de segurança para tratamento de falhas:

* `PDOException`: Define estritamente que o bloco tratará exceções disparadas pelo PDO/MySQL.
* `$e`: Variável que armazena o objeto da exceção contendo as informações detalhadas sobre a falha ocorrida.

---

### 2️⃣ Concatenação de Strings com Ponto (`.`)

Para unir uma string estática a uma variável ou retorno de método no PHP, utiliza-se o operador de ponto (`.`):

```php
// ❌ INCORRETO (Sintaxe inválida):
die("Erro de conexão" $e->getMessage());
die("Erro de conexão" . $e getMessage());

// ✅ CORRETO (Ponto separando o texto da chamada do método):
die("Erro de conexão: " . $e->getMessage());

```

---

### 3️⃣ Acesso a Métodos de Objetos (`->`)

Diferente de linguagens que utilizam o ponto (`.`) para invocar métodos de objetos, o PHP utiliza o operador de seta (`->`):

```php
// ❌ INCORRETO:
$e.getMessage();
$e getMessage();

// ✅ CORRETO (Invoca o método getMessage() do objeto $e):
$e->getMessage();

```

---
############################
## 🚀 Resumo Prático de Verificação

Antes de testar a aplicação, confirme o checklist de validação:

* [ ] As variáveis `$servidor`, `$banco`, `$usuario` e `$senha` estão declaradas corretamente.
* [ ] A DSN utiliza a sintaxe `dbname=$banco;charset=utf8mb4`.
* [ ] A linha `$pdo->setAttribute(...)` está presente após a instância.
* [ ] O bloco de captura utiliza a classe `PDOException $e`.
* [ ] O encerramento de exceção utiliza a concatenação correta: `die("Texto: " . $e->getMessage());`.

> 💡 **Nota de Boas Práticas:**
> A senha `'123@Mudar'` foi utilizada aqui para fins de ambiente de desenvolvimento local. Em ambientes de pr
