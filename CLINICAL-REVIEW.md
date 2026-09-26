# Revisão clínica das ferramentas Elucenia · roteiro para o médico revisor

Cada ferramenta da organização github.com/Elucenia reproduz uma fórmula ou escore publicado. A aritmética, a robustez e a segurança do código foram auditadas e verificadas por reimplementação independente (25/09/2026). O que falta, e só um médico pode dar, é a **revisão clínica**: confirmar que a fórmula, a versão, a população, as unidades e os limites estão certos para uso assistencial no Brasil.

Este roteiro é o que cada revisor preenche por ferramenta. O resultado vai para `tool.json` (campo `review`) e para a seção "Situação" do `README.md`.

## 1. Quem revisa

- Médico com registro ativo no CRM, da especialidade indicada em `tool.json` → `spec`.
- Sem conflito de interesse com o instrumento (autor, licenciante ou distribuidor comercial do escore).
- Identificado por nome, CRM e data no registro da revisão. A responsabilidade é nominal, como em qualquer parecer.

## 2. O que conferir, ferramenta por ferramenta

| # | Item | Onde olhar | Aprova se |
|---|---|---|---|
| 1 | **Fonte** | `tool.json` → `sources` | A referência é a publicação original ou a diretriz vigente, com ano; o DOI ou link abre. |
| 2 | **Versão** | `formula`, `sources` | A versão implementada é a que a especialidade usa hoje (ex.: CKD-EPI 2021, não 2009; Duke-ISCVID 2023). Se houver versão mais nova, registrar. |
| 3 | **Fórmula e pesos** | `formula`, `fields`, `README.md` | Cada coeficiente, ponto de corte e peso confere com a fonte, item por item. |
| 4 | **Unidades** | `fields` → `unit` | Unidades e conversões são as usadas no Brasil (mg/dL, mEq/L, mmol/L onde couber) e estão explícitas no rótulo. |
| 5 | **Intervalos** | `fields` → `min`, `max` | Os limites de entrada são clinicamente plausíveis e rejeitam absurdos sem bloquear casos reais (ex.: sódio 100–180). |
| 6 | **População** | `lead`, `tips` | A página diz para quem a ferramenta vale (adultos, crianças, gestantes, UTI) e para quem não vale. |
| 7 | **Interpretação** | `bands`, `table`, `tips` | Faixas, classes e textos de interpretação são os da fonte, sem conduta terapêutica ("tome", "suspenda") e sem promessa. |
| 8 | **Casos de referência** | `examples.json` | Pelo menos 3 casos, um deles de erro de domínio, com valores esperados calculados pela fonte, não pela ferramenta. Adicionar um caso da própria prática. |
| 9 | **Avisos** | `index.html`, `README.md` | "Não substitui avaliação médica" presente; limitações relevantes citadas (ex.: APACHE II calibrado nos anos 1980). |
| 10 | **Risco de dano** | julgamento do revisor | Se a ferramenta calcula dose, volume ou reposição, a revisão exige um segundo revisor e teste com casos-limite antes de sair de `restricted`. |

## 3. Como registrar

No `tool.json`:

```json
"review": {
  "status": "clinically-reviewed",
  "reviewer": "Nome Completo · CRM-UF 00000 · especialidade",
  "date": "AAAA-MM-DD",
  "guideline": "Fonte e versão confirmadas (ex.: KDIGO 2024)",
  "notes": "O que foi ajustado ou o que o usuário precisa saber",
  "secondReviewer": "obrigatório para dose, volume ou reposição"
}
```

Estados possíveis de `status`: `needs-review` (padrão) → `clinically-reviewed` (aprovada) ou `restricted` (bloqueada: o adaptador devolve `REVIEW_REQUIRED` até nova revisão). Uma ferramenta aprovada volta para `needs-review` quando a diretriz muda.

No `README.md`, seção "Situação": trocar "Revisão documental e clínica independente pendente" por "Revisão clínica: Nome, CRM, data, fonte confirmada".

## 4. Ordem sugerida

1. As 21 ferramentas `restricted` (dose, volume, reposição): só saem do bloqueio com dois revisores.
2. Escores de risco com conduta associada (Wells, Genebra, PERC, CURB-65, qSOFA, NEWS2, HEART, GRACE, TIMI, CHA₂DS₂-VASc, HAS-BLED, Caprini, Pádua, Khorana).
3. Calculadoras laboratoriais e renais (CKD-EPI, Cockcroft-Gault, Schwartz, FENa, ânion gap, osmolaridade, sódio corrigido, cálcio corrigido).
4. Escalas e questionários (PHQ-9, GAD-7, EPDS, AUDIT, CAGE, GDS-15, SRQ-20): confirmar a versão validada em português e as autorizações de uso do instrumento (ver `NOTICE`).
5. O restante, por especialidade.

## 5. O que a revisão não faz

Não valida a ferramenta para uso regulatório (ANVISA) nem a torna produto médico. Ela atesta que a reprodução é fiel à fonte e adequada à população indicada. O uso assistencial continua sob responsabilidade do médico que a utiliza, como diz cada `README.md`.

Elucenia · contato@elucenia.org · Copyright (c) 2026 Elucenia · Felipe Guedes.
