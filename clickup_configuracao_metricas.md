# Guia de Configuração ClickUp - Sistema de Métricas

## 1. Configuração de Campos Customizados

### 1.1 Lista: "Orquestrador - Tarefas"

#### Campos de Processo
```json
{
  "tempo_inicio": {
    "type": "date_time",
    "required": true,
    "default": "task_created"
  },
  "tempo_conclusao": {
    "type": "date_time",
    "required": false,
    "auto_fill": "status_completed"
  },
  "tempo_ciclo_horas": {
    "type": "number",
    "required": false,
    "calculated": true,
    "formula": "(tempo_conclusao - tempo_inicio) / 3600000"
  },
  "agente_responsavel": {
    "type": "dropdown",
    "options": [
      "Gerenciador de Sessão",
      "Planejador de Tarefas", 
      "Despachante de Agentes",
      "Agente Especialista",
      "Sistema de Feedback"
    ],
    "required": true
  },
  "tipo_erro": {
    "type": "dropdown",
    "options": [
      "Timeout de Agente",
      "Falha de Validação",
      "Erro de Sintaxe",
      "Falha de Comunicação",
      "Recurso Indisponível",
      "Outro"
    ],
    "required": false
  },
  "retrabalho_necessario": {
    "type": "checkbox",
    "default": false
  },
  "handoff_duration_min": {
    "type": "number",
    "required": false
  }
}
```

#### Campos de Qualidade
```json
{
  "aprovado_primeira_tentativa": {
    "type": "checkbox",
    "default": false
  },
  "qualidade_tecnica": {
    "type": "rating",
    "scale": 5,
    "required": true
  },
  "feedback_detalhado": {
    "type": "long_text",
    "required": false
  },
  "criterios_qualidade": {
    "type": "checklist",
    "options": [
      "Código segue padrões",
      "Testes adequados",
      "Documentação completa",
      "Performance aceitável",
      "Segurança validada"
    ]
  }
}
```

#### Campos de Adoção
```json
{
  "usuario_solicitante": {
    "type": "people",
    "required": true
  },
  "equipe_solicitante": {
    "type": "dropdown",
    "options": [
      "Frontend",
      "Backend", 
      "DevOps",
      "QA",
      "Mobile",
      "Data Science"
    ],
    "required": true
  },
  "categoria_tarefa": {
    "type": "dropdown",
    "options": [
      "Bug Fix",
      "Nova Feature",
      "Refatoração",
      "Documentação",
      "Teste",
      "Deploy",
      "Análise"
    ],
    "required": true
  },
  "complexidade": {
    "type": "dropdown",
    "options": [
      "Simples (< 2h)",
      "Média (2-8h)",
      "Complexa (8-24h)",
      "Muito Complexa (> 24h)"
    ],
    "required": true
  },
  "nps_score": {
    "type": "rating",
    "scale": 10,
    "required": false
  },
  "data_primeiro_uso": {
    "type": "date",
    "required": false
  }
}
```

## 2. Configuração de Status

### 2.1 Workflow de Status
```yaml
Status_Flow:
  - name: "Solicitado"
    color: "#d3d3d3"
    type: "open"
  
  - name: "Em Análise"
    color: "#ffa500"
    type: "custom"
    
  - name: "Planejamento"
    color: "#87ceeb"
    type: "custom"
    
  - name: "Em Execução"
    color: "#1e90ff"
    type: "custom"
    
  - name: "Em Revisão"
    color: "#9370db"
    type: "custom"
    
  - name: "Aguardando Feedback"
    color: "#ff6347"
    type: "custom"
    
  - name: "Concluído"
    color: "#32cd32"
    type: "closed"
    
  - name: "Cancelado"
    color: "#ff0000"
    type: "closed"
```

## 3. Automações ClickUp

### 3.1 Automação: Cálculo Automático de Tempo de Ciclo
```yaml
Automation_1:
  name: "Calcular Tempo de Ciclo"
  trigger:
    type: "status_changed"
    condition: "status equals 'Concluído'"
  actions:
    - type: "update_custom_field"
      field: "tempo_conclusao"
      value: "{{current_datetime}}"
    - type: "calculate_field"
      field: "tempo_ciclo_horas"
      formula: "(tempo_conclusao - tempo_inicio) / 3600000"
```

