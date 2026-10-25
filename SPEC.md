# NextStep — Especificação do Projeto (SPEC)

> Plataforma de Manutenção Baseada na Condição (CBM) de Escadas Rolantes · Cliente/desafio: Motiva · FIAP 2ESPR · São Paulo, 2026
> Fonte: vault Obsidian `NextStep/` (Project Charter, requisitos, arquitetura, design system) + análise de [`index.html`](index.html).
> Versão 0.1 · 2026-10-25 · Status: rascunho para aprovação do squad

**Legenda de origem:** 📄 Charter (PDF) · 🧩 derivado do Charter · 💡 proposta ainda não validada · 🎨 Brand Board.
**Legenda de status no protótipo:** ✅ existe · 🟡 parcial / só visual · ❌ não existe · ⚠️ contradiz o escopo.

---

## 1. Resumo

O NextStep é uma plataforma web que usa dados de sensores IoT para classificar a condição de escadas rolantes de metrô em **Normal / Atenção / Crítico**, alertar anomalias e apoiar a priorização de ordens de serviço. É **apoio à decisão, nunca controle autônomo** 📄.

**Problema:** manutenção guiada por calendário + falhas corretivas inesperadas, com informação fragmentada → custo, indisponibilidade e transtorno ao passageiro 📄.

**Entrega-alvo:** um MVP que demonstra os conceitos de CBM com dados simulados. Não é implantação produtiva.

## 2. Estado atual do protótipo

`index.html` (arquivo único, ~1.000 linhas, sem build). Stack: HTML/CSS/JS puro, Three.js r128, Chart.js, Font Awesome, fonte Sora.

| Capacidade | Status | Observação |
|---|---|---|
| Sidebar com logo SVG e navegação | 🟡 | Itens só visuais; só "Visão Geral" existe. O **logo SVG já está no arquivo** — reaproveitar como asset oficial |
| 4 KPIs (total/normal/atenção/crítico) | 🟡 | Valores fixos (148/134/11/3), **não** calculados da lista (que tem 9 itens) |
| Grid de cards de equipamentos (temp + vibração + status) | ✅ | 9 equipamentos fixos em JS |
| Busca e filtro | 🟡 | Campos desenhados, sem função |
| Modal de detalhe com gêmeo digital 3D | ✅ | Three.js; luz/cor pulsante por status |
| Painel de telemetria (temp, vibração, tensão/corrente, tensão do corrimão) | 🟡 | Só temp e vibração mudam; os demais são fixos |
| Gráfico de histórico (2 h) | 🟡 | Dados **aleatórios** gerados no cliente |
| Texto de diagnóstico por status | 🟡 | Texto fixo por cor, não vem de regras |
| Emitir OS / agendar vistoria | 🟡 | Apenas `alert()` |
| Manual técnico | ❌ | Botão sem ação |
| Alertas, relatórios, configurações, indicadores | ❌ | |
| Login / papéis | ❌ | Usuário "Carlos Eduardo" fixo |
| Backend, banco, ingestão, simulador | ❌ | |
| Testes, acessibilidade, responsivo | ❌ | Layout fixo em `100vh`, `overflow:hidden` |

## 3. Divergências a corrigir (protótipo × documentação)

