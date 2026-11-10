# Passagem de contexto: Jornada do Cabaneiro (atualizado em 11/11/2026)

Documento para uma nova conversa (ou outro Claude) continuar o trabalho sem perder nada. Leia inteiro antes de mexer. Complementa os documentos do projeto `plataforma-cabana-school-e-jornada.md` (mapa geral, parcialmente desatualizado) e `contrato-curso-html.md` (histórico até outubro). Quando houver conflito, **vale este documento**.

---

## 1. Quem pede e como trabalhar

- **Nayuri**, DHO (Desenvolvimento Humano Organizacional) do Cabana Burger, rede com cerca de 27 lojas em SP e RJ.
- Escreve em português do Brasil, informal, às vezes por voz (com erros de transcrição: interprete pelo contexto).
- **Preferências fixas:**
  - Sem travessão (o traço longo) em nenhum texto entregue.
  - Sem "cara de IA": frases diretas, sem floreio, sem listas de efeito.
  - Sem caixa alta nas peças (acha agressivo). Rótulos em letra normal.
  - Sempre "o Cabana" (masculino), nunca "a Cabana".
  - Gosta que você faça perguntas de detalhamento quando houver decisão dela a tomar (de preferência com botões de escolha).
  - Peças visuais seguem o design system **Cabana Digital** (artefato `https://claude.ai/artifact/2QQT3zTyqwnVsB3wvSc1Le`): ler o README e o Guia para IA, partir do modelo certo e rodar o verificador.
  - Valida conceito e desenho antes da versão final. Mostre imagem (captura) antes de dizer que está pronto.
  - Não gosta de conteúdo escondido (abas e sanfonas) em cursos e de texto longo.
- **Sempre teste antes de entregar** (sintaxe com `node --check` em cada bloco de script e captura com Playwright) e diga com honestidade o que não deu para testar (ex.: Firebase de verdade).

## 2. O que é a Jornada