### 3.2 Automação: Alerta de Retrabalho
```yaml
Automation_2:
  name: "Alerta Retrabalho"
  trigger:
    type: "custom_field_changed"
    field: "retrabalho_necessario"
    condition: "equals true"
  actions:
    - type: "add_tag"
      tag: "Retrabalho"
    - type: "notify_assignee"
      message: "⚠️ Tarefa marcada para retrabalho. Revisar processo."
    - type: "create_comment"
      comment: "Retrabalho identificado. Analisar causa raiz."
```

### 3.3 Automação: Coleta de Feedback
```yaml
Automation_3:
  name: "Solicitar Feedback"
  trigger:
    type: "status_changed"
    condition: "status equals 'Concluído'"
  actions:
    - type: "wait"
      duration: "1 hour"
    - type: "send_form"
      form_id: "feedback_form"
      recipient: "{{usuario_solicitante}}"
```

### 3.4 Automação: Escalação por Tempo
```yaml
Automation_4:
  name: "Escalação Tempo Limite"
  trigger:
    type: "time_in_status"
    status: "Em Execução"
    duration: "6 hours"
  actions:
    - type: "add_tag"
      tag: "Atenção-Tempo"
    - type: "notify_watchers"
      message: "🚨 Tarefa em execução há mais de 6 horas"
    - type: "change_priority"
      priority: "High"
```

### 3.5 Automação: Tracking de Handoff
```yaml
Automation_5:
  name: "Calcular Handoff"
  trigger:
    type: "assignee_changed"
  actions:
    - type: "create_time_entry"
      description: "Handoff para {{new_assignee}}"
    - type: "update_custom_field"
      field: "handoff_timestamp"
      value: "{{current_datetime}}"
```

## 4. Configuração de Dashboards

### 4.1 Dashboard Operacional

#### Widget 1: Tarefas por Status (Tempo Real)
```json
{
  "type": "pie_chart",
  "title": "Distribuição por Status",
  "data_source": "tasks",
  "group_by": "status",
  "filters": {
    "date_range": "last_24_hours"
  }
}
```

#### Widget 2: Tempo Médio de Ciclo
```json
{
  "type": "line_chart",
  "title": "Tempo Médio de Ciclo (Horas)",
  "data_source": "custom_field",
  "field": "tempo_ciclo_horas",
  "aggregation": "average",
  "time_period": "last_7_days",
  "goal_line": 4
}
```

#### Widget 3: Taxa de Falhas por Agente
```json
{
  "type": "bar_chart",
  "title": "Erros por Agente",
  "data_source": "tasks",
  "x_axis": "agente_responsavel",
  "y_axis": "count",
  "filters": {
    "tipo_erro": "not_empty",
    "date_range": "last_7_days"
  }
}
```

### 4.2 Dashboard Executivo

#### Widget 1: KPIs Principais
```json
{
  "type": "scorecard",
  "title": "KPIs Principais",
  "metrics": [
    {
      "name": "Throughput Semanal",
      "value": "{{completed_tasks_week}}",
      "target": 50,
      "format": "number"
    },
    {
      "name": "NPS Médio",
      "value": "{{avg_nps_score}}",
      "target": 7.0,
      "format": "decimal"
    },
    {
      "name": "Taxa Retrabalho",
      "value": "{{rework_percentage}}",
      "target": 15,
      "format": "percentage",
      "inverse": true
    }
  ]
}
```

#### Widget 2: Tendência Mensal
```json
{
  "type": "multi_line_chart",
  "title": "Tendências Mensais",
  "lines": [
    {
      "name": "Throughput",
      "data_source": "completed_tasks",
      "aggregation": "count_per_week"
    },
    {
      "name": "Qualidade Média",
      "data_source": "qualidade_tecnica",
      "aggregation": "average_per_week"
    }
  ],
  "time_range": "last_3_months"
}
```

### 4.3 Dashboard de Melhoria

#### Widget 1: Análise de Retrabalho
```json
{
  "type": "funnel_chart",
  "title": "Análise de Retrabalho por Categoria",
  "data_source": "tasks",
  "stages": [
    "categoria_tarefa",
    "retrabalho_necessario"
  ],
  "filters": {
    "retrabalho_necessario": true,
    "date_range": "last_month"
  }
}
```

