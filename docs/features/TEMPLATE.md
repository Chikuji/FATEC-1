# Especificação de Funcionalidade: [NOME_DA_FUNCIONALIDADE]

- **Criado em**: [DATA]
- **Status**: Rascunho

## Resumo Executivo

<!--
  Resuma a especificação em três a cinco pontos principais.
  Sirva como referência rápida para o agente e os desenvolvedores.
  Atualize sempre que a especificação mudar significativamente.
-->

- **Objetivo**: [Uma frase que descreva o que a funcionalidade faz]
- **Usuário principal**: [Quem usa a funcionalidade]
- **Valor entregue**: [Por que isso importa]
- **Escopo**: [O que está incluído e excluído]
- **Tipo de mudança**: [new surface | additive to existing | modifies existing boundary | removes existing surface]
- **Descreve recurso de IA**: [yes | no]
- **Critério de sucesso principal**: [Métrica mais importante]

<!--
  Descreve recurso de IA:
    Responda "yes" quando a funcionalidade incorporar um agente de IA que
    tome decisões, invoque ferramentas ou aja em nome de um usuário. Quando
    a resposta for "yes", preencha a seção "Custos de IA" abaixo. As
    restrições comportamentais do agente (regras que deve respeitar mesmo
    quando as ferramentas permitirem outra ação) pertencem à seção geral
    "Invariantes", não a um bloco separado específico de IA.
    Responda "no" para funcionalidades de produto que não incorporem
    comportamento de IA.
-->

Para funcionalidades cujo tipo de mudança não seja "new surface", descreva a semântica de recuperação na seção Compatibilidade e Transição abaixo.

## Fora do Escopo *(obrigatório)*

<!--
  Declare explicitamente o que NÃO faz parte desta funcionalidade.
  Proteja o escopo contra ampliações implícitas e alinhe as expectativas.
  Inclua pelo menos um item.
-->

- [O que está fora do escopo e o motivo da exclusão]

## Premissas

<!--
  Documente os padrões razoáveis assumidos durante a especificação.
  Exemplo: "Autenticação baseada em sessão (padrão da web, sem requisito explícito)"
-->

- [Premissa e justificativa]

## Custos de IA *(obrigatório quando "Descreve recurso de IA" for "yes" e omitido nos demais casos)*

<!--
  Preencha esta seção somente quando o Resumo Executivo declarar
  "Descreve recurso de IA: yes". Este bloco registra os compromissos
  operacionais de custo da funcionalidade para o comportamento de IA integrado.

  As restrições comportamentais do agente (regras que deve respeitar mesmo
  quando as ferramentas permitirem outra ação) pertencem à seção geral
  Invariantes. A composição dos serviços (como o recurso de IA se divide
  em subserviços ou subagentes) pertence a Requisitos e Cenários e Testes
  de Usuário.

  Referência de nível do modelo (escolha um nível por etapa de IA quando relevante):
    Reasoning  - maior capacidade de raciocínio, lento e caro por chamada
                 use para: planejamento em várias etapas, síntese complexa, refatoração difícil
    Frontier   - capacidade geral de ponta, com equilíbrio entre velocidade e custo
                 use para: agentes com muitas ferramentas, geração de código, experiência conversacional
    Mid        - capaz, porém mais barato e rápido que Frontier
                 use para: resumos de rotina, classificação contextual, RAG
    Fast       - pequeno, rápido, barato e com profundidade de raciocínio limitada
                 use para: classificação em alto volume, extração simples, experiência sensível à latência

  Quando o runtime escolhe o modelo (uma plataforma de IA gerenciada ou
  assistente de desenvolvimento hospedado cuja seleção do modelo é
  controlada pela plataforma, não pelo autor da especificação), defina cada
  campo como "N/A - model chosen by runtime" e acrescente uma linha com o
  nome do runtime que controla a escolha. Este bloco também garante que os
  compromissos operacionais sejam considerados.

  Observação sobre a posição: esta seção aparece antes de Cenários e Testes
  de Usuário porque o orçamento de custos influencia quais requisitos são
  economicamente viáveis.
-->

