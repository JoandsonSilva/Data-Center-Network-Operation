# Data Center Network Operations Lab

Laboratório prático de redes desenvolvido no **Cisco Packet Tracer** para simular a infraestrutura e a rotina operacional básica de um pequeno Data Center.

O projeto é construído progressivamente a partir da perspectiva de um **Data Center Technician**, com foco em disponibilidade, conectividade, troubleshooting, incident management, validação e documentação técnica.

---

# Objetivo

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

> Como os componentes se comunicam, como uma falha impacta o serviço e como localizar o problema de forma estruturada antes de executar qualquer ação corretiva.

---

# Cenário do laboratório

O ambiente simula um pequeno Data Center responsável por disponibilizar uma aplicação Web corporativa.

O serviço principal utilizado no laboratório é:

```text
perfumaria.radiante
```

O laboratório possui dois perfis principais.

## Usuário corporativo

Responsável por acessar a aplicação hospedada no Data Center.

Para o usuário, a infraestrutura interna é transparente.

O serviço deve estar disponível através de:

```text
http://perfumaria.radiante
```

## Data Center Technician

Responsável por:

- acompanhar a infraestrutura;
- validar conectividade;
- diagnosticar falhas;
- avaliar impacto;
- realizar troubleshooting;
- identificar a camada afetada;
- executar ações dentro de sua responsabilidade;
- escalar quando necessário;
- validar a recuperação;
- documentar incidentes.

---

# Topologia

A primeira versão da infraestrutura utiliza:

- 1 PC de usuário;
- 2 switches Cisco 2960;
- 1 roteador Cisco ISR 4331;
- 1 servidor DNS;
- 1 servidor Web.

Estrutura:

```text
             USER NETWORK
            192.168.10.0/24

               USER-01
            192.168.10.10
                  |
                  |
             SW-USER-01
                  |
                  |
              R-EDGE-01
        192.168.10.1 / 192.168.20.1
                  |
                  |
              SW-DC-01
             /         \
            /           \
        DNS-01         WEB-01
   192.168.20.10    192.168.20.20

           DATA CENTER NETWORK
             192.168.20.0/24
```

O roteador conecta duas redes distintas:

```text
USER NETWORK
192.168.10.0/24
```

e:

```text
DATA CENTER NETWORK
192.168.20.0/24
```

---

# Conexões físicas

| Origem | Porta | Destino | Porta | Cabo |
|---|---|---|---|---|
| USER-01 | FastEthernet0 | SW-USER-01 | FastEthernet0/1 | Copper Straight-Through |
| SW-USER-01 | GigabitEthernet0/1 | R-EDGE-01 | GigabitEthernet0/0/0 | Copper Straight-Through |
| R-EDGE-01 | GigabitEthernet0/0/1 | SW-DC-01 | GigabitEthernet0/1 | Copper Straight-Through |
| DNS-01 | FastEthernet0 | SW-DC-01 | FastEthernet0/1 | Copper Straight-Through |
| WEB-01 | FastEthernet0 | SW-DC-01 | FastEthernet0/2 | Copper Straight-Through |

---

# Plano de endereçamento

| Dispositivo | Interface | Endereço IP | Máscara | Função |
|---|---|---|---|---|
| R-EDGE-01 | Gi0/0/0 | `192.168.10.1` | `/24` | Gateway da rede de usuários |
| USER-01 | Fa0 | `192.168.10.10` | `/24` | Usuário corporativo |
| R-EDGE-01 | Gi0/0/1 | `192.168.20.1` | `/24` | Gateway do Data Center |
| DNS-01 | Fa0 | `192.168.20.10` | `/24` | Servidor DNS |
| WEB-01 | Fa0 | `192.168.20.20` | `/24` | Servidor Web |

---

# Roteamento

O `R-EDGE-01` possui uma interface em cada rede.

## Interface da rede de usuários

```text
Gi0/0/0
192.168.10.1
```

## Interface da rede do Data Center

```text
Gi0/0/1
192.168.20.1
```