| # | Protótipo hoje | Documentação | Ação |
|---|---|---|---|
| D1 ⚠️ | Texto: "**Desligamento automático de segurança foi acionado**"; status "Paralisada" | Sistema **não** controla/desliga equipamentos (Escopo, RNF-01) | Remover; descrever "condição crítica detectada — recomendada intervenção" |
| D2 ⚠️ | "**Diagnóstico IA**" | ML avançado está fora; diagnóstico é por **regras** e **preliminar** (RN-05) | Renomear "Diagnóstico preliminar" + selo "requer validação técnica" |
| D3 ⚠️ | Botão "Emitir OS de Emergência" cria e "envia via Push" direto | OS só nasce após **ação humana explícita** com confirmação; sem integração produtiva (RN-06) | Drawer de criação pré-preenchido + confirmação; sem "push" simulado como real |
| D4 | "100% dos sensores online", "Suporte 24/7", "Integrado com sensores IoT" | Dados são simulados; sem promessas não comprovadas (RNF-10) | Selo **Dados simulados** global; remover afirmações falsas |
| D5 | Dados aleatórios no gráfico | Histórico vem do banco; simulador com `seed` | Trocar por série real (simulada) da API |
| D6 | Cor primária `#7045ff`, fundo `#111`, verde/amarelo/vermelho neon | Brand Board: Anil `#5E22F3`; status AA no Design System | Migrar para tokens do DS (seção 9) |
| D7 | Status só por cor + ícone pequeno | Status sempre com ícone + rótulo; contraste AA | Já tem rótulo; revisar ícones e contraste |
| D8 | Chart.js com fonte "Inter" | Fonte única Sora | Corrigir `Chart.defaults.font.family` |
| D9 | Animação de pulso permanente em críticos | `prefers-reduced-motion`; sem piscar > 3 Hz | Respeitar a preferência; reduzir a 3D estático |
| D10 | Sem distinção de papel | RBAC: Técnico / Coordenador / Operador | Implementar RF-10 |

## 4. Objetivos e escopo

### 4.1 Objetivos 📄
O1 centralizar em dashboard · O2 exibir temperatura/vibração/outros · O3 classificar Normal/Atenção/Crítico · O4 detectar anomalias por regras/limites · O5 históricos e tendências · O6 diagnósticos preliminares · O7 priorizar intervenções · O8 apoiar emissão/acompanhamento de OS · O9 indicadores MTBF, MTTR, disponibilidade.

### 4.2 Dentro do escopo (IN) 📄
Dados reais ou **simulados** · temperatura, vibração, corrente, tensão, velocidade (+ óleo/óptico se houver) · regras, limiares e classificação · alertas por gravidade · dashboard por equipamento/linha/estação/criticidade · séries temporais · consulta de dados técnicos, ocorrências e OS · APIs simuladas.

### 4.3 Fora do escopo (OUT) 📄
Desenvolver/instalar sensores · controle ou desligamento remoto · ML avançado · priorização por fluxo de passageiros · app nativo / offline / geolocalização · integrações produtivas com a Motiva · previsões quantitativas de falha · substituir inspeções ou decisão técnica.

### 4.4 Restrição operacional ⚠️ 📄
A preventiva mensal **P1 é obrigatória**. A condição pode apoiar o planejamento das demais (**P6, P9, P12, P18, PD48**), sempre como recomendação validada por humano.

## 5. Usuários e papéis

| Papel (`role`) | Persona | Pode |
|---|---|---|
| `TECNICO` | Carlos Eduardo — técnico de manutenção | Ver equipamentos/alertas, diagnóstico, registrar execução de OS, manual técnico |
| `COORDENADOR` | Mariana Costa — engenheira/coordenadora | Tudo do técnico + validar diagnóstico, criar/atribuir OS, configurar limites, indicadores |
| `OPERADOR` | Rafael Almeida — supervisor de operações | Leitura de status/alertas/disponibilidade por linha/estação |

Personas são **hipóteses de design** (desk research); validar com usuários reais 📄.

## 6. Requisitos funcionais

Prioridade MoSCoW. Coluna **Proto** = situação no `index.html`.