- **Compromisso de nível do modelo** (por etapa, quando relevante): [nome-da-etapa para nível (Reasoning / Frontier / Mid / Fast), com justificativa em uma linha]
- **Orçamento de latência**: p50=[valor]; p95=[valor]; p99=[valor]. *Comportamento ao exceder o limite:* [degradar | alertar | interromper]
- **Invariante de estabilidade do prompt**: [Quais elementos do prompt permanecem estáveis entre execuções para permitir cache. O que violaria o invariante e exigiria uma alteração na especificação.]
- **Limite de custo por chamada**: limite máximo=[tokens ou valor em dinheiro]. *Comportamento ao exceder o limite:* [escalar | interromper | degradar]
- **Escalonamento de incidentes de custo**: [Qual condição relacionada ao custo aciona uma interrupção ou revisão humana]

<!--
  Exemplo (remova ao preencher esta especificação para uma funcionalidade real):

  - Compromisso de nível do modelo:
    - etapa "intent-classify" -> Fast (classificador de alto volume e baixa ambiguidade)
    - etapa "draft-summary" -> Frontier (resultado coerente com vários parágrafos)
    - etapa "escalation-decision" -> Reasoning (encaminha para uma pessoa, pois a qualidade do raciocínio é mais importante que o custo)
  - Orçamento de latência: p50=400ms; p95=1.2s; p99=3s. Comportamento ao exceder o limite: degradar (ignorar draft-summary e retornar somente o resultado do classificador)
  - Invariante de estabilidade do prompt: mensagem do sistema e esquema de ferramentas fixos durante uma janela de 24h. Uma mudança no esquema de ferramentas exige alteração na especificação.
  - Limite de custo por chamada: limite máximo=$0.05 por chamado. Comportamento ao exceder o limite: escalar (encaminhar para uma pessoa e registrar o incidente).
  - Escalonamento de incidentes de custo: qualquer janela de 1 hora em que 5% das chamadas excedam o limite aciona uma interrupção e uma análise posterior.

  Exemplo gerenciado pelo runtime (use este formato quando a plataforma escolher o modelo):

  - Compromisso de nível do modelo: N/A - model chosen by runtime ([nome da plataforma] seleciona conforme o plano do usuário e a lógica de seleção da plataforma)
  - Orçamento de latência: N/A - controlado pela plataforma de runtime
  - Invariante de estabilidade do prompt: o corpo do agente e a lista de ferramentas são versionados neste repositório, e as mudanças exigem uma entrada no CHANGELOG
  - Limite de custo por chamada: N/A - cobrança conforme o plano da plataforma de runtime
  - Escalonamento de incidentes de custo: N/A - custo controlado pelo plano da plataforma de runtime
-->

## Cenários e Testes de Usuário *(obrigatório)*

<!--
  IMPORTANTE: Priorize as histórias de usuário como jornadas ordenadas por importância.
  Cada história ou jornada de usuário deve ser testável de forma INDEPENDENTE. Isso
  significa que, mesmo implementando apenas UMA delas, você ainda terá um MVP
  (Produto Mínimo Viável) que entrega valor.

  Atribua prioridades (P1, P2, P3 etc.) a cada história. P1 é a mais crítica.
  Considere cada história um recorte independente de funcionalidade que possa ser:
  - Desenvolvido de forma independente
  - Testado de forma independente
  - Implantado de forma independente
  - Demonstrado aos usuários de forma independente
-->

### História de Usuário 1: [Título Breve] (Prioridade: P1)

[Descreva esta jornada de usuário em linguagem simples]

**Motivo desta prioridade**: [Explique o valor e por que esta história tem este nível de prioridade]

**Teste Independente**: [Descreva como testar esta história de forma independente. Por exemplo: "Pode ser testada por completo ao [ação específica] e entrega [valor específico]"]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]
2. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

### História de Usuário 2: [Título Breve] (Prioridade: P2)

[Descreva esta jornada de usuário em linguagem simples]

**Motivo desta prioridade**: [Explique o valor e por que esta história tem este nível de prioridade]

**Teste Independente**: [Descreva como testar esta história de forma independente]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

### História de Usuário 3: [Título Breve] (Prioridade: P3)

[Descreva esta jornada de usuário em linguagem simples]

**Motivo desta prioridade**: [Explique o valor e por que esta história tem este nível de prioridade]

**Teste Independente**: [Descreva como testar esta história de forma independente]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

[Adicione outras histórias de usuário conforme necessário e atribua uma prioridade a cada uma]

### Casos Extremos

<!--
  AÇÃO NECESSÁRIA: O conteúdo desta seção contém placeholders.
  Preencha-os com os casos extremos adequados.
-->

- O que acontece quando [condição de limite]?
- Como o sistema trata [cenário de erro]?

