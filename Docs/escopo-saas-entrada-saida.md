# Escopo do Projeto — SaaS de Entradas e Saídas (Módulo Financeiro 1)

> **Versão 4** — 30/09/2026. Escopo fechado: define data do lançamento, front-end, forma de pagamento e retenção do log.

> **Legenda**
> - **[definido]** — decidido pelo responsável do projeto.
> - **[proposto]** — sugestão a confirmar.
> - **[pendente]** — decisão ainda em aberto (ver seção 14).

---

## 1. Visão geral

Sistema web (SaaS) para pequenos comércios registrarem entradas e saídas de dinheiro e consultarem relatórios simples de desempenho financeiro.

### 1.1 Justificativa

Grande parte dos CRMs e ERPs disponíveis é complexa e repleta de funções desnecessárias para o pequeno comerciante. O projeto concentra-se em uma única necessidade — controle de entradas e saídas — com o objetivo de conquistar os primeiros clientes, aprender com o mercado e, a partir disso, agregar novos módulos (fornecedores, produtos, estoque, CRM completo).

### 1.2 Princípios

1. Simplicidade de uso acima da quantidade de funções.
2. Começar pequeno, mas com arquitetura preparada para crescer em módulos.
3. Mobile first: a maioria dos usuários registrará lançamentos pelo celular.
4. Nenhum dado financeiro de um cliente pode ser visível a outro.
5. Esta é a base do produto: segurança, integridade e auditoria não são opcionais. **[definido]**

---

## 2. Objetivo e critério de encerramento

**Objetivo:** entregar um MVP funcional que permita a um pequeno comércio registrar entradas e saídas e consultar seus relatórios.

**O projeto (fase 1) está concluído quando:**

- [ ] É possível cadastrar a conta, confirmar o e-mail e acessar o sistema com autenticação.
- [ ] É possível registrar uma entrada.
- [ ] É possível registrar uma saída.
- [ ] É possível criar categorias personalizadas.
- [ ] É possível listar, editar e excluir lançamentos.
- [ ] Toda criação, edição e exclusão fica registrada no log de auditoria.
- [ ] É possível gerar o relatório simples (barras de entrada e saída + balanço).
- [ ] É possível gerar o relatório detalhado (por categoria).
- [ ] Os relatórios podem ser filtrados por período mensal, trimestral, semestral e anual.
- [ ] O sistema está publicado com HTTPS e backup automático fora do servidor.

---

## 3. Público-alvo e conta

- **Público:** pequenos comércios (mercearias, lojas de bairro, salões, lanchonetes, prestadores de serviço).
- **Perfil técnico esperado:** baixo; o sistema deve ser utilizável sem treinamento.

### 3.1 Estrutura de conta [definido]

- O **usuário é o proprietário da empresa**. Pequenos comércios não têm gestores intermediários.
- No MVP, **cada empresa tem um único usuário**.
- O cadastro cria, em uma única etapa, a empresa e seu proprietário.

**Decisão técnica [definido]:** apesar da relação 1:1 no MVP, o banco mantém as tabelas `empresa` e `usuario` separadas, com o campo `papel` já previsto. O custo é mínimo, e isso permite, no futuro, adicionar funcionários ou múltiplas lojas sem reestruturar os dados.

---

## 4. Funcionalidades do MVP

### 4.1 Autenticação e conta

| ID | Requisito | Status |
|---|---|---|
| AUT-01 | Cadastro com nome, e-mail, senha e nome da empresa | [definido] |
| AUT-02 | Confirmação de e-mail obrigatória antes do primeiro acesso | [definido] |
| AUT-03 | Login com e-mail e senha | [definido] |
| AUT-04 | Logout | [definido] |
| AUT-05 | Recuperação de senha por e-mail | [definido] |
| AUT-06 | Aceite de Termos de Uso e Política de Privacidade no cadastro (LGPD) | [definido] |

> **Dependência:** AUT-02 e AUT-05 exigem serviço de envio de e-mail transacional (SMTP próprio ou provedor como Amazon SES, Brevo, Mailgun). Recomenda-se provedor externo, pois e-mails enviados de servidor próprio costumam cair em spam. Configurar SPF, DKIM e DMARC no domínio.

### 4.2 Tela inicial [definido]

- Dois cards de destaque: **Entrada** e **Saída**.
- Ao tocar em um card, abre-se o formulário correspondente.

