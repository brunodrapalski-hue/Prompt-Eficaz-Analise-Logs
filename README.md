# Análise de Logs de Infraestrutura com IA

Este repositório define a entrega para o **Challenge 2**, cujo objetivo é criar um prompt capaz de apoiar a análise de logs de infraestrutura utilizando uma IA.

A proposta foi construir um prompt que não apenas identificasse mensagens de erro, mas organizasse a análise de forma próxima a um processo real de troubleshooting: localizar os eventos relevantes, relacionar possíveis causas e consequências, diferenciar o que está evidenciado do que ainda é hipótese e transformar a interpretação em próximos passos práticos.

Depois de definir essa estrutura, usei o mesmo cenário de log para validar o prompt em quatro ferramentas diferentes: 

```bash
ChatGPT, Claude, Gemini e Microsoft Copilot.
```
Cada teste foi realizado em uma chat novo e sem referências anteriores, e as respostas foram preservadas no repositório para permitir a comparação posterior.

Não determinei qual IA é melhor, mas observei como cada uma interpretaria as mesmas instruções e as mesmas evidências. A partir disso, também fiz uma leitura comparativa das respostas, destacando diferenças de profundidade, organização, nível de inferência e aderência às regras definidas no prompt.

<br>

## Entregáveis

| Arquivo | Conteúdo |
|---|---|
| [prompt.md](prompt.md) | Prompt completo, pronto para ser utilizado em uma IA |
| [exemplo-log.txt](exemplo-log.txt) | Trecho de log utilizado na validação |
| [resposta-esperada.md](resposta-esperada.md) | Interpretação de referência para os eventos do log |
| [validacao/](validacao/) | Resultados obtidos ao testar o prompt em diferentes IAs |

<br>

## Como o prompt foi construído

Procurei responder aos desafios comuns em análises feitas por IA: receber uma resposta tecnicamente convincente, mas baseada em informações que não estavam presentes no log. Por isso, o prompt foi estruturado para manter a análise presa às evidências disponíveis, diferenciar fatos de hipóteses, considerar a relação entre os eventos e terminar com próximos passos práticos de investigação.

| Instrução do prompt | Intenção |
|---|---|
| **Atuar como analista de infraestrutura** | Direcionar o vocabulário e o nível técnico da resposta |
| **Basear a análise apenas no log** | Reduzir conclusões baseadas em eventos, equipamentos ou configurações não informados |
| **Citar horário e mensagem** | Permitir que cada conclusão seja conferida diretamente no log |
| **Diferenciar evidência de hipótese** | Evitar tratar uma possibilidade como causa confirmada |
| **Relacionar causa e consequência** | Evitar transformar eventos derivados do mesmo incidente em problemas independentes |
| **Priorizar os problemas** | Organizar a resposta considerando severidade e possível impacto |
| **Informar impacto e ação** | Transformar a interpretação em próximos passos úteis |
| **Sugerir uma forma de confirmação** | Permitir continuidade da investigação quando a plataforma puder ser identificada |
| **Informar o que falta** | Deixar claro quando o trecho de log não é suficiente para concluir a causa |

<br>

## Como usar

1. Abra o arquivo [prompt.md](prompt.md).
2. Copie o prompt completo.
3. Cole o conteúdo em uma IA de sua preferência.
4. Substitua `COLE O LOG AQUI` pelo trecho de log que deseja analisar, mantendo as tags `<log>` e `</log>`.
5. Envie a mensagem e compare a análise com as evidências presentes no log.

> Para reproduzir exatamente o cenário usado neste projeto, utilize o arquivo [exemplo-log.txt](exemplo-log.txt).

<br>

## Exemplo utilizado na validação

Para validar o prompt, utilizei o trecho de log fornecido no próprio desafio, mantendo o conteúdo das mensagens e organizando cada evento em uma única linha no arquivo [exemplo-log.txt](exemplo-log.txt).

O trecho contém, em uma sequência curta de eventos:

- queda de uma interface;
- reconvergência do Spanning Tree;
- alteração da root bridge;
- duplex mismatch;
- violação de Port Security;
- alteração de configuração via acesso remoto.

Esse conjunto foi útil para testar se a IA consegue diferenciar eventos relacionados entre si de problemas independentes.

<br>

## Resposta esperada

O arquivo [resposta-esperada](resposta-esperada.md) serve como referência para avaliar a interpretação produzida pela IA.

A intenção não é exigir uma resposta textual idêntica, mas verificar se a análise:

- identifica os principais eventos do log;
- relaciona a queda da interface com a reconvergência do Spanning Tree;
- diferencia fatos de hipóteses;
- trata o duplex mismatch e a violação de Port Security como problemas independentes;
- evita afirmar uma causa raiz quando o log não fornece evidências suficientes;
- indica próximos passos coerentes para continuar a investigação.

<br>

 ## Validação em diferentes IAs

Como validação complementar, executei o prompt em quatro ferramentas diferentes:

- ChatGPT `Plano Plus Alta`
- Claude `Plano Pro Opus Alto`
- Gemini  `Plano 3.1 Pro`
- Microsoft Copilot  `Plano Microsoft 365`

> Os testes foram feitos em conversas novas, sem referências anteriores, utilizando o mesmo trecho de log.

O objetivo não foi comparar ou classificar os modelos, mas observar se as instruções principais do prompt continuavam sendo respeitadas em ferramentas diferentes: identificar os eventos relevantes, relacionar causa e consequência, diferenciar evidência de hipótese e sugerir próximos passos coerentes.

> As respostas completas foram preservadas na pasta [validacao](validacao/) para permitir a conferência dos resultados.

<br>

## O que observei na validação

As respostas variaram em profundidade e forma de apresentação, mas os principais eventos do log foram identificados de maneira consistente. A queda da interface e os eventos seguintes de Spanning Tree foram, em geral, relacionados como parte do mesmo incidente. O duplex mismatch e a violação de Port Security também foram reconhecidos como problemas distintos.

As maiores diferenças apareceram no nível de inferência adotado por cada IA e na forma de organizar os eventos. Isso reforçou a importância de uma das regras do prompt: separar claramente o que está evidenciado no log do que ainda é hipótese.

<br>

## Conclusão

A principal preocupação nesta entrega foi tornar a análise útil e verificável: partir das evidências presentes no log, evitar conclusões sem suporte, organizar os eventos por prioridade e transformar a interpretação em próximos passos de investigação.

Os arquivos deste repositório permitem reproduzir o teste, comparar os resultados e revisar as decisões adotadas na construção do prompt.
