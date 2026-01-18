# Templates para Retrospectivas - Sistema de Métricas

## 1. Template: Retrospectiva Operacional Semanal

### 📋 Informações da Reunião
- **Duração**: 30 minutos
- **Frequência**: Semanal (toda segunda-feira, 9h)
- **Participantes**: Tech Lead, Product Owner, Scrum Master
- **Facilitador**: Scrum Master

### 📊 Agenda Padrão

#### 1. Review de Métricas (10 min)
```markdown
## Métricas da Semana {{semana}}/{{ano}}

### 🎯 KPIs Principais
| Métrica | Valor Atual | Meta | Status | Tendência |
|---------|-------------|------|--------|-----------|
| Throughput | {{valor}} tarefas | 50 | {{status}} | {{trend}} |
| Tempo Médio de Ciclo | {{valor}} horas | <4h | {{status}} | {{trend}} |
| Taxa de Retrabalho | {{valor}}% | <15% | {{status}} | {{trend}} |
| Taxa de Falhas | {{valor}}% | <5% | {{status}} | {{trend}} |
| NPS Médio | {{valor}} | >7.0 | {{status}} | {{trend}} |

### 📈 Comparativo com Semana Anterior
- Throughput: {{variacao}}
- Tempo de Ciclo: {{variacao}}
- Qualidade: {{variacao}}
```

#### 2. Análise de Destaques (10 min)
```markdown
### ✅ Sucessos da Semana
- [ ] {{sucesso_1}}
- [ ] {{sucesso_2}}
- [ ] {{sucesso_3}}

### ⚠️ Pontos de Atenção
- [ ] {{atencao_1}} - Impacto: {{impacto}} - Ação: {{acao}}
- [ ] {{atencao_2}} - Impacto: {{impacto}} - Ação: {{acao}}

### 🚨 Problemas Críticos
- [ ] {{problema_1}} - Severidade: {{sev}} - Owner: {{owner}} - Prazo: {{prazo}}
- [ ] {{problema_2}} - Severidade: {{sev}} - Owner: {{owner}} - Prazo: {{prazo}}
```

#### 3. Plano de Ação (10 min)
```markdown
### 🎯 Ações para Próxima Semana

#### Ações Imediatas (24-48h)
- [ ] {{acao_1}} - Responsável: {{pessoa}} - Prazo: {{data}}
- [ ] {{acao_2}} - Responsável: {{pessoa}} - Prazo: {{data}}

#### Ações de Melhoria (1 semana)
- [ ] {{melhoria_1}} - Responsável: {{pessoa}} - Prazo: {{data}}
- [ ] {{melhoria_2}} - Responsável: {{pessoa}} - Prazo: {{data}}

#### Experimentos/Testes
- [ ] {{experimento_1}} - Hipótese: {{hipotese}} - Métrica: {{metrica}}
```

---

## 2. Template: Análise de Tendências Mensal

### 📋 Informações da Reunião
- **Duração**: 60 minutos
- **Frequência**: Mensal (primeira sexta-feira do mês, 14h)
- **Participantes**: Toda equipe + Stakeholders
- **Facilitador**: Product Owner

### 📊 Agenda Padrão

#### 1. Apresentação de Tendências (20 min)
```markdown
## Análise Mensal - {{mes}}/{{ano}}

### 📈 Evolução dos KPIs

#### Throughput
- Média mensal: {{valor}} tarefas/semana
- Variação vs. mês anterior: {{variacao}}%
- Tendência: {{tendencia}}
- Análise: {{analise}}

#### Qualidade
- Tempo médio de ciclo: {{valor}} horas
- Taxa de retrabalho: {{valor}}%
- Qualidade técnica média: {{valor}}/5
- Aprovação primeira tentativa: {{valor}}%

#### Satisfação
- NPS médio: {{valor}}
- Usuários ativos: {{valor}}
- Feedback qualitativo: {{resumo}}

### 🎯 Performance vs. Metas Trimestrais
| KPI | Meta Trimestral | Progresso Atual | Projeção | Status |
|-----|-----------------|-----------------|----------|--------|
| Throughput | {{meta}} | {{atual}} | {{projecao}} | {{status}} |
| Tempo Ciclo | {{meta}} | {{atual}} | {{projecao}} | {{status}} |
| NPS | {{meta}} | {{atual}} | {{projecao}} | {{status}} |
| ROI | {{meta}} | {{atual}} | {{projecao}} | {{status}} |
```

