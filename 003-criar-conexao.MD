Criando a Conexão Segura com Banco de Dados no PHP (PDO)
Neste tutorial, você aprenderá a construir do zero o arquivo conexao.php utilizando PDO (PHP Data Objects).

Trataremos de boas práticas de segurança, configuração do modo de erros e, principalmente, como evitar os erros de sintaxe mais comuns na concatenação e chamada de métodos no PHP.

🛠️ O Código Completo (conexao.php)
PHP
<?php

$servidor = "localhost";
$banco    = "projeto";
$usuario  = "projeto";
$senha    = "123@Mudar";

try {
    $pdo = new PDO("mysql:host=$servidor;dbname=$banco;charset=utf8mb4", $usuario, $senha);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die("Erro de conexão: " . $e->getMessage());
}
📖 Passo a Passo e Explicação Detalhada
1. Parâmetros de Acesso
Definimos as variáveis que contêm as credenciais do banco MySQL:

$servidor: Endereço do servidor de banco de dados (ex: localhost).

$banco: Nome exato do banco de dados ao qual queremos conectar.

$usuario e $senha: Credenciais de acesso configuradas no MySQL.

2. O Bloco try e a Conexão
Dentro do bloco try, tentamos estabelecer a conexão:

PHP
$pdo = new PDO("mysql:host=$servidor;dbname=$banco;charset=utf8mb4", $usuario, $senha);
Atenção no DSN: A string de conexão (mysql:host=...) precisa receber a variável do banco (dbname=$banco). Cuidado para não confundir o nome da variável $banco com o valor direto contido nela.

3. Configurando o Modo de Erros
PHP
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
O que faz: Por padrão, o PDO pode falhar silenciosamente. Essa linha altera a configuração para que qualquer falha (senha incorreta, banco inexistente, SQL errado) interrompa a execução no try e envie um aviso diretamente para o catch.

PDO::ATTR_ERRMODE: A regra que queremos alterar (Error Mode).

PDO::ERRMODE_EXCEPTION: O comportamento desejado (Disparar Exceção).

⚠️ Dúvidas Frequentes e Armadilhas de Sintaxe
Durante a construção deste código, pequenos detalhes de sintaxe podem travar a aplicação. Veja o que observar para não errar:

1. Como funciona o catch (PDOException $e)?
O catch é o filtro de segurança. Ele captura exceções específicas:

PDOException: Especifica que este bloco só tratará erros vindos do banco de dados via PDO. Se o erro for de outra natureza no PHP, esse bloco o ignorará para que o filtro correto trate.

$e: É a variável que armazena o objeto contendo as informações detalhadas do erro.

2. A "Cola" da Concatenação: O Ponto (.)
Para juntar uma frase fixa com uma variável ou método no PHP, utilizamos o ponto (.):

PHP
// ❌ INCORRETO: Esquecer o ponto ou colocar no lugar errado
die("Erro de conexão" $e->getMessage());
die("Erro de conexão" . $e getMessage());

// ✅ CORRETO: Ponto separando o texto do objeto
die("Erro de conexão: " . $e->getMessage());
3. A Setinha (->) para Chamada de Métodos
Diferente de linguagens que usam ponto (.) para acessar propriedades e métodos de um objeto, o PHP utiliza a setinha (->):

PHP
// ❌ INCORRETO: Usar espaço sem a seta
$e getMessage()

// ✅ CORRETO: Chamar o método getMessage() a partir do objeto $e
$e->getMessage()
🚀 Resumo Prático de Verificação
Antes de rodar seu código, confira o checklist:

[ ] As variáveis de servidor, banco, usuário e senha estão declaradas.

[ ] O DSN utiliza a variável $banco em dbname=$banco.

[ ] A linha $pdo->setAttribute(...) está presente.

[ ] O bloco catch utiliza PDOException $e.

[ ] O encerramento do erro usa die("Texto" . $e->getMessage());.