| ID | Requisito | Prio | Proto |
|---|---|---|---|
| **RF-01** Dashboard | | | |
| RF-01.1 | Listar equipamentos com status, estação, linha, última leitura | M | 🟡 |
| RF-01.2 | Filtrar por linha, estação, criticidade, tipo de alerta | M | 🟡 |
| RF-01.3 | KPIs **calculados**: total, por criticidade, alertas abertos, OS abertas | M | 🟡 |
| RF-01.4 | Visão por linha → estação | S | ❌ |
| **RF-02** Telemetria | | | |
| RF-02.1 | Temperatura, vibração, corrente, tensão, velocidade com unidade e timestamp | M | 🟡 |
| RF-02.2 | Atualização sem recarregar (SSE) | M | ❌ |
| RF-02.3 | Óleo e sensores ópticos quando existirem | C | ❌ |
| RF-02.4 | Sinalizar **simulado** e **desatualizado** | M | ❌ |
| **RF-03** Classificação | | | |
| RF-03.1 | Normal/Atenção/Crítico conforme RN-01 | M | 🟡 (rótulo fixo) |
| RF-03.2 | Mostrar parâmetros que motivaram o status | M | 🟡 |
| RF-03.3 | Registrar mudanças de estado | M | ❌ |
| **RF-04** Alertas | | | |
| RF-04.1–.2 | Gerar alerta ao violar limite; com gravidade, parâmetro, valor × limite, hora | M | ❌ |
| RF-04.3 | Ciclo Aberto → Reconhecido → Resolvido | M | ❌ |
| RF-04.4 | Deduplicação/histerese | S | ❌ |
| RF-04.5 | CRUD de limites (Coordenador) | M | ❌ |
| **RF-05** Histórico | | | |
| RF-05.1 | Série temporal por parâmetro com períodos 24h/7d/30d | M | 🟡 (mock) |
| RF-05.2 | Linhas de limite no gráfico | S | ❌ |
| RF-05.3 | Comparar dois equipamentos | S | ❌ |
| RF-05.4 | Histórico de estados, alertas e OS | M | ❌ |
| **RF-06** Diagnóstico | | | |
| RF-06.1–.2 | Hipótese + ação por regras, rotulado "preliminar" | M | 🟡 ⚠️ |
| RF-06.3 | Confirmar/rejeitar | S | ❌ |
| RF-06.4 | Manual técnico do modelo | C | ❌ |
| **RF-07** Priorização | | | |
| RF-07.1–.2 | Lista por score com justificativa | M/S | ❌ |
| **RF-08** Ordens de serviço | | | |
| RF-08.1 | Criar OS só por ação humana confirmada | M | 🟡 ⚠️ |
| RF-08.2–.3 | Atribuir; fluxo Aberta→Atribuída→Em execução→Concluída/Cancelada | M | ❌ |
| RF-08.4 | Registro de execução | M | ❌ |
| RF-08.5 | Vínculo com plano P1…PD48 | S | ❌ |
| **RF-09** Indicadores | | | |
| RF-09.1 | Disponibilidade, MTBF, MTTR | M | ❌ |
| RF-09.2 | Exportar CSV/PDF | S | ❌ |
| **RF-10** Acesso | | | |
| RF-10.1 | Autenticação + 3 papéis | M | ❌ |
| RF-10.2 | Auditoria | M | ❌ |
| RF-10.3 | Responsivo / uso em celular | S | ❌ |

**Won't (agora):** controle remoto, abertura automática de OS, ML avançado, app nativo, integração produtiva, geolocalização.

**Item extra do protótipo (decisão pendente):** o **gêmeo digital 3D** não está no Charter. Proposta 💡: manter como visualização **opcional** do detalhe (destacar componente associado ao parâmetro em violação), carregado sob demanda e com fallback 2D — não é requisito do MVP.

## 7. Regras de negócio

Valores são **placeholders de demonstração** 💡 — validar com a Engenharia de Manutenção.

- **RN-01 Classificação:** cada parâmetro tem limites `atencao`/`critico` (e mínimos quando aplicável). Estado do equipamento = pior estado entre parâmetros válidos.