Como as duas redes estão diretamente conectadas ao roteador, ele realiza a comunicação entre:

```text
192.168.10.0/24
```

e:

```text
192.168.20.0/24
```

sem necessidade, neste estágio do laboratório, de rotas estáticas adicionais.

---

# Serviço Web

O `WEB-01` utiliza:

```text
IP:
192.168.20.20

Gateway:
192.168.20.1

DNS:
192.168.20.10
```

O serviço HTTP foi ativado e inicialmente validado diretamente através do endereço:

```text
http://192.168.20.20
```

Fluxo:

```text
USER-01
   |
   |
Rede de usuários
   |
   |
R-EDGE-01
   |
   |
Rede do Data Center
   |
   |
WEB-01
   |
   |
HTTP
```

Esse teste permitiu validar que o serviço Web podia ser acessado através das duas redes.

---

# DNS

O servidor `DNS-01` utiliza:

```text
192.168.20.10
```

Foi criado um registro DNS associando:

```text
perfumaria.radiante
```

ao endereço:

```text
192.168.20.20
```

Representação:

```text
perfumaria.radiante
        |
        |
      DNS-01
        |
        |
192.168.20.20
        |
        |
      WEB-01
```

Após a configuração, o usuário passou a acessar o serviço através de:

```text
http://perfumaria.radiante
```

em vez de utilizar diretamente o endereço IP do servidor.

---

# Baseline operacional

Após a configuração inicial da infraestrutura, foi realizado um conjunto de testes para estabelecer o **estado saudável conhecido do ambiente**.

Foram validados:

- conectividade do usuário com seu gateway;
- comunicação com o servidor DNS;
- comunicação com o servidor Web;
- roteamento entre as duas redes;
- resolução DNS;
- serviço HTTP;
- acesso ao portal através do nome configurado.

Estado saudável:

```text
Gateway                OK
Switching               OK
Roteamento              OK
DNS                     OK
WEB Server              OK
HTTP                    OK
Resolução por nome      OK
Portal                  OK
```

O endereço:

```text
http://perfumaria.radiante
```

está acessível a partir do `USER-01`.

Este estado passa a ser considerado o **baseline operacional** do laboratório.

Os cenários de troubleshooting são comparados com esse estado conhecido de funcionamento normal.

---

# Metodologia de troubleshooting

Os incidentes seguem o fluxo:

```text
ALERTA / INCIDENTE
        |
        v
IDENTIFICAR IMPACTO
        |
        v
COLETAR EVIDÊNCIAS
        |
        v
LOCALIZAR A FALHA
        |
        v
CRIAR HIPÓTESE
        |
        v
EXECUTAR AÇÃO
        |
        v
VALIDAR AMBIENTE
        |
        v
MONITORAR
        |
        v
DOCUMENTAR
```

O objetivo é evitar correções aleatórias.

Antes de qualquer intervenção é necessário entender:

- qual serviço foi impactado;
- onde a falha pode estar;
- quais dispositivos estão envolvidos;
- se outros serviços podem ser afetados;
- qual ação deve ser executada;
- se existe necessidade de escalonamento;
- como validar que o ambiente retornou ao estado normal.

---

# Incident Management

O laboratório utiliza uma central única de operações no **Jira Service Management** para registrar, investigar e documentar incidentes técnicos.

A central criada para os projetos foi denominada:

```text
Technical Operations Center
```

A proposta é utilizar essa central não apenas para este laboratório, mas também para futuros projetos envolvendo:

- Data Center;
- Linux;
- Cloud;
- aplicações Web;
- infraestrutura;
- automação;
- outros laboratórios técnicos.

## Fluxo operacional

```text
INCIDENT RECEIVED
        |
        v
TRIAGE
        |
        v
INVESTIGATION
        |
        v
ROOT CAUSE
        |
        v
CORRECTIVE ACTION
        |
        v
VALIDATION
        |
        v
RESOLUTION
```

Cada incidente deverá registrar:

