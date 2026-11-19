# Especificação de Migração: [NOME_DO_SISTEMA]

- **Criado em**: [DATA]
- **Status**: Rascunho
- **Tipo**: [Lift-and-Shift | Rehost | Replatform-Lite]

## Resumo Executivo

<!--
  Resuma o escopo da migração nos principais pontos de decisão.
  Sirva como referência rápida para agentes e desenvolvedores.
  Atualize sempre que a especificação mudar significativamente.
-->

- **Objetivo**: [Uma frase que descreva o que será migrado e por quê]
- **Ambiente de origem**: [Onde o sistema é executado atualmente]
- **Ambiente de destino**: [Onde o sistema será executado após a migração]
- **Escopo**: [Sistemas e componentes incluídos nesta migração]
- **Meta de indisponibilidade**: [Sem indisponibilidade | Janela de manutenção planejada de X | Máximo de X minutos]
- **Critério de sucesso principal**: [Métrica mais importante, por exemplo: "Nenhuma perda de dados e menos de cinco minutos de indisponibilidade"]

## Fora do Escopo *(obrigatório)*

<!--
  Declare explicitamente o que NÃO faz parte desta migração.
  Isso evita modernizações acidentais, a causa mais comum de falha em migrações lift-and-shift.
  Inclua pelo menos um item.
-->

- [O que está excluído e o motivo da exclusão]

## Premissas

<!--
  Documente as premissas de infraestrutura e ambiente.
  Exemplos:
  - "A latência de rede entre a origem e o destino é equivalente à latência local atual"
  - "O mesmo mecanismo e a mesma versão do banco de dados estão disponíveis no ambiente de destino"
  - "O mecanismo de autenticação não será alterado durante a migração"
-->

- [Premissa e justificativa]

## Mapeamento do Sistema *(obrigatório)*

<!--
  Mapeamento de componentes do ambiente de origem para o de destino.
  Todos os componentes no escopo devem aparecer aqui.
  Este é o principal insumo para gerar IaC.
-->

| Componente | Origem | Destino | Observações sobre a Migração |
|-----------|--------|--------|-----------------|
| [Serviço de API] | [VM (Linux, Ubuntu 22.04)] | [Azure VM / AKS] | [Conteinerização ou rehosting direto] |
| [Banco de Dados] | [SQL Server 2019] | [Azure SQL Managed Instance] | [Compatibilidade de backup e restauração] |
| [Fila de Mensagens] | [RabbitMQ 3.12] | [Azure Service Bus] | [Necessário mapear protocolos] |
| [Armazenamento] | [Montagem NFS] | [Azure Blob Storage] | [Necessário mapear caminhos] |

## Paridade de Ambiente *(obrigatório)*

<!--
  Defina restrições explícitas para equivalência entre os ambientes.
  O ambiente de destino deve atender a todos os requisitos de paridade listados.
  Documente divergências com justificativa e avaliação de riscos.
-->

- **Versões do runtime**: [Liste versões específicas que DEVEM corresponder, por exemplo: ".NET 8.0.x", "Node.js 20.x"]
- **Versões do sistema operacional**: [Por exemplo: "Ubuntu 22.04 LTS ou equivalente"]
- **Versões do banco de dados**: [Por exemplo: "Nível de compatibilidade 150 do SQL Server 2019"]
- **Equivalência de configuração**: [Por exemplo: "Todas as variáveis de ambiente e arquivos de configuração devem produzir comportamento de runtime idêntico"]
- **Dependências**: [Por exemplo: "Todos os pacotes de terceiros devem usar a mesma versão principal"]
- **Divergências**: [Quaisquer lacunas conhecidas de paridade e suas justificativas]

## Cenários e Testes de Migração *(obrigatório)*

<!--
  Sequências de migração testáveis que substituem as histórias de usuário.
  Cada cenário representa uma fase crítica da migração que deve ser validada.
  Atribua números às fases (P1 = deve ser concluída primeiro, P2 = depende de P1 etc.).
  Diferentemente das histórias de funcionalidades, estas fases costumam ser
  sequenciais e não podem ser implantadas de forma independente.
-->

### Cenário de Migração 1: [Título Breve] (Fase: P1)

[Descreva esta fase da migração em linguagem simples]

**Motivo desta fase**: [Explique a ordem das dependências e a criticidade]

**Validação**: [Como verificar se esta fase foi concluída com sucesso]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação de migração], **Então** [resultado esperado]
2. **Dado** [estado inicial], **Quando** [ação de migração], **Então** [resultado esperado]