| Parâmetro | Unidade | Atenção | Crítico |
|---|---|---|---|
| Temperatura do motor | °C | ≥ 70 | ≥ 85 |
| Vibração (RMS) | mm/s | ≥ 4,5 | ≥ 7,1 |
| Corrente | A | ≥ 110% nominal | ≥ 125% nominal |
| Tensão | V | fora de ±5% | fora de ±10% |
| Velocidade | m/s | desvio > 5% | desvio > 10% |

  > Os valores do protótipo (87 °C / 12,4 mm/s = Crítico; 58–65 °C / 5,4–6,2 mm/s = Atenção; ≤ 42 °C = Normal) são consistentes com estes limites, exceto ER-35 (58 °C e 5,8 mm/s → Atenção só por vibração).
- **RN-02 Dado inválido/ausente:** fora de faixa física → descartado; sem leitura > 5 min → `SEM_SINAL` (nunca assume Normal).
- **RN-03 Histerese:** entrar em Atenção/Crítico após 2 leituras consecutivas; sair após 3 abaixo do limite; um alerta aberto por (equipamento, parâmetro, gravidade).
- **RN-04 Score de prioridade:** `100·(0,50·estado + 0,20·tendência + 0,15·alertas + 0,15·tempo_sem_manutenção)`; pesos configuráveis; sempre exibir decomposição.
- **RN-05 Diagnóstico:** tabela sintoma→hipótese→ação, editável; saída sempre "preliminar — requer validação técnica"; feedback armazenado.
- **RN-06 OS:** nunca automática; estados `ABERTA → ATRIBUIDA → EM_EXECUCAO → CONCLUIDA | CANCELADA`; conclusão exige ação registrada.
- **RN-07 Indicadores:** Disponibilidade = (total − indisponível)/total; MTBF = tempo operacional/nº falhas; MTTR = tempo de reparo/nº reparos. Dados simulados → aviso "não representa resultados reais".
- **RN-08 Preventivas:** P1 sempre obrigatória; condição só **recomenda** sobre P6/P9/P12/P18/PD48.
- **RN-09 Segurança:** nenhuma regra emite comando a equipamento.

## 8. Arquitetura proposta 💡

Monólito modular; fronteiras claras para evoluir.

```
Simulador ──POST /telemetria──▶ Ingestão (porta/adaptador) ──▶ Motor de regras ──▶ PostgreSQL
                                                              │                       ▲
Web (React PWA) ◀── REST + SSE ── API ◀──────────────────────┴───────────────────────┘
```

| Camada | Escolha (ADR-001, *proposto*) | Alternativas |
|---|---|---|
| Front-end | React + TypeScript + Vite, PWA, ECharts/Recharts | manter HTML/JS e modularizar |
| 3D (opcional) | Three.js (já usado) via import dinâmico | — |
| Back-end | Node.js + NestJS | Java/Spring · Python/FastAPI |
| Banco | PostgreSQL (+ TimescaleDB opcional) | — |
| Tempo real | Server-Sent Events | WebSocket |
| Auth | JWT + RBAC | — |
| Infra | Docker Compose, GitHub Actions | — |

**Módulos:** `equipamentos`, `telemetria`, `condicao`, `alertas`, `diagnostico`, `priorizacao`, `ordens-servico`, `indicadores`, `identidade`.

### 8.1 Modelo de dados (resumo)
`linha → estacao → equipamento → sensor → leitura(ts, valor, valida)` · `limite` (por modelo/equipamento) · `estado_historico` · `alerta` · `diagnostico` / `regra_diagnostico` · `ordem_servico` → `execucao` · `usuario` · `auditoria` · `indicador_snapshot`. Tempo em UTC.

