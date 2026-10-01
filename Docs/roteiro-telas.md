# Roteiro de Telas — SaaS de Entradas e Saídas (MVP)

> **Versão 1** — 30/09/2026. Documento descritivo para orientar os protótipos no Figma. Complementa o `escopo-saas-entrada-saida.md` (versão 4).

> **Legenda**
> - **[definido]** — decidido no escopo.
> - **[proposto]** — sugestão a confirmar.

---

## 1. Premissa: usuário com baixo letramento tecnológico

Grande parte dos proprietários de pequenos comércios usa o celular principalmente para mensagens e redes sociais, tem pouca familiaridade com sistemas de gestão e pode ter dificuldade com termos técnicos, telas carregadas e ações escondidas. Muitos usam o sistema com pressa, em pé, no balcão, às vezes com uma só mão.

**Consequência para o projeto:** cada tela deve ser compreensível sem explicação, e todo erro deve ser fácil de desfazer.

---

## 2. Diretrizes de interface [proposto]

### 2.1 Linguagem

| Diretriz | Exemplo |
|---|---|
| Usar palavras do dia a dia, não termos técnicos ou contábeis | "Dinheiro que entrou" em vez de "Receita"; "Sobrou" em vez de "Saldo positivo" |
| Frases curtas, na voz ativa | "Digite o valor" em vez de "O campo valor deve ser preenchido" |
| Botões dizem exatamente o que fazem | "Salvar saída" em vez de "OK" ou "Enviar" |
| Mensagens de erro explicam como corrigir | "Digite um valor maior que zero" em vez de "Valor inválido" |
| Nunca exibir códigos ou termos de sistema | Nunca "Erro 500", "token", "sessão", "request" |
| Tratamento consistente | Sempre "você"; sem gírias; conforme a norma ortográfica vigente |

### 2.2 Vocabulário padronizado da interface

| Termo interno (escopo) | Texto na tela |
|---|---|
| Lançamento | Registro |
| Entrada | Entrada — "Dinheiro que entrou" |
| Saída | Saída — "Dinheiro que saiu" |
| Balanço positivo | Sobrou |
| Balanço negativo | Faltou |
| Categoria | Tipo de gasto / Tipo de entrada |
| Excluir | Apagar |
| Período | Mês, Trimestre, Semestre, Ano |

> Recomendação: validar esse vocabulário com 2 ou 3 comerciantes durante o teste dos protótipos.

### 2.3 Visual e toque

| Diretriz | Especificação |
|---|---|
| Área de toque | Mínimo de 48 × 48 px; botões principais com altura mínima de 56 px |
| Tamanho de texto | Corpo mínimo de 16 px; valores monetários em destaque (28 px ou mais) |
| Contraste | Mínimo WCAG AA (4,5:1 para texto) |
| Ícones | Sempre acompanhados de texto; nunca apenas o ícone |
| Cores de entrada e saída | Verde para entrada e vermelho para saída, **sempre** reforçados por ícone (seta para baixo/para cima) e texto, para atender pessoas com daltonismo |
| Uma ação principal por tela | Um único botão de destaque; ações secundárias com menor peso visual |
| Sem gestos escondidos | Nada que dependa apenas de deslizar, pressionar e segurar ou tocar duas vezes |
| Seleção por blocos | Categorias e formas de pagamento como botões grandes, não como listas suspensas (*dropdown*) |
| Teclado adequado | Valor abre o teclado numérico; e-mail abre o teclado de e-mail |
| Posição dos botões principais | Na parte inferior da tela, ao alcance do polegar |

### 2.4 Comportamento

| Diretriz | Especificação |
|---|---|
| Confirmação visível | Após salvar, mensagem clara: "Saída de R$ 50,00 salva" |
| Desfazer | Mensagem de confirmação com botão "Desfazer" por alguns segundos após salvar ou apagar |
| Confirmação antes de apagar | Sempre, repetindo o que será apagado |
| Proteção contra toque duplo | Botão "Salvar" desativado enquanto salva, evitando registros duplicados |
| Não perder o que foi digitado | Ao tentar sair de um formulário preenchido: "Você não salvou. Deseja sair mesmo assim?" |
| Sessão prolongada | Manter o usuário conectado no dispositivo por período longo (ex.: 30 dias), evitando logins frequentes; encerrar a sessão após troca de senha |
| Ajuda contextual | Pequenos textos de apoio abaixo dos campos, não ícones de interrogação |

