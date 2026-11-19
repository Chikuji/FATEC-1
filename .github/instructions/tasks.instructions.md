<!-- devsquad-template: .github/instructions/tasks.instructions.md vunknown sha=3f63686c57bb -->
---
name: 'Listas de Tarefas'
description: 'Diretrizes para criar e editar arquivos de decomposição de tarefas'
applyTo: 'docs/features/**/tasks.md'
---

Ao editar listas de tarefas, siga estas regras:

- As tarefas DEVEM ser organizadas por história de usuário para permitir implementação independente.
- Formato de cada tarefa: `- [ ] [P?] Descrição com caminho do arquivo`
- [P] indica uma tarefa que pode ser executada em paralelo.
- Fases obrigatórias: Preparação, Fundamentação, Histórias de Usuário (P1, P2, P3...) e Finalização.
- Dentro de cada história: Modelos -> Serviços -> Endpoints -> Integração.
- Cada fase deve ser um incremento completo e testável de forma independente. A primeira tarefa de cada história de usuário deve ser um tracer bullet: um recorte vertical mínimo, de ponta a ponta e por todas as camadas, que comprove o funcionamento da arquitetura. As tarefas seguintes completam os casos restantes, o tratamento de erros e os casos extremos.
- NÃO gere tarefas de teste separadas. Os testes fazem parte dos critérios de aceitação de cada tarefa. O agente de implementação verifica a cobertura ao concluir a tarefa.
- ADRs ausentes devem ser tratados como tarefas bloqueadoras na fase de Fundamentação.
- Ao criar itens de trabalho no quadro, aplique a lista de verificação da skill `work-item-creation`.
