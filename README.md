<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cadastro de Equipamentos e Usuários</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f7f6;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .form-container {
            background-color: #fff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 500px;
        }
        h2 {
            margin-top: 0;
            color: #333;
            text-align: center;
            border-bottom: 2px solid #28a745;
            padding-bottom: 10px;
            margin-bottom: 20px;
        }
        .section-title {
            font-size: 14px;
            font-weight: bold;
            color: #28a745;
            margin-top: 20px;
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            color: #555;
            font-size: 14px;
        }
        input[type="text"], 
        input[type="email"], 
        input[type="password"], 
        input[type="tel"] {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 14px;
        }
        input:focus {
            border-color: #28a745;
            outline: none;
        }
        .btn-container {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
        button {
            flex: 1;
            padding: 12px;
            border: none;
            color: white;
            font-size: 15px;
            border-radius: 4px;
            cursor: pointer;
            transition: background 0.3s;
            font-weight: bold;
        }
        .btn-salvar {
            background-color: #28a745;
        }
        .btn-salvar:hover {
            background-color: #218838;
        }
        .btn-atualizar {
            background-color: #007bff;
        }
        .btn-atualizar:hover {
            background-color: #0069d9;
        }
        #mensagem {
            margin-top: 15px;
            text-align: center;
            font-weight: bold;
        }
    </style>
</head>
<body>

<div class="form-container">
    <h2>Cadastro de Inventário</h2>
    
    <form id="meuFormulario" action="https://script.google.com/macros/s/AKfycbw-mAEmAhKHT5A8QlWxpXgR9mHKc-PdCY-_rlN76_MiElDjEkEKu42W4ahIZHTux5GO0g/exec" method="POST">
        
        <input type="hidden" id="acao" name="acao" value="cadastrar">

        <div class="section-title">Dados do Usuário</div>
        <div class="form-group">
            <label for="email">E-mail (Identificador único):</label>
            <input type="email" id="email" name="email" required>
        </div>
        <div class="form-group">
            <label for="senha">Senha do Computador (ou Nova Senha):</label>
            <input type="password" id="senha" name="senha" required>
        </div>
        <div class="form-group">
            <label for="nome">Nome:</label>
            <input type="text" id="nome" name="nome">
        </div>
        <div class="form-group">
            <label for="fone">Telefone:</label>
            <input type="tel" id="fone" name="fone" placeholder="(00) 00000-0000">
        </div>

        <div class="section-title">Equipamentos (Apenas para novos cadastros)</div>
        <div class="form-group">
            <label for="computadorTAG">Computador TAG:</label>
            <input type="text" id="computadorTAG" name="computadorTAG">
        </div>
        <div class="form-group">
            <label for="computadorNAME">Computador SN:</label>
            <input type="text" id="computadorNAME" name="computadorNAME">
        </div>
        <div class="form-group">
            <label for="monitor">Monitor:</label>
            <input type="text" id="monitor" name="monitor">
        </div>
        <div class="form-group">
            <label for="cadeira">Cadeira:</label>
            <input type="text" id="cadeira" name="cadeira">
        </div>
        <div class="form-group">
            <label for="mesa">Mesa:</label>
            <input type="text" id="mesa" name="mesa">
        </div>

        <div class="btn-container">
            <button type="button" class="btn-salvar" onclick="enviarFormulario('cadastrar')">Salvar Novo</button>
            <button type="button" class="btn-atualizar" onclick="enviarFormulario('atualizar')">Atualizar Senha</button>
        </div>
    </form>
    <div id="mensagem"></div>
</div>

<script>
    const form = document.getElementById('meuFormulario');
    const mensagemDiv = document.getElementById('mensagem');
    const acaoInput = document.getElementById('acao');

    function enviarFormulario(tipoAcao) {
        // Define a ação no campo oculto
        acaoInput.value = tipoAcao;

        // Validação básica: para atualizar senha, e-mail e senha são obrigatórios
        const email = document.getElementById('email').value;
        const senha = document.getElementById('senha').value;
        
        if (!email || !senha) {
            mensagemDiv.style.color = 'red';
            mensagemDiv.innerText = 'Por favor, preencha E-mail e Senha.';
            return;
        }

        // Se for cadastro novo, valida o resto de forma simples (opcional)
        if (tipoAcao === 'cadastrar' && !document.getElementById('nome').value) {
            mensagemDiv.style.color = 'red';
            mensagemDiv.innerText = 'Preencha o campo Nome para um novo cadastro.';
            return;
        }

        mensagemDiv.style.color = 'black';
        mensagemDiv.innerText = tipoAcao === 'cadastrar' ? 'Cadastrando...' : 'Atualizando senha...';

        const formData = new FormData(form);

        fetch(form.action, {
            method: 'POST',
            body: formData
        })
        .then(response => response.text())
        .then(texto => {
            if (texto.includes("Sucesso") || texto.includes("atualizada")) {
                mensagemDiv.style.color = 'green';
                mensagemDiv.innerText = tipoAcao === 'cadastrar' ? 'Dados cadastrados com sucesso!' : 'Senha atualizada com sucesso!';
                form.reset();
            } else {
                mensagemDiv.style.color = 'red';
                mensagemDiv.innerText = texto; // Exibe o erro retornado pelo Google Script (ex: Usuário não encontrado)
            }
        })
        .catch(error => {
            mensagemDiv.style.color = 'red';
            mensagemDiv.innerText = 'Erro na comunicação com o servidor.';
            console.error('Erro:', error);
        });
    }
</script>

</body>
</html>