### 8.2 API (`/api/v1`, JWT)
| Método | Rota | Papel |
|---|---|---|
| POST | `/auth/login` | público |
| GET | `/equipamentos`, `/equipamentos/{id}` | todos |
| GET | `/equipamentos/{id}/leituras?tipo&de&ate&agregacao` | todos |
| GET | `/equipamentos/{id}/estados` | todos |
| POST | `/telemetria` (lote) | token de serviço |
| GET / PATCH | `/alertas`, `/alertas/{id}` | todos / TECNICO, COORD |
| GET / PUT | `/limites` | todos / COORD |
| GET / PATCH | `/equipamentos/{id}/diagnosticos`, `/diagnosticos/{id}` | TECNICO, COORD |
| GET | `/priorizacao` | TECNICO, COORD |
| POST / PATCH | `/ordens-servico`, `/ordens-servico/{id}` | COORD |
| POST | `/ordens-servico/{id}/execucao` | TECNICO |
| GET | `/indicadores?escopo&periodo` | COORD, OPERADOR |
| GET | `/stream` (SSE) | todos |

**Garantia de design:** não existe rota que envie comando a equipamento.

### 8.3 Simulador de sensores
Processo separado; `valor(t) = base + sazonalidade + ruído + desvio_do_cenário`; `seed` reprodutível; marca `simulado:true`. Cenários: `saudavel`, `degradacao_rolamento`, `sobrecarga`, `queda_tensao`, `sensor_mudo`, `outlier`. Suporta *backfill* de histórico para demo. Os 9 equipamentos do protótipo viram o **seed inicial** (códigos `ER-04`, `ER-12`, `ER-01`, `ER-08`, `ER-22`, `ER-35`, `ER-02`, `ER-15`, `ER-16`, com linhas/estações atuais).

## 9. Design system 🎨 💡

Base: Brand Board (Anil `#5E22F3`, Branco, Preto, Neutros 20/40/60; fonte **Sora**). Detalhes e tokens no vault: `NextStep/10 - Design System/`.

### 9.1 Mapeamento de tokens (protótipo → DS)
| Protótipo | Novo token | Valor |
|---|---|---|
| `--c-primary #7045ff` | `--primary` / `--ns-anil` | `#5E22F3` |
| `--c-primary-dark #4912ff` | `--primary-hover` | `#4B16CC` |
| `--c-primary-light #9778ff` | `--link` (dark) | `#B9A0FF` |
| `--c-bg #111111` | `--bg` (dark) | `#0B0B14` |
| `--c-card #2a2a2a` | `--surface` (dark) | `#14141F` |
| `--c-text-muted #999` | `--text-muted` (dark) | `#A5A5B9` |
| `--c-green #00e676` | `--normal-fg` (dark) / `-solid` | `#3DD68C` / `#12B76A` |
| `--c-yellow #ffea00` | `--atencao-fg` (dark) / `-solid` | `#F5A524` / `#F5A524` |
| `--c-red #ff1744` | `--critico-fg` (dark) / `-solid` | `#FF6B5E` / `#D92D20` |

O protótipo é **tema escuro**; o DS define claro (padrão) e escuro via `prefers-color-scheme` + `data-theme`.

### 9.2 Regras essenciais
- Status sempre **ícone + rótulo + cor** (● Normal · ▲ Atenção · ■ Crítico · ○ Sem sinal).
- Neutro 40 (`#A5A5B9`) **nunca** como texto sobre branco (2,42:1).
- Alvos de toque ≥ 44 px; foco visível; WCAG 2.1 AA.
- Componentes de domínio: `StatusBadge`, `SimBadge`, `StaleBadge`, `KpiCard`, `EquipmentCard`, `TelemetryTile`, `TimeSeriesChart`, `AlertItem`, `DiagnosisCard`, `PriorityScore`, `WorkOrderCard`.
- Microcopy: "Preliminar — requer validação técnica" · "Dados simulados — não representam a operação real" · "Sem leitura há N min — status pode estar desatualizado".

## 10. Telas

