# Data Center Network Operations Lab

Laboratório prático de redes desenvolvido no **Cisco Packet Tracer** para simular a infraestrutura de um pequeno Data Center.

## Objetivo

Construir e operar progressivamente uma infraestrutura de rede de Data Center, aplicando conceitos de:

* networking;
* endereçamento IP;
* switches e roteadores;
* servidores;
* segmentação de redes;
* conectividade;
* troubleshooting;
* documentação de incidentes.

O foco do projeto não é apenas criar uma topologia funcional, mas compreender o comportamento da infraestrutura e desenvolver uma metodologia de diagnóstico e resolução de problemas.

##  Metodologia

O laboratório seguirá o fluxo:

```text
Implementação
      ↓
Teste
      ↓
Falha
      ↓
Diagnóstico
      ↓
Correção
      ↓
Validação
      ↓
Documentação
```

##  Tecnologia

* Cisco Packet Tracer

##  Status

🟡 **Em desenvolvimento — planejamento da arquitetura inicial.**

## 🏢 Cenário do laboratório

O ambiente simulará um pequeno Data Center responsável por hospedar uma aplicação web corporativa.

O serviço será acessado por usuários externos ao ambiente de servidores.

Inicialmente, o Data Center será responsável por disponibilizar:

* serviço web;
* resolução de nomes através de DNS;
* infraestrutura de rede necessária para acesso ao serviço.

O laboratório será desenvolvido a partir da perspectiva de um **Data Center Technician**, responsável por manter a disponibilidade do ambiente e diagnosticar falhas de conectividade e serviço.

```text
Usuário
   ↓
Rede
   ↓
Data Center
   ├── DNS
   └── Web Server
```

