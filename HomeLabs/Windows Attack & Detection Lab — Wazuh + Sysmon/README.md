
# Windows Attack & Detection Lab — Wazuh + Sysmon
## Sobre o projeto
Laboratório de Blue Team desenvolvido para simular atividades suspeitas contra um endpoint Windows e analisar a telemetria gerada utilizando Wazuh, Sysmon, Windows Event Logs e Microsoft Defender.
O objetivo foi relacionar ações executadas no endpoint com os eventos gerados, realizando análise e triagem semelhantes às atividades encontradas em um ambiente SOC.

## Ambiente
- Windows 11 — endpoint monitorado
- Kali Linux — máquina utilizada para simulação das atividades
- Wazuh — SIEM e análise dos eventos
- Sysmon — telemetria adicional do endpoint
- Windows Event Viewer — análise dos eventos locais

## Atividades realizadas

Durante o laboratório foram simuladas e analisadas atividades como:
- SYN Scan e reconhecimento de serviços
- Criação de regra customizada no Wazuh para detecção de Port Scan
- Brute Force contra contas locais
- Análise de falhas e sucessos de autenticação
- Atividades pós-comprometimento e enumeração do sistema
- Monitoramento de processos com Sysmon
- Uso de LOLBins, incluindo `certutil.exe`
- Detecção e bloqueio pelo Microsoft Defender
- Simulação de mecanismos de persistência
- Enumeração de contas locais
- Criação e monitoramento de Honeyfile

## Detecção customizada no Wazuh
Durante o SYN Scan, os eventos individuais não produziram um alerta que representasse diretamente o comportamento de Port Scan.
Foi criada uma regra customizada utilizando eventos Windows 5152 e correlação temporal para identificar múltiplas conexões bloqueadas em um curto período.
Durante o teste foram registrados mais de 8.000 eventos relacionados a conexões bloqueadas, enquanto a regra de correlação foi acionada 273 vezes, facilitando a análise da atividade.

## Principais eventos analisados
| Event ID | Descrição |
|---|---|
| 4624 | Logon bem-sucedido |
| 4625 | Falha de autenticação |
| 5152 | Conexão bloqueada pela Windows Filtering Platform |
| Sysmon 1 | Criação de processo |
| 1116 / 1117 | Detecção e ação do Microsoft Defender |
| 4663 | Acesso a objeto/arquivo monitorado |

## Resultado
O laboratório permitiu acompanhar diferentes etapas de uma simulação de comprometimento e analisar os rastros deixados no endpoint.
O foco principal foi entender como eventos individuais podem ser utilizados e correlacionados durante uma investigação, desde atividades de reconhecimento e autenticação até persistência e acesso a arquivos monitorados.
