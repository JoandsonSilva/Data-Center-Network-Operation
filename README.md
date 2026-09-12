#  Data Center Network Operations Lab

Laboratório prático de redes desenvolvido no **Cisco Packet Tracer** para simular a infraestrutura e a rotina operacional básica de um pequeno Data Center.

O projeto é construído progressivamente a partir da perspectiva de um **Data Center Technician**, com foco em disponibilidade, conectividade, troubleshooting, validação e documentação técnica.

---

##  Objetivo

Construir e operar uma infraestrutura simulada de Data Center aplicando conceitos de:

* networking;
* endereçamento IP;
* switches e roteadores;
* servidores;
* segmentação de redes;
* DNS;
* HTTP;
* roteamento;
* conectividade;
* troubleshooting;
* incidentes;
* documentação operacional.

O objetivo não é apenas construir uma topologia funcional, mas compreender **como os componentes se comunicam, como as falhas podem ocorrer e como diagnosticar problemas de forma estruturada**.

---

#  Cenário do laboratório

O ambiente simula um pequeno Data Center responsável por disponibilizar uma aplicação Web corporativa.

O serviço principal utilizado no laboratório é:

```text
perfumaria.radiante
```

O ambiente possui dois perfis principais:

### Usuário corporativo

Responsável por acessar a aplicação hospedada no Data Center.

### Data Center Technician

Responsável por:

* acompanhar a infraestrutura;
* validar conectividade;
* diagnosticar falhas;
* avaliar impacto;
* realizar troubleshooting;
* corrigir problemas dentro de sua responsabilidade;
* validar a recuperação;
* documentar incidentes.

---

#  Topologia

A primeira versão da infraestrutura utiliza:

* 1 PC de usuário;
* 2 switches Cisco 2960;
* 1 roteador Cisco ISR 4331;
* 1 servidor DNS;
* 1 servidor Web.

Estrutura:

```text
USER-01
192.168.10.10
      │
      ▼
SW-USER-01
      │
      ▼
R-EDGE-01
      │
      ▼
SW-DC-01
   ┌──┴───────────────┐
   ▼                  ▼
DNS-01              WEB-01
192.168.20.10       192.168.20.20
```

O roteador conecta duas redes distintas:

```text
USER NETWORK
192.168.10.0/24

DATA CENTER NETWORK
192.168.20.0/24
```

---

#  Plano de endereçamento

| Dispositivo | Interface | Endereço IP     | Máscara | Função                      |
| ----------- | --------- | --------------- | ------- | --------------------------- |
| R-EDGE-01   | Gi0/0/0   | `192.168.10.1`  | `/24`   | Gateway da rede de usuários |
| USER-01     | Fa0       | `192.168.10.10` | `/24`   | Usuário corporativo         |
| R-EDGE-01   | Gi0/0/1   | `192.168.20.1`  | `/24`   | Gateway do Data Center      |
| DNS-01      | Fa0       | `192.168.20.10` | `/24`   | Servidor DNS                |
| WEB-01      | Fa0       | `192.168.20.20` | `/24`   | Servidor Web                |

---

#  Roteamento

O `R-EDGE-01` possui uma interface em cada uma das redes:

```text
Gi0/0/0
192.168.10.1
```

e:

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

#  Serviço Web

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

Isso permitiu validar:

```text
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
```

---

#  DNS

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
        ↓
      DNS-01
        ↓
192.168.20.20
        ↓
      WEB-01
