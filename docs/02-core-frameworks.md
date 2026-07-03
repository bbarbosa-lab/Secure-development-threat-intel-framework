# 02 - Principais Frameworks do Threat-Informed Secure Development

Este documento explica brevemente cada framework usado neste repositório e por que ele foi escolhido.

## NIST SSDF v1.1
O framework mais completo para estruturar o **processo** de desenvolvimento seguro em uma organização. Usado como espinha dorsal deste modelo.

## OWASP Top 10 2025 + OWASP Top 10 API 2023
As listas mais usadas no mundo para priorizar riscos em aplicações web e APIs. Essenciais para qualquer pessoa que desenvolve software exposto na internet.

## MITRE ATT&CK
Permite que você pense como um atacante real. Em vez de "vou proteger contra SQL Injection", você pensa "como eu mitigo as técnicas T1059 e T1190 que estão sendo usadas ativamente contra aplicações como a minha?".

## MITRE CVE + CWE + NVD
Fornece dados objetivos sobre vulnerabilidades conhecidas. Permite automação de priorização via CVSS e mapeamento para fraquezas raiz (CWE) e riscos OWASP.

## STIX / TAXII + MISP
Padrões abertos para trocar inteligência de ameaças de forma estruturada e automatizada. Permite que seu código "fale a mesma língua" que ferramentas profissionais de threat intel.

## CIS Benchmarks
Guias prescritivos de configuração segura para sistemas operacionais, containers, bancos de dados e cloud. Excelente para a fase Protect do SSDF.

Esses frameworks, quando usados juntos, criam um sistema muito mais robusto do que usar qualquer um isoladamente.