Plataforma de onboarding e acompanhamento dos primeiros 90 dias. É o Cabana School original rodando com outra "marca" (perfil `jornadaCD`, visual do design system: Geist, verde #00630F, laranja #FF5D31 só em detalhes, esquilo em momentos de celebração).

**Três arquivos HTML grandes** (CSS e JS dentro, imagens em base64):
- `index.html`: login (CPF + senha; senha inicial = 6 últimos dígitos do CPF).
- `home.html`: área do colaborador, do gerente, do coordenador e do gerente de operações. Linha `window.MARCA_ATIVA = 'jornadaCD'` (trocar para `'jornadaDS'` volta ao visual anterior).
- `admin.html`: painel do DHO (só Admin).

**Onde mora:**
- GitHub, organização `cabanaburger`, conta `falaecabana@gmail.com` (senha nunca vai em documento).
  - `jornada-do-cabaneiro` → `cabanaburger.github.io/jornada-do-cabaneiro/` (a Jornada de verdade: index, home, admin, curso.html, tela-conclusao-trilha.html, manifest-jornada.json, ícones/PNG, pasta `assets`).
  - `demo-jornadadocabaneiro` → `cabanaburger.github.io/demo-jornadadocabaneiro/` (demonstração com Firebase falso e dados inventados; um arquivo `index.html`).
  - `cabana-school` (original): hospeda `assets/cabana-quiz-tracker.js`, que os cursos HTML usam para registrar conclusão (`window.CabanaSchool.reportResult(score,total)`). Não apagar.
- Firebase, projeto `cabana-school-c` (Auth com e-mail `{cpf}@cabana.internal`, Firestore, Storage dos cursos HTML, Functions: `resetSenhaColaborador`, `excluirColaborador`, `confirmarPresencaEncontro`, `marcarPresencaManual`).

**Publicar:** GitHub → repositório → Add file → Upload files → arrastar o arquivo com o mesmo nome → Commit changes. **Nunca pelo lápis/renomear do GitHub** (arquivo grande vira vazio). Depois esperar ~3 min e abrir com Ctrl+Shift+R (cache). Para conferir se subiu, clonar o repositório público e comparar com o arquivo entregue (o GitHub às vezes grava com CRLF; comparar ignorando `\r`).

**Estado em 10/11/2026:** login, home e admin publicados nas versões mais novas, exceto o último `admin.html` (botão "Baixar a base atual" e "outros nomes" de cargo) e o `home.html` da carta nova, que estavam para ser subidos. Ainda com título antigo no GitHub: `curso.html`, `tela-conclusao-trilha.html`, `assets/curso-qualidade_2.html`. Demo no GitHub está desatualizada.

## 3. Regras de negócio (o que não pode quebrar)

**Elegibilidade e início**
- Só quem está nos **primeiros 90 dias de casa** (pela admissão) gera pendência/cobrança: trilhas, checklist, ponto de 30 dias, pesquisas. Pessoa antiga ou sem data de admissão aparece na equipe, sem cobrança.
- **Início da Jornada = 11/11/2026** (dia do treinamento dos gerentes). Configurável em `configuracoes/jornada.Inicio` (painel: Configurações → Início da Jornada nas lojas). Padrão no código: `'2026-11-11'`.
- Para quem foi admitido antes do início:
  - Checklist: só contam as etapas cujo prazo (pela admissão) ainda não tinha vencido no início **e** se a pessoa tinha até 30 dias de casa no início. Funções: `diasAntesInicio(c)`, regra `diasAntesInicio(c) <= Math.min(b.prazo, 30)`.
  - Feedbacks: o de 15 só se a pessoa tinha até 15 dias de casa no início; o de 45, até 45 dias.
  - Trilhas: os 7 dias contam a partir do início (`diasPrazo(c)` = dias desde max(admissão, início)).
- Antes do início nada aparece como vencido no painel de indicadores (rankings mostram trilhas concluídas por loja).

**Trilhas**
- Cada pessoa vê as trilhas publicadas do seu público (cargo/macrocargo, loja/regional, tipo de local, CLT/PJ). Admin vê todas, com o público de cada uma ("Para: Loja").
- Trilha atrasada = pendente depois de 7 dias (contados como acima), ou vencida/reprovada, e só dentro dos 90 dias.

**Checklist de integração (gerente marca; 17 itens; `CHECKLIST_BLOCOS` no home)**
- Antes de chegar (prazo dia 1): a1 Orientador e escala; a2 1º dia na agenda; a3 Uniforme e armário; a4 Cadastro conferido (Apontatu); a5 Time avisado; a6 Mensagem de confirmação.
- 1º dia (prazo 3): b1 Boas-vindas e tour; b2 Uniforme, ponto, escala e canais; **b3 Trilhas concluídas (automático)**; b4 Conversa de dúvidas; b5 Código de Conduta; b6 Ficha de EPI; b7 Sombra, almoço e cardápio.
- Primeira semana (prazo 8): c1 Feedback por turno; c2 Fechamento da semana.
- 15 dias (bloco `d30`, prazo 25): d1 Conversa de 15 dias; **d2 Colaborador respondeu "Seus primeiros 15 dias" (automático; conta no total, nunca deixa atrasado)**.
- 45 dias (bloco `d45`, prazo 55): e1 Conversa de 45 dias.
- Total: 18 itens.
- Vale de 15 dias antes da admissão até o dia 90. Só o gestor direto (`CpfGestor`) marca. Liderança (cargo começa com "Coordenador" ou "Gerente") não tem checklist.
- Dados: `checklistIntegracao/{cpf}.Itens.{id} = {feito, em (ISO), por}`.

**Feedbacks (gerente) nos dias 15 e 45, e "Seus primeiros 15 dias" (colaborador)** (mudou em 11/11/2026; antes era um "feedback" só, no dia 30. Na tela o nome agora é **Feedback**; no código e nas coleções continuam os nomes antigos: `checkins30`, `checkins45`, `abrirCheckin30`)
- Gerente: **dois feedbacks com as mesmas perguntas**, para comparar a evolução: dia 15 (prazo até o dia 25) e dia 45 (prazo até o dia 55). Valem para **loja, escritório e fábrica**, até o dia 90. Liberam no dia e ficam "pendentes" até o registro; depois do prazo aparecem como atrasados.
- Dia 15 grava em `checkins30/{cpf}` (nome da coleção mantido por compatibilidade); dia 45 em `checkins45/{cpf}`. Ambos com `Marco` (15 ou 45), faróis `Integracao`, `Treinamento` (= aprendizado), `Comportamento`, `Potencial`, comentários obrigatórios em amarelo/vermelho, `DataConversa`, `ResumoFeedback`, `ProximosPassos`.
- No código da home: `MARCOS_PONTO = [15, 45]`, `PRAZO_PONTO = 10`, `statusPontoM(c, m)`, `colPonto(m)`, `cachePonto(m)`; o formulário é um só (`abrirCheckin30(cpf, marco)`).
- Colaborador: **só a versão dos primeiros 15 dias** ("Seus primeiros 15 dias no Cabana"), do dia 15 ao 44. No dia 45 o colaborador responde apenas a pesquisa de 45 dias (formulário), que já existia. Pop-up 1x por dia (localStorage `jornada-1mes-{cpf}`), não abre se outro aviso estiver na tela, com uma pergunta por vez (abertura com o esquilo, 4 perguntas com barra de progresso, comentário opcional e tela de agradecimento; funções `pmDesenhar`, `pmIr`, `pmEscolher`). Também aparece como passo no cartão "Sua chegada", logo depois das trilhas (libera em X dias / Responder / Respondido). 3 faróis (Chegada, Seguranca, Tratamento), Futuro (sim/talvez/nao), Comentario. Grava `checkins30Colab/{cpf}` e marca `colaboradores/{cpf}.Checkin30ColabEm`. **Leem as respostas: o DHO, o Coordenador de Operações e o Gerente de Operações. O gerente da loja não lê**; ele vê apenas que a pessoa respondeu. O texto do pop-up avisa isso ao colaborador.

**Pesquisas (pop-up com Responder, Já respondi, Fechar)**
- 20 dias: só lojas marcadas como novas (`configuracoes/pesquisas.LojasPesquisa20` + `Link20`), janela 20 a 44. Não muda fase.
- 45 dias (45 a 89) e 90 dias (90 a 134): links `Link45`, `Link90`. "Já respondi" grava `Pesquisa{N}Respondida` (autodeclarado; a pesquisa é confidencial).

**Fases da Jornada:** 1 Gente Boa chegou; 2 Gente Boa na rotina (concluiu as trilhas); 3 Já faz parte (respondeu a de 45); 4 Virou Cabaneiro (respondeu a de 90 → certificado de boas-vindas). Campo `FaseJornada`.

**Parecer de desligamento**
- Para **toda saída de loja a partir do início da Jornada (11/11)**, qualquer tempo de casa. Pop-up ao gerente + lista "Saíram do time: falta o parecer" em Minha equipe e Pendências. `desligamentos/{cpf}` com Motivo, Detalhamento (sempre obrigatório), Iniciativa, TempoDeCasaDias. Leem o conteúdo: o DHO, o Coordenador de Operações e o Gerente de Operações; o gerente da loja não vê o próprio parecer depois de enviado.

**Telas de gestão**
- Gerente de loja: Minha equipe (5 números: pessoas, em integração, checklist atrasado, trilha atrasada, feedbacks; uma linha por pessoa com checklist, trilhas e "Conversas de acompanhamento" com o ponto de 15 e o de 45, cada um com o botão de preencher) e Pendências (bloco "Feedbacks · 15 e 45 dias").
- Coordenador de Operações (Minha regional) e Gerente de Operações (Minhas regionais): os números da integração por gerente/loja e, pelo botão **Ver feedbacks e pareceres** (desde 11/11/2026), o **conteúdo** dos feedbacks de 15 e 45 dias, da versão do colaborador e dos pareceres de desligamento das suas regionais, com filtros de regional, loja e nome (`abrirFeedbacksRegional`). Cargos exatos: `COORDENADOR DE OPERAÇÕES`, `GERENTE DE OPERAÇÕES`. A regra do Firebase libera a leitura por `ehVisaoRegional()` (não restringe por regional no servidor; a tela filtra).
- Link "Trilha de admissão" (`https://cabana-rh.bi-253.workers.dev/`) no menu do topo, ao lado de Sobre (e no menu do avatar no celular), só para: Admin, Diretoria contendo "recursos humanos", gerente/coordenador de loja (Local = Loja), cargos "de operações" ou "regional".

**Carta de boas-vindas (home, até 180 dias):** montada por pessoa (`montarCartaCD`): papel bege, selo, nome em Fraunces laranja, esquilo, 3 pilares com ícone, cartão verde com loja/gestor/chegada, progresso das trilhas, dicas por local (`CARTA_DICAS`: Loja, Cozinha Central, Casa Cabana; as do escritório e da fábrica ainda precisam ser validadas pela Nayuri), assinatura "Com carinho, time Cabana" e lettering.

**Certificados:** desenhados no navegador (`window.cdCertificado`), modelo "D": moldura verde, logotipo bege, cartão branco, "CERTIFICADO" + "de conclusão" (Fraunces laranja), nome em verde, trilha numa pílula Limonada, esquilo pequeno. Tipos: trilha, encontro, final de programa, Virou Cabaneiro.

## 4. Painel do DHO (admin.html)

- **Colaboradores:** busca por nome, CPF, loja, cargo ou gestor; filtros status (abre só ativos), loja, regional, local; ordem alfabética; contagem.
- **Sincronizar base** (Colaboradores → ⚙️ Importar / sincronizar dados):
  - "Só adicionar e atualizar" (planilha parcial não desativa ninguém). Sem a marcação, quem não está na planilha vira inativo.
  - Botões: Baixar modelo da planilha; **Baixar a base atual** (mesmo formato, CPF como texto); Gerar arquivo de acessos (carga grande via `firebase auth:import`, opcional).
  - Grava em lotes de 400 (limite de 500 do Firebase).
  - Contas de login criadas pelo navegador: limite de ~100 por hora por conexão. Faltantes ficam em "Acessos que faltaram criar" (localStorage `jornada-acessos-pendentes`) com botão para tentar depois.
  - CPF com 9 ou 10 dígitos ganha zero à esquerda.
  - Cargo e loja: reconhecidos sem ligar para maiúscula, acento, ponto e espaço; cargo também pelos **outros nomes** cadastrados (`cargos/{id}.Variacoes`), gravando o nome oficial.
  - Nova pessoa ganha `PrecisaTrocarSenha: true`.
- **Cadastro manual:** cargo com busca digitando e lista em ordem alfabética.
- **Pesquisas:** links de 20/45/90 e lojas novas (botões).
- **Relatórios & adesão:** Colaboradores (ordem alfabética, busca, filtros de situação e de trilhas), Pareceres, **Feedbacks** (15 e 45 dias juntos, com a etiqueta do dia em cada registro; lista de aguardando com atraso; exportação com a coluna "Ponto (dias)"), **15 dias · colaborador** (lado a lado com o ponto de 15 do gerente, destaca divergências, exporta).
- **Cargos:** em Colaboradores → ⚙️ Importar / sincronizar dados → Importar cargos: botões "⬇ Modelo" (planilha com Cargo, Macrocargo e Outros nomes, já com os cargos cadastrados) e "Ver cargos e outros nomes" (janela Cargos & macrocargos). A importação acrescenta outros nomes aos cargos existentes e cria os novos; não apaga nada.
- **Indicadores da Jornada** (menu Dados, formato Power BI, 4 páginas): Visão geral, Integração, Desligamentos e tendência, **Detalhe por loja** (situação Em dia / Fora do prazo / Dentro do prazo; trilhas no prazo, fora, a fazer, atrasadas; checklist; feedbacks; pareceres; clique abre cada pessoa). Índice da loja = média com mesmo peso de checklist no prazo, trilhas até o 7º dia, feedbacks registrados (cada ponto de 15 ou 45 dias com prazo vencido conta uma vez) e parecer em dia, **só com itens já vencidos**; loja em conformidade se índice ≥ 80%. Nomes de loja escritos de jeitos diferentes são agrupados e padronizados. Código em `indicadores.js` (dentro do admin, procure "INDICADORES DA JORNADA").
- **Configurações:** Início da Jornada nas lojas.

## 5. Firestore: coleções principais

`colaboradores/{cpf}` (Nome, Apelido, Loja, Regional, Cargo, Macrocargo, Nivel, Local, Status, InativadoEm, Admissao, Gestor, CargoGestor, CpfGestor, TipoContratacao, Diretoria, FaseJornada, PrimeiroAcesso, Pesquisa{20,45,90}{Fechada,Respondida}, Checkin30ColabEm, PrecisaTrocarSenha), `cursos`, `progresso/{cpf}_{cursoId}` (Status `concluido`, ConcluidoEm), `checklistIntegracao`, `checkins30` (ponto do dia 15), `checkins45` (ponto do dia 45), `checkins30Colab` (primeiros 15 dias do colaborador), `desligamentos`, `configuracoes/{pesquisas, jornada, loja, identidade}`, `cargos` (Nome, Macrocargo, Variacoes), `lojas` (Nome, Regional), `publicos`, `trilhas`, `programas`, `codigosPrograma`, `presencasPrograma`, `biblioteca`, `banners`.

**Regras do Firebase (texto completo; a versão abaixo inclui o bloco `checkins45` e a leitura regional de `desligamentos` e `checkins30Colab`, que precisam ser publicados junto com as mudanças de 11/11/2026):**

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // ===== NOVO (out/2026): funções de apoio para as visões de Coordenador e Gerente de Operações =====
    function eu() { return request.auth.token.email.split('@')[0]; }
    function euDoc() { return get(/databases/$(database)/documents/colaboradores/$(eu())).data; }
    function ehVisaoRegional() {
      return euDoc().Cargo in ['COORDENADOR DE OPERAÇÕES', 'GERENTE DE OPERAÇÕES'];
    }

    match /colaboradores/{userId} {
      // NOVO: Gerente de Operações lê todos; Coordenador de Operações lê quem é da mesma Regional dele.
      allow read: if request.auth != null &&
        (request.auth.token.email.split('@')[0] == userId ||
         resource.data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin" ||
         euDoc().Cargo == 'GERENTE DE OPERAÇÕES' ||
         (euDoc().Cargo == 'COORDENADOR DE OPERAÇÕES' && resource.data.Regional == euDoc().Regional));
      allow write: if request.auth != null &&
        (request.auth.token.email.split('@')[0] == userId ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
    }
    match /progresso/{docId} {
      // NOVO: "|| ehVisaoRegional()" no fim da leitura (coordenador e gerente de operações somam trilhas por lista de CPFs).
      allow read: if request.auth != null &&
        (request.auth.token.firebase.sign_in_provider == 'anonymous' ||
         request.auth.token.email.split('@')[0] == resource.data.Cpf ||
         get(/databases/$(database)/documents/colaboradores/$(resource.data.Cpf)).data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin" ||
         ehVisaoRegional());
      allow write: if request.auth != null &&
        (request.auth.token.firebase.sign_in_provider == 'anonymous' ||
         request.auth.token.email.split('@')[0] == request.resource.data.Cpf ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
    }
    match /cursos/{cursoId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /biblioteca/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /categorias/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /categoriasBiblioteca/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /publicos/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /lojas/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /cargos/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /configuracoes/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /rankingPublico/{uid} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        (request.auth.uid == uid ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
    }
    match /itensLoja/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /resgates/{docId} {
      allow read: if request.auth != null &&
        (request.auth.token.email.split('@')[0] == resource.data.Cpf ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
      allow create: if request.auth != null &&
        request.auth.token.email.split('@')[0] == request.resource.data.Cpf;
      allow update, delete: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /trilhas/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    match /banners/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    // desligamentos e checkins30: doc id = CPF do colaborador. Só o gestor direto dessa pessoa
    // (CpfGestor no cadastro dela) ou um Admin pode ler/escrever — mesma regra usada em
    // progresso pra "painel do gestor" (jornada do cabaneiro, setembro/2026).
    // O parecer de desligamento continua só do gestor direto e do Admin (DHO).
    match /desligamentos/{cpf} {
      // NOVO (nov/2026): coordenador e gerente de operações também leem os pareceres ("|| ehVisaoRegional()").
      allow read: if request.auth != null &&
        (get(/databases/$(database)/documents/colaboradores/$(cpf)).data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin" ||
         ehVisaoRegional());
      allow write: if request.auth != null &&
        (get(/databases/$(database)/documents/colaboradores/$(cpf)).data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
    }
    match /checkins30/{cpf} {
      // NOVO: "|| ehVisaoRegional()" na leitura, só para o coordenador e o gerente de operações verem
      // QUANTOS pontos de checagem estão pendentes. A tela deles mostra apenas números, nunca o conteúdo.
      allow read: if request.auth != null &&
        (get(/databases/$(database)/documents/colaboradores/$(cpf)).data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin" ||
         ehVisaoRegional());
      allow write: if request.auth != null &&
        (get(/databases/$(database)/documents/colaboradores/$(cpf)).data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
    }
    // ===== NOVO (nov/2026): ponto de checagem do dia 45 (o do dia 15 continua em checkins30). Mesma regra do checkins30.
    match /checkins45/{cpf} {
      allow read: if request.auth != null &&
        (get(/databases/$(database)/documents/colaboradores/$(cpf)).data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin" ||
         ehVisaoRegional());
      allow write: if request.auth != null &&
        (get(/databases/$(database)/documents/colaboradores/$(cpf)).data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
    }
    // ===== NOVO (out/2026): checklist de integração por pessoa. doc id = CPF do colaborador.
    // Leitura: qualquer pessoa logada (a Home busca por lista de CPFs e o coordenador/gerente de operações
    // somam os números). Escrita: só o gestor direto da pessoa ou um Admin.
    match /checklistIntegracao/{cpf} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        (get(/databases/$(database)/documents/colaboradores/$(cpf)).data.CpfGestor == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
    }
    // ===== NOVO (nov/2026): "Seu 1º mês", o que a própria pessoa responde por volta do dia 30.
    // doc id = CPF da pessoa. Só a própria pessoa grava (e só no próprio CPF). Leem: ela, o Admin (DHO) e a liderança regional.
    // O gerente da loja NÃO lê: ele só vê que a pessoa respondeu, pelo campo Checkin30ColabEm no cadastro dela.
    match /checkins30Colab/{cpf} {
      // NOVO (nov/2026): coordenador e gerente de operações também leem ("|| ehVisaoRegional()"). O gerente da loja continua sem acesso.
      allow read: if request.auth != null &&
        (request.auth.token.email.split('@')[0] == cpf ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin" ||
         ehVisaoRegional());
      allow create, update: if request.auth != null &&
        request.auth.token.email.split('@')[0] == cpf &&
        request.resource.data.Cpf == cpf;
      allow delete: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    // programas presenciais (treinamento em sala, com encontros/presença/certificado — setembro/2026):
    // leitura liberada igual curso/trilha (é o que faz a "trilha" aparecer na Home de quem for
    // público-alvo); escrita só Admin, porque quem cria/edita programa e encontro é sempre o
    // admin.html.
    match /programas/{docId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    // codigosPrograma/{programaId}_{encontroId}: guarda o código secreto de cada encontro separado
    // do doc de programas de propósito — se ficasse dentro de "programas" (que todo mundo lê pra
    // montar a tela), o código apareceria pro navegador de qualquer colaborador mesmo sem mostrar
    // na tela (bastaria abrir o F12). Aqui só quem está marcado como facilitador daquele encontro
    // (FacilitadorCpf) ou um Admin consegue ler; só Admin escreve (cria/regenera pelo admin.html).
    match /codigosPrograma/{docId} {
      allow read: if request.auth != null &&
        (resource.data.FacilitadorCpf == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
      allow write: if request.auth != null &&
        get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin";
    }
    // presencasPrograma/{programaId}_{encontroId}_{cpf}: registro de presença confirmada. Leitura:
    // a própria pessoa, o facilitador daquele encontro, ou Admin. Escrita: NUNCA direto do app —
    // "allow write: if false" força toda confirmação (por código ou manual) a passar pelas Cloud
    // Functions confirmarPresencaEncontro/marcarPresencaManual, que usam o Admin SDK (que ignora
    // essas regras) depois de validar de verdade quem está confirmando. É a trava principal contra
    // alguém marcar presença por fora sem ter ido no encontro.
    match /presencasPrograma/{docId} {
      allow read: if request.auth != null &&
        (resource.data.Cpf == request.auth.token.email.split('@')[0] ||
         resource.data.FacilitadorCpf == request.auth.token.email.split('@')[0] ||
         get(/databases/$(database)/documents/colaboradores/$(request.auth.token.email.split('@')[0])).data.Nivel == "Admin");
      allow write: if false;
    }
  }
}
```

## 6. Como editar os arquivos

- Os HTML têm ~2 MB por causa das imagens em base64. Não leia inteiro: troque cada `data:...;base64,...` por um marcador numa cópia, use grep e leia por faixas de linha; aplique a edição no original (as linhas não mudam).
- **A fonte de verdade é o arquivo publicado no GitHub.** As mudanças desta fase foram feitas por scripts de patch (`patch_cd.py` sobre a home base, `patch_admin.py` sobre o admin, `indicadores.js`, `cd.css`, `patch_demo.py`), que dependem dos arquivos base desta conversa. Numa conversa nova, edite direto o `home.html`/`admin.html` atuais (peça para a Nayuri anexar, ou clone o repositório público).
- Teste sem login real: Playwright + módulos do Firebase trocados por versões falsas que leem um banco em memória (ver `harness-capturas-home.md` no projeto). Para o admin, além do Firebase falso, sirva o `xlsx.full.min.js` (cdnjs) localmente.
- Sempre: `node --check` em cada bloco de script, captura de tela no computador e no celular (390 px), e conferir que trocar `MARCA_ATIVA` para `'jornadaDS'` não quebra.

## 7. Materiais entregues

- Guia de quem cobre as férias (Word/PDF, 4 páginas, atualizado em 09/11).
- GitHub e Firebase da Jornada (Word/PDF, 4 páginas).
- PowerPoint do treinamento dos gerentes (19 slides) e guia rápido do gerente: **desatualizados** (falam em 16 itens no checklist e em ponto de 30 dias; hoje são 18 itens e dois feedbacks, nos dias 15 e 45; também não mostram a lista de desligados, "Seus primeiros 15 dias" nem a carta). Pendente a Nayuri decidir o visual (Jornada ou versão de loja).
- Convite do treinamento 11/11 às 14h30, no Teams (PNG quadrado).
- Curso de Hospitalidade ajustado (guia de pendências no rodapé, cartões sem bege na frente, quiz com correção na hora e revisão, mínimo 80%, refaz inteiro; foto sem barba). Publicar pelo painel: Editar curso → arquivo → **Publicar curso** (só escolher o arquivo não salva).
- Curso de Benefícios: feito em outra conversa ("Atualização do treinamento de benefícios", 23/10), versões loja (`trilha_beneficios_cabana_22.html`) e escritório.

## 8. Pendências e decisões em aberto

- Publicar a regra do Firebase com o bloco `checkins45` **antes** de subir a home e o admin novos (senão o ponto de 45 dias não salva).
- Subir no GitHub: `admin.html` mais recente, `home.html` (carta nova), `curso.html`, `tela-conclusao-trilha.html`, `assets/curso-qualidade_2.html`, e o `index.html` da demo.
- 12 acessos que não foram criados no limite do Firebase (usar o botão do painel).
- Padronizar nomes de lojas na base (ex.: "Alphaville" e "ALPHAVILLE") e talvez criar "outros nomes" também para lojas (ofertado, sem resposta).
- Validar as dicas da carta para escritório e fábrica.
- Atualizar PowerPoint e guia rápido do gerente.
- Antes das férias da Nayuri: trocar a senha da conta e ativar verificação em duas etapas (com celular ou códigos de backup acessíveis a quem fica), conferir o cartão do Firebase (Functions dependem do plano pago), salvar uma cópia dos arquivos e das regras numa pasta do time, definir quem conduz o treinamento do dia 11 e quem responde por questões técnicas.
