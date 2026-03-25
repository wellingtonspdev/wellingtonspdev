<!-- HEADER ANIMADO E DINÂMICO (DARK/LIGHT MODE) -->
<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=24&pause=1000&color=FFFFFF&center=true&vCenter=true&width=600&lines=Software+Engineer;Applied+AI+%26+Integrations;Cloud+FinOps+Enthusiast">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=24&pause=1000&color=000000&center=true&vCenter=true&width=600&lines=Software+Engineer;Applied+AI+%26+Integrations;Cloud+FinOps+Enthusiast" alt="Typing SVG">
  </picture>
</div>

<!-- LINKS SOCIAIS MINIMALISTAS -->
<div align="center">
  <br />
  <a href="https://www.linkedin.com/in/wellingtonsp-dev" target="_blank">
    <img src="https://img.shields.io/badge/-LinkedIn-black?style=flat&logo=linkedin&logoColor=white" alt="LinkedIn" />
  </a>
  &nbsp;
  <a href="https://github.com/wellingtonspdev" target="_blank">
    <img src="https://img.shields.io/badge/-GitHub-black?style=flat&logo=github&logoColor=white" alt="GitHub" />
  </a>
  &nbsp;
  <a href="mailto:wellingtonsp.dev@gmail.com" target="_blank">
    <img src="https://img.shields.io/badge/-E--mail-black?style=flat&logo=gmail&logoColor=white" alt="E-mail" />
  </a>
  &nbsp;
  <a href="https://wellingtonspdev.github.io" target="_blank">
    <img src="https://img.shields.io/badge/-Portfolio-black?style=flat&logo=googlechrome&logoColor=white" alt="Portfolio" />
  </a>
  <br />
  <br />
</div>

---

## ✦ Sobre Mim

> **Engenharia de Stakeholders & Soluções Centradas no Usuário**

Graduando em Engenharia de Software (FATEC) e Pesquisador de IA (CNPq), combinando rigor acadêmico com 5+ anos de experiência prévia em Customer Experience (Varejo). Essa trajetória me garante forte "Engenharia de Stakeholders" e um foco inabalável em arquitetar soluções que resolvem dores reais de negócio.

Atualmente, minha atuação técnica está concentrada nos pilares de:
- **Inteligência Artificial (Applied AI):** Implementação de arquiteturas RAG (Local/Cloud) e IA Semântica isolada.
- **Backend & Integrações:** Construção de sistemas distribuídos resilientes utilizando Webhooks e padrões como *Transactional Outbox*.
- **Cloud FinOps:** Design de infraestrutura com foco absoluto em *Zero OpEx* e *Unit Economics* escaláveis.

<br />

## ✦ Tech Stack Principal

> Ferramental consolidado para construção de sistemas distribuídos e IAs aplicadas.

| Core & Runtimes | Data & Messaging | Infra & Cloud | Frontend & UI |
| :---: | :---: | :---: | :---: |
| <a href="https://skillicons.dev"><img src="https://skillicons.dev/icons?i=python,nodejs,ts&theme=dark" /></a> | <a href="https://skillicons.dev"><img src="https://skillicons.dev/icons?i=postgres,redis&theme=dark" /></a> | <a href="https://skillicons.dev"><img src="https://skillicons.dev/icons?i=docker,aws&theme=dark" /></a> | <a href="https://skillicons.dev"><img src="https://skillicons.dev/icons?i=react&theme=dark" /></a> |

<br />

---

## ✦ Cases de Arquitetura (System Design / Private Repos)

Abaixo estão arquiteturas que projetei e implementei em cenários de alta complexidade de negócio e integrações críticas.

### 1. WSP Finance (SaaS B2B2C)
Plataforma financeira desenhada com foco em auditoria automatizada e otimização agressiva de custos operacionais.
- **Linter Fiscal:** Motor de IA Semântica isolada utilizando Google Vertex AI para análise e validação de regras de negócio em documentos fiscais.
- **Infraestrutura Zero OpEx:** Armazenamento em larga escala de notas fiscais utilizando Cloudflare R2, eliminando completamente as taxas de egresso tradicionais.