#### 2. Identificação de Padrões (20 min)
```markdown
### 🔍 Padrões Identificados

#### Padrões Positivos
1. **{{padrao_1}}**
   - Observação: {{observacao}}
   - Impacto: {{impacto}}
   - Recomendação: {{recomendacao}}

2. **{{padrao_2}}**
   - Observação: {{observacao}}
   - Impacto: {{impacto}}
   - Recomendação: {{recomendacao}}

#### Padrões Problemáticos
1. **{{problema_1}}**
   - Observação: {{observacao}}
   - Causa raiz: {{causa}}
   - Plano de ação: {{plano}}

2. **{{problema_2}}**
   - Observação: {{observacao}}
   - Causa raiz: {{causa}}
   - Plano de ação: {{plano}}

### 📊 Análise por Categoria
- **Por Tipo de Tarefa**: {{analise}}
- **Por Equipe**: {{analise}}
- **Por Agente**: {{analise}}
- **Por Complexidade**: {{analise}}
```

#### 3. Planejamento de Melhorias (20 min)
```markdown
### 🚀 Iniciativas de Melhoria

#### Melhorias Aprovadas
1. **{{melhoria_1}}**
   - Objetivo: {{objetivo}}
   - Métrica de sucesso: {{metrica}}
   - Responsável: {{pessoa}}
   - Prazo: {{prazo}}
   - Recursos necessários: {{recursos}}

2. **{{melhoria_2}}**
   - Objetivo: {{objetivo}}
   - Métrica de sucesso: {{metrica}}
   - Responsável: {{pessoa}}
   - Prazo: {{prazo}}
   - Recursos necessários: {{recursos}}

#### Propostas em Avaliação
- [ ] {{proposta_1}} - Avaliação até: {{data}}
- [ ] {{proposta_2}} - Avaliação até: {{data}}

#### Experimentos para Próximo Mês
- [ ] {{experimento_1}} - Hipótese: {{hipotese}}
- [ ] {{experimento_2}} - Hipótese: {{hipotese}}
```

---

## 3. Template: Planejamento Estratégico Trimestral

### 📋 Informações da Reunião
- **Duração**: 120 minutos
- **Frequência**: Trimestral
- **Participantes**: Leadership + Equipe técnica + Stakeholders
- **Facilitador**: Product Owner + Tech Lead

### 📊 Agenda Padrão

#### 1. Review Completo de Performance (30 min)
```markdown
## Review Trimestral Q{{trimestre}}/{{ano}}

### 🎯 Atingimento de Metas

| Objetivo | Meta | Resultado | Atingimento | Análise |
|----------|------|-----------|-------------|---------|
| Throughput | {{meta}} | {{resultado}} | {{percent}}% | {{analise}} |
| Tempo de Ciclo | {{meta}} | {{resultado}} | {{percent}}% | {{analise}} |
| Qualidade | {{meta}} | {{resultado}} | {{percent}}% | {{analise}} |
| Satisfação | {{meta}} | {{resultado}} | {{percent}}% | {{analise}} |
| ROI | {{meta}} | {{resultado}} | {{percent}}% | {{analise}} |

### 📈 Evolução Trimestral
- **Mês 1**: {{resumo_mes1}}
- **Mês 2**: {{resumo_mes2}}
- **Mês 3**: {{resumo_mes3}}

### 🏆 Principais Conquistas
1. {{conquista_1}}
2. {{conquista_2}}
3. {{conquista_3}}

### 📚 Principais Aprendizados
1. {{aprendizado_1}}
2. {{aprendizado_2}}
3. {{aprendizado_3}}
```

