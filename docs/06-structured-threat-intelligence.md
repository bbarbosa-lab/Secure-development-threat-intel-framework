# 06 - Inteligência de Ameaças Estruturada (STIX, TAXII, MISP, Talos)

Este documento explica como usar **inteligência de ameaças de forma estruturada e automatizada**, conectando diretamente com o conteúdo do seu curso de Cyber Threat Management.

---

## Por que Threat Intelligence Estruturada Importa?

A maioria dos desenvolvedores usa threat intelligence de forma **manual e reativa**:
- Ler relatórios de vez em quando
- Copiar listas de IOCs manualmente
- Não integrar dados de threat intel no código ou no pipeline

O objetivo deste documento é mostrar como fazer de forma **estruturada, automatizada e integrada** ao ciclo de desenvolvimento.

---

## Os Padrões Fundamentais: STIX + TAXII + CybOX

### STIX 2.1 (Structured Threat Information Expression)

**O que é?**
STIX é uma linguagem padronizada (baseada em JSON) para descrever threat intelligence de forma estruturada e legível por máquina.

**O que você pode representar com STIX?**
- Indicators of Compromise (IOCs): IPs, domínios, hashes de arquivos, URLs, user-agents
- TTPs (Tactics, Techniques, Procedures)
- Threat Actors e Campaigns
- Vulnerabilidades (CVEs)
- Malware
- Relatórios de threat intelligence

**Exemplo simples de Indicator em STIX 2.1:**

```json
{
  "type": "indicator",
  "spec_version": "2.1",
  "id": "indicator--8e2e2d2b-17d4-4cbf-938f-98ee46b3cd3f",
  "created": "2026-07-01T12:00:00.000Z",
  "modified": "2026-07-01T12:00:00.000Z",
  "name": "Malicious IP - Known C2 Server",
  "pattern": "[ipv4-addr:value = '203.0.113.42']",
  "pattern_type": "stix",
  "valid_from": "2026-07-01T12:00:00.000Z",
  "labels": ["malicious-activity"]
}
```

### TAXII 2.1 (Trusted Automated Exchange of Indicator Information)

**O que é?**
TAXII é o protocolo (baseado em HTTPS) usado para **transportar** conteúdo STIX entre sistemas.

Pense assim:
- **STIX** = o formato/conteúdo (como JSON)
- **TAXII** = o envelope/transporte (como HTTPS)

### CybOX (Cyber Observable Expression)

CybOX foi incorporado ao STIX 2.x. Ele descreve **observáveis** de forma padronizada (IPs, arquivos, processos, etc.).

---

## MISP - Malware Information Sharing Platform

### O que é?
MISP é uma plataforma **open-source** para compartilhar, armazenar e consumir indicadores de comprometimento (IOCs) de forma estruturada.

- Usada por mais de **6.000 organizações** globalmente
- Suporta exportação em **STIX 2.1** e outros formatos
- Tem API excelente e biblioteca Python (`pymisp`)
- Pode ser usada de forma comunitária ou auto-hospedada

### Por que MISP é excelente para desenvolvedores?

- Você pode **consumir** IOCs de instâncias públicas ou comunitárias
- Você pode **exportar** eventos de segurança da sua aplicação em formato estruturado
- A integração via API é relativamente simples
- É gratuito e open-source

### Exemplo de uso prático com Python

```python
from pymisp import PyMISP

# Conectar a uma instância MISP
misp = PyMISP('https://misp.example.com', 'sua-api-key', ssl=True)

# Buscar eventos recentes
events = misp.search(controller='events', last='7d')

# Extrair IOCs de IPs maliciosos
for event in events:
    for attr in event.get('Attribute', []):
        if attr['type'] == 'ip-src':
            print(f"IP malicioso: {attr['value']}")
```

---

## Cisco Talos Threat Intelligence

### O que é?
Uma das maiores equipes comerciais de threat intelligence do mundo. Mantém dados usados por produtos Cisco e também disponibiliza informações gratuitas.

### O que oferece de útil?

- Research reports de alta qualidade
- Reputação de IPs, domínios e arquivos
- Regras para Snort e ClamAV (open source)
- Dados que podem ser consumidos via APIs e feeds

### Como usar na prática

