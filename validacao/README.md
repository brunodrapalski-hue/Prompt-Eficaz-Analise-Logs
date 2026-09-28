# Validação do prompt

Respostas reais de quatro IAs ao [prompt](../prompt.md), executado com o [exemplo de log](../exemplo-log.txt).

| IA | Plano / modelo | Arquivo |
|---|---|---|
| ChatGPT | ChatGPT Plus, esforço alto | [chatgpt.md](chatgpt.md) |
| Gemini | Gemini 3.1 Pro | [gemini.md](gemini.md) |
| Claude | Claude Pro, Opus 5.5, esforço alto | [claude.md](claude.md) |
| Copilot | Microsoft 365 Copilot | [copilot.md](copilot.md) |

## Condições do teste

- Cada IA foi usada em um chat novo, sem referências a conversas anteriores.
- A última instrução do prompt testado foi "Responda de forma curta, objetiva.", sem "e em português do Brasil".
- O log foi colado como estava no PDF do desafio, com mensagens quebradas em duas linhas e uma linha extra (`Unset`).
- No ChatGPT, no Gemini e no Claude, depois da resposta, foi feita a pergunta "Porque respondeu com verificação em formato de CLI?".