**Complementos [proposto]:**

- Resumo do mês atual abaixo dos cards (total de entradas, total de saídas, saldo).
- Lista dos últimos lançamentos, com acesso à edição.
- Acesso aos relatórios e às categorias pelo menu.

### 4.3 Registro de saída [definido]

| Campo | Tipo | Obrigatório | Observação |
|---|---|---|---|
| Valor | Decimal (R$) | Sim | Maior que zero |
| Categoria | Seleção | Sim | Categorias de saída + botão "Nova categoria" |
| Descrição | Texto | Condicional | Obrigatória quando a categoria for "Outras" |
| Forma de pagamento | Seleção | Sim | Ver seção 4.3.2 |
| Parcelas | Inteiro | Condicional | Apenas para "Cartão de crédito parcelado" |
| Data | Seletor | Sim | Ver seção 4.3.1 |

#### 4.3.1 Data do lançamento [definido — D-06]

Seletor rápido com três opções, abaixo do valor:

| Opção | Comportamento |
|---|---|
| **Hoje** | Selecionada por padrão. Nenhum toque adicional é necessário. |
| **Ontem** | Um toque. Cobre o caso retroativo mais comum. |
| **Outra data** | Abre o calendário para escolher a data. |

- No caso mais frequente, o usuário não interage com a data, mantendo a proposta de menor esforço.
- A data escolhida pode ser alterada na edição do lançamento.
- O sistema grava duas datas distintas: `data_lancamento` (informada pelo usuário) e `criado_em` (momento real do registro, definido pelo servidor). A segunda nunca é editável e serve à auditoria.

#### 4.3.2 Forma de pagamento [definido — D-11]

| Opção | Código |
|---|---|
| Pix | `PIX` |
| Dinheiro | `DINHEIRO` |
| Cartão de débito | `DEBITO` |
| Cartão de crédito | `CREDITO` |
| Cartão de crédito parcelado | `CREDITO_PARCELADO` |
| Boleto **[proposto]** | `BOLETO` |
| Transferência (TED/DOC) **[proposto]** | `TRANSFERENCIA` |

**Regras [proposto]:**

- O campo vem pré-selecionado com a última forma de pagamento usada pelo usuário naquele tipo (entrada ou saída), reduzindo toques.
- Ao escolher "Cartão de crédito parcelado", surge o campo **número de parcelas** (2 a 24).
- **No MVP, o parcelamento é informativo:** o valor total é registrado uma única vez, na data do lançamento. A distribuição das parcelas nos meses seguintes pertence ao módulo de contas a pagar e a receber (Financeiro 2).

### 4.4 Registro de entrada [definido]

Mesmos campos e regras da saída, com categorias próprias de entrada.

### 4.5 Categorias [definido]

- Cada empresa recebe um conjunto de categorias pré-existentes no cadastro.
- Há um botão **"Nova categoria"** no formulário de lançamento e na tela de categorias.
- Categorias podem ser renomeadas e desativadas.
- Categorias com lançamentos vinculados **não são excluídas**, apenas desativadas (preserva o histórico e os relatórios).

**Categorias padrão:**

| Saídas | Entradas |
|---|---|
| Mercadorias / Fornecedores | Vendas |
| Aluguel | Prestação de serviços |
| Energia elétrica | Recebimento de clientes (fiado/prazo) |
| Água | Aporte do proprietário |
| Internet e telefone | Outras |
| Salários e pró-labore | |
| Impostos e taxas | |
| Manutenção | |
| Transporte e combustível | |
| Taxas de cartão / maquininha | |
| Outras | |

### 4.6 Gestão de lançamentos [definido]

| ID | Requisito |
|---|---|
| LAN-01 | Listar lançamentos com filtro por período, tipo e categoria |
| LAN-02 | Editar valor, categoria e descrição de um lançamento |
| LAN-03 | Excluir lançamento, com confirmação explícita ("Tem certeza?") |
| LAN-04 | A exclusão é lógica: o registro permanece no banco, marcado como excluído |
| LAN-05 | Toda edição e exclusão gera registro de auditoria com os dados anteriores e novos |
| LAN-06 | Paginação da lista |

### 4.7 Auditoria [definido]

Objetivo: saber **quem fez o quê, quando e de onde**, servindo de base para o pilar do não repúdio.

