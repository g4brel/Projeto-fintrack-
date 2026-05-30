# Projeto-fintrack-


    

```react
import React, { useState } from 'react';
import { 
  LayoutDashboard, TrendingUp, Target, Sliders, Plus, X, Loader2, ArrowLeft, 
  Camera, ChevronRight, AlertTriangle, Eye, EyeOff, CheckCircle2, Trophy,
  ArrowUpCircle, ArrowDownCircle
} from 'lucide-react';

export default function App() {
  // --- BASE DE DADOS LOCAL DE UTILIZADORES (Dinâmica para suportar novos registos) ---
  const [registeredUsers, setRegisteredUsers] = useState([
    { name: 'Gabriel Silva', email: 'gabriel@fintrack.pt', password: '1234' }
  ]);

  // --- CONTROLO DE NAVEGAÇÃO SPA ---
  const [currentScreen, setCurrentScreen] = useState('login'); // login, recuperar, registar, app
  const [activeTab, setActiveTab] = useState('dashboard'); // dashboard, gastos, metas, definicoes
  const [activeSubScreen, setActiveSubScreen] = useState('menu'); // menu, perfil (dentro de definicoes)
  
  // --- SISTEMA DE COR DE TEMA DINÂMICA ---
  const [themeColor, setThemeColor] = useState('#E51D3E'); // Crimson Red original do Lucidspark

  // --- CONTROLOS DOS MODAIS (BOTTOM SHEET) ---
  const [showNovoLancamento, setShowNovoLancamento] = useState(false);
  const [showNovaMeta, setShowNovaMeta] = useState(false);

  // --- ESTADO DE CARREGAMENTOS E NOTIFICAÇÕES TOAST ---
  const [isLoading, setIsLoading] = useState(false);
  const [toast, setToast] = useState(null);
  const [loginError, setLoginError] = useState(false);

  // --- CONTROLO DE VISIBILIDADE DA SENHA ---
  const [showPassword, setShowPassword] = useState(false);

  // --- UTILIZADOR AUTENTICADO ---
  const [loggedUser, setLoggedUser] = useState(null);

  // --- DADOS FINANCEIROS INICIAIS ---
  const [lucros, setLucros] = useState([
    { id: 1, desc: 'Salário Principal', val: 4500.00 }
  ]);
  const [gastos, setGastos] = useState([
    { id: 1, cat: 'Alimentação', desc: 'Supermercado', val: 120.00 },
    { id: 2, cat: 'Transporte', desc: 'Combustível', val: 150.00 },
    { id: 3, cat: 'Lazer', desc: 'Cinema e Jantar', val: 200.50 }
  ]);

  const [metas, setMetas] = useState([
    { id: '1', nome: 'Viagem Japão', atual: 4200, total: 5000, cor: '#F59E0B' },
    { id: '2', nome: 'Novo Carro', atual: 3125, total: 12500, cor: '#3B82F6' }
  ]);

  // --- FORMULÁRIOS DE CRIAÇÃO ---
  const [novoValor, setNovoValor] = useState('');
  const [novaDescricao, setNovaDescricao] = useState('');
  const [tipoTransacao, setTipoTransacao] = useState('despesa'); // despesa, receita
  const [categoriaTransacao, setCategoriaTransacao] = useState('Alimentação');
  const [metaNome, setMetaNome] = useState('');
  const [metaTotal, setMetaTotal] = useState('');

  // --- FORMULÁRIO PERFIL EDITÁVEL ---
  const [editNome, setEditNome] = useState('Gabriel Silva');
  const [editEmail, setEditEmail] = useState('gabriel@fintrack.pt');
  const [editPhone, setEditPhone] = useState('+351 912 345 678');

  // Notificações Toast de Sistema
  const showToast = (msg, type) => {
    setToast({ msg, type });
    setTimeout(() => setToast(null), 3000);
  };

  // Cálculos Automáticos de Lucros, Despesas e Saldo
  const totalReceitas = lucros.reduce((acc, curr) => acc + curr.val, 0);
  const totalDespesas = gastos.reduce((acc, curr) => acc + curr.val, 0);
  const saldoFinal = totalReceitas - totalDespesas;

  // Lógica de Login Dinâmica (Pesquisa na lista de registados)
  const handleLogin = (e) => {
    e.preventDefault();
    setIsLoading(true);
    setLoginError(false);

    const emailInput = e.target.email.value.trim().toLowerCase();
    const passwordInput = e.target.password.value;

    setTimeout(() => {
      // Procura o utilizador correspondente na lista
      const userFound = registeredUsers.find(
        user => user.email === emailInput && user.password === passwordInput
      );

      if (userFound) {
        setLoggedUser(userFound);
        setEditNome(userFound.name);
        setEditEmail(userFound.email);
        setCurrentScreen('app');
        setLoginError(false);
        showToast(`Bem-vindo, ${userFound.name}!`, "success");
      } else {
        setLoginError(true);
        showToast("E-mail ou Palavra-passe incorretos", "error");
      }
      setIsLoading(false);
    }, 600);
  };

  // Lógica de Criar Conta (Suporta múltiplos utilizadores)
  const handleRegister = (e) => {
    e.preventDefault();
    const name = e.target.regName.value.trim();
    const email = e.target.regEmail.value.trim().toLowerCase();
    const password = e.target.regPass.value;

    if (registeredUsers.some(user => user.email === email)) {
      showToast("E-mail já registado!", "error");
      return;
    }

    // Adiciona o novo utilizador à lista
    setRegisteredUsers([...registeredUsers, { name, email, password }]);
    showToast("Conta criada! Já pode iniciar sessão.", "success");
    setCurrentScreen('login');
  };

  // Adicionar Lançamento Dinâmico (Dedução de Saldo e Adição Imediata na Lista)
  const handleAddTransaction = (e) => {
    e.preventDefault();
    const valorNum = parseFloat(novoValor);
    if (isNaN(valorNum) || valorNum <= 0) {
      showToast("Insira um valor válido!", "error");
      return;
    }

    if (tipoTransacao === 'despesa') {
      const novaDespesa = {
        id: Date.now(),
        cat: categoriaTransacao,
        desc: novaDescricao || 'Gasto Geral',
        val: valorNum
      };
      setGastos([novaDespesa, ...gastos]);
      showToast("Despesa registada com sucesso!", "success");
    } else {
      const novoLucro = {
        id: Date.now(),
        desc: novaDescricao || 'Receita Geral',
        val: valorNum
      };
      setLucros([novoLucro, ...lucros]);
      showToast("Lucro adicionado com sucesso!", "success");
    }

    setNovoValor('');
    setNovaDescricao('');
    setShowNovoLancamento(false);
  };

  // Criar Nova Meta
  const handleCreateGoal = (e) => {
    e.preventDefault();
    const totalNum = parseFloat(metaTotal);
    if (!metaNome || isNaN(totalNum) || totalNum <= 0) return;

    setMetas([...metas, {
      id: Date.now().toString(),
      nome: metaNome,
      atual: 0,
      total: totalNum,
      cor: themeColor 
    }]);

    setMetaNome('');
    setMetaTotal('');
    setShowNovaMeta(false);
    setActiveTab('metas');
    showToast("Nova meta guardada com sucesso!", "success");
  };

  // Guardar Dados do Perfil de Utilizador
  const handleSaveProfile = (e) => {
    e.preventDefault();
    const updatedUsers = registeredUsers.map(user => {
      if (user.email === loggedUser.email) {
        return { ...user, name: editNome, email: editEmail };
      }
      return user;
    });

    setRegisteredUsers(updatedUsers);
    setLoggedUser({ ...loggedUser, name: editNome, email: editEmail });
    setActiveSubScreen('menu');
    showToast("Perfil atualizado!", "success");
  };

  // Limites Orçamentais
  const alimentacaoLimite = 400.00;
  const transporteLimite = 200.00;
  const lazerLimite = 500.00;

  const totalAlimentacao = gastos.filter(g => g.cat === 'Alimentação').reduce((a, b) => a + b.val, 0);
  const totalTransporte = gastos.filter(g => g.cat === 'Transporte').reduce((a, b) => a + b.val, 0);
  const totalLazer = gastos.filter(g => g.cat === 'Lazer').reduce((a, b) => a + b.val, 0);

  return (
    <div className="min-h-screen bg-[#0F172A] flex items-center justify-center p-0 md:p-8 select-none font-sans text-slate-800">
      
      {/* NOTIFICAÇÃO TOAST */}
      {toast && (
        <div className="fixed top-6 z-[999] px-6 py-4 rounded-2xl shadow-2xl font-bold flex items-center gap-3 bg-white text-slate-900 border border-slate-100 max-w-sm">
          {toast.type === 'success' ? <CheckCircle2 className="text-emerald-500" size={20}/> : <AlertTriangle className="text-red-500" size={20}/>}
          <span className="text-sm">{toast.msg}</span>
        </div>
      )}

      {/* DISPOSITIVO MÓVEL EMULADO */}
      <div className="w-full max-w-[400px] h-screen md:h-[820px] bg-slate-50 md:rounded-[3rem] md:shadow-[0_0_0_12px_#1e293b] overflow-hidden flex flex-col relative border border-slate-200">
        
        {/* Barra de Estado do Telemóvel */}
        <div className="bg-white px-8 pt-4 pb-2 flex justify-between items-center text-xs font-bold text-slate-800 tracking-tight shrink-0 select-none z-40 border-b border-slate-50">
          <span>23:34</span>
          <div className="flex items-center gap-2">
            <span className="text-[10px] bg-slate-100 px-1.5 py-0.5 rounded font-black">5G</span>
            <div className="w-5 h-2.5 border border-slate-800 rounded-sm p-0.5 flex items-center"><div className="w-3.5 h-full bg-slate-800 rounded-xs"></div></div>
          </div>
        </div>

        {/* --- TELA 1: LOGIN (COM SENHA OCULTÁVEL CLEAN) --- */}
        {currentScreen === 'login' && (
          <div className="flex-grow bg-white flex flex-col justify-between p-8 overflow-y-auto">
            <div className="space-y-10 my-auto">
              
              {/* Brand Box Crimson */}
              <div 
                style={{ backgroundColor: themeColor }}
                className="text-white p-8 rounded-[2.5rem] text-center shadow-xl space-y-4 transition-all duration-300"
              >
                <div className="w-16 h-16 bg-white/20 rounded-2xl mx-auto flex items-center justify-center">
                  <TrendingUp size={32} className="text-white" />
                </div>
                <div>
                  <h2 className="text-3xl font-black italic tracking-tighter">FinTrack</h2>
                  <p className="text-white/60 text-[10px] font-black uppercase tracking-widest mt-1">Gestão Inteligente</p>
                </div>
              </div>

              {/* Form Box */}
              <form onSubmit={handleLogin} className="space-y-4">
                {loginError && (
                  <div className="bg-red-50 border border-red-100 p-3 rounded-xl flex items-center justify-center gap-2 text-red-500 text-xs font-black">
                    <AlertTriangle size={14} />
                    <span>Dados incorretos. Verifique os acessos.</span>
                  </div>
                )}

                <div className="space-y-1">
                  <label className="text-[10px] font-black text-slate-400 uppercase ml-1">E-mail</label>
                  <input name="email" type="email" required placeholder="gabriel@fintrack.pt" className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none text-slate-800 font-bold focus:border-red-500 transition-all text-sm" />
                </div>

                <div className="space-y-1">
                  <div className="flex justify-between items-center px-1">
                    <label className="text-[10px] font-black text-slate-400 uppercase">Palavra-passe</label>
                    <button type="button" onClick={() => setCurrentScreen('recuperar')} className="text-[10px] font-black text-slate-400 hover:text-red-500 uppercase">Esqueceu-se?</button>
                  </div>
                  
                  {/* Container de Senha com Ícone de Olho Integrado */}
                  <div className="relative flex items-center">
                    <input 
                      name="password" 
                      type={showPassword ? "text" : "password"} 
                      required 
                      placeholder="••••" 
                      className="w-full p-4 pr-12 bg-slate-50 border border-slate-100 rounded-2xl outline-none text-slate-800 font-bold focus:border-red-500 transition-all text-sm tracking-widest" 
                    />
                    <button 
                      type="button"
                      onClick={() => setShowPassword(!showPassword)}
                      className="absolute right-4 p-1 text-slate-400 hover:text-slate-600 transition-colors"
                    >
                      {showPassword ? <EyeOff size={18} /> : <Eye size={18} />}
                    </button>
                  </div>
                </div>

                <button 
                  type="submit" 
                  style={{ backgroundColor: themeColor }}
                  className="w-full py-4 text-white font-black rounded-2xl shadow-lg transition-all active:scale-95 flex items-center justify-center text-sm"
                >
                  {isLoading ? <Loader2 className="animate-spin" size={20}/> : "ENTRAR"}
                </button>
              </form>
            </div>

            <button onClick={() => setCurrentScreen('registar')} className="text-center text-xs font-black text-slate-400 hover:text-red-500 py-4 uppercase tracking-widest">
              Criar uma nova conta
            </button>
          </div>
        )}

        {/* --- TELA 2: RECUPERAR SENHA --- */}
        {currentScreen === 'recuperar' && (
          <div className="flex-grow bg-white p-8 flex flex-col justify-between">
            <div>
              <button onClick={() => setCurrentScreen('login')} className="flex items-center gap-1.5 text-xs font-black text-slate-400 hover:text-red-500 uppercase py-2">
                <ArrowLeft size={16}/> Voltar
              </button>
              
              <div className="space-y-6 mt-12">
                <h2 className="text-3xl font-black text-slate-800">Esqueceu a senha?</h2>
                <p className="text-slate-400 font-bold text-sm leading-relaxed">Introduza o seu e-mail para receber as instruções de recuperação.</p>
                
                <div className="space-y-4 pt-4">
                  <input type="email" placeholder="seu@email.com" className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800 focus:border-red-500" />
                  <button 
                    onClick={() => { showToast("E-mail enviado!", "success"); setCurrentScreen('login'); }} 
                    style={{ backgroundColor: themeColor }}
                    className="w-full py-4 text-white font-black rounded-2xl shadow-lg"
                  >
                    Enviar E-mail
                  </button>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* --- TELA 3: CRIAR CONTA (CORRIGIDA E INTEGRADA) --- */}
        {currentScreen === 'registar' && (
          <div className="flex-grow bg-white p-8 flex flex-col justify-between">
            <div>
              <button onClick={() => setCurrentScreen('login')} className="flex items-center gap-1.5 text-xs font-black text-slate-400 hover:text-red-500 uppercase py-2">
                <ArrowLeft size={16}/> Voltar
              </button>
              
              <div className="space-y-6 mt-8">
                <h2 className="text-3xl font-black text-slate-800">Criar Conta</h2>
                
                <form onSubmit={handleRegister} className="space-y-4 pt-2">
                  <input name="regName" required placeholder="Nome Completo" className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800" />
                  <input name="regEmail" type="email" required placeholder="E-mail" className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800" />
                  <input name="regPass" type="password" required placeholder="Palavra-passe" className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800" />
                  <button 
                    type="submit" 
                    style={{ backgroundColor: themeColor }}
                    className="w-full py-4 text-white font-black rounded-2xl shadow-lg mt-2"
                  >
                    Registar
                  </button>
                </form>
              </div>
            </div>
          </div>
        )}

        {/* --- CONTEÚDO PRINCIPAL (DASHBOARD & TABS) --- */}
        {currentScreen === 'app' && (
          <div className="flex-grow flex flex-col justify-between overflow-hidden relative">
            <div className="flex-grow overflow-y-auto p-6 space-y-6 pb-24">
              
              {/* TAB: DASHBOARD */}
              {activeTab === 'dashboard' && (
                <div className="space-y-6">
                  <div className="flex justify-between items-center">
                    <div>
                      <p className="text-[10px] font-black text-slate-400 uppercase tracking-widest">Bem-vindo de volta</p>
                      <h2 className="text-2xl font-black text-slate-800">Olá, {loggedUser?.name.split(' ')[0]}</h2>
                    </div>
                    <div 
                      style={{ backgroundColor: themeColor }}
                      className="w-10 h-10 rounded-full flex items-center justify-center text-white font-bold uppercase shadow-sm"
                    >
                      {loggedUser?.name.charAt(0)}
                    </div>
                  </div>

                  {/* Card de Saldo Geral Crimson */}
                  <div 
                    style={{ backgroundColor: themeColor }}
                    className="text-white p-8 rounded-[2.5rem] shadow-xl shadow-red-500/10 flex flex-col justify-between min-h-[160px] relative overflow-hidden transition-all duration-300"
                  >
                    <span className="text-[10px] font-black uppercase tracking-widest opacity-70">Saldo Disponível</span>
                    <h3 className="text-4xl font-black tracking-tighter mt-4">
                      € {saldoFinal.toLocaleString('pt-PT', { minimumFractionDigits: 2 })}
                    </h3>
                  </div>

                  {/* Receitas / Despesas Row */}
                  <div className="grid grid-cols-2 gap-4">
                    <div className="bg-white p-5 rounded-3xl border border-slate-100 flex flex-col justify-between min-h-[110px]">
                      <span className="text-[10px] font-black text-slate-400 uppercase tracking-widest">Receitas</span>
                      <h4 className="text-lg font-black text-emerald-500 mt-2">
                        + € {totalReceitas.toFixed(2)}
                      </h4>
                    </div>
                    <div className="bg-white p-5 rounded-3xl border border-slate-100 flex flex-col justify-between min-h-[110px]">
                      <span className="text-[10px] font-black text-slate-400 uppercase tracking-widest">Despesas</span>
                      <h4 className="text-lg font-black mt-2" style={{ color: themeColor }}>
                        - € {totalDespesas.toFixed(2)}
                      </h4>
                    </div>
                  </div>
                </div>
              )}

              {/* TAB: GERÊNCIA DE GASTOS */}
              {activeTab === 'gastos' && (
                <div className="space-y-6">
                  <h2 className="text-2xl font-black text-slate-800">Gerência de Gastos</h2>
                  
                  <div className="bg-white p-6 rounded-3xl border border-slate-100">
                    <span className="text-[10px] font-black text-slate-400 uppercase tracking-widest">Total Utilizado</span>
                    <h3 className="text-3xl font-black mt-1" style={{ color: themeColor }}>
                      € {totalDespesas.toLocaleString('pt-PT', { minimumFractionDigits: 2 })}
                    </h3>

                    {/* Barras de Progresso Dinâmicas */}
                    <div className="mt-6 space-y-4">
                      <div className="space-y-2">
                        <div className="flex justify-between text-xs font-bold text-slate-700">
                          <span>Alimentação</span>
                          <span>€ {totalAlimentacao.toFixed(2)} / € {alimentacaoLimite.toFixed(2)}</span>
                        </div>
                        <div className="w-full h-2.5 bg-slate-100 rounded-full overflow-hidden">
                          <div 
                            style={{ width: `${Math.min((totalAlimentacao/alimentacaoLimite)*100, 100)}%`, backgroundColor: themeColor }}
                            className="h-full rounded-full transition-all duration-300" 
                          />
                        </div>
                      </div>

                      <div className="space-y-2">
                        <div className="flex justify-between text-xs font-bold text-slate-700">
                          <span>Transportes</span>
                          <span>€ {totalTransporte.toFixed(2)} / € {transporteLimite.toFixed(2)}</span>
                        </div>
                        <div className="w-full h-2.5 bg-slate-100 rounded-full overflow-hidden">
                          <div 
                            style={{ width: `${Math.min((totalTransporte/transporteLimite)*100, 100)}%` }}
                            className="h-full bg-amber-500 rounded-full transition-all duration-300" 
                          />
                        </div>
                      </div>

                      <div className="space-y-2">
                        <div className="flex justify-between text-xs font-bold text-slate-700">
                          <span>Lazer e Viagens</span>
                          <span>€ {totalLazer.toFixed(2)} / € {lazerLimite.toFixed(2)}</span>
                        </div>
                        <div className="w-full h-2.5 bg-slate-100 rounded-full overflow-hidden">
                          <div 
                            style={{ width: `${Math.min((totalLazer/lazerLimite)*100, 100)}%` }}
                            className="h-full bg-indigo-500 rounded-full transition-all duration-300" 
                          />
                        </div>
                      </div>
                    </div>
                  </div>

                  {/* Listagem de Despesas */}
                  <div className="space-y-3">
                    <h3 className="text-xs font-black text-slate-400 uppercase tracking-widest pl-1">Histórico Recente</h3>
                    {gastos.map(g => (
                      <div key={g.id} className="bg-white p-4 rounded-2xl border border-slate-100 flex justify-between items-center">
                        <div>
                          <p className="font-bold text-slate-800">{g.desc}</p>
                          <p className="text-[10px] text-slate-400 font-bold uppercase">{g.cat}</p>
                        </div>
                        <span className="font-black text-sm text-red-500">- €{g.val.toFixed(2)}</span>
                      </div>
                    ))}
                  </div>
                </div>
              )}

              {/* TAB: MINHAS METAS */}
              {activeTab === 'metas' && (
                <div className="space-y-6">
                  <div className="flex justify-between items-center">
                    <h2 className="text-2xl font-black text-slate-800">Minhas Metas</h2>
                    <button 
                      onClick={() => setShowNovaMeta(true)}
                      style={{ backgroundColor: themeColor }}
                      className="p-2 text-white rounded-xl active:scale-95 transition-all"
                    >
                      <Plus size={20}/>
                    </button>
                  </div>
                  
                  <div className="space-y-4">
                    {metas.map(meta => {
                      const perc = Math.min((meta.atual / meta.total) * 100, 100);
                      return (
                        <div key={meta.id} className="bg-white p-6 rounded-3xl border border-slate-100 space-y-3">
                          <div className="flex justify-between items-center">
                            <span className="font-black text-slate-800 text-lg">{meta.nome}</span>
                            <span className="text-xs font-bold text-slate-400">€ {meta.atual} / € {meta.total}</span>
                          </div>
                          <div className="w-full h-3 bg-slate-100 rounded-full overflow-hidden">
                            <div 
                              style={{ 
                                width: `${perc}%`, 
                                backgroundColor: meta.cor === '#E51D3E' ? themeColor : meta.cor 
                              }}
                              className="h-full rounded-full transition-all duration-300" 
                            />
                          </div>
                        </div>
                      );
                    })}
                  </div>
                </div>
              )}

              {/* TAB: DEFINIÇÕES (Com Perfil de Utilizador e Seletor de Cores) */}
              {activeTab === 'definicoes' && (
                <div className="space-y-6">
                  
                  {activeSubScreen === 'menu' && (
                    <>
                      <h2 className="text-2xl font-black text-slate-800">Definições</h2>
                      
                      {/* OPÇÃO DE COR DO PROTÓTIPO */}
                      <div className="bg-white p-6 rounded-3xl border border-slate-100 space-y-4">
                        <h3 className="text-xs font-black text-slate-400 uppercase tracking-widest">Seletor de Cor de Tema</h3>
                        <div className="flex gap-3 justify-between">
                          {[
                            { color: '#E51D3E', label: 'Carmesim' },
                            { color: '#2563EB', label: 'Azul' },
                            { color: '#10B981', label: 'Verde' },
                            { color: '#F59E0B', label: 'Âmbar' },
                            { color: '#8B5CF6', label: 'Roxo' }
                          ].map(item => (
                            <button 
                              key={item.color} 
                              onClick={() => { setThemeColor(item.color); showToast(`Tema atualizado!`, "success"); }}
                              style={{ backgroundColor: item.color }}
                              className={`w-10 h-10 rounded-full border-4 shadow-lg transition-all transform hover:scale-110 active:scale-90 ${
                                themeColor === item.color ? 'border-slate-800 scale-105' : 'border-white'
                              }`}
                              title={item.label}
                            />
                          ))}
                        </div>
                      </div>

                      {/* Lista de Configurações */}
                      <div className="bg-white rounded-3xl border border-slate-100 overflow-hidden font-bold text-sm text-slate-700">
                        <div 
                          onClick={() => {
                            setEditNome(loggedUser?.name || userName);
                            setEditEmail(loggedUser?.email || userEmail);
                            setActiveSubScreen('perfil');
                          }}
                          className="p-5 flex justify-between items-center hover:bg-slate-50 cursor-pointer transition-colors"
                        >
                          <span className="flex items-center gap-2">👤 Perfil de Utilizador</span>
                          <ChevronRight size={16} />
                        </div>
                      </div>

                      <button 
                        onClick={() => { setCurrentScreen('login'); setLoginError(false); showToast("Sessão Terminada", "error"); }}
                        className="w-full py-4 bg-red-50 hover:bg-red-100 text-[#E51D3E] font-black rounded-2xl border border-red-100 transition-all text-sm"
                      >
                        Terminar Sessão
                      </button>
                    </>
                  )}

                  {/* SUB-TELA: EDITAR PERFIL */}
                  {activeSubScreen === 'perfil' && (
                    <div className="space-y-6">
                      <div className="flex items-center gap-2">
                        <button onClick={() => setActiveSubScreen('menu')} className="text-slate-400 hover:text-slate-700 p-1">
                          <ArrowLeft size={20} />
                        </button>
                        <h2 className="text-2xl font-black text-slate-800">Editar Perfil</h2>
                      </div>

                      <div className="bg-white p-6 rounded-3xl border border-slate-100 space-y-6 shadow-sm">
                        <div className="relative w-24 h-24 mx-auto">
                          <div 
                            style={{ backgroundColor: themeColor }}
                            className="w-24 h-24 rounded-full flex items-center justify-center text-white text-3xl font-black shadow-lg"
                          >
                            {editNome.charAt(0)}
                          </div>
                          <div className="absolute bottom-0 right-0 p-2 bg-slate-900 text-white rounded-full border-2 border-white cursor-pointer hover:scale-105 transition-transform">
                            <Camera size={14} />
                          </div>
                        </div>

                        <form onSubmit={handleSaveProfile} className="space-y-4">
                          <div className="space-y-1">
                            <label className="text-[10px] font-black text-slate-400 uppercase">Nome Completo</label>
                            <input 
                              type="text" 
                              required 
                              value={editNome} 
                              onChange={e => setEditNome(e.target.value)} 
                              className="w-full p-3.5 bg-slate-50 border border-slate-200 rounded-xl outline-none font-bold text-slate-800 focus:border-red-500"
                            />
                          </div>

                          <div className="space-y-1">
                            <label className="text-[10px] font-black text-slate-400 uppercase">E-mail</label>
                            <input 
                              type="email" 
                              required 
                              value={editEmail} 
                              onChange={e => setEditEmail(e.target.value)} 
                              className="w-full p-3.5 bg-slate-50 border border-slate-200 rounded-xl outline-none font-bold text-slate-800 focus:border-red-500"
                            />
                          </div>

                          <div className="space-y-1">
                            <label className="text-[10px] font-black text-slate-400 uppercase">Telemóvel</label>
                            <input 
                              type="text" 
                              required 
                              value={editPhone} 
                              onChange={e => setEditPhone(e.target.value)} 
                              className="w-full p-3.5 bg-slate-50 border border-slate-200 rounded-xl outline-none font-bold text-slate-800 focus:border-red-500"
                            />
                          </div>

                          <button 
                            type="submit"
                            style={{ backgroundColor: themeColor }}
                            className="w-full py-4 text-white font-black rounded-2xl shadow-lg mt-2 transition-transform active:scale-95"
                          >
                            Guardar Perfil
                          </button>
                        </form>
                      </div>
                    </div>
                  )}

                </div>
              )}

            </div>

            {/* TAB BAR DO DISPOSITIVO */}
            <div className="absolute bottom-0 left-0 right-0 bg-white border-t border-slate-100 p-6 flex justify-between items-center z-40">
              <button onClick={() => { setActiveTab('dashboard'); setActiveSubScreen('menu'); }} style={{ color: activeTab === 'dashboard' ? themeColor : '#94A3B8' }} className="p-2"><LayoutDashboard /></button>
              <button onClick={() => { setActiveTab('gastos'); setActiveSubScreen('menu'); }} style={{ color: activeTab === 'gastos' ? themeColor : '#94A3B8' }} className="p-2"><TrendingUp /></button>
              
              {/* Botão de Adicionar Flutuante (+) que abre Bottom Sheet */}
              <button 
                onClick={() => setShowNovoLancamento(true)}
                style={{ backgroundColor: themeColor }}
                className="w-14 h-14 text-white rounded-full flex items-center justify-center shadow-lg -mt-10 border-4 border-white active:scale-95 transition-transform"
              >
                <Plus size={28}/>
              </button>

              <button onClick={() => { setActiveTab('metas'); setActiveSubScreen('menu'); }} style={{ color: activeTab === 'metas' ? themeColor : '#94A3B8' }} className="p-2"><Target /></button>
              <button onClick={() => { setActiveTab('definicoes'); setActiveSubScreen('menu'); }} style={{ color: activeTab === 'definicoes' ? themeColor : '#94A3B8' }} className="p-2"><Sliders /></button>
            </div>

          </div>
        )}

        {/* --- MODAL BOTTOM SHEET: NOVO LANÇAMENTO (GASTOS E LUCROS COM ESCOLHA DE CATEGORIA) --- */}
        {showNovoLancamento && (
          <div className="absolute inset-0 bg-slate-900/40 backdrop-blur-xs z-50 flex flex-col justify-end">
            <div className="bg-white rounded-t-[2.5rem] p-8 space-y-6 shadow-2xl border-t border-slate-100">
              <div className="flex justify-between items-center">
                <h3 className="text-xl font-black text-slate-800">Novo Lançamento</h3>
                <button onClick={() => setShowNovoLancamento(false)} className="p-2 bg-slate-100 rounded-full text-slate-500"><X size={16}/></button>
              </div>

              <form onSubmit={handleAddTransaction} className="space-y-4">
                <div className="space-y-1">
                  <label className="text-[10px] font-black text-slate-400 uppercase ml-1">Valor (€)</label>
                  <input 
                    name="valor" 
                    type="number" 
                    step="0.01" 
                    required 
                    value={novoValor} 
                    onChange={e => setNovoValor(e.target.value)}
                    placeholder="€ 0,00" 
                    className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-black text-slate-800 text-lg" 
                  />
                </div>

                <div className="space-y-1">
                  <label className="text-[10px] font-black text-slate-400 uppercase ml-1">Descrição</label>
                  <input 
                    name="descricao" 
                    required 
                    value={novaDescricao} 
                    onChange={e => setNovaDescricao(e.target.value)}
                    placeholder="Ex: Almoço de Trabalho" 
                    className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800" 
                  />
                </div>

                <div className="grid grid-cols-2 gap-4">
                  <div className="space-y-1">
                    <label className="text-[10px] font-black text-slate-400 uppercase ml-1">Tipo</label>
                    <select 
                      value={tipoTransacao} 
                      onChange={e => setTipoTransacao(e.target.value)}
                      className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800 appearance-none"
                    >
                      <option value="despesa">Despesa (Gasto)</option>
                      <option value="receita">Receita (Lucro)</option>
                    </select>
                  </div>
                  
                  <div className="space-y-1">
                    <label className="text-[10px] font-black text-slate-400 uppercase ml-1">Categoria de Gasto</label>
                    {tipoTransacao === 'despesa' ? (
                      <select 
                        value={categoriaTransacao} 
                        onChange={e => setCategoriaTransacao(e.target.value)}
                        className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800 appearance-none"
                      >
                        <option value="Alimentação">Alimentação</option>
                        <option value="Transporte">Transportes</option>
                        <option value="Lazer">Lazer e Viagens</option>
                      </select>
                    ) : (
                      <div className="w-full p-4 bg-slate-100 text-slate-400 border border-slate-200 rounded-2xl font-bold text-sm">
                        Não Aplicável
                      </div>
                    )}
                  </div>
                </div>

                <button 
                  type="submit" 
                  style={{ backgroundColor: themeColor }}
                  className="w-full py-4 text-white font-black rounded-2xl shadow-lg mt-2 transition-transform active:scale-95"
                >
                  Confirmar Lançamento
                </button>
              </form>
            </div>
          </div>
        )}

        {/* --- MODAL BOTTOM SHEET: NOVA META --- */}
        {showNovaMeta && (
          <div className="absolute inset-0 bg-slate-900/40 backdrop-blur-xs z-50 flex flex-col justify-end">
            <div className="bg-white rounded-t-[2.5rem] p-8 space-y-6 shadow-2xl border-t border-slate-100">
              <div className="flex justify-between items-center">
                <h3 className="text-xl font-black text-slate-800">Nova Meta</h3>
                <button onClick={() => setShowNovaMeta(false)} className="p-2 bg-slate-100 rounded-full text-slate-500"><X size={16}/></button>
              </div>

              <form onSubmit={handleCreateGoal} className="space-y-4">
                <div className="space-y-1">
                  <label className="text-[10px] font-black text-slate-400 uppercase ml-1">Nome da Meta</label>
                  <input 
                    name="metaNome" 
                    required 
                    value={metaNome} 
                    onChange={e => setMetaNome(e.target.value)}
                    placeholder="Ex: Carta de Condução" 
                    className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800" 
                  />
                </div>

                <div className="space-y-1">
                  <label className="text-[10px] font-black text-slate-400 uppercase ml-1">Objetivo Total (€)</label>
                  <input 
                    name="metaTotal" 
                    type="number" 
                    required 
                    value={metaTotal} 
                    onChange={e => setMetaTotal(e.target.value)}
                    placeholder="1000.00" 
                    className="w-full p-4 bg-slate-50 border border-slate-100 rounded-2xl outline-none font-bold text-slate-800" 
                  />
                </div>

                <button 
                  type="submit" 
                  style={{ backgroundColor: themeColor }}
                  className="w-full py-4 text-white font-black rounded-2xl shadow-lg mt-2"
                >
                  Criar Objetivo
                </button>
              </form>
            </div>
          </div>
        )}

      </div>
    </div>
  );
}

```

obs: o codigo foi corrigido pela ia porem feito por     mim 
feito para financas 
