# 03 - Ciclo de Vida Seguro com NIST SSDF v1.1

O **NIST Secure Software Development Framework (SSDF)** v1.1 (SP 800-218) é atualmente o framework mais completo e autoritativo para estruturar o desenvolvimento seguro em uma organização.

Ele organiza as práticas em quatro grandes fases:

## 1. Prepare the Organization (PO)
- Definir políticas, requisitos de segurança e cultura
- Treinar equipes
- Identificar e avaliar riscos de forma contínua
- **Onde threat intel entra forte**: PO.2 (Identify and Assess Risks) — usar ATT&CK + feeds atuais para priorizar riscos reais, não genéricos.

## 2. Protect the Software (PS)
- Gerenciar componentes de terceiros com segurança
- Proteger dados sensíveis (criptografia + key management)
- Configuração segura de builds e deployments
- Proteger o próprio código-fonte
- **Onde threat intel entra**: PS.2 (3rd Party) via NVD + SCA tools. PS.3 via CIS Benchmarks + threat-informed hardening.

## 3. Produce Well-Secured Software (PW)
- Práticas de codificação segura
- Revisão de código + testes de segurança (SAST, DAST, IAST)
- Gerenciar dívida técnica de segurança
- **Onde threat intel entra**: PW.2 — escrever testes baseados em técnicas reais do ATT&CK (abuse cases).

## 4. Respond to Vulnerabilities (RV)
- Processo de resposta a vulnerabilidades descobertas
- Resposta a incidentes de segurança
- Lições aprendidas e feedback para o processo
- **Onde threat intel entra forte**: RV.1 e RV.2 — usar feeds para priorizar patching e melhorar detecção.

## Por que SSDF é melhor que "só seguir OWASP Top 10"?

OWASP Top 10 é excelente para **priorizar riscos**.  
SSDF é excelente para **estruturar o processo** que previne, detecta e corrige esses riscos de forma sustentável.

O ideal é usar os dois juntos (como fazemos neste repositório).

## Recomendação Prática

Comece implementando as práticas de **Prepare** e **Protect** (especialmente gestão de dependências e configuração segura). Depois avance para **Produce** com revisões e testes threat-informed. Por último, maturate a fase de **Respond**.

Esse ciclo, quando bem executado, reduz drasticamente o custo e o risco de segurança ao longo do tempo.