**Eventos auditados:**

| Categoria | Eventos |
|---|---|
| Conta | Cadastro, confirmação de e-mail, login bem-sucedido, login com falha, logout, troca e recuperação de senha |
| Lançamentos | Criação, edição, exclusão |
| Categorias | Criação, renomeação, desativação |
| Privacidade | Aceite de termos, solicitação de exclusão de conta |

**Dados registrados em cada evento:** usuário, empresa, ação, entidade e identificador afetados, dados anteriores, dados novos, data e hora (UTC), endereço IP e identificação do navegador.

**Regras [proposto]:**

| ID | Regra |
|---|---|
| AUD-01 | O log é **somente de inserção**: o usuário de banco da aplicação não tem permissão de `UPDATE` nem `DELETE` na tabela de auditoria. |
| AUD-02 | O registro de auditoria é gravado **na mesma transação** da operação auditada: se um falhar, ambos são desfeitos. |
| AUD-03 | Cada registro guarda o hash do registro anterior (encadeamento), tornando detectável qualquer adulteração direta no banco. |
| AUD-04 | Senhas, tokens e códigos nunca são gravados no log. |
| AUD-05 | No MVP, o log é consultado apenas pelo administrador do sistema; exibição ao cliente fica para versões futuras. |
| AUD-06 | Retenção de **5 anos e 1 dia** [definido — D-14]. Uma rotina agendada elimina os registros vencidos, executada com credencial de banco própria, distinta da usada pela aplicação (que continua sem permissão de exclusão). |
| AUD-07 | Antes de cada eliminação, grava-se o hash do último registro eliminado como ponto de ancoragem, para que o encadeamento dos registros restantes continue verificável. |
| AUD-08 | Quando o titular solicitar a exclusão da conta (LGPD), os registros de auditoria são mantidos até o fim do prazo de retenção, com dados pessoais identificáveis (nome, e-mail, IP) anonimizados. |

> **Observação sobre não repúdio:** o log com encadeamento de hash garante **rastreabilidade e integridade** (quem fez, o que fez, quando e prova de que o registro não foi alterado). O não repúdio em sentido jurídico pleno exige assinatura digital com certificado (ICP-Brasil). A estrutura proposta já é a base correta para evoluir nessa direção, se necessário.

### 4.8 Relatório simples [definido]

- Duas barras: **total de entradas** e **total de saídas**.
- **Balanço final** = entradas − saídas, com destaque visual para saldo positivo ou negativo.

### 4.9 Relatório detalhado [definido]

- Entradas e saídas agrupadas por categoria.
- Subtotal por categoria e percentual sobre o total do seu tipo.
- Opção de expandir cada categoria para ver seus lançamentos. [proposto]

### 4.10 Períodos dos relatórios [definido]

| Período | Recorte [proposto] |
|---|---|
| Mensal | Mês civil (1º ao último dia) |
| Trimestral | Jan–mar, abr–jun, jul–set, out–dez |
| Semestral | Jan–jun, jul–dez |
| Anual | Ano civil |

- **Período padrão:** mês atual. [proposto]
- **Navegação:** botões "anterior" e "próximo" para consultar períodos passados (ex.: agosto, 2º trimestre). [proposto]
- Relatório sem lançamentos exibe mensagem orientativa, não erro.
- Exportação em PDF ou CSV: **fora do MVP**; reavaliar com os clientes piloto.

---

## 5. Fora do escopo do MVP

- Múltiplos usuários por empresa (funcionários) e múltiplas lojas por usuário.
- Cadastro de fornecedores, clientes e produtos.
- Controle de estoque.
- Contas a pagar e a receber (lançamentos futuros, vencimentos, parcelamento).
- Lançamentos recorrentes.
- Distribuição de parcelas de cartão de crédito nos meses futuros.
- Exportação de relatórios (PDF/CSV).
- Integração bancária, Pix ou maquininhas.
- Emissão de nota fiscal.
- Aplicativo nativo (o MVP é web responsivo; PWA pode ser avaliado depois).
- Cobrança automatizada da assinatura.

---

## 6. Regras de negócio