### 2.5 Navegação

Barra inferior fixa com quatro itens, sempre com ícone e texto:

| Item | Destino |
|---|---|
| Início | T07 |
| Registros | T11 |
| Relatórios | T15 |
| Mais | T18 |

---

## 3. Mapa de telas

| Código | Tela | Grupo |
|---|---|---|
| T01 | Entrar | Acesso |
| T02 | Criar conta | Acesso |
| T03 | Confirme seu e-mail | Acesso |
| T04 | E-mail confirmado | Acesso |
| T05 | Esqueci minha senha | Acesso |
| T06 | Criar nova senha | Acesso |
| T07 | Início | Principal |
| T08 | Registrar saída | Registro |
| T09 | Registrar entrada | Registro |
| T10 | Novo tipo (categoria) | Registro |
| T11 | Registros (lista) | Registro |
| T12 | Detalhe e edição do registro | Registro |
| T13 | Confirmação para apagar | Registro |
| T14 | Primeiro acesso (boas-vindas) | Principal |
| T15 | Relatório resumido | Relatórios |
| T16 | Relatório detalhado | Relatórios |
| T17 | Meus tipos (categorias) | Configuração |
| T18 | Mais (menu da conta) | Configuração |
| T19 | Estados globais | Todas |

### 3.1 Fluxo principal

```
Criar conta (T02) → Confirme seu e-mail (T03) → E-mail confirmado (T04)
      → Entrar (T01) → Boas-vindas (T14, só na primeira vez) → Início (T07)

Início (T07) ──→ Registrar saída (T08) ──→ salva ──→ Início (T07) com confirmação
             └─→ Registrar entrada (T09) ──→ salva ──→ Início (T07) com confirmação

Registros (T11) → Detalhe (T12) → Editar ou Apagar (T13)
Relatórios (T15) → Ver por tipo (T16)
```

---

## 4. Descrição das telas

### T01 — Entrar

**Objetivo:** acessar a conta.

**Elementos, em ordem:**

1. Logotipo ou nome do produto (provisório, D-12 adiada).
2. Título: "Entrar".
3. Campo **E-mail**.
4. Campo **Senha**, com botão "Mostrar senha" (texto, não apenas ícone de olho).
5. Botão principal: **"Entrar"**.
6. Link: "Esqueci minha senha" → T05.
7. Separador e texto: "Ainda não tem conta?" + botão secundário **"Criar conta grátis"** → T02.

**Comportamento:**

- Manter conectado por padrão (sem caixa "lembrar de mim", que confunde).
- Após muitas tentativas incorretas, bloqueio temporário (RNF-07).

**Mensagens:**

| Situação | Texto |
|---|---|
| E-mail ou senha incorretos | "E-mail ou senha incorretos. Confira e tente de novo." |
| E-mail ainda não confirmado | "Você ainda não confirmou seu e-mail. Veja sua caixa de entrada." + botão "Enviar o e-mail de novo" |
| Bloqueio por tentativas | "Muitas tentativas. Aguarde 15 minutos ou toque em 'Esqueci minha senha'." |

---

### T02 — Criar conta

**Objetivo:** criar a empresa e o usuário proprietário.

**Formato [proposto]:** dividido em **duas etapas curtas**, com indicador "Passo 1 de 2", para não intimidar.

**Passo 1 — Sobre você**

1. Campo **Seu nome**.
2. Campo **E-mail** — texto de apoio: "Vamos enviar um e-mail para confirmar."
3. Campo **Crie uma senha** — texto de apoio: "Pelo menos 8 letras ou números." + botão "Mostrar senha".
4. Botão principal: **"Continuar"**.

**Passo 2 — Sobre seu negócio**

1. Campo **Nome do seu negócio** — exemplo no campo: "Ex.: Mercadinho São José".
2. Caixa de marcação: "Li e aceito os **Termos de Uso** e a **Política de Privacidade**" (links abrem os textos).
3. Botão principal: **"Criar minha conta"**.
4. Botão secundário: "Voltar".

**Mensagens:**