#### 2. Análise de ROI e Valor (30 min)
```markdown
### 💰 Análise de ROI

#### Investimento Total
- Desenvolvimento: R$ {{valor}}
- Operação: R$ {{valor}}
- Manutenção: R$ {{valor}}
- **Total**: R$ {{total}}

#### Retorno Gerado
- Tempo economizado: {{horas}} horas
- Valor por hora: R$ {{valor}}
- **Economia total**: R$ {{economia}}

#### ROI Calculado
- **ROI**: {{roi}}%
- **Payback**: {{payback}} meses
- **Valor presente líquido**: R$ {{vpl}}

### 📊 Valor Qualitativo
- Melhoria na qualidade: {{analise}}
- Satisfação da equipe: {{analise}}
- Redução de stress: {{analise}}
- Capacitação técnica: {{analise}}
```

#### 3. Definição de Metas Próximo Trimestre (30 min)
```markdown
### 🎯 Metas Q{{proximo_trimestre}}/{{ano}}

#### Metas Quantitativas
| KPI | Meta Atual | Nova Meta | Justificativa |
|-----|------------|-----------|---------------|
| Throughput | {{atual}} | {{nova}} | {{justificativa}} |
| Tempo de Ciclo | {{atual}} | {{nova}} | {{justificativa}} |
| Taxa de Retrabalho | {{atual}} | {{nova}} | {{justificativa}} |
| NPS | {{atual}} | {{nova}} | {{justificativa}} |
| ROI | {{atual}} | {{nova}} | {{justificativa}} |

#### Metas Qualitativas
1. **{{meta_qual_1}}**
   - Descrição: {{descricao}}
   - Métrica de acompanhamento: {{metrica}}

2. **{{meta_qual_2}}**
   - Descrição: {{descricao}}
   - Métrica de acompanhamento: {{metrica}}

### 🚀 Iniciativas Estratégicas
1. **{{iniciativa_1}}**
   - Objetivo: {{objetivo}}
   - Impacto esperado: {{impacto}}
   - Recursos: {{recursos}}
   - Timeline: {{timeline}}

2. **{{iniciativa_2}}**
   - Objetivo: {{objetivo}}
   - Impacto esperado: {{impacto}}
   - Recursos: {{recursos}}
   - Timeline: {{timeline}}
```

#### 4. Roadmap de Melhorias (30 min)
```markdown
### 🗺️ Roadmap Próximo Trimestre

#### Mês 1 - {{mes}}
- [ ] {{item_1}} - Owner: {{pessoa}}
- [ ] {{item_2}} - Owner: {{pessoa}}
- [ ] {{item_3}} - Owner: {{pessoa}}

#### Mês 2 - {{mes}}
- [ ] {{item_1}} - Owner: {{pessoa}}
- [ ] {{item_2}} - Owner: {{pessoa}}
- [ ] {{item_3}} - Owner: {{pessoa}}

#### Mês 3 - {{mes}}
- [ ] {{item_1}} - Owner: {{pessoa}}
- [ ] {{item_2}} - Owner: {{pessoa}}
- [ ] {{item_3}} - Owner: {{pessoa}}

### 🔬 Experimentos Planejados
1. **{{experimento_1}}**
   - Hipótese: {{hipotese}}
   - Métrica: {{metrica}}
   - Duração: {{duracao}}

2. **{{experimento_2}}**
   - Hipótese: {{hipotese}}
   - Métrica: {{metrica}}
   - Duração: {{duracao}}

### 📋 Próximos Passos
- [ ] {{passo_1}} - Responsável: {{pessoa}} - Prazo: {{data}}
- [ ] {{passo_2}} - Responsável: {{pessoa}} - Prazo: {{data}}
- [ ] {{passo_3}} - Responsável: {{pessoa}} - Prazo: {{data}}
```

