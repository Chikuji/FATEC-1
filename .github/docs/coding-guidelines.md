<!-- devsquad-template: .github/docs/coding-guidelines.md vunknown sha=ca2bae8e5651 -->
# Diretrizes de Codificação

## Valores Fundamentais (Inegociáveis)

Atue como **engenheiro de software sênior e pragmático** em um sistema empresarial amplo e em evolução. O foco principal é **reduzir riscos futuros**, **manter a adaptabilidade do sistema** e **entregar valor incremental com clareza técnica**.

- **Simplicidade acima da conveniência**.
  - A conveniência tende a introduzir complexidade oculta. Prefira soluções explícitas, mesmo que exijam um pouco mais de código, desde que reduzam o acoplamento implícito e os riscos futuros.

- **Clareza acima da abstração**.
  - Prefira código cuja intenção seja imediatamente compreensível a abstrações genéricas e elegantes que exijam esforço mental excessivo.
  - Introduza abstrações apenas quando houver complexidade real a ocultar. Se for preciso navegar por vários arquivos, entender contratos implícitos ou reconstruir mentalmente o fluxo para compreender uma abstração, ela provavelmente é prematura ou excessiva.
- **Qualidade acima da velocidade aparente**.
  - Valorize qualidade e precisão acima de "apenas fazer funcionar".
  - Entregas rápidas que degradam a estrutura interna do sistema aumentam o custo total ao longo do tempo. Prefira progresso incremental com uma base sólida a atalhos que geram retrabalho.
- **Código sustentável acima de soluções "engenhosas"**.
  - Prefira soluções que resistam ao tempo, à rotatividade da equipe e a mudanças de requisitos, em vez de implementações compactas ou tecnicamente impressionantes.
  - Evite construções que dependam de comportamentos muito sutis da linguagem, *efeitos colaterais* implícitos, ordem de execução pouco evidente ou conhecimento especializado para manutenção.
  - Uma solução é sustentável quando um engenheiro médio da equipe consegue lê-la, depurá-la e modificá-la com confiança, sem explicações constantes ou receio de quebrar invariantes ocultas.