| Situação | Texto |
|---|---|
| E-mail já cadastrado | "Este e-mail já tem uma conta. Deseja entrar?" + botão "Entrar" |
| Senha curta | "A senha precisa ter pelo menos 8 caracteres." |
| Termos não aceitos | "Para continuar, marque que aceita os termos." |

---

### T03 — Confirme seu e-mail

**Objetivo:** orientar o usuário a abrir o e-mail de confirmação.

**Elementos:**

1. Ilustração simples de envelope.
2. Título: "Falta pouco!"
3. Texto: "Enviamos um e-mail para **fulano@email.com**. Abra o e-mail e toque no botão **Confirmar**."
4. Texto de apoio: "Não encontrou? Veja também a pasta **Spam** ou **Lixo eletrônico**."
5. Botão secundário: **"Enviar o e-mail de novo"** (desativado por 60 segundos após cada envio, com contagem visível).
6. Link: "Digitei o e-mail errado" → volta ao passo 1 da T02 com os dados preenchidos.

**E-mail de confirmação [proposto]:** assunto claro ("Confirme seu cadastro"), um único botão grande "Confirmar meu e-mail", sem textos longos.

---

### T04 — E-mail confirmado

**Elementos:**

1. Ícone de confirmação.
2. Título: "Pronto! Sua conta está ativa."
3. Botão principal: **"Começar"** → entra direto (sem pedir login novamente, se tecnicamente seguro) → T14.

**Estado de erro:**

| Situação | Texto |
|---|---|
| Link vencido ou já usado | "Este link não vale mais. Toque abaixo para receber um novo." + botão "Enviar novo link" |

---

### T05 — Esqueci minha senha

1. Título: "Esqueceu a senha?"
2. Texto: "Digite seu e-mail. Vamos enviar um link para você criar uma nova senha."
3. Campo **E-mail**.
4. Botão principal: **"Enviar link"**.

**Após enviar:** "Se este e-mail tiver uma conta, você receberá o link em alguns minutos. Veja também a pasta Spam." (A mensagem é a mesma exista ou não a conta, por segurança.)

---

### T06 — Criar nova senha

1. Título: "Crie uma nova senha".
2. Campo **Nova senha** + "Mostrar senha".
3. Campo **Repita a nova senha**.
4. Botão principal: **"Salvar nova senha"**.

**Após salvar:** "Senha alterada! Entre com a nova senha." → T01.

| Situação | Texto |
|---|---|
| Senhas diferentes | "As duas senhas estão diferentes. Digite de novo." |
| Link vencido | Igual à T04 |

---

### T14 — Boas-vindas (primeiro acesso)

**Objetivo:** explicar o sistema em até três telas curtas, apenas no primeiro acesso.

| Tela | Texto | Imagem |
|---|---|---|
| 1 de 3 | "Entrou dinheiro? Toque em **Entrada**." | Card verde de entrada |
| 2 de 3 | "Saiu dinheiro? Toque em **Saída**." | Card vermelho de saída |
| 3 de 3 | "Veja quanto sobrou no mês em **Relatórios**." | Barra de navegação com "Relatórios" destacado |

- Botões: **"Próximo"** e, na última, **"Começar a usar"**.
- Link discreto "Pular" em todas.
- Pode ser revista depois em T18 → "Como usar".

---

### T07 — Início

**Objetivo:** registrar rapidamente e ver o resumo do mês.

**Elementos, em ordem:**

1. Saudação: "Olá, [nome]" e o nome do negócio.
2. **Dois cards grandes, lado a lado ou empilhados** [definido]:

| Card | Conteúdo |
|---|---|
| Entrada | Cor verde, ícone de seta para baixo (dinheiro chegando), texto "**Entrada**" e subtítulo "Dinheiro que entrou" |
| Saída | Cor vermelha, ícone de seta para cima (dinheiro saindo), texto "**Saída**" e subtítulo "Dinheiro que saiu" |

3. **Resumo do mês** [proposto]: título "Setembro de 2026" e três linhas:
   - "Entrou: R$ 0,00"
   - "Saiu: R$ 0,00"
   - "**Sobrou: R$ 0,00**" (ou "**Faltou: R$ 0,00**" em vermelho)
4. **Últimos registros** [proposto]: os 5 mais recentes, cada um com ícone de entrada ou saída, tipo, valor e data ("Hoje", "Ontem" ou dd/mm). Toque abre a T12.
5. Link: "Ver todos os registros" → T11.
6. Barra de navegação inferior.