| ID | Regra |
|---|---|
| RN-01 | Todo lançamento pertence a exatamente uma empresa, um usuário e uma categoria. |
| RN-02 | O valor é sempre positivo; o sentido (entrada ou saída) é dado pelo campo `tipo`. |
| RN-03 | A categoria de um lançamento deve ser do mesmo tipo do lançamento e estar ativa. |
| RN-04 | Um usuário só acessa dados da própria empresa (isolamento verificado no back-end em toda requisição). |
| RN-05 | Balanço = soma das entradas − soma das saídas no período filtrado. |
| RN-06 | Lançamentos excluídos não entram em relatórios nem na lista. |
| RN-07 | Moeda única: Real (BRL). |
| RN-08 | A data do lançamento é, por padrão, a data atual no fuso `America/Sao_Paulo`; o usuário pode escolher "Ontem" ou outra data passada. A data e hora reais do registro são gravadas separadamente pelo servidor e nunca são editáveis. |
| RN-11 | Relatórios consideram a `data_lancamento` (data informada), não a data do registro. |
| RN-12 | Todo lançamento tem uma forma de pagamento. |
| RN-13 | O número de parcelas só existe, e é obrigatório, quando a forma de pagamento for "Cartão de crédito parcelado". |
| RN-14 | Registros de auditoria são mantidos por 5 anos e 1 dia e eliminados após esse prazo. |
| RN-09 | A categoria "Outras" exige descrição. |
| RN-10 | Nenhuma alteração de dados de negócio ocorre sem o respectivo registro de auditoria. |

---

## 7. Validações (front-end e back-end) [definido]

A validação do front-end melhora a experiência; a do back-end garante a integridade. **O back-end nunca confia no front-end.**

| Campo | Regras |
|---|---|
| Valor | Obrigatório; numérico; maior que zero; no máximo 2 casas decimais; limite máximo (ex.: R$ 9.999.999.999,99); aceitar vírgula como separador decimal na digitação; rejeitar letras e símbolos |
| Descrição | Máximo de 255 caracteres; remover espaços nas extremidades; obrigatória para "Outras"; tratar como texto (proteção contra XSS) |
| Categoria | Obrigatória; deve existir, estar ativa, pertencer à empresa e ser do tipo correto |
| Data do lançamento | Obrigatória (padrão: hoje); data válida; não pode ser futura; sem limite retroativo no MVP |
| Forma de pagamento | Obrigatória; deve ser um dos valores da lista |
| Parcelas | Inteiro de 2 a 24; obrigatório somente para crédito parcelado; rejeitado nas demais formas |
| Nome da categoria | Obrigatório; máximo de 60 caracteres; único por empresa e tipo (sem diferenciar maiúsculas) |
| E-mail | Formato válido; único no sistema |
| Senha | Mínimo de 8 caracteres |

**Regra técnica essencial [definido]:** valores monetários nunca são armazenados em ponto flutuante.

- Banco: `NUMERIC(12,2)`.
- Java: `BigDecimal`.
- JavaScript: trabalhar em centavos (inteiros) ou com biblioteca decimal; formatar com `Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' })`.

---

## 8. Integridade dos dados (ACID) [definido]

O PostgreSQL já é um banco ACID por padrão. A garantia efetiva depende de como a aplicação usa as transações:

| Propriedade | Aplicação no projeto |
|---|---|
| **Atomicidade** | Lançamento e registro de auditoria gravados na mesma transação (`@Transactional`); falha em um desfaz ambos. |
| **Consistência** | Restrições no próprio banco: `NOT NULL`, `CHECK (valor > 0)`, chaves estrangeiras, `UNIQUE`. O banco rejeita dados inválidos mesmo que a aplicação falhe. |
| **Isolamento** | Nível padrão `READ COMMITTED`, suficiente para o MVP; relatórios leem apenas dados confirmados. |
| **Durabilidade** | Garantida pelo PostgreSQL (WAL), complementada por backup diário fora do servidor. |

---

## 9. Modelo de dados inicial [proposto]

