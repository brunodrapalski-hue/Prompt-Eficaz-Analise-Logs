# Prompt para análise de logs de infraestrutura

Prompt para que uma IA (ChatGPT, Claude, Gemini, Copilot etc.) leia um trecho de log bruto e:

- identifique erros, falhas e comportamentos anômalos;
- explique resumidamente o que pode estar acontecendo;
- sugira uma solução simples.

## Entregáveis

| Arquivo | Conteúdo |
|---|---|
| [prompt.md](prompt.md) | O prompt completo, pronto para colar na IA |
| [exemplo-log.txt](exemplo-log.txt) | O log de exemplo do desafio |
| [resposta-esperada.md](resposta-esperada.md) | A resposta esperada, com a interpretação de cada mensagem relevante |
| [validacao/](validacao/) | Respostas reais de quatro IAs ao prompt |

## Como usar

1. Abra o [prompt.md](prompt.md) e copie o bloco de texto.
2. Cole na IA, substitua `COLE O LOG AQUI` pelo log, mantendo as tags `<log>` e `</log>`, e envie.

> Antes de colar logs reais em uma IA pública, remova ou mascare dados sensíveis (IPs públicos, nomes de clientes, usuários, senhas).

## Como o prompt foi construído

| Instrução do prompt | Por quê |
|---|---|
| **Papel** ("atue como um analista de infraestrutura") | Direciona o vocabulário e o nível técnico da resposta |
| **Apenas o que está no log**, sem inventar | Reduz respostas com eventos, equipamentos ou configurações imaginados |
| **Citar horário e mensagem** | Permite conferir cada conclusão contra o log |
| **Evidenciado × hipótese** | Um log mostra sintomas, não certezas; a resposta precisa deixar isso claro |
| **Causa × consequência** | Evita tratar como problemas separados os eventos que são efeito de outro. No exemplo, os 6 eventos de Spanning Tree são consequência da queda de um link |
| **Resumo primeiro, depois problemas por prioridade** | Quem lê entende a situação em segundos. A prioridade considera a severidade e o impacto, e não a ordem cronológica |
| **Evidência, diagnóstico, impacto, ação e confirmação** para cada problema | Transforma a análise em próximos passos práticos |
| **Comandos só da plataforma identificada** | A resposta pode ser executada; se a plataforma não for clara, a IA não inventa comandos |
| **Informar os dados que faltam** | Quando o log não basta, a resposta orienta a continuação da análise |
| **Resposta curta, em português** | Atende ao pedido de explicação resumida e solução simples |

## Exemplo de log

O [exemplo-log.txt](exemplo-log.txt) é o log fornecido no desafio. No PDF, algumas mensagens estavam quebradas em duas linhas; aqui, cada evento ocupa uma linha, como em um log real. O conteúdo das mensagens não foi alterado.

O log mostra, em 13 segundos:

- a queda de um link, com a reconvergência do Spanning Tree que ela provocou;
- um duplex mismatch;
- uma violação de port security;
- uma alteração de configuração remota.

É um bom teste para verificar se a IA diferencia causa de consequência e prioriza corretamente.
