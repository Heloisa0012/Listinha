<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Futebol Feminina</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }

        header {
            text-align: center;
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin: 5px 0;
        }

        input[type="text"], input[type="email"], input[type="date"] {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
        }

        input[type="submit"] {
            padding: 10px 20px;
            background-color: #28a745;
            color: white;
            border: none;
            cursor: pointer;
        }

        input[type="submit"]:hover {
            background-color: #218838;
        }

        ul {
            list-style-type: none;
            padding: 0;
        }

        li {
            padding: 10px;
            margin: 10px 0;
            background-color: #f8f9fa;
            border: 1px solid #ddd;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        button {
            padding: 5px 10px;
            background-color: #dc3545;
            color: white;
            border: none;
            cursor: pointer;
        }

        button:hover {
            background-color: #c82333;
        }

        footer {
            margin-top: 20px;
            text-align: center;
        }
    </style>
</head>
<body>
    <header>
        <h1>Mulheres no Futebol</h1>
    </header>

    <main>
        <form id="formCadastro">
            <div>
                <label>Nome:</label>
                <input type="text" id="nome" placeholder="Digite o nome" required>
                <label>Email:</label>
                <input type="email" id="email" placeholder="Digite o email" required>
                <label>Telefone:</label>
                <input type="text" id="telefone" placeholder="Digite o telefone" required>
                <label>Data de Nascimento:</label>
                <input type="date" id="dataNascimento" required>
                <input type="submit" value="Enviar">
            </div>
        </form>

        <h2>Pessoas Cadastradas</h2>
        <ul id="listaPessoas"></ul> <!-- Listagem de pessoas cadastradas -->
    </main>

    <footer>
        <p>2024 - Todos os Direitos Reservados ©</p>
        <p>Entre em contato</p>
        <p>Criado por M. Heloísa P. Sousa</p> <!-- Sua assinatura -->
    </footer>

    <script>
        // Função Factory para criar objetos de pessoa
        function Pessoa(nome, email, telefone, dataNascimento) {
            return {
                nome,
                email,
                telefone,
                dataNascimento
            };
        }

        // Função para salvar uma pessoa no localStorage
        function salvarPessoa(pessoa) {
            let pessoas = JSON.parse(localStorage.getItem('pessoas')) || [];
            pessoas.push(pessoa);
            localStorage.setItem('pessoas', JSON.stringify(pessoas));
            listarPessoas();
        }

        // Função para listar as pessoas salvas
        function listarPessoas() {
            const lista = document.getElementById('listaPessoas');
            lista.innerHTML = ''; // Limpa a lista para evitar duplicação
            let pessoas = JSON.parse(localStorage.getItem('pessoas')) || [];
            
            pessoas.forEach((pessoa, index) => {
                let li = document.createElement('li');
                li.textContent = `${pessoa.nome} - ${pessoa.email} - ${pessoa.telefone} - ${pessoa.dataNascimento}`;
                
                // Botão de excluir
                let btnExcluir = document.createElement('button');
                btnExcluir.textContent = 'Excluir';
                btnExcluir.onclick = () => excluirPessoa(index);
                li.appendChild(btnExcluir);
                
                lista.appendChild(li);
            });
        }

        // Função para excluir uma pessoa
        function excluirPessoa(index) {
            let pessoas = JSON.parse(localStorage.getItem('pessoas')) || [];
            pessoas.splice(index, 1);
            localStorage.setItem('pessoas', JSON.stringify(pessoas));
            listarPessoas();
        }

        // Ação de submit no formulário
        document.getElementById('formCadastro').addEventListener('submit', function (e) {
            e.preventDefault(); // Previne o reload da página

            // Coleta os valores do formulário
            const nome = document.getElementById('nome').value;
            const email = document.getElementById('email').value;
            const telefone = document.getElementById('telefone').value;
            const dataNascimento = document.getElementById('dataNascimento').value;

            // Cria uma nova pessoa usando a Factory
            const novaPessoa = Pessoa(nome, email, telefone, dataNascimento);

            // Salva a pessoa no localStorage
            salvarPessoa(novaPessoa);

            // Limpa o formulário
            document.getElementById('formCadastro').reset();
        });

        // Exibe a lista de pessoas ao carregar a página
        window.onload = listarPessoas;
    </script>
</body>
</html>
