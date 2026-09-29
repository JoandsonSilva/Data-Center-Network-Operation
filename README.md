# 🌐 Data Center Network Operations Lab

Laboratório prático de redes desenvolvido no **Cisco Packet Tracer** para simular a infraestrutura e a rotina operacional básica de um pequeno Data Center.

O projeto é construído progressivamente a partir da perspectiva de um **Data Center Technician**, com foco em disponibilidade, conectividade, troubleshooting, incident management, validação e documentação técnica.

---

# 🎯 Objetivo

Construir e operar uma infraestrutura simulada de Data Center aplicando conceitos de:

- networking;
- endereçamento IPv4;
- switches e roteadores;
- servidores;
- segmentação de redes;
- DNS;
- HTTP;
- roteamento;
- conectividade;
- troubleshooting;
- gerenciamento de incidentes;
- análise de causa raiz;
- documentação operacional.

O objetivo não é apenas construir uma topologia funcional, mas compreender:

> **como os componentes se comunicam, como uma falha impacta o serviço e como localizar o problema de forma estruturada antes de executar qualquer ação corretiva.**

---

# 🏢 Cenário do laboratório

O ambiente simula um pequeno Data Center responsável por disponibilizar uma aplicação Web corporativa.

O serviço principal utilizado no laboratório é:

