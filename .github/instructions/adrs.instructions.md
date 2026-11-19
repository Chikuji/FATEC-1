<!-- devsquad-template: .github/instructions/adrs.instructions.md vunknown sha=f5e8293d4b3b -->
---
name: 'Registros de Decisão de Arquitetura'
description: 'Diretrizes para criar e gerenciar ADRs'
applyTo: 'docs/architecture/decisions/*.md'
---

Ao editar Registros de Decisão de Arquitetura (ADRs), siga estas regras:

- Use o template em `docs/architecture/decisions/ADR-TEMPLATE.md`.
- Nome dos arquivos: `NNNN-domain.md` (use o domínio da decisão, não a opção escolhida).
- O título do ADR deve ser o domínio, por exemplo, "Persistência de Dados", e não "Usar PostgreSQL".
- Campos obrigatórios: Status, Data, Contexto, Prioridades e Requisitos (ordenados), Opções Consideradas (avaliadas conforme as prioridades) e Decisão.
- Defina as prioridades ANTES de listar as opções. Avalie as opções em relação a cada prioridade.
- Não use listas genéricas de vantagens e desvantagens. Avalie cada opção conforme as prioridades ordenadas.
- Status válidos: Proposed, Accepted, Superseded by NNNN.
- Crie ADRs com Status `Proposed` e solicite a revisão de pelo menos outro membro da equipe antes de alterar o status para `Accepted`. O agente deve alertar quando o usuário tentar aceitar um ADR sem revisão.
- Prefira opções fornecidas pelo usuário em vez de opções inventadas pelo agente.
- Nunca infira opções a partir de templates, READMEs genéricos ou conteúdo de placeholders.
- Siga as regras de formatação da skill `documentation-style`.
