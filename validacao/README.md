# Validação do prompt

O [prompt](../prompt.md) foi executado com o [exemplo de log](../exemplo-log.txt) em diferentes IAs. As respostas completas estão nesta pasta, e a tabela abaixo compara cada uma com a [resposta esperada](../resposta-esperada.md).

| Critério | ChatGPT | Gemini | Claude | Copilot |
|---|:-:|:-:|:-:|:-:|
| Identificou a queda da Gi1/0/1 como causa raiz | | | | |
| Tratou os eventos de Spanning Tree como consequência | | | | |
| Explicou a mudança de root bridge | | | | |
| Identificou o duplex mismatch e a correção (mesma configuração nos dois lados) | | | | |
| Identificou a violação de port security e a possibilidade de *err-disabled* | | | | |
| Tratou a alteração via vty0 como ponto de auditoria | | | | |
| Citou a evidência (horário/trecho) em cada conclusão | | | | |
| Começou com um resumo e ordenou os problemas por prioridade | | | | |
| Informou evidência, diagnóstico, impacto, ação e confirmação em cada problema | | | | |
| Não inventou informações ausentes no log | | | | |

Legenda: ✅ atendeu · ⚠️ parcial · ❌ não atendeu

## Respostas

| IA | Data | Arquivo |
|---|---|---|
| ChatGPT | | [chatgpt.md](chatgpt.md) |
| Gemini | | [gemini.md](gemini.md) |
| Claude | | [claude.md](claude.md) |
| Copilot | | [copilot.md](copilot.md) |

## Observações

(Diferenças relevantes entre as respostas e o que elas indicam sobre o prompt.)
