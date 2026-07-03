# 05 - MITRE ATT&CK para Desenvolvedores

Este documento ensina como usar o **MITRE ATT&CK** de forma prática no dia a dia do desenvolvimento de software, especialmente para quem está construindo APIs e aplicações web.

---

## Por que ATT&CK é Diferente de Outros Frameworks?

A maioria dos frameworks de segurança fala sobre **vulnerabilidades** (CVEs) ou **riscos genéricos** (OWASP).

O MITRE ATT&CK fala sobre **como atacantes reais operam** — as técnicas que eles usam para conseguir acesso inicial, executar código, persistir, escalar privilégios, etc.

Isso muda completamente a qualidade do seu threat modeling.

---

## Conceitos Fundamentais

| Conceito | Definição | Exemplo |
|----------|-----------|---------|
| **Tática** | O objetivo do atacante | Initial Access, Execution, Persistence, Privilege Escalation, Credential Access, Defense Evasion, Discovery, Lateral Movement, Collection, Command and Control, Exfiltration, Impact |
| **Técnica** | Como o atacante alcança o objetivo tático | T1190 - Exploit Public-Facing Application |
| **Sub-técnica** | Detalhes específicos da técnica | T1552.001 - Credentials In Files |

---

## Técnicas ATT&CK Mais Relevantes para Desenvolvedores de APIs

### 1. Initial Access

| Técnica | ID | Descrição | Como se aplica a APIs |
|---------|----|-----------|-----------------------|
| Exploit Public-Facing Application | T1190 | Explorar vulnerabilidades em aplicações expostas na internet | Injeção, broken access control, SSRF, componentes vulneráveis em APIs públicas |
| Valid Accounts | T1078 | Usar credenciais válidas de usuários legítimos | Account takeover, uso de credenciais vazadas, session hijacking |

**Pergunta para threat modeling:**
"Se eu fosse um atacante, como eu conseguiria acesso inicial a esta API sem ter credenciais?"

---

### 2. Execution

| Técnica | ID | Descrição | Como se aplica a APIs |
|---------|----|-----------|-----------------------|
| Command and Scripting Interpreter | T1059 | Executar comandos via interpretadores (bash, PowerShell, Python, etc.) | Command injection, template injection, eval de código não confiável |
| Exploit Public-Facing Application | T1190 | (também execução) | RCE via vulnerabilidades em frameworks ou dependências |

**Pergunta para threat modeling:**
"Quais inputs da minha API poderiam permitir que um atacante executasse código no servidor?"

---

### 3. Persistence

| Técnica | ID | Descrição | Como se aplica a APIs |
|---------|----|-----------|-----------------------|
| Account Manipulation | T1098 | Modificar contas para manter acesso | Criação de contas backdoor, modificação de privilégios via API |
| Web Shell | T1505.003 | Implantar web shell para acesso persistente | Upload de arquivos maliciosos que permitem execução de comandos |

---

### 4. Privilege Escalation

| Técnica | ID | Descrição | Como se aplica a APIs |
|---------|----|-----------|-----------------------|
| Exploitation for Privilege Escalation | T1068 | Explorar vulnerabilidades para escalar privilégios | Vulnerabilidades em componentes que rodam com privilégios elevados |
| Valid Accounts | T1078 | Usar contas com mais privilégios do que o esperado | Broken access control permitindo escalada de privilégios |

**Pergunta para threat modeling:**
"Se um atacante conseguir acesso com privilégios baixos, como ele conseguiria escalar para privilégios mais altos?"

---

### 5. Credential Access

| Técnica | ID | Descrição | Como se aplica a APIs |
|---------|----|-----------|-----------------------|
| Unsecured Credentials | T1552 | Encontrar credenciais armazenadas de forma insegura | Credenciais em código, arquivos de configuração, variáveis de ambiente expostas, logs |
| Credentials from Password Stores | T1555 | Acessar gerenciadores de senha ou caches | Roubo de tokens de sessão, cookies, tokens JWT mal protegidos |

**Pergunta para threat modeling:**
"Onde minha aplicação armazena ou manipula credenciais e tokens? Como um atacante poderia acessá-los?"

---

### 6. Defense Evasion

| Técnica | ID | Descrição | Como se aplica a APIs |
|---------|----|-----------|-----------------------|
| Masquerading | T1036 | Disfarçar malware ou atividade maliciosa | User-agents falsos, IPs de proxies residenciais, comportamento que imita usuários legítimos |
| Impair Defenses | T1562 | Desabilitar ou enfraquecer mecanismos de defesa | Desabilitar logging, modificar configurações de segurança via API comprometida |

---

### 7. Discovery

| Técnica | ID | Descrição | Como se aplica a APIs |
|---------|----|-----------|-----------------------|
| Account Discovery | T1087 | Descobrir informações sobre contas | Enumeração de usuários via API (user enumeration) |
| Network Service Discovery | T1046 | Descobrir serviços na rede | SSRF para mapear infraestrutura interna |
| Permission Groups Discovery | T1069 | Descobrir grupos e permissões | Broken access control permitindo listagem de roles/permissions |