## 5. Formulários de Feedback

### 5.1 Formulário de Satisfação
```yaml
Form_Feedback:
  name: "Avaliação de Satisfação - Orquestrador"
  fields:
    - name: "nps_score"
      type: "rating"
      scale: 10
      question: "Qual a probabilidade de recomendar o orquestrador?"
      required: true
      
    - name: "qualidade_tecnica"
      type: "rating"
      scale: 5
      question: "Como avalia a qualidade técnica da entrega?"
      required: true
      
    - name: "tempo_resposta"
      type: "rating"
      scale: 5
      question: "O tempo de resposta atendeu suas expectativas?"
      required: true
      
    - name: "facilidade_uso"
      type: "rating"
      scale: 5
      question: "Como avalia a facilidade de uso?"
      required: true
      
    - name: "sugestoes"
      type: "long_text"
      question: "Sugestões de melhoria:"
      required: false
      
    - name: "problemas_encontrados"
      type: "long_text"
      question: "Problemas ou dificuldades encontradas:"
      required: false
```

## 6. Configuração de Notificações

### 6.1 Alertas Críticos
```yaml
Critical_Alerts:
  - name: "Taxa de Falhas Alta"
    condition: "error_rate > 20%"
    recipients: ["tech_lead", "product_owner"]
    frequency: "immediate"
    
  - name: "Tempo de Ciclo Excessivo"
    condition: "avg_cycle_time > 8 hours"
    recipients: ["tech_lead"]
    frequency: "immediate"
    
  - name: "NPS Baixo"
    condition: "avg_nps < 5.0"
    recipients: ["product_owner", "scrum_master"]
    frequency: "daily"
```

### 6.2 Relatórios Automáticos
```yaml
Automated_Reports:
  - name: "Relatório Semanal Operacional"
    frequency: "weekly"
    day: "monday"
    time: "09:00"
    recipients: ["tech_team"]
    content: ["throughput", "cycle_time", "error_rate"]
    
  - name: "Relatório Mensal Executivo"
    frequency: "monthly"
    day: "first_monday"
    time: "10:00"
    recipients: ["leadership", "stakeholders"]
    content: ["kpis", "trends", "roi_analysis"]
```

## 7. Configuração de Integrações

### 7.1 Integração com Slack
```yaml
Slack_Integration:
  webhook_url: "{{slack_webhook}}"
  channels:
    - name: "#orquestrador-alerts"
      alerts: ["critical", "warnings"]
    - name: "#orquestrador-reports"
      reports: ["weekly", "monthly"]
  message_templates:
    critical: "🚨 ALERTA: {{alert_type}} - {{description}}"
    warning: "⚠️ ATENÇÃO: {{alert_type}} - {{description}}"
    report: "📊 {{report_type}}: {{summary}}"
```

### 7.2 Integração com Email
```yaml
Email_Integration:
  smtp_config:
    server: "{{smtp_server}}"
    port: 587
    security: "TLS"
  templates:
    - name: "weekly_report"
      subject: "Relatório Semanal - Orquestrador"
      template_file: "weekly_report.html"
    - name: "critical_alert"
      subject: "🚨 ALERTA CRÍTICO - Orquestrador"
      template_file: "critical_alert.html"
```

## 8. Backup e Manutenção

### 8.1 Backup de Dados
```yaml
Backup_Strategy:
  frequency: "daily"
  retention: "90_days"
  export_format: "json"
  fields_included: "all_custom_fields"
  storage_location: "{{backup_storage}}"
```

### 8.2 Manutenção Regular
```yaml
Maintenance_Tasks:
  - name: "Limpeza de dados antigos"
    frequency: "monthly"
    action: "archive_completed_tasks_older_than_6_months"
    
  - name: "Validação de automações"
    frequency: "weekly"
    action: "test_all_automations"
    
  - name: "Atualização de dashboards"
    frequency: "monthly"
    action: "refresh_dashboard_cache"
```

---

*Este guia deve ser seguido pela equipe técnica para implementar o sistema de métricas no ClickUp. Qualquer dúvida ou problema na implementação deve ser escalado para o Tech Lead.*

