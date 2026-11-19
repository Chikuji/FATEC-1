<!-- devsquad-template: .github/instructions/migration-specs.instructions.md vunknown sha=f0b00cbefd3a -->
---
name: 'Especificações de Migração'
description: 'Diretrizes para criar e editar especificações de migração'
applyTo: 'docs/migrations/**/spec.md'
---

Ao editar especificações de migração, siga estas regras:

- Concentre-se **no que** precisa ser migrado e **para onde**, nunca em como implementar (código IaC ou scripts).
- Escreva para responsáveis por infraestrutura e partes interessadas, não apenas para desenvolvedores.
- Cada cenário de migração deve ter um número de fase (P1, P2, P3) que reflita a ordem das dependências de execução. Diferentemente das histórias de funcionalidade, as fases de migração costumam ser sequenciais e não podem ser implantadas de forma independente.
- Cada requisito funcional deve afirmar a paridade comportamental: após a migração, o sistema DEVE se comportar de forma idêntica, a menos que uma divergência seja documentada e justificada explicitamente.
- Os requisitos não funcionais são obrigatórios. As tolerâncias de latência, throughput, disponibilidade e taxa de erro devem ser quantificadas em relação às linhas de base da origem.
- O mapeamento do sistema deve incluir todos os componentes no escopo, com origem e destino.
- A paridade de ambiente deve listar restrições específicas de versão para runtime, sistema operacional, banco de dados e dependências.
- A migração de dados deve incluir regras de validação, como contagens de linhas, checksums e integridade referencial.
- O plano de cutover deve ser uma sequência ordenada de etapas, cada uma com um critério claro de sucesso.
- O plano de cutover deve tratar explicitamente o redirecionamento de consumidores, incluindo consumidores atrasados (DNS ou strings de conexão em cache, filas pendentes e tarefas em lote atrasadas), e o mecanismo que os impede de acessar a origem desativada.
- Cada especificação de migração deve conter uma seção `Registro de Evolução da Especificação` com pelo menos uma linha que registre a versão atual. Cada mudança posterior acrescenta uma linha com versão, data, resumo da mudança, motivo e autor. Os valores válidos para o motivo são `new work`, `drift`, `external constraint`, um dos três valores `failure (<category>)` definidos em `debugging-recovery/references/failure-taxonomy.md` (`spec`, `validation`, `agent`) ou `other (<short reason>)` como alternativa de transição.
- O plano de reversão deve especificar as condições de acionamento, as etapas de reversão e o tempo máximo para concluí-la.
- O plano de reversão deve declarar se o ambiente de origem pode ler com segurança o estado produzido pelo destino durante o período de sobreposição ou indicar que a reversão só é viável antes de uma etapa específica do cutover.
- Os critérios de sucesso devem incluir métricas de integridade dos dados, tempo de indisponibilidade, paridade e teste de reversão.
- Os critérios de conformidade devem conter: ID, cenário, entrada ou condição e resultado esperado.
- Inclua pelo menos três casos de conformidade: validação de paridade, falha ou reversão e caso extremo.
- Use no máximo três marcadores [NEEDS CLARIFICATION].
- Use `YYYY-MM-DD` como formato padrão para datas, inclusive `Criado em`. Outros formatos são permitidos se o repositório consumidor já tiver uma convenção estabelecida. Depois de escolhido, aplique o formato de maneira consistente em todas as especificações do repositório.
- Use o template em `docs/migrations/TEMPLATE.md` como referência de estrutura.
- Siga as regras de formatação da skill `documentation-style`.
