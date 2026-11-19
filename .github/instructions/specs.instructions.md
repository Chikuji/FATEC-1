<!-- devsquad-template: .github/instructions/specs.instructions.md vunknown sha=538e3f162d61 -->
---
name: 'Especificações de Funcionalidades'
description: 'Diretrizes para criar e editar especificações de funcionalidades'
applyTo: 'docs/features/**/spec.md'
---

Ao editar especificações de funcionalidades, siga estas regras:

- Concentre-se **no que** os usuários precisam e **por quê**, nunca em como implementar.
- Escreva para as partes interessadas do negócio, não para desenvolvedores.
- Priorize cada história de usuário (P1, P2, P3) e torne-a testável de forma independente.
- A especificação deve descrever um recorte vertical (tracer bullet) que atenda a três condições. Primeiro, deve ter pelo menos um caso de conformidade executado de ponta a ponta, não apenas uma verificação unitária. Segundo, alguém de fora da equipe de implementação deve poder demonstrá-lo, acioná-lo ou observá-lo sem consultar uma especificação futura. Terceiro, deve alcançar todas as camadas exigidas pela funcionalidade (dados, lógica e interface). Cada recorte atravessa todas as camadas de integração, não apenas uma camada horizontal. O dimensionamento é feito pela skill `complexity-analysis` durante a decomposição, não na especificação. Se um recorte for grande demais para estimar, a decomposição apontará isso.
- Cada requisito funcional deve ser testável e inequívoco. Termos vagos como "rápido", "fácil" e "intuitivo" devem ser quantificados.
- Os critérios de sucesso devem ser mensuráveis e independentes de tecnologia.
- Os critérios de conformidade devem conter: ID, cenário, entrada e resultado esperado.
- Inclua pelo menos três casos de conformidade: caminho principal, cenário de erro e caso extremo.
- Use no máximo três marcadores [NEEDS CLARIFICATION].
- O resumo executivo deve declarar um tipo de mudança: `new surface`, `additive to existing`, `modifies existing boundary` ou `removes existing surface`.
- O resumo executivo deve declarar `Descreve recurso de IA: yes | no`. Quando o valor for `yes`, a especificação deve preencher a seção `Custos de IA` (compromisso de nível do modelo, orçamento de latência, invariante de estabilidade do prompt, limite de custo por chamada e escalonamento de incidentes de custo). As restrições comportamentais do agente pertencem à seção geral `Invariantes`. A composição dos serviços pertence a `Requisitos` e `Cenários e Testes de Usuário`.
- Cada especificação deve conter uma seção `Registro de Evolução da Especificação` com pelo menos uma linha que registre a versão atual. Cada mudança posterior acrescenta uma linha com versão, data, resumo da mudança, motivo e autor. Os valores válidos para o motivo são `new work`, `drift`, `external constraint`, um dos três valores `failure (<category>)` definidos em `debugging-recovery/references/failure-taxonomy.md` (`spec`, `validation`, `agent`) ou `other (<short reason>)` como alternativa de transição.
- Quando o tipo de mudança não for `new surface`, a seção `Compatibilidade e Transição` é obrigatória e pelo menos um caso de conformidade (CC-C*) deve cobrir coexistência de versões, comportamento de consumidores atrasados ou reversão diante de estado gravado pela nova versão.
- Use `YYYY-MM-DD` como formato padrão para datas, inclusive `Criado em`. Outros formatos são permitidos se o repositório consumidor já tiver uma convenção estabelecida. Depois de escolhido, aplique o formato de maneira consistente em todas as especificações do repositório.
- Use o template em `docs/features/TEMPLATE.md` como referência de estrutura.
- Siga as regras de formatação da skill `documentation-style`.