### Modos de Falha *(inclua se a funcionalidade tiver dependências externas ou estado compartilhado)*

<!--
  Documente como o sistema deve se comportar em condições reais de falha.
  É obrigatório quando a funcionalidade envolver APIs externas, bancos de dados,
  filas de mensagens, usuários simultâneos, estado distribuído ou modificar um
  limite existente (API, esquema de eventos ou formato dos dados armazenados).
  Omita esta seção para funcionalidades locais, de usuário único e sem estado,
  que não tenham uma versão anterior.
-->

- O que acontece quando [dependência externa] fica indisponível ou excede o tempo limite?
- O que acontece quando dois usuários executam [ação] simultaneamente no mesmo recurso?
- O que acontece quando [operação] falha parcialmente, por exemplo, quando o pagamento é cobrado, mas a confirmação não é enviada?
- Qual modelo de consistência é necessário? (imediata, eventual ou de melhor esforço)
- O que acontece durante uma implantação parcial quando alguns consumidores ainda executam a versão anterior?
- O que acontece quando um consumidor atrasado (fila pendente, cliente em cache ou aplicativo móvel) retoma o processamento depois que o produtor avançou?
- O que acontece se a reversão for acionada depois que o estado tiver sido gravado no novo formato?

## Requisitos *(obrigatório)*

<!--
  AÇÃO NECESSÁRIA: O conteúdo desta seção contém placeholders.
  Preencha-os com os requisitos funcionais adequados.
-->

### Requisitos Funcionais

- **FR-001**: O sistema DEVE [recurso específico, por exemplo: "permitir que usuários criem contas"]
- **FR-002**: O sistema DEVE [recurso específico, por exemplo: "validar endereços de e-mail"]
- **FR-003**: Os usuários DEVEM poder [interação principal, por exemplo: "redefinir a senha"]
- **FR-004**: O sistema DEVE [requisito de dados, por exemplo: "persistir as preferências dos usuários"]
- **FR-005**: O sistema DEVE [comportamento, por exemplo: "registrar todos os eventos de segurança"]

*Exemplo de como marcar requisitos pouco claros:*

- **FR-006**: O sistema DEVE autenticar usuários por meio de [NEEDS CLARIFICATION: método de autenticação não especificado, e-mail e senha, SSO ou OAuth?]
- **FR-007**: O sistema DEVE reter os dados dos usuários por [NEEDS CLARIFICATION: período de retenção não especificado]

### Entidades Principais *(inclua se a funcionalidade envolver dados)*

- **[Entidade 1]**: [O que representa e seus principais atributos, sem detalhes de implementação]
- **[Entidade 2]**: [O que representa e suas relações com outras entidades]

## Critérios de Sucesso *(obrigatório)*

<!--
  AÇÃO NECESSÁRIA: Defina critérios de sucesso mensuráveis.
  Eles devem ser independentes de tecnologia e permitir medição.
-->

### Resultados Mensuráveis

- **SC-001**: [Métrica mensurável, por exemplo: "Usuários criam uma conta em menos de dois minutos"]
- **SC-002**: [Métrica mensurável, por exemplo: "O sistema atende mil usuários simultâneos sem degradação"]
- **SC-003**: [Métrica de satisfação do usuário, por exemplo: "90% dos usuários concluem a tarefa principal na primeira tentativa"]
- **SC-004**: [Métrica de negócio, por exemplo: "Reduzir em 50% os chamados de suporte relacionados a [X]"]

## Critérios de Conformidade *(obrigatório)*

<!--
  Testes de aceitação derivados diretamente da especificação.
  Toda implementação deve atender a estes critérios.
  Formato: Entrada -> Resultado Esperado (independente de tecnologia).
-->

### Casos de Conformidade

| ID | Cenário | Entrada | Resultado Esperado |
|----|----------|-------|-----------------|
| CC-001 | [Cenário principal de sucesso] | [Dados de entrada válidos] | [Resultado esperado] |
| CC-002 | [Cenário de erro] | [Dados inválidos ou incompletos] | [Mensagem de erro ou comportamento] |
| CC-003 | [Caso extremo] | [Condição de limite ou extrema] | [Comportamento esperado] |
| CC-004 | [O que NÃO deve acontecer] | [Entrada que poderia causar um comportamento incorreto] | [Comportamento que não deve ocorrer, por exemplo: "Não deve criar registros duplicados"] |
| CC-C01 | [Coexistência de versões ou reversão diante do estado existente] | [Carga gravada ou ação executada pela nova versão] | [A versão anterior continua processando sem erro ou a reversão restaura o comportamento anterior diante dos dados existentes] |