```

Após a configuração, o usuário passou a acessar o serviço através de:

```text
http://perfumaria.radiante
```

em vez de utilizar diretamente o endereço IP do servidor.

---

#  Baseline operacional

Após a configuração inicial da infraestrutura, foi realizado um conjunto de testes para estabelecer o **estado saudável conhecido do ambiente**.

Foram validados:

* conectividade do usuário com seu gateway;
* comunicação com o servidor DNS;
* comunicação com o servidor Web;
* roteamento entre as duas redes;
* resolução DNS;
* serviço HTTP;
* acesso ao portal através do nome configurado.

Estado atual:

```text
Gateway                ✅
Switching               ✅
Roteamento              ✅
DNS                     ✅
WEB Server              ✅
HTTP                    ✅
Resolução por nome      ✅
Portal                  ✅
```

O endereço:

```text
http://perfumaria.radiante
```

está acessível a partir do `USER-01`.

Este estado passa a ser considerado o **baseline operacional** do laboratório.

Os próximos cenários de troubleshooting serão comparados com esse estado conhecido de funcionamento.

---

#  Metodologia de troubleshooting

Os futuros incidentes seguirão o fluxo:

```text
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
```

O objetivo é evitar correções aleatórias.

Antes de qualquer intervenção será necessário entender:

* qual serviço foi impactado;
* onde a falha pode estar;
* quais dispositivos estão envolvidos;
* se outros serviços poderão ser afetados;
* qual ação deve ser executada;
* se existe necessidade de escalonamento;
* como validar que o ambiente voltou ao estado normal.

---

#  Incident Management

Os próximos cenários de falha serão registrados em uma central de operações utilizando **Jira**.

A proposta é utilizar uma única central para incidentes provenientes de diferentes laboratórios e projetos técnicos.

Exemplo futuro:

```text
Technical Operations Center
OPS
```

Os incidentes do laboratório poderão seguir um padrão como:

```text
[DC-NET] Portal corporativo indisponível
```

O ticket deverá registrar:

* impacto;
* ambiente afetado;
* sintomas;
* evidências;
* investigação;
* diagnóstico;
* causa;
* ação executada;
* validação;
* resolução.

A central será configurada antes da execução do primeiro incidente.

---

#  Evidências

As evidências registram apenas **marcos importantes do laboratório**, evitando documentação excessiva de cada pequena configuração.

## 1.0 — Topologia montada

Primeira versão da infraestrutura física criada no Cisco Packet Tracer.

[🔎 Visualizar evidência 1.0](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/1.0-%20Topologia%20montada.png)

---

## 1.1 — IPs do roteador configurados

Configuração e validação das interfaces do `R-EDGE-01` conectadas às redes de usuários e do Data Center.

[🔎 Visualizar evidência 1.1](https://github.com/JoandsonSilva/Data-Center-Network-Operation/blob/main/evidence/1.1%20-%20IPs%20do%20Roteador%20Configurados.png)

---

## 2.0 — USER-01 configurado e gateway validado

Configuração do usuário na rede:

```text
192.168.10.0/24
```

e validação de comunicação com:

```text
192.168.10.1
```

---

## 2.1 — Comunicação entre redes validada

Validação da comunicação entre:

```text
192.168.10.0/24
```

e:

```text
192.168.20.0/24
```

através do `R-EDGE-01`.

---

## 2.2 — WEB-01 configurado e conectividade validada

Configuração do servidor Web e validação de comunicação com outros componentes da infraestrutura.

---

## 3.0 — Serviço HTTP validado

Primeiro acesso realizado ao servidor Web utilizando:

```text
http://192.168.20.20
```

---

## 3.1 — DNS e acesso por nome validados

Configuração do DNS permitindo acesso ao serviço utilizando:

```text
http://perfumaria.radiante
```

---

## 4.0 — Baseline operacional validado

Validação completa do ambiente em estado saudável antes da criação dos primeiros cenários de incidente.

---

#  Conceitos aplicados

Durante o desenvolvimento inicial foram aplicados conceitos de:

* LAN;
* IPv4;
* subnet mask;
* default gateway;
* switching;
* roteamento;
* interfaces de rede;
* DNS;
* HTTP;
* resolução de nomes;
* servidor Web;
* testes com `ping`;
* `ipconfig`;
* `ipconfig /all`;
* Cisco IOS;
* `show ip interface brief`;
* validação de conectividade;
* baseline operacional;
* troubleshooting estruturado.

---

#  Próximas etapas

* [x] Construção da topologia inicial
* [x] Ativação das interfaces do roteador
* [x] Configuração das redes IP
* [x] Configuração do USER-01
* [x] Configuração do DNS-01
* [x] Configuração do WEB-01
* [x] Validação do roteamento
* [x] Ativação do serviço HTTP
* [x] Configuração do DNS
* [x] Acesso através de `perfumaria.radiante`
* [x] Estabelecimento do baseline operacional
* [ ] Configuração da Central de Operações no Jira
* [ ] Criação do primeiro incidente
* [ ] Troubleshooting do primeiro incidente
* [ ] Registro da causa e resolução
* [ ] Novos cenários de falha

---

#  Status

 **Infraestrutura inicial operacional**

O ambiente possui atualmente:

```text
2 redes
1 roteador
2 switches
1 servidor DNS
1 servidor Web
1 usuário corporativo
```

O serviço `perfumaria.radiante` está disponível e funcionando através da infraestrutura simulada.

### Próximo marco

**Configurar a Central de Operações no Jira e preparar o ambiente para o primeiro incidente controlado.**