### 2. Ecossistema VIVA (HealthTech)
Ecossistema de saúde focado em processamento assíncrono de alto volume e inferência de dados sensíveis. Construído com NestJS.
- **Stack de Alta Performance:** Orquestração de filas com BullMQ, caching e *rate-limiting* via Redis.
- **RAG Local:** Implementação de Retrieval-Augmented Generation *on-premise* utilizando \`sqlite-vec\` para garantia de privacidade de dados médicos (*Zero-Trust*).

**Padrão de Resiliência: Transactional Outbox**
Implementado para garantir consistência eventual em integrações de sistemas legados de clínicas, onde a estabilidade da rede não é garantida:

```mermaid
sequenceDiagram
    autonumber
    participant Client as Client Request
    participant API as VIVA Core (NestJS)
    participant DB as PostgreSQL (Outbox)
    participant Worker as Outbox Poller
    participant Queue as BullMQ / Redis

    Client->>API: POST /paciente/prontuario
    activate API
    API->>DB: BEGIN TRANSACTION
    API->>DB: INSERT INTO prontuarios (dados)
    API->>DB: INSERT INTO outbox_events (evento_sync_clinica)
    DB-->>API: COMMIT
    API-->>Client: 201 Created (ACK)
    deactivate API

    loop Every 2s
        Worker->>DB: SELECT * FROM outbox_events WHERE status = 'PENDING'
        activate Worker
        DB-->>Worker: Events List
        Worker->>Queue: Publish Event (Job)
        Worker->>DB: UPDATE outbox_events SET status = 'PROCESSED'
        deactivate Worker
    end
```

### 3. Samurai Pro (Plataforma de Automação B2B)
Motor de automação e integração de processos de negócio, construído com foco em escalabilidade e tolerância a falhas.
- **Core:** Desenvolvido em Python (FastAPI), conteinerizado via Docker.
- **Workflow & Orquestração:** Fluxos complexos orquestrados de forma declarativa via n8n.
- **Auto-healing com IA:** Utilização de IA Multimodal (GPT-4o Vision) para identificar quebras de UI em automações web e disparar rotinas de autocorreção (*Thread* de recuperação).

<br />

---

## ✦ Cloud Unit Economics & FinOps (ADR)

> **Decisão Arquitetural de Armazenamento:** AWS S3 vs Cloudflare R2 para WSP Finance

Como Engineering Manager, priorizo o design de arquiteturas que não apenas escalam tecnicamente, mas também financeiramente. A tabela abaixo ilustra a projeção de economia (FinOps) no armazenamento de 5TB de notas fiscais com 2TB de egresso mensal.

| Provedor / Serviço | Custo Storage (5TB) | Custo Egresso (2TB) | Custo Total Mensal | Impacto (Anualizado) |
| :--- | :---: | :---: | :---: | :---: |
| **AWS S3** (Standard) | ~$115.00 | ~$180.00 | **~$295.00** | ~$3,540.00 |
| **Cloudflare R2** | ~$75.00 | **$0.00** | **~$75.00** | **~$900.00** |

**Decisão (ADR):** A escolha pelo Cloudflare R2 reduziu o custo projetado de egresso a **zero**, gerando uma economia estimada de aproximadamente **74%** ao mês. Este design *Zero OpEx* de egresso é fundamental para manter a margem do produto B2B2C sustentável em larga escala.

<br />

---

<!-- INSERIR GITHUB 3D CONTRIB AQUI -->
<!--
Lembrete: Ativar a GitHub Action 'github-profile-3d-contrib'
(https://github.com/yoshi389111/github-profile-3d-contrib)
para gerar o skyline tridimensional de contribuições nesta seção.
Isso substituirá os "Stats Cards" tradicionais por uma visualização
impactante e elegante da sua constância de código.
-->
<div align="center">
  <p><i>Gráfico de Contribuições 3D (Skyline) a ser renderizado via Action.</i></p>
</div>

<br />
<div align="center">
  <p><i>"Code is poetry, architecture is strategy."</i></p>
</div>