```sql
empresa (
  id              UUID PRIMARY KEY,
  nome            VARCHAR(120) NOT NULL,
  documento       VARCHAR(18),            -- CPF/CNPJ, opcional no MVP
  criado_em       TIMESTAMPTZ NOT NULL
)

usuario (
  id                UUID PRIMARY KEY,
  empresa_id        UUID NOT NULL REFERENCES empresa(id),
  nome              VARCHAR(120) NOT NULL,
  email             VARCHAR(160) NOT NULL UNIQUE,
  email_confirmado  BOOLEAN NOT NULL DEFAULT FALSE,
  senha_hash        VARCHAR(255) NOT NULL,
  papel             VARCHAR(20)  NOT NULL DEFAULT 'PROPRIETARIO',
  ativo             BOOLEAN NOT NULL DEFAULT TRUE,
  termos_aceitos_em TIMESTAMPTZ NOT NULL,
  criado_em         TIMESTAMPTZ NOT NULL
)

categoria (
  id              UUID PRIMARY KEY,
  empresa_id      UUID NOT NULL REFERENCES empresa(id),
  tipo            VARCHAR(10) NOT NULL CHECK (tipo IN ('ENTRADA','SAIDA')),
  nome            VARCHAR(60) NOT NULL,
  padrao          BOOLEAN NOT NULL DEFAULT FALSE,
  ativa           BOOLEAN NOT NULL DEFAULT TRUE,
  UNIQUE (empresa_id, tipo, nome)
)

lancamento (
  id              UUID PRIMARY KEY,
  empresa_id      UUID NOT NULL REFERENCES empresa(id),
  usuario_id      UUID NOT NULL REFERENCES usuario(id),
  categoria_id    UUID NOT NULL REFERENCES categoria(id),
  tipo            VARCHAR(10) NOT NULL CHECK (tipo IN ('ENTRADA','SAIDA')),
  valor           NUMERIC(12,2) NOT NULL CHECK (valor > 0),
  descricao       VARCHAR(255),
  forma_pagamento VARCHAR(20) NOT NULL CHECK (forma_pagamento IN
                    ('PIX','DINHEIRO','DEBITO','CREDITO','CREDITO_PARCELADO',
                     'BOLETO','TRANSFERENCIA')),
  parcelas        SMALLINT CHECK (
                    (forma_pagamento = 'CREDITO_PARCELADO' AND parcelas BETWEEN 2 AND 24)
                    OR (forma_pagamento <> 'CREDITO_PARCELADO' AND parcelas IS NULL)),
  data_lancamento DATE NOT NULL,          -- informada (padrão: hoje)
  criado_em       TIMESTAMPTZ NOT NULL,   -- definida pelo servidor, imutável
  atualizado_em   TIMESTAMPTZ NOT NULL,
  excluido_em     TIMESTAMPTZ,            -- exclusão lógica
  excluido_por    UUID REFERENCES usuario(id)
)

auditoria (
  id                BIGSERIAL PRIMARY KEY,
  empresa_id        UUID,
  usuario_id        UUID,
  acao              VARCHAR(30) NOT NULL,  -- CRIAR, EDITAR, EXCLUIR, LOGIN, LOGIN_FALHO...
  entidade          VARCHAR(40),           -- LANCAMENTO, CATEGORIA, USUARIO...
  entidade_id       UUID,
  dados_anteriores  JSONB,
  dados_novos       JSONB,
  ip                INET,
  user_agent        VARCHAR(255),
  ocorrido_em       TIMESTAMPTZ NOT NULL,
  hash_anterior     CHAR(64),
  hash              CHAR(64) NOT NULL
)

-- Índices
CREATE INDEX idx_lancamento_empresa_data
  ON lancamento (empresa_id, data_lancamento)
  WHERE excluido_em IS NULL;

CREATE INDEX idx_auditoria_empresa_data
  ON auditoria (empresa_id, ocorrido_em);

-- Log somente de inserção para o usuário da aplicação
REVOKE UPDATE, DELETE ON auditoria FROM app_user;
```

**Observações:**

- Todas as tabelas de negócio têm `empresa_id`, o que viabiliza o multi-inquilino e os módulos futuros.
- As categorias padrão são copiadas para cada empresa no cadastro (`padrao = TRUE`), permitindo personalização sem afetar outras empresas.
- Tokens de confirmação de e-mail e de recuperação de senha ficam em tabela própria, com validade e uso único.
- A estrutura do banco é versionada por migrações (Flyway), nunca alterada manualmente.

---

## 10. Requisitos não funcionais