- Environment;
- Service;
- Impact;
- Symptoms;
- Expected Behavior;
- Initial Checks;
- Investigation;
- Root Cause;
- Action Taken;
- Validation;
- Escalation;
- Resolution.

O objetivo é manter um histórico técnico reproduzível dos problemas encontrados durante os laboratórios.

---

# INC-001 - Portal `perfumaria.radiante` indisponível

O primeiro incidente controlado simulou a indisponibilidade da aplicação Web hospedada no `WEB-01`.

## Sintoma inicial

O usuário corporativo tentou acessar:

```text
http://perfumaria.radiante
```

e o portal ficou indisponível.

O incidente foi registrado na central **Technical Operations Center** e iniciou-se um processo estruturado de troubleshooting.

---

## Investigação

### Check 1 - Default Gateway

Foi executado:

```bash
ping 192.168.10.1
```

Resultado:

```text
Packets: Sent = 4
Received = 4
Lost = 0
0% packet loss
```

### Conclusão

A comunicação entre `USER-01` e seu gateway estava operacional.

O primeiro trecho da infraestrutura estava funcionando:

```text
USER-01
   |
   |
SW-USER-01
   |
   |
R-EDGE-01
```

---

### Check 2 - Web Server Connectivity

Foi executado:

```bash
ping 192.168.20.20
```

Resultado:

```text
Packets: Sent = 4
Received = 4
Lost = 0
0% packet loss
```

### Conclusão

O `WEB-01` estava acessível através da infraestrutura de rede.

Isso indicou que estavam operacionais:

- switching;
- roteamento;
- conectividade IP;
- comunicação entre as redes;
- host `WEB-01`.

---

### Check 3 - DNS Resolution

Foi executado:

```bash
ping perfumaria.radiante
```

O nome foi corretamente resolvido para:

```text
192.168.20.20
```

Um primeiro teste apresentou perda de 1 dos 4 pacotes.

Uma segunda validação apresentou:

```text
4 packets sent
4 received
0% packet loss
```

### Conclusão

A resolução DNS estava funcionando normalmente e o domínio estava apontando para o endereço correto do `WEB-01`.

---

### Check 4 - Direct HTTP Access

Foi realizado acesso direto ao servidor através de:

```text
http://192.168.20.20
```

Resultado:

```text
Server reset connection
```

### Conclusão

O servidor estava acessível através da rede, porém o serviço HTTP não estava respondendo corretamente.

Nesse momento, a investigação deixou de focar conectividade e foi direcionada para a camada de serviço.

---

### Check 5 - HTTP Service Status

Foi verificado:

```text
WEB-01
-> Services
-> HTTP
```

Resultado:

```text
HTTP Service: Off
```

A falha foi isolada no serviço HTTP do `WEB-01`.

---

# Root Cause

A causa raiz identificada foi:

> **O serviço HTTP do WEB-01 estava desativado.**

Embora o servidor permanecesse acessível através da rede e respondesse normalmente aos testes ICMP, ele não conseguia disponibilizar a aplicação Web porque o serviço responsável pelas requisições HTTP não estava ativo.

O incidente demonstrou uma diferença importante:

```text
HOST DISPONÍVEL
!=
SERVIÇO DISPONÍVEL
```

Um equipamento pode responder normalmente na rede enquanto uma aplicação ou serviço específico permanece indisponível.

---

# Corrective Action

O serviço:

```text
HTTP: Off
```

foi alterado para:

```text
HTTP: On
```

Nenhuma outra configuração da infraestrutura foi modificada.

---

# Validation

Após a correção foram realizados dois testes.

## Acesso direto pelo IP

```text
http://192.168.20.20
```

Resultado:

```text
Funcionando
```

## Acesso através do DNS

```text
http://perfumaria.radiante
```

Resultado:

```text
Funcionando
```

O serviço retornou ao estado definido no baseline operacional.

---

# Resultado do INC-001

```text
Gateway              OK
Switching             OK
Roteamento            OK
DNS                   OK
WEB-01                OK
HTTP                  FALHA -> RESTAURADO
Portal                RESTAURADO
```

Status final:

```text
RESOLVED
```

