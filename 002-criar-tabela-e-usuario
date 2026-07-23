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