---

## 4. Template: Investigação de Anomalias

### 📋 Quando Usar
- Alertas críticos disparados
- Métricas fora do padrão esperado
- Feedback negativo recorrente
- Falhas sistêmicas

### 🔍 Template de Investigação

```markdown
## Investigação de Anomalia - {{data}}

### 🚨 Descrição do Problema
- **Métrica afetada**: {{metrica}}
- **Valor observado**: {{valor}}
- **Valor esperado**: {{esperado}}
- **Desvio**: {{desvio}}%
- **Período**: {{periodo}}

### 📊 Dados Coletados
- **Tarefas afetadas**: {{numero}}
- **Agentes envolvidos**: {{agentes}}
- **Tipos de erro**: {{tipos}}
- **Padrão temporal**: {{padrao}}

### 🔍 Análise Root Cause

#### Hipóteses Investigadas
1. **{{hipotese_1}}**
   - Evidências: {{evidencias}}
   - Conclusão: {{conclusao}}

2. **{{hipotese_2}}**
   - Evidências: {{evidencias}}
   - Conclusão: {{conclusao}}

#### Causa Raiz Identificada
- **Causa**: {{causa}}
- **Categoria**: [Processo/Tecnologia/Pessoas/Ambiente]
- **Impacto**: {{impacto}}

### 🛠️ Plano de Correção

#### Ações Imediatas (0-24h)
- [ ] {{acao_1}} - Responsável: {{pessoa}}
- [ ] {{acao_2}} - Responsável: {{pessoa}}

#### Ações de Médio Prazo (1-7 dias)
- [ ] {{acao_1}} - Responsável: {{pessoa}}
- [ ] {{acao_2}} - Responsável: {{pessoa}}

#### Ações Preventivas (1-4 semanas)
- [ ] {{acao_1}} - Responsável: {{pessoa}}
- [ ] {{acao_2}} - Responsável: {{pessoa}}

### 📈 Monitoramento
- **Métricas a acompanhar**: {{metricas}}
- **Frequência**: {{frequencia}}
- **Responsável**: {{pessoa}}
- **Critério de sucesso**: {{criterio}}

### 📚 Lições Aprendidas
1. {{licao_1}}
2. {{licao_2}}
3. {{licao_3}}

### 🔄 Follow-up
- **Próxima revisão**: {{data}}
- **Responsável**: {{pessoa}}
- **Critérios para encerramento**: {{criterios}}
```

---

## 5. Checklist de Preparação para Reuniões

### ✅ Retrospectiva Semanal
**24h antes:**
- [ ] Extrair dados do ClickUp
- [ ] Calcular KPIs da semana
- [ ] Identificar anomalias
- [ ] Preparar gráficos/dashboards
- [ ] Revisar ações da semana anterior

**No dia:**
- [ ] Confirmar presença dos participantes
- [ ] Testar equipamentos (se remoto)
- [ ] Ter dados atualizados
- [ ] Preparar template preenchido

### ✅ Análise Mensal
**1 semana antes:**
- [ ] Agendar reunião com todos stakeholders
- [ ] Solicitar inputs das equipes
- [ ] Preparar análise de tendências
- [ ] Revisar metas trimestrais

**3 dias antes:**
- [ ] Finalizar apresentação
- [ ] Enviar pré-leitura para participantes
- [ ] Confirmar agenda e objetivos

### ✅ Planejamento Trimestral
**2 semanas antes:**
- [ ] Agendar com leadership
- [ ] Preparar análise completa de ROI
- [ ] Coletar feedback de todas equipes
- [ ] Revisar roadmap organizacional

**1 semana antes:**
- [ ] Enviar material preparatório
- [ ] Confirmar participação
- [ ] Preparar propostas de metas
- [ ] Alinhar expectativas

---

*Estes templates devem ser adaptados conforme a evolução do sistema e necessidades específicas de cada equipe.*