Fluxo do incidente:

```text
Portal indisponível
        |
        v
Gateway validado
        |
        v
Servidor alcançável
        |
        v
Roteamento validado
        |
        v
DNS validado
        |
        v
HTTP por IP falhou
        |
        v
Serviço HTTP inspecionado
        |
        v
HTTP encontrado Off
        |
        v
HTTP reativado
        |
        v
Acesso por IP validado
        |
        v
Acesso por nome validado
        |
        v
Incidente resolvido
```

---

# Aprendizados do INC-001

## Ping não comprova disponibilidade da aplicação

O `WEB-01` respondia normalmente aos testes de conectividade mesmo quando o HTTP estava indisponível.

Portanto:

```text
Servidor acessível
!=
Aplicação disponível
```

## Troubleshooting deve eliminar possibilidades

A investigação validou progressivamente:

1. gateway;
2. conectividade;
3. roteamento;
4. servidor;
5. DNS;
6. serviço HTTP.

A cada teste, o espaço de investigação foi reduzido.

## Sintoma não é causa

Sintoma:

```text
Portal indisponível
```

Causa:

```text
HTTP Service Off
```

## Correção precisa ser validada

Reativar o HTTP não foi suficiente para considerar o incidente encerrado.

Foi necessário validar:

- acesso direto pelo IP;
- acesso através do domínio;
- retorno ao baseline operacional.

## Documentação faz parte da operação

O incidente foi registrado no Jira contendo:

- impacto;
- sintomas;
- investigação;
- causa raiz;
- ação corretiva;
- validação;
- resolução.

---

# Evidências

As evidências registram apenas **marcos importantes do laboratório**, evitando documentação excessiva de cada pequena configuração.

---

## 1.0 - Topologia montada

Primeira versão da infraestrutura física criada no Cisco Packet Tracer.

[Visualizar evidência 1.0](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/1.0-%20Topologia%20montada.png)

---

## 1.1 - IPs do roteador configurados

Configuração e validação das interfaces do `R-EDGE-01` conectadas às redes de usuários e do Data Center.

[Visualizar evidência 1.1](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/1.1%20-%20IPs%20do%20Roteador%20Configurados.png)

---

## 2.0 - USER-01 configurado e gateway validado

Configuração do usuário na rede:

```text
192.168.10.0/24
```

e validação de comunicação com:

```text
192.168.10.1
```

[Visualizar evidência 2.0](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/2.0%20-%20USER-01%20Configurado%20e%20Gateway%20Validado.png)

---

## 2.1 - Comunicação entre redes validada

Validação da comunicação entre:

```text
192.168.10.0/24
```

e:

```text
192.168.20.0/24
```

através do `R-EDGE-01`.

[Visualizar evidência 2.1](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/2.1%20-%20Comunicac%CC%A7a%CC%83o%20entre%20Redes%20Validada.png)

---

## 2.2 - WEB-01 configurado e conectividade validada

Configuração do servidor Web e validação de comunicação com os demais componentes da infraestrutura.

[Visualizar evidência 2.2](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/2.2%20-%20WEB-01%20Configurado%20e%20Conectividade%20Validada.png)

---

## 3.0 - Serviço HTTP validado

Primeiro acesso realizado ao servidor Web utilizando:

```text
http://192.168.20.20
```

[Visualizar evidência 3.0](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/3.0%20-%20Servic%CC%A7o%20HTTP%20Validado.png)

---

## 3.1 - DNS e acesso por nome validados

Configuração do DNS permitindo acesso ao serviço utilizando:

```text
http://perfumaria.radiante
```

[Visualizar evidência 3.1](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/3.1%20-%20DNS%20e%20Acesso%20por%20Nome%20Validados.png)

---

## 4.0 - Baseline operacional validado

Validação completa do ambiente em estado saudável antes da criação dos primeiros cenários de incidente.

[Visualizar evidência 4.0](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/4.0%20-%20Baseline%20Operacional%20Validado.png)

---

## 5.0 - Central de Operações no Jira criada