**Estados:**

| Situação | Comportamento |
|---|---|
| Nenhum registro ainda | No lugar da lista: "Você ainda não registrou nada. Comece tocando em **Entrada** ou **Saída**." |
| Após salvar um registro | Mensagem na parte inferior: "Saída de R$ 50,00 salva" + botão "Desfazer" |

---

### T08 — Registrar saída

**Objetivo:** registrar uma saída com o mínimo de toques.

**Formato [proposto]:** tela única, com rolagem, campos na ordem abaixo. Título no topo com a cor e o ícone da saída: "**Registrar saída**", e botão "Cancelar".

**Elementos, em ordem:**

1. **Valor** [definido]
   - Rótulo: "Quanto saiu?"
   - Campo grande, com "R$" fixo e formatação automática enquanto digita (digitar 5000 exibe R$ 50,00).
   - Teclado numérico abre automaticamente ao entrar na tela.

2. **Tipo de gasto (categoria)** [definido]
   - Rótulo: "Com o quê?"
   - Grade de botões grandes com ícone e nome (ex.: Mercadorias, Aluguel, Energia, Água...).
   - Categorias mais usadas aparecem primeiro [proposto].
   - Último botão: **"+ Novo tipo"** → T10.
   - "Outras" sempre visível.

3. **Forma de pagamento** [definido]
   - Rótulo: "Como pagou?"
   - Botões grandes com ícone: Pix, Dinheiro, Débito, Crédito, Crédito parcelado, Boleto, Transferência.
   - Pré-selecionada com a última forma usada em saídas.
   - Se "Crédito parcelado": aparece "Em quantas vezes?" com botões − e + (padrão 2; de 2 a 24), evitando digitação.

4. **Data** [definido]
   - Rótulo: "Quando?"
   - Botões: **[Hoje ✓] [Ontem] [Outra data]**.
   - "Outra data" abre o calendário; datas futuras aparecem desativadas.
   - Após escolher, o botão passa a exibir a data: "[12/09 ✓]".

5. **Descrição**
   - Rótulo: "Observação (opcional)"; quando o tipo for "Outras", muda para "O que foi? (obrigatório)".
   - Exemplo no campo: "Ex.: conserto da geladeira".

6. **Botão principal fixo na parte inferior:** "**Salvar saída — R$ 50,00**" (o valor digitado aparece no botão, como última conferência).

**Validações e mensagens (exibidas junto ao campo, em vermelho, com ícone):**

| Situação | Texto |
|---|---|
| Valor vazio ou zero | "Digite quanto saiu." |
| Valor com letras | Não aceita: o teclado é numérico e a máscara ignora letras |
| Valor acima do limite | "Valor muito alto. Confira se digitou certo." |
| Tipo não escolhido | "Escolha com o quê foi o gasto." |
| "Outras" sem descrição | "Escreva o que foi este gasto." |
| Tentar sair com dados preenchidos | "Você não salvou. Deseja sair mesmo assim?" [Continuar registrando] [Sair sem salvar] |

**Após salvar:** volta à T07 com a mensagem de confirmação e o botão "Desfazer".

**Contagem de toques no caso mais comum:** tocar em "Saída" → digitar o valor → tocar no tipo → tocar em "Salvar". Forma de pagamento e data já vêm preenchidas (RNF-03).

---

### T09 — Registrar entrada

Idêntica à T08, com as diferenças:

| Elemento | Texto e visual |
|---|---|
| Título | "**Registrar entrada**", com cor verde e ícone de entrada |
| Valor | "Quanto entrou?" |
| Categoria | "De onde veio?" — Vendas, Serviços, Recebimento de clientes, Aporte do proprietário, Outras |
| Forma de pagamento | "Como recebeu?" — pré-selecionada com a última forma usada em entradas |
| Botão | "**Salvar entrada — R$ 50,00**" |

---

### T10 — Novo tipo (categoria)

**Formato [proposto]:** painel que sobe da parte inferior da tela, sobre o formulário, sem perder o que já foi digitado.

1. Título: "Novo tipo de gasto" (ou "Novo tipo de entrada").
2. Campo **Nome** — exemplo: "Ex.: Embalagens".
3. Escolha de **ícone** [proposto]: grade com cerca de 20 ícones simples, para facilitar o reconhecimento visual.
4. Botão principal: **"Criar e usar"** — cria a categoria e já a seleciona no formulário.
5. Botão secundário: "Cancelar".

