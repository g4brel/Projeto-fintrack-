# Projeto-fintrack-
Um projeto de finanças para poder vcs ter controle de finanças para poder orgânica mais sua vida

```slides
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Protótipo UX Avançado - FinTrack</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:opsz,wght@9..40,400;9..40,500;9..40,700&family=Plus+Jakarta+Sans:wght@500;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        /* 1. CORE VARIABLES & SETUP */
        :root {
            --bg-color: #f8fafc;
            --surface-color: #ffffff;
            --text-primary: #0f172a;
            --text-secondary: #475569;
            --accent-primary: #4f46e5;
            --accent-secondary: #059669;
            --accent-error: #e11d48;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: #e2e8f0;
            display: grid;
            gap: 40px;
            grid-template-columns: 1fr;
            min-height: 100vh;
            place-items: center;
            padding: 40px 0;
            font-family: 'DM Sans', sans-serif;
        }

        /* 2. SLIDE CONTAINER & BACKGROUNDS */
        .slide-container {
            width: 1280px;
            height: 720px;
            background-color: var(--bg-color);
            border-radius: 16px;
            box-shadow: 0 20px 40px rgba(15, 23, 42, 0.1);
            overflow: hidden;
            position: relative;
            display: flex;
            flex-direction: column;
            padding: 60px;
            flex-shrink: 0;
        }

        /* Ambient Background Blobs */
        .slide-container::before {
            content: '';
            position: absolute;
            top: -200px;
            right: -200px;
            width: 700px;
            height: 700px;
            background: radial-gradient(circle, rgba(79, 70, 229, 0.04) 0%, rgba(248, 250, 252, 0) 70%);
            border-radius: 50%;
            z-index: 0;
        }
        .slide-container::after {
            content: '';
            position: absolute;
            bottom: -300px;
            left: -100px;
            width: 800px;
            height: 800px;
            background: radial-gradient(circle, rgba(5, 150, 105, 0.03) 0%, rgba(248, 250, 252, 0) 70%);
            border-radius: 50%;
            z-index: 0;
        }

        .slide-container > * {
            position: relative;
            z-index: 1;
        }

        /* 3. TYPOGRAPHY */
        h1, h2, h3, h4 {
            font-family: 'Plus Jakarta Sans', sans-serif;
            color: var(--text-primary);
        }

        .slide-title {
            font-size: 48px;
            font-weight: 800;
            margin-bottom: 40px;
            text-align: left;
            width: 100%;
            letter-spacing: -1px;
            color: var(--text-primary);
        }

        p {
            font-size: 20px;
            line-height: 1.6;
            color: var(--text-secondary);
        }

        /* Content Area Wrapper */
        .content-area {
            display: flex;
            flex-direction: column;
            flex-grow: 1;
            width: 100%;
            justify-content: center;
        }

        /* 4. LAYOUTS */

        /* Title Slide */
        #slide1, #slide5 {
            background-color: var(--text-primary);
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        #slide1::before, #slide5::before {
            background: radial-gradient(circle, rgba(79, 70, 229, 0.15) 0%, rgba(15, 23, 42, 0) 70%);
        }
        #slide1 h1, #slide5 h2 {
            color: var(--surface-color);
            font-size: 72px;
            letter-spacing: -2px;
            margin-bottom: 24px;
        }
        #slide1 p, #slide5 p {
            color: #94a3b8;
            font-size: 26px;
            max-width: 800px;
            margin: 0 auto;
        }
        .accent-bar {
            width: 120px;
            height: 6px;
            background: var(--accent-primary);
            margin: 40px auto;
            border-radius: 3px;
        }

        /* Section Title Layout */
        .section-title-layout p.pre-title {
            color: var(--accent-primary);
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 3px;
            font-size: 20px;
            margin-bottom: 20px;
        }

        /* Two Column Tiled Text */
        .two-column.tiled {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 50px;
            height: 100%;
            align-items: stretch;
        }
        .two-column.tiled > div {
            background-color: var(--surface-color);
            border-radius: 24px;
            padding: 50px;
            box-shadow: 0 15px 35px rgba(15, 23, 42, 0.04);
            border: 1px solid rgba(0,0,0,0.03);
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        .two-column.tiled h3 {
            color: var(--accent-primary);
            font-size: 36px;
            margin-bottom: 20px;
        }

        /* Tiled Text With Icons */
        .tiled-content {
            display: flex;
            gap: 40px;
            height: 100%;
            align-items: stretch;
            width: 100%;
        }
        .tile {
            background-color: var(--surface-color);
            border-radius: 24px;
            padding: 50px 40px;
            flex: 1;
            text-align: center;
            box-shadow: 0 15px 35px rgba(15, 23, 42, 0.04);
            border: 1px solid rgba(0,0,0,0.03);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .tile .icon {
            width: 80px;
            height: 80px;
            background-color: rgba(79, 70, 229, 0.1);
            color: var(--accent-primary);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            margin-bottom: 30px;
        }
        .tile h3 {
            font-size: 26px;
            margin-bottom: 20px;
            color: var(--text-primary);
        }

        /* Bleed Image Right */
        .slide-container.bleed-image-layout {
            padding: 0;
            background-color: var(--surface-color);
            display: grid;
            grid-template-columns: repeat(2, minmax(0, 1fr));
            align-items: start;
        }
        .bleed-image-layout > .content-container {
            padding: 80px 60px;
            display: flex;
            flex-direction: column;
            height: 100%;
            justify-content: center;
        }
        .bleed-text-side h3 {
            font-size: 40px;
            color: var(--accent-primary);
            margin-bottom: 24px;
        }
        .bleed-text-side p {
            margin-bottom: 20px;
        }
        .bleed-image-layout .image-container {
            height: 100%;
            width: 100%;
            overflow: hidden;
        }
        .bleed-image-layout img.bleed-image-side {
            width: 100%;
            height: 720px;
            object-fit: cover;
            object-position: center left;
        }

        /* Image Right Text Left */
        .two-column {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 60px;
            height: 100%;
        }
        .text-column {
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        .image-wrapper {
            width: 100%;
            height: 480px;
            background-color: #f1f5f9;
            border-radius: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            border: 1px solid rgba(0,0,0,0.05);
            padding: 20px;
        }
        .image-wrapper img {
            width: 100%;
            height: 100%;
            object-fit: contain;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        /* Bullet List Styling */
        .bullet-list {
            width: 100%;
        }
        .bullet-list ul {
            list-style: none;
        }
        .bullet-list li {
            position: relative;
            padding-left: 45px;
            margin-bottom: 24px;
            font-size: 20px;
            color: var(--text-secondary);
        }
        .bullet-list li::before {
            content: '\f058';
            font-family: 'Font Awesome 6 Free';
            font-weight: 900;
            position: absolute;
            left: 0;
            top: 2px;
            color: var(--accent-primary);
            font-size: 26px;
        }
        .bullet-list li strong {
            color: var(--text-primary);
            font-weight: 700;
        }
        .bullet-list li.error-highlight::before {
            content: '\f071';
            color: var(--accent-error);
        }

        /* Timeline Layout */
        .timeline-layout {
            align-items: stretch;
            display: flex;
            height: 100%;
            justify-content: space-between;
            position: relative;
            width: 100%;
        }
        .timeline-layout .timeline-line {
            background-color: #cbd5e1;
            height: 4px;
            left: 0;
            position: absolute;
            top: 50%;
            transform: translateY(-2px);
            width: 100%;
            z-index: 0;
        }
        .timeline-item {
            position: relative;
            text-align: center;
            width: 23%;
        }
        .timeline-item::after {
            background-color: var(--surface-color);
            border-radius: 50%;
            border: 4px solid var(--accent-primary);
            content: '';
            height: 24px;
            left: 50%;
            position: absolute;
            top: 50%;
            transform: translate(-50%, -50%);
            width: 24px;
            z-index: 1;
        }
        .timeline-item .content-wrapper {
            left: 0;
            position: absolute;
            width: 100%;
            padding: 30px;
            background: var(--surface-color);
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(15, 23, 42, 0.05);
            border: 1px solid rgba(0,0,0,0.03);
        }
        .timeline-item:nth-child(odd) .content-wrapper {
            bottom: 50%;
            margin-bottom: 40px;
        }
        .timeline-item:nth-child(even) .content-wrapper {
            margin-top: 40px;
            top: 50%;
        }
        .timeline-item h3 {
            color: var(--accent-primary);
            font-size: 22px;
            margin-bottom: 12px;
        }

        /* Q&A Layout */
        .qa-layout {
            margin: 0 auto;
            text-align: center;
            width: 100%;
        }
        .qa-layout h2 {
            font-size: 72px;
            margin-bottom: 24px;
            letter-spacing: -2px;
            color: var(--text-primary);
        }
        .qa-layout p {
            font-size: 26px;
            margin-bottom: 50px;
        }
        .qa-layout .contact-info {
            display: inline-block;
            padding: 20px 50px;
            background-color: rgba(79, 70, 229, 0.1);
            border-radius: 100px;
            color: var(--accent-primary);
            font-size: 24px;
            font-weight: 700;
        }
    </style>
</head>
<body>

<!-- Slide 1: Title_Slide -->
<div class="slide-container" id="slide1">
    <div class="title-layout">
        <h1>Protótipo UX: FinTrack</h1>
        <div class="accent-bar"></div>
        <p class="subtitle">Atividade Prática – Projeto de Interfaces Gráficas para Web<br>Protótipo Navegável e Sistema de Design</p>
    </div>
</div>

<!-- Slide 2: Two_Column_Tiled_Text -->
<div class="slide-container" id="slide2">
    <h2 class="slide-title">O Desafio do Projeto</h2>
    <div class="content-area">
        <div class="two-column tiled">
            <div>
                <h3>O Objetivo</h3>
                <p>Desenvolver um protótipo UX navegável e detalhado para um Sistema Web, aplicando conceitos avançados de usabilidade, arquitetura de informação e design responsivo, exigidos pela disciplina.</p>
            </div>
            <div>
                <h3>O Tema: FinTrack</h3>
                <p>Escolhemos projetar um aplicativo de <strong>Finanças Pessoais</strong>. Este tema nos permite explorar fluxos de dados complexos, gráficos, inputs de formulário rigorosos e microinterações de feedback de forma muito natural.</p>
            </div>
        </div>
    </div>
</div>

<!-- Slide 3: Tiled_Text_With_Icons -->
<div class="slide-container" id="slide3">
    <h2 class="slide-title">Requisitos Atendidos</h2>
    <div class="content-area">
        <div class="tiled-content">
            <div class="tile">
                <div class="icon"><i class="fa-solid fa-mobile-screen"></i></div>
                <h3>6 Telas Distintas</h3>
                <p>Projetamos o escopo exato: Login, Dashboard, Formulário de Cadastro, Listagem (Extrato), Perfil de Usuário e Feedback Positivo.</p>
            </div>
            <div class="tile">
                <div class="icon"><i class="fa-solid fa-route"></i></div>
                <h3>Fluxos Alternativos</h3>
                <p>Navegação lógica e funcional implementada, incluindo o tratamento visual de erros de login e bloqueios de submissão de dados.</p>
            </div>
            <div class="tile">
                <div class="icon"><i class="fa-solid fa-pen-nib"></i></div>
                <h3>Consistência Visual</h3>
                <p>Paleta de cores padronizada, tipografia legível e validações através de microinterações para guiar a atenção do usuário perfeitamente.</p>
            </div>
        </div>
    </div>
</div>

<!-- Slide 4: Bleed_Image_Right -->
<div class="slide-container bleed-image-layout" id="slide4">
    <div class="content-container">
        <h2 class="slide-title">Design System</h2>
        <div class="content-area">
            <div class="bleed-text-side">
                <h3>Identidade Visual</h3>
                <p>Estabelecemos uma linguagem focada na usabilidade e confiança. Utilizamos o contraste cognitivo natural: tons de <strong>Verde</strong> para receitas e <strong>Vermelho</strong> para despesas.</p>
                <p>A tipografia limpa em interfaces densas garante escaneabilidade, enquanto botões e áreas de clique possuem proporções ideais para toques precisos, reduzindo a carga cognitiva.</p>
            </div>
        </div>
    </div>
    <div class="image-container">
        <img class="bleed-image-side" src="http://googleusercontent.com/image_collection/image_retrieval/8330714046996646351" alt="Design system, color palette and typography">
    </div>
</div>

<!-- Slide 5: Section_Title -->
<div class="slide-container" id="slide5">
    <div class="section-title-layout">
        <p class="pre-title">O Protótipo em Detalhes</p>
        <h2>As 6 Telas do Sistema</h2>
        <div class="accent-bar"></div>
        <p style="color: #94a3b8; font-size: 22px; font-weight: 400; text-transform: none; letter-spacing: 0;">Explorando o fluxo e as decisões de UX</p>
    </div>
</div>

<!-- Slide 6: Image_Right_Text_Left (Tela 1) -->
<div class="slide-container" id="slide6">
    <h2 class="slide-title">1. Tela de Login</h2>
    <div class="content-area">
        <div class="two-column">
            <div class="text-column">
                <div class="bullet-list">
                    <ul>
                        <li><strong>Porta de entrada limpa:</strong> Interface livre de distrações para focar na ação primária de conversão/autenticação.</li>
                        <li class="error-highlight"><strong>Tratamento de Erro (Requisito):</strong> O protótipo demonstra ativamente um caminho alternativo caso as credenciais estejam erradas.</li>
                        <li><strong>Validação Visual:</strong> Bordas e ícones semânticos ficam vermelhos, exibindo uma mensagem de ajuda clara ao invés de códigos genéricos.</li>
                    </ul>
                </div>
            </div>
            <div class="image-wrapper">
                <img src="http://googleusercontent.com/image_collection/image_retrieval/5454645291976204925" alt="Clean mobile app login screen UI design">
            </div>
        </div>
    </div>
</div>

<!-- Slide 7: Image_Right_Text_Left (Tela 2) -->
<div class="slide-container" id="slide7">
    <h2 class="slide-title">2. Dashboard Principal</h2>
    <div class="content-area">
        <div class="two-column">
            <div class="text-column">
                <div class="bullet-list">
                    <ul>
                        <li><strong>Arquitetura da Informação:</strong> Dados cruciais (Saldo Atual, Receitas, Despesas) posicionados no topo para acesso em menos de 2 segundos.</li>
                        <li><strong>Anatomia dos Cards:</strong> Uso de ícones de grande porte e fontes em negrito para facilitar a leitura rápida dos valores financeiros.</li>
                        <li><strong>Contexto Imediato:</strong> Exibição das transações mais recentes logo abaixo, evitando cliques desnecessários para acompanhamento diário.</li>
                    </ul>
                </div>
            </div>
            <div class="image-wrapper">
                <img src="http://googleusercontent.com/image_collection/image_retrieval/10472928051566829903" alt="Personal finance dashboard UI design app">
            </div>
        </div>
    </div>
</div>

<!-- Slide 8: Image_Right_Text_Left (Tela 3) -->
<div class="slide-container" id="slide8">
    <h2 class="slide-title">3. Formulário de Cadastro</h2>
    <div class="content-area">
        <div class="two-column">
            <div class="text-column">
                <div class="bullet-list">
                    <ul>
                        <li><strong>Seletor em Toggle:</strong> Troca rápida e visual entre registrar 'Receita' ou 'Despesa' no topo do formulário.</li>
                        <li><strong>Prevenção de Erros:</strong> Inputs numéricos bloqueiam caracteres inválidos e o botão "Salvar" reage instantaneamente aos campos obrigatórios faltantes.</li>
                        <li><strong>Clareza no Input:</strong> Labels bem contrastadas, com "placeholders" que guiam e facilitam o preenchimento dos dados.</li>
                    </ul>
                </div>
            </div>
            <div class="image-wrapper">
                <img src="http://googleusercontent.com/image_collection/image_retrieval/4516869966298086259" alt="Mobile app form input transaction UI design">
            </div>
        </div>
    </div>
</div>

<!-- Slide 9: Image_Right_Text_Left (Tela 4) -->
<div class="slide-container" id="slide9">
    <h2 class="slide-title">4. Listagem / Extrato</h2>
    <div class="content-area">
        <div class="two-column">
            <div class="text-column">
                <div class="bullet-list">
                    <ul>
                        <li><strong>Controle Total:</strong> Tela inteiramente dedicada ao histórico, providenciando scroll infinito e dados tabulares organizados.</li>
                        <li><strong>Filtros Avançados:</strong> Inclusão de um dropdown intuitivo para segmentar o extrato por período temporal (mês/ano).</li>
                        <li><strong>Legibilidade:</strong> Valores positivos exibidos em verde e alinhados à direita, facilitando a varredura visual dos olhos.</li>
                    </ul>
                </div>
            </div>
            <div class="image-wrapper">
                <img src="http://googleusercontent.com/image_collection/image_retrieval/9906571027341182939" alt="Mobile app transaction history list UI design">
            </div>
        </div>
    </div>
</div>

<!-- Slide 10: Image_Right_Text_Left (Tela 5 e 6) -->
<div class="slide-container" id="slide10">
    <h2 class="slide-title">5. Perfil & 6. Feedback</h2>
    <div class="content-area">
        <div class="two-column">
            <div class="text-column">
                <div class="bullet-list">
                    <ul>
                        <li><strong>Tela 5 - Configurações:</strong> Agrupamento inteligente de preferências do sistema e dados do usuário em "switches" táteis.</li>
                        <li><strong>Tela 6 - Feedback:</strong> Interrompe o fluxo após o cadastro de uma transação com uma mensagem animada e focada de sucesso.</li>
                        <li><strong>Fechando o Ciclo UX:</strong> O feedback positivo reduz a ansiedade do usuário, confirmando que a ação no sistema foi concluída.</li>
                    </ul>
                </div>
            </div>
            <div class="image-wrapper">
                <img src="http://googleusercontent.com/image_collection/image_retrieval/8182750772502701629" alt="Mobile app user profile settings UI design">
            </div>
        </div>
    </div>
</div>

<!-- Slide 11: Timeline -->
<div class="slide-container" id="slide11">
    <h2 class="slide-title">Metodologia de Criação</h2>
    <div class="content-area">
        <div class="timeline-layout">
            <div class="timeline-line"></div>
            <div class="timeline-item">
                <div class="content-wrapper">
                    <h3>1. Pesquisa & Escopo</h3>
                    <p>Definição do tema "FinTrack", requisitos das 6 telas e regras de negócio essenciais.</p>
                </div>
            </div>
            <div class="timeline-item">
                <div class="content-wrapper">
                    <h3>2. Wireframing</h3>
                    <p>Desenho da arquitetura da informação e do fluxo crítico de navegação (Baixa fidelidade).</p>
                </div>
            </div>
            <div class="timeline-item">
                <div class="content-wrapper">
                    <h3>3. UI & Design System</h3>
                    <p>Aplicação de cores, criação dos componentes, padronização tipográfica e iconografia.</p>
                </div>
            </div>
            <div class="timeline-item">
                <div class="content-wrapper">
                    <h3>4. Alta Fidelidade</h3>
                    <p>Prototipagem interativa final, mapeando os caminhos alternativos de erro e acerto.</p>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- Slide 12: Q&A -->
<div class="slide-container" id="slide12">
    <div class="qa-layout">
        <h2>Conclusão da Atividade</h2>
        <div class="contact-info">
            
        </div>
    </div>
</div>

</body>
</html>

```