| Tela | Papel principal | RF | No protótipo |
|---|---|---|---|
| Dashboard (KPIs, alertas críticos, lista por prioridade, filtros) | Mariana, Rafael | RF-01 | 🟡 "Visão Geral" |
| Detalhe do equipamento (telemetria, gráfico, "por que este status?", diagnóstico, manual, abas de histórico/alertas/OS, 3D opcional) | Carlos | RF-02/05/06 | 🟡 modal |
| Alertas | todos | RF-04 | ❌ ("Alertas Ativos" só no menu) |
| Ordens de serviço (kanban + tabela; criação em drawer) | Mariana, Carlos | RF-08 | ❌ |
| Execução em campo (mobile) | Carlos | RF-08.4 | ❌ |
| Indicadores | Mariana | RF-09 | ❌ ("Relatórios Técnicos" só no menu) |
| Limites | Mariana | RF-04.5 | ❌ ("Configurações" só no menu) |
| Login | todos | RF-10 | ❌ |

Navegação (renomear para alinhar ao DS): Dashboard · Equipamentos · Alertas · Ordens de serviço · Indicadores · Configurações. Remover o cartão "Suporte Técnico 24/7" (D4).

## 11. Requisitos não funcionais 💡

| ID | Requisito | Meta |
|---|---|---|
| RNF-01 | Sem endpoints de comando a equipamento | por design |
| RNF-02 | Auth + RBAC; HTTPS; senhas com hash forte | 100% das rotas protegidas |
| RNF-03 | Auditoria de mutações | 100% |
| RNF-04 | Dashboard | p95 < 2 s com 200 equipamentos |
| RNF-05 | Latência leitura → tela | < 10 s |
| RNF-06 | WCAG 2.1 AA; tarefas em ≤ 3 cliques | — |
| RNF-07 | Falha da ingestão não derruba a UI; aviso de dado velho | — |
| RNF-08 | Limites e regras de diagnóstico em banco | — |
| RNF-09 | Ingestão trocável (simulador → real) | — |
| RNF-10 | Dados simulados sinalizados em toda a UI | selo visível |
| RNF-11 | `docker compose up` para demo | — |
| RNF-12 | Cobertura de testes no domínio | ≥ 80% |
| RNF-13 | pt-BR; UTC no banco, `America/Sao_Paulo` na UI | — |
| RNF-14 | LGPD: mínimo de dados pessoais | — |

## 12. Critérios de aceite (principais)

1. **Crítico em < 10 s:** leitura simulada ≥ limite crítico por 2 ciclos → equipamento vira Crítico, alerta aparece no dashboard, KPI atualiza.
2. **Explicável:** abrir um equipamento Atenção/Crítico mostra qual parâmetro violou qual limite.
3. **OS humana:** a partir de um alerta, Coordenador abre drawer pré-preenchido, confirma, e só então a OS existe; Técnico atribuído registra execução e conclui; indicadores refletem.
4. **RBAC:** Operador não cria OS nem edita limites; Técnico não edita limites.
5. **Sensor mudo:** sem leitura > 5 min → `SEM_SINAL`, nunca "Normal".
6. **Transparência:** todo dado simulado exibe o selo; diagnóstico exibe "preliminar".
7. **Sem comando:** o OpenAPI não contém rota de acionamento/desligamento.
8. **Acessibilidade:** navegação por teclado completa; contraste AA verificado; `prefers-reduced-motion` respeitado.

## 13. Plano de evolução a partir do protótipo

| Fase | Entrega | Marco |
|---|---|---|
| 0 — Saneamento (1–2 dias) | Corrigir D1–D4, D8: textos de escopo, selo "Dados simulados", remover falsas promessas, fonte do gráfico | Protótipo honesto com o Charter |
| 1 — Tokens e modularização | Aplicar tokens do DS (claro/escuro), separar CSS/JS, logo como asset, KPIs calculados da lista | Base visual alinhada à marca |
| 2 — Backend e dados | Fundação (repo, CI, Docker), auth/RBAC, cadastro, simulador, ingestão, detalhe com leituras reais (S3–S4) | **M1** leituras simuladas na tela |
| 3 — Condição e alertas | Classificação, histerese, alertas, limites, dashboard dinâmico (S5) | **M2** MVP monitora e alerta |
| 4 — Decisão | Histórico/tendências, diagnóstico por regras, priorização (S6) | |
| 5 — Execução | OS completas (S7) | |
| 6 — Indicadores e polimento | MTBF/MTTR/disponibilidade, responsivo/PWA, a11y, E2E, demo (S8) | **M3** MVP completo |