| ID | Requisito | Status |
|---|---|---|
| RNF-01 | Interface responsiva, mobile first | [definido] |
| RNF-02 | Protótipos validados no Figma antes da implementação | [definido] |
| RNF-03 | Registro de um lançamento em no máximo 3 toques após a tela inicial | [proposto] |
| RNF-04 | Tempo de resposta da API inferior a 500 ms nas operações comuns | [proposto] |
| RNF-05 | Comunicação exclusivamente por HTTPS | [definido] |
| RNF-06 | Senhas armazenadas com hash forte (BCrypt ou Argon2) | [definido] |
| RNF-07 | Limite de tentativas de login (proteção contra força bruta) | [definido] |
| RNF-08 | Backup diário automático do banco, com cópia fora do servidor desde a fase 1 e teste periódico de restauração | [definido] |
| RNF-09 | Logs técnicos de erro separados do log de auditoria, sem dados sensíveis | [definido] |
| RNF-10 | Conformidade com a LGPD: termos, política de privacidade, exclusão de conta a pedido do titular | [definido] |
| RNF-11 | Acessibilidade básica: contraste adequado, rótulos em campos, navegação por teclado | [proposto] |
| RNF-12 | Idioma e formatos em pt-BR (moeda, data dd/mm/aaaa) | [definido] |
| RNF-13 | Testes automatizados das regras de negócio, validações e isolamento entre empresas | [definido] |

---

## 11. Arquitetura e tecnologias

### 11.1 Diretriz de arquitetura [definido — D-13]

**Monólito modular**. Um único back-end, implantado como uma só aplicação, organizado internamente em módulos independentes:

```
back-end/
├── compartilhado/  (segurança, auditoria, utilitários)
├── conta/          (empresa, usuário, autenticação)
├── financeiro/     (categorias, lançamentos, relatórios)   ← MVP
├── fornecedores/   (futuro)
├── produtos/       (futuro)
└── estoque/        (futuro)
```

- Cada módulo tem suas próprias entidades, serviços, controladores e tabelas.
- Um módulo só acessa outro por meio de sua interface pública, nunca diretamente pelas classes internas ou tabelas.
- Verificação automatizada das fronteiras entre módulos (Spring Modulith ou ArchUnit).
- API REST versionada (`/api/v1/...`), documentada com OpenAPI/Swagger.
- Front-end e back-end como projetos separados, comunicando-se apenas pela API.

### 11.2 Back-end [definido]

**Java (versão LTS atual) + Spring Boot**, executado em contêiner Docker.

Componentes: Spring Web, Spring Security, Spring Data JPA, Bean Validation, Flyway, Spring Modulith.

### 11.3 Front-end [definido — D-08]

**React + TypeScript + Vite.**

| Necessidade | Biblioteca [proposto] |
|---|---|
| Formulários e validação | React Hook Form + Zod (mesmas regras de validação do back-end, espelhadas no front) |
| Comunicação com a API | TanStack Query (cache, carregamento e tratamento de erros) |
| Gráficos | Recharts |
| Rotas | React Router |
| Estilos | Tailwind CSS ou CSS Modules, com componentes acessíveis (ex.: Radix UI / shadcn/ui) |
| Instalação no celular | PWA (vite-plugin-pwa) |

Motivos da escolha: maior ecossistema e mercado, bibliotecas maduras para cada necessidade acima, abundância de documentação e exemplos, e boa aderência a uma API REST com contrato OpenAPI (os tipos TypeScript podem ser gerados automaticamente a partir dela).

### 11.4 Banco de dados [definido]

PostgreSQL (versão estável atual), com as garantias descritas na seção 8.

### 11.5 Autenticação [definido]

Sessão em cookie `HttpOnly`, `Secure` e `SameSite`, com front-end e back-end sob o mesmo domínio. Reavaliar caso surja aplicativo nativo.

---

## 12. Infraestrutura e implantação

| Fase | Ambiente | Status |
|---|---|---|
| Fase 1 | Servidor próprio | [definido] |
| Fase 2 | VPS hospedada (backups e disponibilidade) | [definido] |

**Diretrizes para permitir a migração sem retrabalho [definido]:**

- Toda a aplicação em **Docker Compose** (front, back, PostgreSQL, proxy reverso).
- Proxy reverso com certificado TLS automático (Caddy, Traefik ou Nginx + Certbot).
- Configurações por variáveis de ambiente, sem credenciais no código.
- Backup do PostgreSQL (`pg_dump`) diário, com cópia **fora do servidor** desde a fase 1.
- Ambientes separados: desenvolvimento e produção.
- Repositório Git com integração contínua (build e testes a cada envio).
- Provedor de e-mail transacional configurado (seção 4.1).