```text
perfumaria.radiante

O laboratório possui dois perfis principais.

Usuário corporativo

Responsável por acessar a aplicação hospedada no Data Center.

Para o usuário, a infraestrutura interna é transparente.

O serviço deve simplesmente estar disponível através de:

http://perfumaria.radiante
Data Center Technician

Responsável por:

acompanhar a infraestrutura;
validar conectividade;
diagnosticar falhas;
avaliar impacto;
realizar troubleshooting;
identificar a camada afetada;
executar ações dentro de sua responsabilidade;
escalar quando necessário;
validar a recuperação;
documentar incidentes.
🖥️ Topologia

A primeira versão da infraestrutura utiliza:

1 PC de usuário;
2 switches Cisco 2960;
1 roteador Cisco ISR 4331;
1 servidor DNS;
1 servidor Web.

Estrutura:

             USER NETWORK
            192.168.10.0/24

               USER-01
            192.168.10.10
                  │
                  ▼
             SW-USER-01
                  │
                  ▼
              R-EDGE-01
        192.168.10.1 / 192.168.20.1
                  │
                  ▼
              SW-DC-01
             ┌────┴─────┐
             ▼          ▼
          DNS-01      WEB-01
      192.168.20.10  192.168.20.20

           DATA CENTER NETWORK
             192.168.20.0/24

O roteador conecta duas redes distintas:

USER NETWORK
192.168.10.0/24

e:

DATA CENTER NETWORK
192.168.20.0/24
🔌 Conexões físicas
Origem	Porta	Destino	Porta	Cabo
USER-01	FastEthernet0	SW-USER-01	FastEthernet0/1	Copper Straight-Through
SW-USER-01	GigabitEthernet0/1	R-EDGE-01	GigabitEthernet0/0/0	Copper Straight-Through
R-EDGE-01	GigabitEthernet0/0/1	SW-DC-01	GigabitEthernet0/1	Copper Straight-Through
DNS-01	FastEthernet0	SW-DC-01	FastEthernet0/1	Copper Straight-Through
WEB-01	FastEthernet0	SW-DC-01	FastEthernet0/2	Copper Straight-Through
🌐 Plano de endereçamento
Dispositivo	Interface	Endereço IP	Máscara	Função
R-EDGE-01	Gi0/0/0	192.168.10.1	/24	Gateway da rede de usuários
USER-01	Fa0	192.168.10.10	/24	Usuário corporativo
R-EDGE-01	Gi0/0/1	192.168.20.1	/24	Gateway do Data Center
DNS-01	Fa0	192.168.20.10	/24	Servidor DNS
WEB-01	Fa0	192.168.20.20	/24	Servidor Web
🔀 Roteamento

O R-EDGE-01 possui uma interface em cada rede.

Interface da rede de usuários
Gi0/0/0
192.168.10.1
Interface da rede do Data Center
Gi0/0/1
192.168.20.1

Como as duas redes estão diretamente conectadas ao roteador, ele realiza a comunicação entre:

192.168.10.0/24

e:

192.168.20.0/24

sem necessidade, neste estágio do laboratório, de rotas estáticas adicionais.

🌍 Serviço Web

O WEB-01 utiliza:

IP:
192.168.20.20

Gateway:
192.168.20.1

DNS:
192.168.20.10

O serviço HTTP foi ativado e inicialmente validado diretamente através do endereço:

http://192.168.20.20

Fluxo:

USER-01
   ↓
Rede de usuários
   ↓
R-EDGE-01
   ↓
Rede do Data Center
   ↓
WEB-01
   ↓
HTTP

Esse teste permitiu validar que o serviço Web podia ser acessado através das duas redes.

🔎 DNS

O servidor DNS-01 utiliza:

192.168.20.10

Foi criado um registro DNS associando:

perfumaria.radiante

ao endereço:

192.168.20.20

Representação:

perfumaria.radiante
        ↓
      DNS-01
        ↓
192.168.20.20
        ↓
      WEB-01

Após a configuração, o usuário passou a acessar o serviço através de:

http://perfumaria.radiante

em vez de utilizar diretamente o endereço IP do servidor.

✅ Baseline operacional

Após a configuração inicial da infraestrutura, foi realizado um conjunto de testes para estabelecer o estado saudável conhecido do ambiente.

Foram validados:

conectividade do usuário com seu gateway;
comunicação com o servidor DNS;
comunicação com o servidor Web;
roteamento entre as duas redes;
resolução DNS;
serviço HTTP;
acesso ao portal através do nome configurado.

Estado saudável:

Gateway                ✅
Switching               ✅
Roteamento              ✅
DNS                     ✅
WEB Server              ✅
HTTP                    ✅
Resolução por nome      ✅
Portal                  ✅

O endereço:

http://perfumaria.radiante

está acessível a partir do USER-01.

Este estado passa a ser considerado o baseline operacional do laboratório.

Os cenários de troubleshooting são comparados com esse estado conhecido de funcionamento normal.

🔄 Metodologia de troubleshooting

Os incidentes seguem o fluxo:

ALERTA / INCIDENTE
        ↓
IDENTIFICAR IMPACTO
        ↓
COLETAR EVIDÊNCIAS
        ↓
LOCALIZAR A FALHA
        ↓
CRIAR HIPÓTESE
        ↓
EXECUTAR AÇÃO
        ↓
VALIDAR AMBIENTE
        ↓
MONITORAR
        ↓
DOCUMENTAR

O objetivo é evitar correções aleatórias.

Antes de qualquer intervenção é necessário entender:

qual serviço foi impactado;
onde a falha pode estar;
quais dispositivos estão envolvidos;
se outros serviços podem ser afetados;
qual ação deve ser executada;
se existe necessidade de escalonamento;
como validar que o ambiente retornou ao estado normal.
🎫 Incident Management

O laboratório utiliza uma central única de operações no Jira Service Management para registrar, investigar e documentar incidentes técnicos.

A central criada para os projetos foi denominada:

Technical Operations Center

A proposta é utilizar essa central não apenas para este laboratório, mas também para futuros projetos envolvendo:

Data Center;
Linux;
Cloud;
aplicações Web;
infraestrutura;
automação;
outros laboratórios técnicos.
Fluxo operacional
INCIDENT RECEIVED
        ↓
TRIAGE
        ↓
INVESTIGATION
        ↓
ROOT CAUSE
        ↓
CORRECTIVE ACTION
        ↓
VALIDATION
        ↓
RESOLUTION

Cada incidente deverá registrar:

Environment;
Service;
Impact;
Symptoms;
Expected Behavior;
Initial Checks;
Investigation;
Root Cause;
Action Taken;
Validation;
Escalation;
Resolution.

O objetivo é manter um histórico técnico reproduzível dos problemas encontrados durante os laboratórios.

🚨 INC-001 — Portal perfumaria.radiante indisponível

O primeiro incidente controlado simulou a indisponibilidade da aplicação Web hospedada no WEB-01.

Sintoma inicial

O usuário corporativo tentou acessar:

http://perfumaria.radiante

e o portal ficou indisponível.

O incidente foi registrado na central Technical Operations Center e iniciou-se um processo estruturado de troubleshooting.

🔍 Investigação
Check 1 — Default Gateway

Foi executado:

ping 192.168.10.1

Resultado:

Packets: Sent = 4
Received = 4
Lost = 0
0% packet loss
Conclusão

A comunicação entre USER-01 e seu gateway estava operacional.

O primeiro trecho da infraestrutura estava funcionando:

USER-01
   ↓
SW-USER-01
   ↓
R-EDGE-01
Check 2 — Web Server Connectivity

Foi executado:

ping 192.168.20.20

Resultado:

Packets: Sent = 4
Received = 4
Lost = 0
0% packet loss
Conclusão

O WEB-01 estava acessível através da infraestrutura de rede.

Isso indicou que estavam operacionais:

switching;
roteamento;
conectividade IP;
comunicação entre as redes;
host WEB-01.
Check 3 — DNS Resolution

Foi executado:

ping perfumaria.radiante

O nome foi corretamente resolvido para:

192.168.20.20

Um primeiro teste apresentou perda de 1 dos 4 pacotes.

Uma segunda validação apresentou:

4 packets sent
4 received
0% packet loss
Conclusão

A resolução DNS estava funcionando normalmente e o domínio estava apontando para o endereço correto do WEB-01.

Check 4 — Direct HTTP Access

Foi realizado acesso direto ao servidor através de:

http://192.168.20.20

Resultado:

Server reset connection
Conclusão

O servidor estava acessível através da rede, porém o serviço HTTP não estava respondendo corretamente.

Nesse momento, a investigação deixou de focar conectividade e foi direcionada para a camada de serviço.

Check 5 — HTTP Service Status

Foi verificado:

WEB-01
→ Services
→ HTTP

Resultado:

HTTP Service: Off

A falha foi isolada no serviço HTTP do WEB-01.

🎯 Root Cause

A causa raiz identificada foi:

O serviço HTTP do WEB-01 estava desativado.

Embora o servidor permanecesse acessível através da rede e respondesse normalmente aos testes ICMP, ele não conseguia disponibilizar a aplicação Web porque o serviço responsável pelas requisições HTTP não estava ativo.

O incidente demonstrou uma diferença importante:

HOST DISPONÍVEL
≠
SERVIÇO DISPONÍVEL

Um equipamento pode responder normalmente na rede enquanto uma aplicação ou serviço específico permanece indisponível.

🔧 Corrective Action

O serviço:

HTTP: Off

foi alterado para:

HTTP: On

Nenhuma outra configuração da infraestrutura foi modificada.

✅ Validation

Após a correção foram realizados dois testes.

Acesso direto pelo IP
http://192.168.20.20

Resultado:

✅ Funcionando
Acesso através do DNS
http://perfumaria.radiante

Resultado:

✅ Funcionando

O serviço retornou ao estado definido no baseline operacional.

📌 Resultado do INC-001
Gateway              ✅
Switching             ✅
Roteamento            ✅
DNS                   ✅
WEB-01                ✅
HTTP                  ❌ → ✅
Portal                ✅ Restaurado

Status final:

RESOLVED

Fluxo do incidente:

Portal indisponível
        ↓
Gateway validado
        ↓
Servidor alcançável
        ↓
Roteamento validado
        ↓
DNS validado
        ↓
HTTP por IP falhou
        ↓
Serviço HTTP inspecionado
        ↓
HTTP encontrado Off
        ↓
HTTP reativado
        ↓
Acesso por IP validado
        ↓
Acesso por nome validado
        ↓
Incidente resolvido
🧠 Aprendizados do INC-001
Ping não comprova disponibilidade da aplicação

O WEB-01 respondia normalmente aos testes de conectividade mesmo quando o HTTP estava indisponível.

Portanto:

Servidor acessível
≠
Aplicação disponível
Troubleshooting deve eliminar possibilidades

A investigação validou progressivamente:

gateway;
conectividade;
roteamento;
servidor;
DNS;
serviço HTTP.

A cada teste, o espaço de investigação foi reduzido.

Sintoma não é causa

Sintoma:

Portal indisponível

Causa:

HTTP Service Off
Correção precisa ser validada

Reativar o HTTP não foi suficiente para considerar o incidente encerrado.

Foi necessário validar:

acesso direto pelo IP;
acesso através do domínio;
retorno ao baseline operacional.
Documentação faz parte da operação

O incidente foi registrado no Jira contendo:

impacto;
sintomas;
investigação;
causa raiz;
ação corretiva;
validação;
resolução.
📸 Evidências

As evidências registram apenas marcos importantes do laboratório, evitando documentação excessiva de cada pequena configuração.

1.0 — Topologia montada

Primeira versão da infraestrutura física criada no Cisco Packet Tracer.

Visualizar evidência 1.0

1.1 — IPs do roteador configurados

Configuração e validação das interfaces do R-EDGE-01 conectadas às redes de usuários e do Data Center.

Visualizar evidência 1.1

2.0 — USER-01 configurado e gateway validado

Configuração do usuário na rede:

192.168.10.0/24

e validação de comunicação com:

192.168.10.1

Visualizar evidência 2.0

2.1 — Comunicação entre redes validada

Validação da comunicação entre:

192.168.10.0/24

e:

192.168.20.0/24

através do R-EDGE-01.

Visualizar evidência 2.1

2.2 — WEB-01 configurado e conectividade validada

Configuração do servidor Web e validação de comunicação com os demais componentes da infraestrutura.

Visualizar evidência 2.2

3.0 — Serviço HTTP validado

Primeiro acesso realizado ao servidor Web utilizando:

http://192.168.20.20

Visualizar evidência 3.0

3.1 — DNS e acesso por nome validados

Configuração do DNS permitindo acesso ao serviço utilizando:

http://perfumaria.radiante

Visualizar evidência 3.1

4.0 — Baseline operacional validado

Validação completa do ambiente em estado saudável antes da criação dos primeiros cenários de incidente.

Visualizar evidência 4.0

5.0 — Central de Operações no Jira criada

Criação da central Technical Operations Center utilizando Jira Service Management para registrar e acompanhar incidentes provenientes dos laboratórios técnicos.

Visualizar evidência 5.0

5.2 — INC-001 Portal indisponível

Registro do primeiro incidente controlado, no qual o usuário não consegue acessar o serviço perfumaria.radiante.

Visualizar evidência 5.2

5.3 — INC-001 HTTP Service Off identificado

Durante o troubleshooting, a falha foi isolada no serviço HTTP do WEB-01, encontrado em estado Off.

Visualizar evidência 5.3

5.4 — INC-001 Serviço restaurado

Após a reativação do serviço HTTP, o acesso ao portal foi novamente validado através de perfumaria.radiante.

Visualizar evidência 5.4

🧠 Conceitos aplicados

Durante o desenvolvimento foram aplicados conceitos de:

LAN;
IPv4;
subnet mask;
default gateway;
switching;
roteamento;
interfaces de rede;
DNS;
HTTP;
resolução de nomes;
servidor Web;
testes com ping;
ipconfig;
ipconfig /all;
Cisco IOS;
show ip interface brief;
validação de conectividade;
baseline operacional;
troubleshooting estruturado;
Jira Service Management;
Incident Management;
triagem de incidentes;
análise de impacto;
isolamento de falhas;
troubleshooting por camadas;
análise de causa raiz;
ação corretiva;
validação pós-correção;
documentação de incidentes.
🗺 Próximas etapas
 Construção da topologia inicial
 Ativação das interfaces do roteador
 Configuração das redes IP
 Configuração do USER-01
 Configuração do DNS-01
 Configuração do WEB-01
 Validação do roteamento
 Ativação do serviço HTTP
 Configuração do DNS
 Acesso através de perfumaria.radiante
 Estabelecimento do baseline operacional
 Configuração da Central de Operações no Jira
 Definição do padrão de Incident Management
 Criação do primeiro incidente
 Troubleshooting do INC-001
 Identificação da causa raiz
 Correção e validação do serviço
 Registro e encerramento do INC-001
 Executar INC-002
 Simular falha relacionada ao DNS
 Ampliar os cenários de troubleshooting
 Introduzir novos componentes de infraestrutura
📌 Status

🟢 Infraestrutura operacional e fase de Incident Management iniciada

O ambiente possui atualmente:

2 redes
1 roteador
2 switches
1 servidor DNS
1 servidor Web
1 usuário corporativo
1 central de incidentes no Jira

O serviço:

perfumaria.radiante

está operacional.

O laboratório possui:

baseline validado;
central de operações;
metodologia de troubleshooting;
fluxo de Incident Management;
primeiro incidente diagnosticado e resolvido.
Incidentes
INC-001 — Portal indisponível

Root Cause:
HTTP Service Off

Corrective Action:
HTTP Service On

Status:
RESOLVED
🚀 Próximo marco

O próximo cenário planejado será:

INC-002 — Falha relacionada ao DNS

O objetivo será trabalhar um comportamento diferente do primeiro incidente:

Acesso por IP      ✅
Acesso por nome    ❌

permitindo praticar troubleshooting focado em resolução de nomes e isolamento de falhas na camada DNS.


Um ajuste que ficou especialmente melhor nessa versão é que o README agora mostra uma **história completa**: arquitetura → baseline → incidente → investigação → causa → correção → validação → aprendizado. Isso deixa o projeto muito mais forte para alguém que chegar ao seu GitHub e quiser entender não apenas o que você montou, mas **como você pensa diante de uma falha operacional**.