| Situação | Texto |
|---|---|
| Nome vazio | "Digite um nome." |
| Nome já existe | "Você já tem um tipo com este nome." |

---

### T11 — Registros (lista)

**Objetivo:** encontrar, conferir e corrigir registros.

**Elementos:**

1. Título: "Registros".
2. **Seletor de mês**: "◀ Setembro de 2026 ▶" (setas grandes).
3. **Filtro simples** em botões: [Todos] [Entradas] [Saídas].
4. Lista agrupada por dia, com o cabeçalho "Hoje", "Ontem" ou "Terça, 28/09":
   - Cada item: ícone e cor (entrada ou saída), nome do tipo, observação resumida (se houver), forma de pagamento em texto pequeno, valor alinhado à direita (entradas com "+", saídas com "−").
   - Toque no item abre a T12.
5. Carregamento de mais itens ao chegar ao fim da lista, com botão visível "Ver mais" (evita rolagem infinita, que desorienta).

**Estados:**

| Situação | Texto |
|---|---|
| Mês sem registros | "Nenhum registro em setembro." |
| Filtro sem resultados | "Nenhuma saída em setembro." |

> Filtro por categoria (LAN-01) [proposto]: acesso pelo relatório detalhado (T16), tocando em uma categoria, para não sobrecarregar esta tela.

---

### T12 — Detalhe e edição do registro

**Objetivo:** conferir, corrigir ou apagar um registro.

**Modo de visualização (padrão):**

1. Cabeçalho com cor e ícone: "Saída" ou "Entrada".
2. Valor em destaque.
3. Linhas: Tipo, Forma de pagamento (e parcelas, se houver), Data, Observação.
4. Texto pequeno: "Registrado em 30/09/2026 às 16:52" (data real do registro, não editável).
5. Botão principal: **"Corrigir"** → abre o mesmo formulário da T08/T09, preenchido.
6. Botão secundário, em vermelho e afastado do principal: **"Apagar"** → T13.

**Após corrigir:** "Registro corrigido" + botão "Desfazer".

---

### T13 — Confirmação para apagar

**Formato:** caixa de diálogo.

1. Título: "Apagar este registro?"
2. Texto que repete o registro: "Saída de **R$ 50,00** — Energia — 28/09".
3. Botões: **"Não apagar"** (destaque neutro) e **"Sim, apagar"** (vermelho).

**Após apagar:** volta à T11 com a mensagem "Registro apagado" + botão "Desfazer".

---

### T15 — Relatório resumido

**Objetivo:** mostrar, de forma imediata, se sobrou ou faltou dinheiro no período.

**Elementos, em ordem:**

1. Título: "Relatórios".
2. **Seletor de período** [definido]: botões [Mês] [Trimestre] [Semestre] [Ano].
3. **Navegação**: "◀ Setembro de 2026 ▶" (ou "◀ 3º trimestre de 2026 ▶", "◀ 2º semestre de 2026 ▶", "◀ 2026 ▶").
4. **Gráfico de duas barras** [definido]:
   - Barra verde "Entrou" e barra vermelha "Saiu", com o valor escrito acima de cada barra (a pessoa não precisa interpretar a escala).
5. **Resultado em destaque** [definido]:
   - "**Sobrou R$ 1.250,00**" em verde, ou "**Faltou R$ 320,00**" em vermelho, com ícone correspondente.
   - Frase de apoio: "Entrou mais do que saiu" / "Saiu mais do que entrou".
6. Botão principal: **"Ver por tipo"** → T16.

**Estados:**

| Situação | Texto |
|---|---|
| Período sem registros | "Nenhum registro neste período." + botão "Registrar agora" → T07 |
| Período em andamento | Texto pequeno: "Mês em andamento — valores até hoje." |

---

### T16 — Relatório detalhado

**Objetivo:** mostrar para onde foi e de onde veio o dinheiro.

**Elementos:**

