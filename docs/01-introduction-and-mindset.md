# 01 - Introdução e Mindset: Por que "Threat-Informed" Secure Development?

## O Problema que Este Framework Resolve

A maioria dos times de desenvolvimento ainda trata segurança como uma **fase** ou um **checklist** que aparece no final do sprint ou depois que o auditor encontra algo.

Isso gera dois problemas graves:

1. **Falso senso de segurança** — "Passamos no SAST/DAST, então está seguro."
2. **Reatividade cara** — Vulnerabilidades são descobertas tarde, quando o custo de correção é 10x~100x maior.

O resultado? Aplicações que são "seguras o suficiente para passar no scan", mas que caem facilmente diante de um atacante real que usa técnicas atuais (não as de 5 anos atrás).

## O Mindset Correto

> **Segurança não é sobre adicionar controles. É sobre remover as condições que permitem que um ataque tenha sucesso.**

Um sistema seguro é aquele que foi **projetado desde o início** para frustrar as técnicas que atacantes reais usam hoje.

Isso exige três coisas:

- **Entender o adversário** (MITRE ATT&CK + Threat Intelligence real)
- **Conhecer os riscos priorizados** para o tipo de aplicação que você está construindo (OWASP Top 10 Web + API)
- **Ter um processo estruturado** que transforma esse conhecimento em decisões de design, código e pipeline (NIST SSDF)

## Por que "Threat-Informed"?

Threat Intelligence não é só para times de SOC ou Red Team.

Quando bem usado no desenvolvimento, ele responde perguntas críticas como:

- Quais técnicas de ataque estão sendo usadas ativamente contra APIs/web apps neste momento? (Talos, MISP, OTX)
- Quais CVEs novas e exploráveis afetam as bibliotecas que usamos? (NVD)
- Quais padrões de comportamento (TTPs) os atacantes estão usando contra aplicações semelhantes à nossa? (MITRE ATT&CK)
- Como os incidentes reais estão acontecendo em organizações do nosso setor? (FIRST, SANS ISC)

Com essas respostas, você para de "adivinhar" o que proteger e começa a **priorizar com base em evidência**.

## Defesa em Profundidade + Threat Intelligence

O conceito clássico de **Defense in Depth** (várias camadas de proteção) continua válido. Porém, em 2026, as camadas precisam ser **informadas por inteligência de ameaças atualizada**.

Exemplo prático:
- Camada 1: WAF + API Gateway (bloqueia padrões conhecidos de ataque)
- Camada 2: Middleware de Threat Intel (bloqueia IPs/domínios maliciosos reportados em feeds recentes)
- Camada 3: Validação rigorosa de autorização no código (mitiga BOLA — OWASP API #1)
- Camada 4: Logging estruturado + detecção de anomalias (permite resposta rápida e feedback para threat intel)

Quando uma camada falha (e eventualmente falha), as outras ainda sustentam a defesa.

## Como Este Repositório te Ajuda a Adotar Esse Mindset

Este repositório não é uma lista de ferramentas. É um **modelo de pensamento** + **mapeamentos práticos** + **exemplos de código** que mostram como aplicar esse mindset no dia a dia de quem desenvolve APIs e aplicações web/mobile.

Os documentos e mapeamentos foram construídos para responder:

- Como o NIST SSDF se conecta com OWASP e ATT&CK na prática?
- Onde injetar threat intelligence (MISP, NVD, Talos) dentro do ciclo de desenvolvimento?
- Quais controles concretos (em código, pipeline e arquitetura) mitigam os riscos mais importantes hoje?

Se você internalizar esse modelo, sua forma de pensar sobre segurança muda permanentemente — de "vou rodar o scanner" para "vou projetar este endpoint de forma que as técnicas Txxxx do ATT&CK não funcionem aqui".

Esse é o nível de maturidade que recrutadores e times maduros de AppSec/DevSecOps procuram em 2026.