Criação da central **Technical Operations Center** utilizando Jira Service Management para registrar e acompanhar incidentes provenientes dos laboratórios técnicos.

[Visualizar evidência 5.0](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/5.0%20-%20Central%20de%20Operac%CC%A7o%CC%83es%20no%20Jira%20Criada.png)

---

## 5.2 - INC-001 Portal indisponível

Registro do primeiro incidente controlado, no qual o usuário não consegue acessar o serviço `perfumaria.radiante`.

[Visualizar evidência 5.2](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/5.2%20-%20INC-001%20Portal%20Indisponivel.png)

---

## 5.3 - INC-001 HTTP Service Off identificado

Durante o troubleshooting, a falha foi isolada no serviço HTTP do `WEB-01`, encontrado em estado `Off`.

[Visualizar evidência 5.3](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/5.3%20-%20INC-001%20HTTP%20Service%20Off%20Identificado.png)

---

## 5.4 - INC-001 Serviço restaurado

Após a reativação do serviço HTTP, o acesso ao portal foi novamente validado através de `perfumaria.radiante`.

[Visualizar evidência 5.4](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/5.4%20-%20INC-001%20Servic%CC%A7o%20Restaurado.png)

---

# Conceitos aplicados

Durante o desenvolvimento foram aplicados conceitos de:

- LAN;
- IPv4;
- subnet mask;
- default gateway;
- switching;
- roteamento;
- interfaces de rede;
- DNS;
- HTTP;
- resolução de nomes;
- servidor Web;
- testes com `ping`;
- `ipconfig`;
- `ipconfig /all`;
- Cisco IOS;
- `show ip interface brief`;
- validação de conectividade;
- baseline operacional;
- troubleshooting estruturado;
- Jira Service Management;
- Incident Management;
- triagem de incidentes;
- análise de impacto;
- isolamento de falhas;
- troubleshooting por camadas;
- análise de causa raiz;
- ação corretiva;
- validação pós-correção;
- documentação de incidentes.

---

# Próximas etapas

- [x] Construção da topologia inicial
- [x] Ativação das interfaces do roteador
- [x] Configuração das redes IP
- [x] Configuração do USER-01
- [x] Configuração do DNS-01
- [x] Configuração do WEB-01
- [x] Validação do roteamento
- [x] Ativação do serviço HTTP
- [x] Configuração do DNS
- [x] Acesso através de `perfumaria.radiante`
- [x] Estabelecimento do baseline operacional
- [x] Configuração da Central de Operações no Jira
- [x] Definição do padrão de Incident Management
- [x] Criação do primeiro incidente
- [x] Troubleshooting do INC-001
- [x] Identificação da causa raiz
- [x] Correção e validação do serviço
- [x] Registro e encerramento do INC-001
- [ ] Executar INC-002
- [ ] Simular falha relacionada ao DNS
- [ ] Ampliar os cenários de troubleshooting
- [ ] Introduzir novos componentes de infraestrutura

---

# Status

**Infraestrutura operacional e fase de Incident Management iniciada**

O ambiente possui atualmente:

```text
2 redes
1 roteador
2 switches
1 servidor DNS
1 servidor Web
1 usuário corporativo
1 central de incidentes no Jira
```

O serviço:

```text
perfumaria.radiante
```

está operacional.

O laboratório possui:

- baseline validado;
- central de operações;
- metodologia de troubleshooting;
- fluxo de Incident Management;
- primeiro incidente diagnosticado e resolvido.

## Incidentes

```text
INC-001 - Portal indisponível

Root Cause:
HTTP Service Off

Corrective Action:
HTTP Service On

Status:
RESOLVED
```

---

# Próximo marco

O próximo cenário planejado será:

```text
INC-002 - Falha relacionada ao DNS
```

O objetivo será trabalhar um comportamento diferente do primeiro incidente:

```text
Acesso por IP      FUNCIONANDO
Acesso por nome    FALHANDO
```

permitindo praticar troubleshooting focado em resolução de nomes e isolamento de falhas na camada DNS.