1. Mesmo seletor de período e navegação da T15 (o período escolhido é mantido).
2. Abas: **[Saídas] [Entradas]** (Saídas primeiro, pois é a principal dúvida do comerciante: "para onde foi meu dinheiro?") [proposto].
3. Total da aba: "Total que saiu: R$ 3.400,00".
4. Lista de categorias, ordenadas da maior para a menor:
   - Ícone, nome, valor, percentual e uma barra horizontal proporcional.
   - Exemplo: "Mercadorias — R$ 1.800,00 — 53%".
5. Toque em uma categoria expande a lista dos registros dela; toque em um registro abre a T12.

> Gráfico de pizza não é recomendado: é mais difícil de ler do que barras horizontais, especialmente com muitas categorias.

**Estado vazio:** "Nenhuma saída neste período."

---

### T17 — Meus tipos (categorias)

**Objetivo:** gerenciar categorias.

1. Título: "Meus tipos".
2. Abas: **[Tipos de gasto] [Tipos de entrada]**.
3. Lista com ícone e nome; ao lado, botão "Editar".
4. Botão principal: **"+ Novo tipo"** → T10.
5. Na edição: renomear, trocar ícone e botão **"Esconder este tipo"**.
   - Texto de apoio: "Os registros antigos continuam nos relatórios. O tipo só não aparece mais para novos registros."
6. Seção recolhida no fim: "Tipos escondidos" com opção "Mostrar de novo".

> "Esconder" é o termo da interface para "desativar" (seção 4.5 do escopo).

---

### T18 — Mais (menu da conta)

Lista de opções grandes, com ícone e texto:

| Opção | Destino |
|---|---|
| Meus tipos | T17 |
| Meus dados | Nome, e-mail e nome do negócio (editáveis) |
| Trocar senha | Senha atual, nova senha, repetir nova senha |
| Como usar | Revê a T14 |
| Ajuda e contato | Canal de suporte (ex.: WhatsApp ou e-mail) [proposto] |
| Termos de Uso e Privacidade | Textos legais |
| Sair | Confirmação: "Deseja sair? Você precisará entrar de novo." |
| Excluir minha conta | Ao final, em vermelho, com explicação clara e dupla confirmação (LGPD) |

---

### T19 — Estados globais

Aplicam-se a todas as telas.

| Situação | Comportamento e texto |
|---|---|
| Carregando | Esqueleto da tela em cinza (não apenas um ícone girando); após 3 segundos: "Carregando..." |
| Sem internet | Faixa no topo: "Sem internet. Verifique sua conexão." Botões de salvar desativados, com o que foi digitado preservado |
| Erro no servidor | "Algo deu errado do nosso lado. Tente de novo em instantes." + botão "Tentar de novo" |
| Sessão encerrada | "Por segurança, entre de novo." → T01, retornando depois à tela em que estava |
| Registro salvo | Mensagem na parte inferior por cerca de 5 segundos, com "Desfazer" |

> Registro sem internet (modo offline) não está no MVP. Avaliar na versão PWA, pois é uma necessidade real em comércios com conexão instável.

---

## 5. Itens para validar no teste com comerciantes

1. O vocabulário da seção 2.2 é entendido sem explicação?
2. A pessoa encontra sozinha onde registrar uma saída de ontem?
3. A pessoa entende o que é "Sobrou" e "Faltou" no relatório?
4. A pessoa consegue corrigir um valor digitado errado?
5. Os ícones das categorias são reconhecidos?
6. Quanto tempo leva para registrar uma saída comum? (meta: menos de 15 segundos)

---

## 6. Resumo das decisões propostas neste roteiro

| # | Proposta |
|---|---|
| P-01 | Vocabulário simplificado na interface (seção 2.2) |
| P-02 | Cadastro dividido em duas etapas |
| P-03 | Boas-vindas de três telas no primeiro acesso |
| P-04 | Resumo do mês e últimos registros na tela inicial |
| P-05 | Formulário de registro em tela única, com valor exibido no botão de salvar |
| P-06 | Categorias e formas de pagamento como botões grandes com ícone |
| P-07 | Ícone escolhido pelo usuário ao criar categoria |
| P-08 | "Desfazer" após salvar, corrigir e apagar |
| P-09 | Relatório detalhado com barras horizontais, sem gráfico de pizza |
| P-10 | Sessão prolongada no dispositivo (ex.: 30 dias) |
| P-11 | Canal de ajuda e contato no menu |
| P-12 | Barra de navegação inferior com quatro itens |