| Uso | Como Fazer | Dificuldade |
|-----|------------|-------------|
| Ler research reports | Acessar o blog do Talos regularmente | Baixa |
| Usar reputação de IPs | Verificar se há API gratuita ou usar via produtos Cisco | Média |
| Usar regras Snort | Baixar regras Snort Community e adaptar para detecção | Média |

---

## Outras Fontes de Threat Intelligence Úteis

| Fonte | Tipo | Uso Recomendado | Dificuldade de Integração |
|-------|------|-----------------|---------------------------|
| **NVD (NIST)** | Vulnerabilidades (CVE) | Consultar via API no CI/CD para verificar CVEs de dependências | Baixa (boa documentação) |
| **AbuseIPDB** | IP reputation | Middleware para bloquear IPs maliciosos | Baixa |
| **AlienVault OTX** | IOCs comunitários | Consumir pulses e IOCs via API | Baixa-Média |
| **SANS ISC / DShield** | Early warning + reputation | Monitorar tendências globais de ataque | Baixa |
| **FIRST** | Incident Response | Melhores práticas de resposta a incidentes | Baixa (leitura) |

---

## Como Integrar Threat Intelligence no Seu Código (Exemplos Práticos)

### 1. Middleware de Proteção (FastAPI)

```python
from fastapi import Request, HTTPException
import requests

async def threat_intel_middleware(request: Request, call_next):
    client_ip = request.client.host
    
    # Verificar reputação do IP (exemplo com AbuseIPDB)
    is_malicious = check_ip_reputation(client_ip)
    
    if is_malicious:
        raise HTTPException(status_code=403, detail="Access denied")
    
    response = await call_next(request)
    return response
```

### 2. Verificação de CVEs no CI/CD

```python
import nvdlib

def check_cves_for_package(package_name, version):
    cves = nvdlib.searchCVE(
        virtualMatchString=f'cpe:2.3:a:*:{package_name}:{version}:*:*:*:*:*:*:*'
    )
    critical_cves = [cve for cve in cves if cve.v31severity == 'CRITICAL']
    return critical_cves
```

### 3. Logging Estruturado com STIX-ready format

```python
import json
from datetime import datetime

def log_security_event(event_type, details):
    event = {
        "type": "security-event",
        "timestamp": datetime.utcnow().isoformat(),
        "event_type": event_type,
        "details": details,
        "stix_ready": True  # Flag para indicar que pode ser convertido para STIX
    }
    print(json.dumps(event))
```

---

## Recomendação de Implementação Gradual

| Fase | O que Implementar | Tempo Estimado | Valor Agregado |
|------|-------------------|----------------|----------------|
| **1. Fundação** | Consultar NVD via API no CI/CD | 1-2 semanas | Alto (bloqueia builds com CVEs críticas) |
| **2. Proteção Runtime** | Middleware simples com AbuseIPDB ou lista de IPs bloqueados | 2-3 semanas | Médio-Alto |
| **3. Estruturação** | Logging estruturado + integração básica com MISP | 3-4 semanas | Alto |
| **4. Maturidade** | Consumo automatizado de MISP + exportação de eventos em STIX | 1-2 meses | Muito Alto |

---

## Resumo

| Conceito | O que é | Por que importa para desenvolvedores |
|----------|---------|--------------------------------------|
| **STIX 2.1** | Formato padronizado para threat intelligence | Permite consumir e compartilhar threat intel de forma automatizada |
| **TAXII 2.1** | Protocolo para transportar STIX | Permite integração automatizada entre sistemas |
| **MISP** | Plataforma open-source para compartilhar IOCs | Melhor forma prática de começar a usar threat intel estruturada |
| **Cisco Talos** | Equipe de threat intelligence de alto nível | Research de qualidade + dados de reputação |
| **NVD + CVE** | Base de vulnerabilidades conhecidas | Essencial para gestão de dependências |

---

**Threat intelligence estruturada transforma segurança de reativa para proativa.** Quando sua aplicação consegue consumir IOCs e TTPs de forma automatizada, ela fica muito mais resistente a ataques conhecidos.

**Próximo documento:** [07-modelo-integrado.md](./07-modelo-integrado.md) — O modelo completo unindo todos os frameworks.