---

### 8. Collection + Exfiltration

| Técnica | ID | Descrição | Como se aplica a APIs |
|---------|----|-----------|-----------------------|
| Data from Local System | T1005 | Coletar dados do sistema comprometido | APIs que expõem dados sensíveis de forma excessiva |
| Exfiltration Over C2 Channel | T1041 | Exfiltrar dados através do canal de comando e controle | APIs que permitem download massivo de dados sem rate limiting adequado |

---

## Como Fazer Threat Modeling com ATT&CK (Passo a Passo)

### Passo 1: Definir o Escopo
- Qual funcionalidade / endpoint / fluxo você está analisando?
- Qual é o tipo de aplicação? (API pública, interna, com dados sensíveis, etc.)

### Passo 2: Selecionar Técnicas Relevantes
Use o ATT&CK Navigator ou busque no site do MITRE pelas técnicas mais comuns contra o seu tipo de aplicação.

**Técnicas mais comuns contra APIs web:**
- T1190 - Exploit Public-Facing Application
- T1078 - Valid Accounts
- T1552 - Unsecured Credentials
- T1134 - Access Token Manipulation
- T1059 - Command and Scripting Interpreter
- T1005 - Data from Local System

### Passo 3: Para cada técnica, responder 3 perguntas

1. **Como um atacante poderia aplicar esta técnica contra minha aplicação?**
   (Seja específico: qual endpoint, qual input, qual fluxo)

2. **Quais controles de segurança eu já tenho que mitigam esta técnica?**
   (Seja honesto. Se não tem controle, anote como gap)

3. **O que eu preciso implementar para mitigar ou detectar esta técnica?**
   (Controle preventivo, detectivo, ou ambos)

### Passo 4: Documentar e Priorizar
- Documente os riscos identificados
- Priorize com base em: facilidade de exploração + impacto + existência de threat intel indicando exploração ativa

### Passo 5: Implementar Controles e Testes
- Implemente os controles identificados
- Crie testes (manuais ou automatizados) que validem se a técnica foi mitigada

---

## Exemplo Prático: Threat Modeling de uma API de Usuários

**Funcionalidade:** Endpoint `GET /api/v1/users/{user_id}` que retorna dados de um usuário.

**Técnica ATT&CK:** T1078 - Valid Accounts + T1134 - Access Token Manipulation

**Análise:**

| Pergunta | Resposta |
|----------|----------|
| Como um atacante aplicaria esta técnica? | Um atacante com token JWT válido de um usuário comum tenta acessar dados de outro usuário alterando o `user_id` na URL ou manipulando claims do token |
| Controles existentes | Autenticação via JWT está implementada. Não há validação de que o `user_id` pertence ao usuário autenticado |
| Gap identificado | Broken Access Control (A01) |
| Controle a implementar | Validação server-side: o `user_id` da URL deve corresponder ao `sub` do JWT ou o usuário deve ter permissão explícita para acessar outros usuários |
| Teste recomendado | Teste automatizado tentando acessar `user_id` de outro usuário com token de usuário comum |

---

## Ferramentas Recomendadas

| Ferramenta | Uso | Link |
|------------|-----|------|
| **ATT&CK Navigator** | Visualizar e filtrar técnicas por plataforma, grupo, etc. | https://mitre-attack.github.io/attack-navigator/ |
| **ATT&CK Website** | Buscar técnicas e sub-técnicas | https://attack.mitre.org/ |
| **ATT&CK Groups** | Ver técnicas usadas por grupos específicos de threat actors | https://attack.mitre.org/groups/ |
| **mitreattack-python** | Biblioteca Python para consultar ATT&CK programaticamente | Disponível no PyPI |

---

## Resumo: Como Usar ATT&CK no Seu Dia a Dia

| Momento | Como Usar ATT&CK | Entregável |
|---------|------------------|------------|
| **Threat Modeling** | Selecionar técnicas relevantes e responder as 3 perguntas | Documento de threat model por funcionalidade |
| **Code Review** | Perguntar "Esta implementação mitiga as técnicas ATT&CK identificadas?" | Code review com foco em segurança |
| **Testes de Segurança** | Criar testes que validam mitigação das técnicas | Testes de segurança mais direcionados |
| **Incident Response** | Categorizar incidentes usando táticas e técnicas ATT&CK | Análise de incidentes mais estruturada |
| **Treinamento do Time** | Usar exemplos reais de ATT&CK para ensinar como atacantes pensam | Time mais consciente sobre segurança |

---

**ATT&CK transforma threat modeling de um exercício genérico em uma análise concreta baseada em como atacantes realmente operam.**

**Próximo documento:** [06-inteligencia-de-ameacas-estruturada.md](./06-inteligencia-de-ameacas-estruturada.md) — STIX, TAXII, MISP e como integrar threat intelligence no seu código.