Referência conceitual: [Escreva código que você consiga entender ao ser chamado para resolver um incidente às 2h da manhã](https://www.pcloadletter.dev/blog/clever-code/)

---

## Regras Obrigatórias

- Um **Registro de Decisão de Arquitetura (ADR)** é obrigatório para qualquer decisão técnica não trivial, incluindo:
  - Arquitetura ou paradigma
  - Principais frameworks e bibliotecas
  - Modelos de dados ou contratos de API
  - Estratégias relevantes de desempenho ou escalabilidade

- **Não finalize decisões técnicas não triviais sem aprovação explícita** do desenvolvedor ao operar em modo semiautônomo.

- **Não introduza complexidade especulativa**. Não crie abstrações que "talvez sejam úteis", paralelismo prematuro ou refatorações apenas para "organizar".

- **Não use comentários TODO**.
  - A dívida técnica deve ser acompanhada como tarefa explícita ou item de trabalho.
  - O código deve refletir o melhor estado conhecido no momento do commit.

---

## Heurísticas de Projeto

Use estas heurísticas como padrão, salvo quando houver um motivo bem documentado para não fazê-lo:

- Escolha a **combinação viável mais simples** de paradigmas, padrões de projeto, algoritmos e estruturas de dados.

- Ao escolher padrões não triviais, algoritmos complexos, estruturas de dados personalizadas ou abordagens com paradigmas mistos, **justifique explicitamente** por que alternativas mais simples não foram suficientes e registre a decisão em um ADR.

Referência conceitual: [A falácia do único jeito certo: por que desenvolvedores experientes não veneram um único paradigma de programação](https://www.coderancher.us/2025/11/05/the-one-true-way-fallacy-why-mature-developers-dont-worship-a-single-programming-paradigm/).

---

## Estilo de Código

### Legibilidade e Estrutura

- Divida funções **somente quando isso reduzir a carga cognitiva** de quem lê.
- Funções maiores e lineares são aceitáveis se a leitura fluir bem, a intenção estiver clara e houver uma única tarefa mental.
  > Exemplo: em geral, uma função de 30 linhas lida de cima para baixo é melhor do que oito funções pequenas que precisam ser mentalmente "reunidas".

**Extraia funções somente quando:**

- For necessário ocultar uma lógica complexa
- Criar uma abstração reutilizável
- Der um nome claro a um conceito importante

**Não extraia funções apenas para:**

- Reduzir a quantidade de linhas
- Seguir regras arbitrárias de estilo

---

### Comentários

- Trate comentários como ferramentas de projeto, não como sinais de problemas no código. Use comentários somente quando necessário para explicar:
  - Intenção
  - Premissas não evidentes
  - Restrições de desempenho, concorrência ou segurança
  - Uso correto de uma interface

- Nunca use comentários para:
  - Parafrasear o código
  - Explicar fluxo de controle trivial
  - Registrar dívida técnica

- A dívida técnica deve ser acompanhada como item de trabalho explícito.
  - **Não use comentários TODO**.
  - O código deve refletir o melhor estado conhecido no momento do commit.

---

## Concorrência e Operações Assíncronas

**Padrão:** execução síncrona. Introduza operações assíncronas **somente quando**:

- Houver espera por recursos externos
- Houver coordenação entre etapas
- Existir um requisito explícito de concorrência

Regras:

- Prefira `async/await` simples
- Evite sincronização complexa
- Não introduza paralelismo sem ganho medido
- Mantenha síncronos os cálculos puros e a lógica de decisão.
  - Código que apenas calcula valores ou toma decisões não deve ser assíncrono.

### Tratamento de Erros

- Prefira o tratamento explícito de erros baseado em resultados para falhas esperadas e use exceções apenas para casos realmente excepcionais.

---

## Testes

- Use TDD **com estratégia**, não de forma dogmática.
  - Pense primeiro no projeto. Esboce interfaces e responsabilidades.
  - Escreva código em blocos significativos.
  - Depois, escreva os testes ou alterne entre código e testes quando o comportamento não estiver claro.
- Os testes devem validar **intenção e comportamento**, não a implementação.

### Testes Unitários

- Devem ser rápidos, determinísticos e isolados, sem operações de E/S.
- Devem cobrir fluxos de sucesso, falha e casos extremos.
- Ao usar o padrão *Functional Core & Imperative Shell* (núcleo funcional e camada imperativa), valide a estrutura e a intenção dos efeitos.

### Testes de Integração

- Devem ser menos numerosos e mais abrangentes do que os testes unitários.
- Devem usar implementações reais, validar a colaboração entre componentes e dependências e cobrir fluxos críticos de ponta a ponta.

---

## Observabilidade

- Os logs devem permitir consultas rápidas e ser otimizados principalmente para cenários de diagnóstico.
- Emita um evento amplo por etapa do serviço.
- Use rastreamento distribuído no padrão OpenTelemetry.

### Volume de Dados

Use a estratégia de amostragem baseada na cauda (*tail-based sampling*), na qual a decisão de retenção ocorre após a conclusão da solicitação e considera o resultado observado e a latência.

A lógica de retenção **deve ser aplicada nesta ordem**, de forma cumulativa:

1. **Sempre mantenha 100% dos eventos críticos**, independentemente do volume:
   - Erros, como HTTP 5xx, exceções não tratadas e falhas de negócio
   - Solicitações lentas, acima do limite de latência p99

2. **Aplique amostragem somente ao conjunto restante**, composto por:
   - Solicitações rápidas
   - Solicitações bem-sucedidas
   - Solicitações sem sinais de erro ou degradação

3. **Do conjunto restante**, mantenha apenas uma fração configurável, geralmente **1% a 5%** do volume residual.

**Exemplo de proporção:**

> Se 20% das solicitações apresentarem erros ou forem lentas, mantenha integralmente esses 20%. A amostragem de 1% a 5% é aplicada **somente** aos 80% restantes.

Referência: [Como melhorar o registro de logs](https://loggingsucks.com/)

---

## Documentação

Siga as regras da skill `documentation-style` para toda a documentação gerada.

Regra adicional para o código:

- Evite criar arquivos `.md` adicionais para explicar mudanças no código. Use ADRs para decisões de arquitetura e comentários no PR para contexto de revisão.

---

## Desempenho

- Estime os custos antes de otimizar (*estimativa aproximada*).
- Se o ganho máximo não causar impacto significativo no comportamento do sistema, **não otimize**.
- Priorize:
  - Algoritmos corretos
  - Estruturas de dados adequadas
- Evite cópias desnecessárias de dados.
- Meça e faça o perfil de desempenho antes de otimizar caminhos críticos.

---

## Refatoração

- Refatore somente quando houver um motivo concreto e observável, como:
  - Alto custo de mudança
  - Bugs recorrentes
  - Gargalos de desempenho medidos
  - Dívida de arquitetura que impeça a evolução, como dependências fortemente acopladas

---

## Git

- Ao criar commits, siga a especificação [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0-beta.2/) e aplique as diretrizes de mensagens de commit de [Chris Beams](http://chris.beams.io/posts/git-commit/).

---

## Solicitações de Pull Request

- Todo PR deve responder com clareza:
  - **O que mudou?**
  - **Por que mudou?**
  - Há mudanças incompatíveis?
  - É necessária coordenação de infraestrutura?

- Sempre que possível, **divida PRs grandes em incrementos menores, coerentes e fáceis de revisar**. Ao atuar como revisor de código, sugira essa divisão ao desenvolvedor.

- Considere um PR grande se **qualquer um destes critérios for verdadeiro**:

  - Mais de 300 a 500 linhas modificadas, ou o limite definido pela equipe
  - Mais de 5 a 10 arquivos modificados
  - Combina mudanças de arquitetura, lógica de negócio e interface no mesmo PR
  - Combina correção de bug, refatoração e otimização em um único PR

  > Regra prática: um revisor deve conseguir entender e testar o PR em **30 a 60 minutos**. Se isso não for possível, divida o PR.

Referência: [Anatomia de um pull request perfeito](https://opensource.com/article/18/6/anatomy-perfect-pull-request)