---

## 13. Processo de desenvolvimento [proposto]

1. Fechar as decisões pendentes (seção 14).
2. Prototipar no Figma: cadastro, confirmação de e-mail, login, recuperação de senha, tela inicial, formulário de lançamento, nova categoria, lista, relatório simples, relatório detalhado, estados vazios e mensagens de erro.
3. Validar os protótipos, se possível, com 2 ou 3 comerciantes reais.
4. Configurar repositório, Docker Compose, banco e migrações.
5. Implementar por fatias verticais (banco → API → tela), nesta ordem:
   1. Estrutura base: módulos, segurança e auditoria.
   2. Cadastro, confirmação de e-mail, login e recuperação de senha.
   3. Categorias padrão e criação de categorias.
   4. Registro de entrada e saída.
   5. Lista, edição e exclusão.
   6. Relatório simples.
   7. Relatório detalhado e navegação por períodos.
6. Publicar no servidor com HTTPS e backup.
7. Testar com clientes piloto e coletar retorno.

---

## 14. Decisões

| # | Decisão | Resultado | Status |
|---|---|---|---|
| D-01 | Papel "Operador" no MVP? | Não; campo `papel` previsto no banco | [definido] |
| D-02 | Recuperação de senha por e-mail no MVP? | Sim | [definido] |
| D-03 | Confirmação de e-mail no MVP? | Sim — transmite confiabilidade ao produto novo | [definido] |
| D-04 | Categorias personalizadas no MVP? | Sim, com botão "Nova categoria" | [definido] |
| D-05 | Descrição obrigatória? | Apenas para a categoria "Outras" | [definido] |
| D-06 | Data do lançamento | Seletor "Hoje" (padrão) / "Ontem" / "Outra data"; data informada e data de registro gravadas separadamente | [definido] |
| D-07 | Exportação PDF/CSV no MVP? | Não; avaliar com clientes piloto | [definido] |
| D-08 | Framework de front-end | React + TypeScript + Vite | [definido] |
| D-09 | Back-end | Java + Spring Boot | [definido] |
| D-10 | Modelo de cobrança | Piloto gratuito; definir plano antes da fase 2 | [definido] |
| D-11 | Forma de pagamento | Pix, dinheiro, débito, crédito, crédito parcelado (+ boleto e transferência, propostos) | [definido] |
| D-12 | Nome do produto e domínio | Adiado até conhecer melhor o produto | [adiado] |
| D-13 | Arquitetura do back-end | Monólito modular | [definido] |
| D-14 | Tempo de retenção do log de auditoria | 5 anos e 1 dia | [definido] |

---

## 15. Evolução futura (roadmap indicativo)

| Módulo | Conteúdo |
|---|---|
| Financeiro 2 | Contas a pagar e a receber, distribuição de parcelas nos meses, recorrência, relatório por forma de pagamento, exportações |
| Fornecedores | Cadastro e vínculo com saídas |
| Produtos | Cadastro, preço de custo e venda |
| Estoque | Movimentações vinculadas a entradas e saídas |
| Clientes / CRM | Cadastro, histórico de compras, fiado |
| Plataforma | Cobrança da assinatura, funcionários com permissões, múltiplas lojas, consulta de auditoria pelo cliente, assinatura digital |

A prioridade entre os módulos deve ser definida pelo retorno dos clientes do MVP.

---

## 16. Glossário

| Termo | Significado |
|---|---|
| Lançamento | Registro de uma entrada ou saída de dinheiro |
| Entrada | Dinheiro que entra no caixa da empresa |
| Saída | Dinheiro que sai do caixa da empresa |
| Balanço | Diferença entre entradas e saídas no período |
| Empresa / Inquilino | Loja cliente do SaaS; seus dados são isolados dos demais |
| Proprietário | Único usuário da empresa no MVP |
| MVP | Produto mínimo viável: menor versão útil para validar com clientes |
| Exclusão lógica | Registro marcado como excluído, sem apagá-lo fisicamente do banco |
| Auditoria | Registro imutável de quem fez o quê, quando e de onde |
| ACID | Atomicidade, Consistência, Isolamento e Durabilidade das transações |
| Monólito modular | Aplicação única, dividida internamente em módulos com fronteiras rígidas |