Backlog detalhado e estimativas (≈ 132 pontos): `NextStep/07 - Planejamento/Backlog do Produto`.

## 14. Qualidade

- **Unidade:** classificação, histerese, score, indicadores, diagnóstico (tabela-verdade de fronteiras: = limite, ±ε, picos, inválido).
- **Integração:** API + Postgres, RBAC, auditoria, ingestão.
- **E2E (Playwright):** fluxos 1, 3, 4, 5 da seção 12.
- **Não funcional:** k6 (carga), axe (a11y), checklist OWASP.
- **Definition of Done:** critérios atendidos, PR revisado, CI verde, sem violação de segurança, auditoria, selo simulado, pt-BR/AA, docs atualizadas.

## 15. Riscos

| Risco | Mitigação |
|---|---|
| Sem dados reais da Motiva | Simulador + adaptador pronto |
| Limites sem validação técnica | Configuráveis; rotulados como demonstração; validar com Engenharia |
| Personas não validadas | Sessões com usuários |
| Escopo crescer (ML, integrações, app nativo, 3D) | Respeitar IN/OUT; mudanças via backlog |
| Diagnóstico lido como decisão autônoma | Rótulo "preliminar"; sem OS automática |
| Fadiga de alertas | Histerese e deduplicação |
| Promessas de ganho sem prova | Resultados como "objetivos"; linha de base futura |

## 16. Questões em aberto

- Limites reais por modelo de escada; existência de dados históricos reais e formato.
- Significado exato e periodicidade de P6, P9, P12, P18, PD48.
- Marco do MTTR e definição de "indisponível" para disponibilidade.
- Sensores disponíveis de fato (óleo, ópticos); taxonomia oficial de falhas.
- Quem pode criar OS (só coordenação ou técnicos também).
- Aprovar/trocar o stack (ADR-001); papéis do squad (PO, SM, Tech lead); duração das sprints.
- Manter ou não o gêmeo digital 3D no MVP.
- Notificações além da tela (e-mail/push) no MVP.

## 17. Rastreabilidade com o vault Obsidian

| Seção desta spec | Notas em `NextStep/` |
|---|---|
| 1, 4 | `01 - Visao/*`, `04 - Escopo/Escopo` |
| 5 | `03 - Usuarios/*` |
| 6 | `05 - Requisitos/Requisitos Funcionais`, `Histórias de Usuário` |
| 7 | `05 - Requisitos/Regras de Negócio` |
| 8 | `06 - Arquitetura/*`, `ADR-001 Stack Tecnológica` |
| 9, 10 | `10 - Design System/*` |
| 11 | `05 - Requisitos/Requisitos Não Funcionais` |
| 13, 15 | `07 - Planejamento/Roadmap`, `Backlog do Produto`, `Riscos` |
| 14 | `08 - Qualidade/*`, `07 - Planejamento/Definition of Done` |
| 16 | `Questões em Aberto` |

## 18. Referências 📄

1. KONE — *24/7 Connected Services: Intelligent Predictive Maintenance*.
2. MTR Corporation — *Consultancy Services: Smart Operation and Maintenance* (2024).
3. LA Metro — *Engineering Support Services for Vertical Transportation System*, rel. 2023-0620.
4. Ensafi, Thabet, Afsari, Yang — *Challenges and gaps with user-led decision-making for prioritizing maintenance work orders*, J. Building Engineering 66 (2023).