---

### Cenário de Migração 2: [Título Breve] (Fase: P2)

[Descreva esta fase da migração em linguagem simples]

**Motivo desta fase**: [Explique a ordem das dependências e a criticidade]

**Validação**: [Como verificar se esta fase foi concluída com sucesso]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação de migração], **Então** [resultado esperado]

---

### Cenário de Migração 3: [Título Breve] (Fase: P3)

[Descreva esta fase da migração em linguagem simples]

**Motivo desta fase**: [Explique a ordem das dependências e a criticidade]

**Validação**: [Como verificar se esta fase foi concluída com sucesso]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação de migração], **Então** [resultado esperado]

---

[Adicione outros cenários conforme necessário e mantenha a ordem de dependência entre as fases]

### Cenários de Risco

<!--
  Modos de falha e caminhos de recuperação que devem ser testados.
  Inclua pelo menos dois: um relacionado a dados e outro à infraestrutura.
-->

- O que acontece quando [a sincronização de dados falha durante a transferência]?
- O que acontece quando [o ambiente de destino fica inacessível durante o cutover]?
- Como o sistema trata [a reversão após a troca parcial do tráfego]?

## Estratégia de Migração *(obrigatório)*

<!--
  Abordagem de alto nível para executar a migração.
  Esta seção orienta a geração de artefatos pelo agente de planejamento (IaC, scripts e pipelines).
-->

- **Abordagem de migração**: [Backup e restauração | CDC (Change Data Capture) | Gravação dupla | Snapshot e reprodução]
- **Modelo de implantação**: [Blue/green | Execução paralela | Cutover no local]
- **Mecanismo de troca de tráfego**: [Troca de DNS | Reconfiguração do balanceador de carga | Feature flag | Roteamento na aplicação]
- **Estratégia de sincronização**: [Única execução | Carga inicial completa e alterações incrementais | Replicação contínua]

## Migração de Dados *(obrigatório)*

<!--
  Plano detalhado para a migração de dados.
  Esta é a parte central de uma migração lift-and-shift. Especificações de dados
  incompletas são a principal causa de falhas de migração.
-->

- **Volume de dados**: [Tamanho total estimado em todos os armazenamentos de dados]
- **Estratégia de sincronização**: [Carga inicial completa e sincronização incremental | Execução única durante a janela de manutenção]
- **Tratamento de alterações**: [Como as mudanças ocorridas durante a janela de migração são capturadas e aplicadas]
- **Regras de validação**:
  - As contagens de linhas devem corresponder na origem e no destino
  - Validação de checksum nas tabelas críticas: [liste as tabelas]
  - Integridade referencial verificada após a migração
  - [Regras adicionais de validação específicas do domínio]
- **Congelamento de dados**: [Quando e como as operações de gravação serão interrompidas antes do cutover]

## Plano de Cutover *(obrigatório)*

<!--
  Etapas ordenadas para executar a migração.
  Cada etapa deve ter um responsável claro, uma estimativa de duração e um critério de sucesso.
  O agente de planejamento usa estas informações para gerar pipelines de implantação.
-->

1. **Validação pré-cutover**: [Verifique a prontidão do ambiente de destino]
2. **Sincronização inicial de dados**: [Transferência completa dos dados]
3. **Sincronização incremental**: [Replicação das alterações pendentes]
4. **Congelamento de gravações**: [Interrompa as gravações no sistema de origem]
5. **Sincronização incremental final**: [Aplique as alterações restantes]
6. **Validação de dados**: [Contagens de linhas, checksums e verificações de integridade]
7. **Troca de tráfego**: [Execute a mudança de DNS ou do balanceador de carga]
8. **Monitoramento pós-cutover**: [Verifique a integridade do sistema durante X minutos]
9. **Declarar sucesso ou reverter**: [Critérios para a decisão]

## Plano de Reversão *(obrigatório)*

<!--
  Etapas para retornar ao ambiente de origem se a migração falhar.
  Devem ser testadas antes do cutover.
-->

- **Acionador da reversão**: [Condições que acionam a reversão, por exemplo: "falha na validação de dados", "taxa de erro acima de X%"]
- **Etapas de reversão**:
  1. [Redirecione o tráfego para o sistema de origem]
  2. [Retome as gravações na origem]
  3. [Verifique se o sistema de origem está operacional]
  4. [Avalie e preserve os dados gravados no destino durante o cutover]
