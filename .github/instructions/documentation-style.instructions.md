<!-- devsquad-template: .github/instructions/documentation-style.instructions.md vunknown sha=adba33ca6141 -->
---
name: 'Estilo da Documentação'
description: 'Regras de formatação e estilo para documentação Markdown'
applyTo: 'docs/**/*.md'
---

Ao editar documentação Markdown, siga estas regras de formatação:

- Não deixe erros ortográficos.
- Não use emojis ou caracteres Unicode decorativos, como setas, marcadores, estrelas, sinais de confirmação ou símbolos semelhantes.
- Não introduza travessões (U+2014) em texto Markdown novo ou reescrito. Reformule a frase.
- Evite ponto e vírgula em prosa nova ou reescrita. Reformule naturalmente com uma conjunção, uma estrutura mais clara ou frases separadas quando apropriado. Preserve o sentido original e evite transformar ideias conectadas em frases curtas e abruptas. Mantenha ponto e vírgula exigido por código, sintaxe técnica ou citações literais.
- Preserve o texto existente fora do escopo solicitado. Não examine nem reescreva documentos apenas para remover travessões ou pontos e vírgulas existentes.
- Não use hífens ou travessões para separar conceitos ("conceito A - conceito B"). Reformule a frase.
- Não use `#<number>` em texto livre, pois o Azure DevOps converte esse formato em link para item de trabalho. Use "primeira opção", "terceira opção" e assim por diante.
- Não use linguagem promocional, como "não é apenas X, é Y", "vai além" ou "mais do que apenas". Descreva diretamente.
- Não use perguntas retóricas seguidas de respostas óbvias. Declare a ideia diretamente.
- Prefira listas e tabelas a parágrafos longos.
- Prefira vocabulário simples e amplamente compreendido. Siga a skill `documentation-style` para exemplos e exceções que preservem a precisão técnica.
- Prefira nomes descritivos e links a identificadores inventados. Siga a skill `documentation-style` para exceções exigidas por regras específicas de artefatos e para preservar referências existentes.
  - Mermaid: identifique a plataforma pelo endereço remoto (`git config --get remote.origin.url`).
    - Azure DevOps (`dev.azure.com` ou `visualstudio.com`): use `:::mermaid` e `:::`
    - GitHub (`github.com`): use um bloco de código com ` ```mermaid ` e ` ``` `
  - Converta blocos existentes para o formato correto da plataforma.
