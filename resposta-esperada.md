# Resposta esperada

Resposta de referência (gabarito) para o [prompt](prompt.md) aplicado ao [exemplo de log](exemplo-log.txt). Ela segue o formato pedido no prompt e serve para avaliar a resposta de qualquer IA.

---

## Resumo

O link GigabitEthernet1/0/1 caiu às 14:01:22 e provocou uma reconvergência do Spanning Tree na VLAN 10, com mudança da root bridge. Nos segundos seguintes aparecem três problemas independentes: uma violação de port security na Gi1/0/5, um duplex mismatch com o switch SW-Backup e uma alteração de configuração feita remotamente. O log é de um switch Cisco IOS.

## Problemas identificados (em ordem de prioridade)

### 1. Queda do link Gi1/0/1, com reconvergência do Spanning Tree

- **Evidência:**
  - `14:01:22.123 %LINK-3-UPDOWN: Interface GigabitEthernet1/0/1, changed state to down` (severidade 3);
  - em seguida, os eventos `SPANTREE` de 14:01:23 a 14:01:28: Topology Change na VLAN0010, Gi1/0/3 para BLOCKING, Gi1/0/4 até FORWARDING e `Root changed ... New root is 00e0.b6ff.ee11, cost is 19, port Gi1/0/4`.
- **Diagnóstico provável:**
  - **Evidenciado:** a Gi1/0/1 caiu na camada física. Não foi um `shutdown` local, que apareceria como "administratively down".
  - **Evidenciado:** os 6 eventos de Spanning Tree são **consequência** da queda. A topologia foi recalculada e a root bridge da VLAN 10 mudou.
  - **Hipótese:** a Gi1/0/1 era o caminho para a root anterior.
  - **Hipótese:** o custo 19 corresponde, no método padrão, a um enlace de 100 Mbps, o que indica um novo caminho mais lento.
- **Impacto:** interrupção temporária do tráfego da VLAN 10 durante a reconvergência e possível lentidão no novo caminho.
- **Ação:**
  - Verificar cabo, transceptor (SFP) e o equipamento vizinho da Gi1/0/1, e restabelecer o link.
  - Depois, definir de forma fixa qual switch deve ser root da VLAN 10 (`spanning-tree vlan 10 root primary` no switch de núcleo).
- **Como confirmar:** `show interfaces GigabitEthernet1/0/1`, `show interfaces transceiver` e `show spanning-tree vlan 10`.

> Observação: os estados da Gi1/0/4 mudam a cada ~2 s, bem abaixo do *forward delay* padrão de 15 s do STP. Vale confirmar se os timers foram alterados.

### 2. Violação de port security na Gi1/0/5

- **Evidência:** `14:01:32.500 %PORT_SECURITY-2-PSECURE_VIOLATION: Security violation occurred on interface GigabitEthernet1/0/5`. É a mensagem de maior severidade do log (2, crítica).
- **Diagnóstico provável:**
  - **Evidenciado:** um dispositivo não autorizado enviou tráfego na porta, ou o limite de endereços MAC foi excedido.
  - **Hipótese:** se o modo de violação for o padrão (*shutdown*), a porta está desativada, em *err-disabled*.
- **Impacto:** o equipamento legítimo dessa porta pode estar sem rede. Se o dispositivo não for autorizado, trata-se de um possível incidente de segurança.
- **Ação:**
  - Identificar o MAC e o dispositivo conectado.
  - Se for legítimo (ex.: equipamento trocado), atualizar o MAC permitido e reativar a porta.
  - Se não for, manter a porta bloqueada e acionar a equipe de segurança.
- **Como confirmar:** `show port-security interface GigabitEthernet1/0/5`, `show port-security address` e `show interfaces status err-disabled`.

### 3. Duplex mismatch com o SW-Backup

- **Evidência:** `14:01:30.000 %CDP-4-DUPLEX_MISMATCH: duplex mismatch discovered on GigabitEthernet1/0/2 (full), with SW-Backup GigabitEthernet0/1 (half)` (severidade 4).
- **Diagnóstico provável:**
  - **Evidenciado:** as duas pontas do link operam em modos diferentes.
  - **Hipótese:** um lado tem velocidade e duplex fixos e o outro está em automático.
  - Este problema é **independente** da queda da Gi1/0/1.
- **Impacto:** colisões, erros de CRC e baixa vazão no link com o switch de backup.
- **Ação:** configurar as duas pontas da mesma forma: ambas em automático, ou ambas fixas com a mesma velocidade e duplex.
- **Como confirmar:** `show interfaces GigabitEthernet1/0/2` (contadores de *late collision* e CRC) e `show interfaces status`, nos dois switches.

### 4. Alteração de configuração via acesso remoto

- **Evidência:** `14:01:35.500 %SYS-5-CONFIG_I: Configured from console by vty0 (192.168.1.5)` (severidade 5).
- **Diagnóstico provável:**
  - **Evidenciado:** a configuração foi alterada em uma sessão remota (Telnet/SSH), a partir de 192.168.1.5, poucos segundos depois dos incidentes.
  - **Hipótese:** pode ser um administrador reagindo aos problemas, ou uma alteração não planejada.
- **Impacto:** a mensagem não indica falha. O risco é a alteração ter sido indevida ou ter introduzido um novo problema.
- **Ação:** confirmar quem fez a alteração e o que mudou, e comparar com a janela de mudanças.
- **Como confirmar:** `show archive log config all` (se o archive estiver habilitado), `show users` e os registros de AAA/TACACS, se houver.

## Dados adicionais necessários

- Qual equipamento está conectado à Gi1/0/1, e qual era a root bridge anterior da VLAN 10.
- A velocidade negociada na Gi1/0/4 (confirma a hipótese do enlace de 100 Mbps).
- O modo de violação configurado na Gi1/0/5 e o MAC que causou a violação.
- O conteúdo da alteração de configuração das 14:01:35 e quem a realizou.
- Logs anteriores a 14:01:22, para ver se houve sinais antes da queda (ex.: erros na interface).