- **Tempo máximo de reversão**: [X minutos entre a decisão e a conclusão da reversão]
- **Compatibilidade do estado durante a reversão**: [Use "A origem consegue ler com segurança o estado gravado pelo destino até a etapa <N>" ou "A reversão só é viável antes da etapa <N>. Depois disso, é necessária uma correção posterior".]
- **Reversão testada**: [Sim/Não, data do último teste]

## Requisitos *(obrigatório)*

### Requisitos Funcionais

<!--
  Requisitos de comportamento do sistema que devem ser atendidos após a migração.
  Concentre-se na paridade: o sistema DEVE se comportar de forma idêntica à origem.
-->

- **FR-001**: O sistema DEVE [requisito de paridade comportamental, por exemplo: "retornar respostas de API idênticas para todos os endpoints existentes"]
- **FR-002**: O sistema DEVE [requisito de dados, por exemplo: "preservar todos os dados históricos sem perda de registros"]
- **FR-003**: O sistema DEVE [requisito de integração, por exemplo: "manter todas as integrações externas existentes"]

### Requisitos Não Funcionais *(obrigatório)*

<!--
  Requisitos de desempenho, confiabilidade e operação para o ambiente de destino.
  Eles definem as divergências aceitáveis em relação ao comportamento do ambiente de origem.
-->

- **NFR-001**: [Paridade de latência, por exemplo: "A latência das respostas de API não deve aumentar mais de 10% em relação à linha de base da origem"]
- **NFR-002**: [Throughput, por exemplo: "O sistema deve processar X solicitações por segundo, conforme o pico atual de produção"]
- **NFR-003**: [Disponibilidade, por exemplo: "SLA de disponibilidade de 99,9% após a migração"]
- **NFR-004**: [Taxa de erro, por exemplo: "A taxa de erro não deve exceder a linha de base atual de X%"]

## Critérios de Sucesso *(obrigatório)*

<!--
  Resultados mensuráveis que definem o sucesso da migração.
  Sempre que possível, eles devem ser verificáveis por testes automatizados.
-->

### Resultados Mensuráveis

- **SC-001**: [Integridade dos dados, por exemplo: "Nenhuma perda de dados, com 100% de correspondência nas contagens de linhas e checksums entre origem e destino"]
- **SC-002**: [Indisponibilidade, por exemplo: "A indisponibilidade total durante o cutover é inferior a X minutos"]
- **SC-003**: [Paridade, por exemplo: "Paridade de 100% das respostas de API validada pelo conjunto de testes de referência"]
- **SC-004**: [Reversão, por exemplo: "A reversão é concluída com sucesso em até X minutos no teste pré-cutover"]
- **SC-005**: [Operação, por exemplo: "Monitoramento e alertas operacionais até X minutos após o cutover"]

## Critérios de Conformidade *(obrigatório)*

<!--
  Testes de aceitação derivados diretamente da especificação.
  Toda implementação deve atender a estes critérios.
  Formato: Entrada/Condição -> Resultado Esperado (independente de tecnologia).
  Inclua pelo menos três casos: validação de paridade, falha ou reversão e caso extremo.
-->

### Casos de Conformidade

| ID | Cenário | Entrada / Condição | Resultado Esperado |
|----|----------|-------------------|-----------------|
| CC-001 | [Paridade dos dados após a sincronização completa] | [Consulta: SELECT COUNT(*) em todas as tabelas] | [Contagens idênticas na origem e no destino] |
| CC-002 | [Paridade da API após o cutover] | [Reprodução no destino do conjunto de solicitações de referência] | [Corpos de resposta e códigos de status idênticos] |
| CC-003 | [Execução da reversão] | [Acionar a reversão durante o cutover] | [Sistema de origem totalmente operacional em até X minutos] |
| CC-004 | [Recuperação após falha parcial] | [Interrupção de rede durante a sincronização incremental] | [Sincronização retomada a partir do último checkpoint, sem corrupção de dados] |

<!--
  Estes critérios servem como contrato entre a especificação e a implementação.
  O agente deve verificar a implementação conforme estes casos.
-->

## Especificações Relacionadas

<!--
  Links bidirecionais para especificações relacionadas.
  Especificações de funcionalidades que dependam desta migração ou sejam afetadas por ela.
  Outras especificações de migração para sistemas relacionados.
-->

- [FS-001: Nome da funcionalidade](../features/<nome-curto>/spec.md) - [Descrição da relação]
- [MS-001: Nome da migração](../migrations/<nome-curto>/spec.md) - [Descrição da relação]

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
