<!-- devsquad-template: .github/instructions/migration-tasks.instructions.md vunknown sha=bd6d004b4dcd -->
---
name: 'Listas de Tarefas de Migração'
description: 'Diretrizes para criar e editar arquivos de decomposição de tarefas de migração'
applyTo: 'docs/migrations/**/tasks.md'
---

Ao editar listas de tarefas de migração, siga estas regras:

- As tarefas DEVEM ser organizadas por fase de migração para refletir dependências sequenciais de execução.
- Formato de cada tarefa: `- [ ] [P?] Descrição com caminho do arquivo`
- [P] indica uma tarefa que pode ser executada em paralelo.
- Fases obrigatórias: Preparação, Fundamentação, Provisionamento de Infraestrutura, Preparação da Migração de Dados, Automação do Cutover, Reversão e Validação, Finalização.
- Dentro de Provisionamento de Infraestrutura: Computação -> Rede -> Armazenamento -> Identidade -> Configuração.
- Dentro de Preparação da Migração de Dados: mecanismo de sincronização inicial -> captura de alterações -> pipeline de validação.
- As dependências entre fases devem ser explícitas. Em geral, as fases são sequenciais e não podem ser implantadas de forma independente.
- NÃO gere tarefas de teste separadas. Os testes fazem parte dos critérios de aceitação de cada tarefa. O agente de implementação verifica a cobertura ao concluir a tarefa.
- ADRs ausentes devem ser tratados como tarefas bloqueadoras na fase de Fundamentação.
- Tarefas de validação de dados e de teste de reversão são obrigatórias em qualquer migração.
- Ao criar itens de trabalho no quadro, aplique a lista de verificação da skill `work-item-creation`.
