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

> 💡 **Nota de Boas Práticas:**
> A senha `'123@Mudar'` foi utilizada aqui para fins de ambiente de desenvolvimento local. Em ambientes de pr