<!--
  Estes critérios servem como contrato entre a especificação e a implementação.
  O agente deve verificar a implementação conforme estes casos.
  Inclua pelo menos um caso negativo (CC-XXX com cenário "Não deve")
  para definir o que o sistema nunca deve fazer.
  Quando o tipo de mudança não for "new surface", inclua pelo menos um caso
  CC-C* (compatibilidade) que cubra coexistência de versões, comportamento de
  consumidores atrasados ou reversão diante de estado gravado pela nova versão.
-->

## Invariantes

<!--
  Propriedades transversais que devem ser SEMPRE verdadeiras,
  independentemente da abordagem de implementação.
  Invariantes descrevem a PROPRIEDADE, não um caso de teste específico.
  Inclua esta seção quando a funcionalidade envolver alterações de estado,
  transações financeiras, acesso simultâneo ou integrações externas.
  Omita esta seção para funcionalidades simples, somente de leitura ou de interface.
-->

- [Propriedade que deve ser sempre verdadeira, por exemplo: "Cada chave de idempotência corresponde a no máximo uma transação"]
- [Restrição de segurança, por exemplo: "O saldo da conta nunca pode ser negativo"]
- [Regra de consistência, por exemplo: "A soma dos itens deve ser igual ao total do pedido"]
- [Invariante de compatibilidade, quando aplicável, por exemplo: "Eventos gravados pela versão N+1 devem continuar desserializáveis pela versão N durante um ciclo de lançamento"]

## Compatibilidade e Transição *(obrigatório quando o tipo de mudança não for "new surface")*

<!--
  Obrigatório quando a funcionalidade modificar, substituir ou remover um limite
  existente: API pública ou interna, payload de evento, formato de dados
  armazenados, contrato voltado ao usuário, parâmetro CLI, chave de configuração
  ou qualquer comportamento observável consumido por outras equipes ou clientes
  de longa duração.
  Para um novo limite puramente aditivo, escreva "N/A: purely additive new surface"
  e omita o restante desta seção.
-->

- **Consumidores existentes**: [Produtores, consumidores ou clientes conhecidos do comportamento atual que devem ser considerados. Inclua consumidores atrasados, como filas pendentes, clientes em cache ou aplicativos móveis.]
- **Compatibilidade retroativa**: [estrita (sem quebra), período tolerante (versões antiga e nova coexistem por N lançamentos) ou quebra com período de descontinuação de pelo menos X]
- **Requisito de coexistência**: [Se as versões N e N+1 devem ser executadas simultaneamente no estado compartilhado e por quanto tempo. Expresse o período mínimo de coexistência em termos de negócio, não de implementação.]
- **Requisito de reversão**: [Se a versão anterior deve conseguir ler com segurança o estado gravado pela nova versão ou se uma correção posterior é aceitável. Inclua o período máximo aceitável para reversão.]
- **Sinal de descontinuação**: [Como os consumidores do fluxo antigo serão informados da remoção. Declare como requisito, não como mecanismo de implementação.]
- **Requisito de telemetria**: [Qual sinal confirma que os consumidores antigos deixaram de usar o fluxo anterior. É obrigatório antes de autorizar qualquer etapa de remoção destrutiva.]

## Especificações Relacionadas

<!--
  Links bidirecionais para especificações relacionadas.
  Especificações de migração que afetam ou dependem desta funcionalidade.
  Outras especificações de funcionalidades com dependências compartilhadas.
-->

- [MS-001: Nome da migração](../migrations/<nome-curto>/spec.md) - [Descrição da relação]
- [FS-001: Nome da funcionalidade](../features/<nome-curto>/spec.md) - [Descrição da relação]

## Registro de Evolução da Especificação *(obrigatório)*

<!--
  Registre aqui cada mudança nesta especificação e seu motivo.
  Este registro mantém o histórico de versões da especificação. O campo
  `Status` no início do documento continua sendo a fonte oficial do estado
  geral atual da especificação (Rascunho, Aprovada, Em Andamento, Validada,
  Substituída). A linha mais recente do registro espelha esse estado na
  coluna Resumo da Mudança.
-->

| Versão | Data | Resumo da Mudança | Motivo | Autor |
|---------|------|----------------|---------|--------|
| 1.0 | [YYYY-MM-DD] | Rascunho inicial | new work | [Nome ou função] |
