<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>📋 Gerador de Pedido - WhatsApp</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap');
        
        * {
            font-family: 'Poppins', sans-serif;
        }
        
        .gradient-bg {
            background: linear-gradient(135deg, #25D366 0%, #128C7E 100%);
        }
        
        .card-shadow {
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
        }
        
        .input-focus:focus {
            border-color: #25D366;
            box-shadow: 0 0 0 3px rgba(37, 211, 100, 0.1);
        }
        
        .btn-whatsapp {
            background: linear-gradient(135deg, #25D366 0%, #128C7E 100%);
            transition: all 0.3s ease;
        }
        
        .btn-whatsapp:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(37, 211, 100, 0.3);
        }

        .btn-clear {
            background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
        }

        .btn-fornecedor {
            background: linear-gradient(135deg, #075E54 0%, #128C7E 100%);
        }

        /* Estilo do menu suspenso */
        .marca-menu {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            background: white;
            border: 1px solid #e5e7eb;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
            z-index: 10;
            display: none;
            margin-top: 4px;
            max-height: 300px;
            overflow-y: auto;
        }

        .marca-menu.show {
            display: block;
        }

        .marca-option {
            padding: 8px 12px;
            cursor: pointer;
            transition: background 0.2s;
            border-bottom: 1px solid #f3f4f6;
            font-weight: 600;
            color: #374151;
        }

        .produto-option {
            padding: 6px 12px 6px 20px;
            cursor: pointer;
            transition: background 0.2s;
            font-size: 0.9em;
        }

        .produto-option:hover {
            background: #eff6ff;
            color: #1e40af;
        }

        .relative {
            position: relative;
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen py-8 px-4">
    
    <div class="max-w-2xl mx-auto">
        
        <!-- Header -->
        <div class="text-center mb-8">
            <h1 class="text-4xl font-bold text-gray-800 mb-2">
                <i class="fas fa-shopping-bag text-green-500"></i> Gerador de Pedido
            </h1>
            <p class="text-gray-600">Preencha os campos e envie direto para o WhatsApp!</p>
        </div>

        <!-- BOTÃO DIRETO FORNECEDOR FIXO -->
        <div class="text-center mb-6">
            <a 
                href="https://wa.me/5511910962883" 
                target="_blank"
                class="inline-flex items-center justify-center px-8 py-4 rounded-full text-white font-bold text-lg btn-fornecedor card-shadow hover:opacity-90 transition-all"
            >
                <i class="fab fa-whatsapp mr-3 text-2xl"></i>
                ENVIAR PEDIDO PARA O FORNECEDOR
                <i class="fas fa-external-link-alt ml-3"></i>
            </a>
            <p class="text-sm text-gray-500 mt-2">📞 Número: 11 91096-2883</p>
        </div>

        <!-- Card Principal -->
        <div class="bg-white rounded-2xl p-6 md:p-8 card-shadow mb-6">
            
            <!-- Campo para digitar outro número -->
            <div class="mb-6">
                <label class="block text-gray-700 font-semibold mb-2 flex items-center">
                    <i class="fas fa-mobile-alt text-blue-500 mr-2 text-xl"></i> Ou enviar para outro número
                </label>
                <input 
                    type="tel" 
                    id="numero" 
                    placeholder="Digite aqui: Ex 11999998888"
                    class="w-full px-4 py-3 rounded-xl border-2 border-gray-200 outline-none transition-all input-focus"
                >
            </div>

            <!-- Área dos Produtos -->
            <div class="bg-green-50 rounded-xl p-4 mb-6">
                <h3 class="font-semibold text-gray-700 mb-4 flex items-center">
                    <i class="fas fa-list-ul mr-2 text-green-600"></i> Produtos do Pedido
                </h3>

                <div id="itensContainer">
                    <div class="item bg-white rounded-lg p-4 mb-3 border border-green-100">
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                            <div>
                                <label class="text-xs text-gray-500 mb-1 block">PRODUTO</label>
                                <input type="text" class="produto w-full px-3 py-2 rounded-lg border border-gray-200 outline-none input-focus" placeholder="Nome do produto">
                            </div>
                            <div class="relative">
                                <label class="text-xs text-gray-500 mb-1 block">MARCA</label>
                                <div class="flex">
                                    <input type="text" class="marca-input w-full px-3 py-2 rounded-lg border border-gray-200 outline-none input-focus" placeholder="Selecione" readonly>
                                    <button onclick="toggleMenu(this)" class="ml-2 px-3 py-2 bg-gray-200 hover:bg-gray-300 rounded-lg transition-all">
                                        <i class="fas fa-chevron-down"></i>
                                    </button>
                                </div>
                                <div class="marca-menu">
                                    <!-- Conteúdo carregado via JS -->
                                </div>
                            </div>
                            <div>
                                <label class="text-xs text-gray-500 mb-1 block">QUANTIDADE</label>
                                <div class="flex">
                                    <input type="number" class="quantidade w-full px-3 py-2 rounded-lg border border-gray-200 outline-none input-focus" placeholder="Qtd" min="1">
                                    <button onclick="removerItem(this)" class="ml-2 px-3 bg-red-100 text-red-500 rounded-lg hover:bg-red-200 transition-all">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Botão Adicionar Mais -->
                <button 
                    onclick="adicionarItem()"
                    class="mt-3 w-full py-2 border-2 border-dashed border-green-400 text-green-600 rounded-lg font-semibold hover:bg-green-100 transition-all"
                >
                    <i class="fas fa-plus mr-2"></i> ADICIONAR MAIS PRODUTOS
                </button>
            </div>

            <!-- Botões de Ação -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <button 
                    onclick="limparTudo()"
                    class="btn-clear text-white py-3 rounded-xl font-semibold flex items-center justify-center"
                >
                    <i class="fas fa-eraser mr-2"></i> LIMPAR TUDO
                </button>
                
                <button 
                    onclick="gerarPedido()"
                    class="btn-whatsapp text-white py-3 rounded-xl font-semibold flex items-center justify-center text-lg"
                >
                    <i class="fab fa-whatsapp mr-2"></i> ENVIAR PEDIDO
                </button>
            </div>
        </div>

        <!-- Preview da Mensagem -->
        <div class="bg-white rounded-2xl p-6 card-shadow">
            <h3 class="font-semibold text-gray-700 mb-3 flex items-center">
                <i class="fas fa-comment-alt text-blue-500 mr-2"></i> Pré-visualização da Mensagem
            </h3>
            <div id="preview" class="bg-gray-50 rounded-lg p-4 text-gray-600 min-h-[80px] italic">
                A mensagem do seu pedido aparecerá aqui... ✍️
            </div>
        </div>
    </div>

    <script>
        // ===== BASE DE DADOS DOS PRODUTOS =====
        const produtosDB = {
            'MAX': [
                '💉DURATESTON R$150',
                '💉ENANTATO R$150',
                '💉MASTERON R$150',
                '💉STANO R$150',
                '💉DECA R$150',
                '💉TREMBO R$150',
                '💉ENAN DE TREMBO R$150',
                '💊OXANDROLONA 10MG',
                '💊OXANDROLONA 20MG',
                '💊STANOZOLOL',
                '💊DIANABOL'
            ],
            'NEOPHAMA': [
                '💉CIPIONATO R$160',
                '💉PROPIONATO R$160',
                '💉DURATESTON R$160',
                '💉ENANTATO R$160',
                '💉MASTERON R$160',
                '💉STANO R$160',
                '💉DECA R$160',
                '💉TREMBO R$160',
                '💉ENAN DE TREMBO R$160',
                '💊OXANDROLONA 10MG',
                '💊OXANDROLONA 20MG',
                '💊STANOZOLOL',
                '💊DIANABOL'
            ],
            'LANDERLAN': [
                '💉ENANTATO R$200',
                '💉ENAN DE TREMBO R$200',
                '💉MASTERON R$200',
                '💉DECA R$200',
                '💉DURATESTON R$200',
                '💊OXANDROLONA 0.5MG R$240'
            ]
        };

        // Função para criar o HTML do menu completo
        function criarConteudoMenu() {
            let html = '';
            for (const marca in produtosDB) {
                html += `<div class="marca-option">🏷️ ${marca}</div>`;
                produtosDB[marca].forEach(produto => {
                    html += `<div class="produto-option" onclick="selecionarProduto(this, '${produto}', '${marca}')">├─ ${produto}</div>`;
                });
            }
            return html;
        }

        // Função para abrir/fechar menu
        function toggleMenu(botao) {
            const container = botao.closest('.relative');
            const menu = container.querySelector('.marca-menu');
            
            if(!menu.innerHTML) {
                menu.innerHTML = criarConteudoMenu();
            }
            
            menu.classList.toggle('show');
        }

        // Função para selecionar produto
        function selecionarProduto(elemento, nomeProduto, nomeMarca) {
            const container = elemento.closest('.relative');
            const inputProduto = container.closest('.item').querySelector('.produto');
            const inputMarca = container.querySelector('.marca-input');
            
            inputProduto.value = nomeProduto;
            inputMarca.value = nomeMarca;
            
            container.querySelector('.marca-menu').classList.remove('show');
            atualizarPreview();
        }

        // Fechar menus ao clicar fora
        document.addEventListener('click', function(e) {
            if (!e.target.closest('.relative')) {
                document.querySelectorAll('.marca-menu').forEach(menu => {
                    menu.classList.remove('show');
                });
            }
        });

        // ===== FUNÇÕES DE GERENCIAMENTO DOS ITENS =====

        function criarNovoItem() {
            const div = document.createElement('div');
            div.className = 'item bg-white rounded-lg p-4 mb-3 border border-green-100';
            div.innerHTML = `
                <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                    <div>
                        <label class="text-xs text-gray-500 mb-1 block">PRODUTO</label>
                        <input type="text" class="produto w-full px-3 py-2 rounded-lg border border-gray-200 outline-none input-focus" placeholder="Nome do produto">
                    </div>
                    <div class="relative">
                        <label class="text-xs text-gray-500 mb-1 block">MARCA</label>
                        <div class="flex">
                            <input type="text" class="marca-input w-full px-3 py-2 rounded-lg border border-gray-200 outline-none input-focus" placeholder="Selecione" readonly>
                            <button onclick="toggleMenu(this)" class="ml-2 px-3 py-2 bg-gray-200 hover:bg-gray-300 rounded-lg transition-all">
                                <i class="fas fa-chevron-down"></i>
                            </button>
                        </div>
                        <div class="marca-menu"></div>
                    </div>
                    <div>
                        <label class="text-xs text-gray-500 mb-1 block">QUANTIDADE</label>
                        <div class="flex">
                            <input type="number" class="quantidade w-full px-3 py-2 rounded-lg border border-gray-200 outline-none input-focus" placeholder="Qtd" min="1">
                            <button onclick="removerItem(this)" class="ml-2 px-3 bg-red-100 text-red-500 rounded-lg hover:bg-red-200 transition-all">
                                <i class="fas fa-trash"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `;
            return div;
        }

        // ✅ FUNÇÃO CORRIGIDA: ADICIONA SEM APAGAR O QUE JÁ ESTÁ LÁ
        function adicionarItem() {
            const container = document.getElementById('itensContainer');
            const novoItem = criarNovoItem();
            container.appendChild(novoItem); // Adiciona no final, mantendo os anteriores
        }

        function removerItem(botao) {
            botao.closest('.item').remove();
            atualizarPreview();
        }

        function limparTudo() {
            document.getElementById('numero').value = '';
            document.getElementById('itensContainer').innerHTML = '';
            document.getElementById('itensContainer').appendChild(criarNovoItem());
            document.getElementById('preview').innerHTML = 'A mensagem do seu pedido aparecerá aqui... ✍️';
        }

        // ===== CÁLCULOS E GERAÇÃO DO PEDIDO =====

        function extrairValor(texto) {
            const match = texto.match(/R\$[\s]*(\d+)/);
            return match ? parseInt(match[1]) : 0;
        }

        document.addEventListener('input', atualizarPreview);

        function atualizarPreview() {
            const itens = document.querySelectorAll('.item');
            let totalUnidades = 0;
            let valorTotal = 0;
            
            let msg = `🛒🔥 *NOVO PEDIDO* 🔥🛒\n\n`;
            msg += `Olá, segue abaixo o meu pedido:\n\n`;
            msg += `📦✨ *LISTA DE PRODUTOS* ✨📦\n`;
            msg += `╔══════════════════════════════╗\n`;

            itens.forEach(item => {
                const nome = item.querySelector('.produto').value || '';
                const marca = item.querySelector('.marca-input').value || '';
                const qtd = item.querySelector('.quantidade').value || '';

                if(nome && qtd) {
                    const qtdNum = parseInt(qtd);
                    totalUnidades += qtdNum;
                    valorTotal += extrairValor(nome) * qtdNum;

                    msg += `\n📦 *${nome.toUpperCase()}*\n`;
                    msg += `🏷️ *MARCA:* ${marca}\n`;
                    msg += `🔢 *QUANTIDADE:* ${qtd} un\n`;
                    msg += `──────────────────────────\n`;
                }
            });

            msg += `╚══════════════════════════════╝\n\n`;
            msg += `📦🔥 *TOTAL DE UNIDADES: ${totalUnidades}* 🔥📦\n`;
            msg += `💰🔥 *VALOR TOTAL: R$${valorTotal.toFixed(2).replace('.', ',')}* 🔥💰\n\n`;
            msg += `✅ *AGUARDAMOS A CONFIRMAÇÃO!* ✅`;

            document.getElementById('preview').innerHTML = msg.replace(/\n/g, '<br>');
        }

        function gerarPedido() {
            const inputNumero = document.getElementById('numero');
            const numeroLimpo = inputNumero.value.replace(/\D/g, '');

            if(!numeroLimpo || numeroLimpo.length < 10) {
                alert('⚠️ Digite um número válido com DDD + 9 dígitos!');
                return;
            }

            const itens = document.querySelectorAll('.item');
            let totalUnidades = 0;
            let valorTotal = 0;
            let msg = `🛒🔥 *NOVO PEDIDO* 🔥🛒\n\n`;
            msg += `Olá, segue abaixo o meu pedido:\n\n`;
            msg += `📦✨ *LISTA DE PRODUTOS* ✨📦\n`;
            msg += `╔══════════════════════════════╗\n`;

            itens.forEach(item => {
                const nome = item.querySelector('.produto').value.trim();
                const marca = item.querySelector('.marca-input').value.trim();
                const qtd = item.querySelector('.quantidade').value.trim();

                if(nome && qtd) {
                    const qtdNum = parseInt(qtd);
                    totalUnidades += qtdNum;
                    valorTotal += extrairValor(nome) * qtdNum;

                    msg += `\n📦 *${nome.toUpperCase()}*\n`;
                    msg += `🏷️ *MARCA:* ${marca}\n`;
                    msg += `🔢 *QUANTIDADE:* ${qtd} un\n`;
                    msg += `──────────────────────────\n`;
                }
            });

            if(totalUnidades === 0) {
                alert('⚠️ Adicione pelo menos um produto com quantidade!');
                return;
            }

            msg += `╚══════════════════════════════╝\n\n`;
            msg += `📦🔥 *TOTAL DE UNIDADES: ${totalUnidades}* 🔥📦\n`;
            msg += `💰🔥 *VALOR TOTAL: R$${valorTotal.toFixed(2).replace('.', ',')}* 🔥💰\n\n`;
            msg += `✅ *AGUARDAMOS A CONFIRMAÇÃO!* ✅`;

            const link = `https://wa.me/55${numeroLimpo}?text=${encodeURIComponent(msg)}`;
            window.open(link, '_blank');
        }

        // Inicializar
        document.addEventListener('DOMContentLoaded', () => {
            document.querySelector('.marca-menu').innerHTML = criarConteudoMenu();
        });
    </script>
</body>
</html>
