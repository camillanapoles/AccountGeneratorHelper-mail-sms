# Especificação do Conector MCP – Chat/Comunicação Síncrona

## Objetivo

Definir o conector de comunicação síncrona entre o orquestrador e o canal de chat adotado (ex.: Slack, Teams ou chat cnmfs em ClickUp), para envio de resumos, pedidos de aprovação e alertas.

## 1. Seleção de Canais para MVP

### Canal Prioritário: ClickUp Chat
**Justificativa**: O time já utiliza ClickUp como ferramenta principal de gestão de trabalho, garantindo adoção imediata e integração natural com o fluxo existente.

**Configuração MVP**:
- **Canal Principal**: Chat do Space "LLM Projects" no ClickUp
- **Canais Secundários**: Chats específicos de tasks críticas
- **Usuários-alvo**: Context Architect, Orquestrador LLM Projects, Codegen

### Expansão Futura
- **Slack**: Para integração com equipes externas
- **Microsoft Teams**: Para comunicação corporativa formal
- **WhatsApp Business**: Para alertas críticos móveis

## 2. Operações MCP Definidas

### 2.1 Operações Básicas de Mensageria

```json
{
  "tools": [
    {
      "name": "send_message",
      "description": "Enviar mensagem em canal específico",
      "inputSchema": {
        "type": "object",
        "properties": {
          "channel_id": {"type": "string", "description": "ID do canal/chat"},
          "message": {"type": "string", "description": "Conteúdo da mensagem"},
          "message_type": {"type": "string", "enum": ["info", "alert", "approval_request", "summary"]},
          "priority": {"type": "string", "enum": ["low", "medium", "high", "critical"]},
          "metadata": {
            "type": "object",
            "properties": {
              "task_id": {"type": "string"},
              "project_id": {"type": "string"},
              "timestamp": {"type": "string"},
              "links": {"type": "array", "items": {"type": "string"}}
            }
          }
        },
        "required": ["channel_id", "message", "message_type"]
      }
    },
    {
      "name": "reply_in_thread",
      "description": "Responder em thread específica",
      "inputSchema": {
        "type": "object",
        "properties": {
          "thread_id": {"type": "string"},
          "message": {"type": "string"},
          "mention_users": {"type": "array", "items": {"type": "string"}}
        },
        "required": ["thread_id", "message"]
      }
    },
    {
      "name": "mention_users",
      "description": "Mencionar usuários específicos",
      "inputSchema": {
        "type": "object",
        "properties": {
          "user_ids": {"type": "array", "items": {"type": "string"}},
          "message": {"type": "string"},
          "urgency": {"type": "string", "enum": ["normal", "urgent"]}
        },
        "required": ["user_ids", "message"]
      }
    },
    {
      "name": "read_messages",
      "description": "Ler mensagens com filtros controlados",
      "inputSchema": {
        "type": "object",
        "properties": {
          "channel_id": {"type": "string"},
          "filters": {
            "type": "object",
            "properties": {
              "since": {"type": "string", "format": "date-time"},
              "message_type": {"type": "string"},
              "from_user": {"type": "string"},
              "contains_keywords": {"type": "array", "items": {"type": "string"}}
            }
          },
          "limit": {"type": "integer", "default": 50}
        },
        "required": ["channel_id"]
      }
    }
  ]
}
```

### 2.2 Operações Avançadas

```json
{
  "tools": [
    {
      "name": "create_approval_request",
      "description": "Criar pedido de aprovação interativo",
      "inputSchema": {
        "type": "object",
        "properties": {
          "title": {"type": "string"},
          "description": {"type": "string"},
          "options": {"type": "array", "items": {"type": "string"}},
          "approvers": {"type": "array", "items": {"type": "string"}},
          "deadline": {"type": "string", "format": "date-time"},
          "context_links": {"type": "array", "items": {"type": "string"}}
        },
        "required": ["title", "description", "approvers"]
      }
    },
    {
      "name": "send_structured_summary",
      "description": "Enviar resumo estruturado de sessão/projeto",
      "inputSchema": {
        "type": "object",
        "properties": {
          "summary_type": {"type": "string", "enum": ["session", "project", "incident", "kaizen"]},
          "title": {"type": "string"},
          "sections": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "section_title": {"type": "string"},
                "content": {"type": "string"},
                "type": {"type": "string", "enum": ["decision", "information", "action_item"]}
              }
            }
          },
          "attachments": {"type": "array", "items": {"type": "string"}}
        },
        "required": ["summary_type", "title", "sections"]
      }
    }
  ]
}
```

## 3. Tipos de Mensagem Padrão

### 3.1 Resumos de Sessão
```json
{
  "message_type": "summary",
  "template": {
    "title": "📋 Resumo da Sessão - {session_name}",
    "sections": [
      {
        "title": "🎯 Decisões Tomadas",
        "type": "decision",
        "format": "• {decision} - Responsável: @{user} - Prazo: {date}"
      },
      {
        "title": "📊 Informações Relevantes",
        "type": "information",
        "format": "• {info_item}"
      },
      {
        "title": "✅ Próximas Ações",
        "type": "action_item",
        "format": "• {action} - @{assignee} - {deadline}"
      }
    ],
    "footer": "🔗 Links: {task_links} | ⏰ Próxima revisão: {next_review}"
  }
}
```

### 3.2 Alertas de Incidente
```json
{
  "message_type": "alert",
  "priority": "critical",
  "template": {
    "title": "🚨 INCIDENTE DETECTADO",
    "format": "**Tipo**: {incident_type}\n**Severidade**: {severity}\n**Descrição**: {description}\n**Ações Automáticas**: {auto_actions}\n**Requer Intervenção**: {requires_human}\n\n🔗 [Ver Detalhes]({incident_link}) | 📋 [Task Relacionada]({task_link})"
  }
}
```

### 3.3 Mudanças de Estado de Projeto
```json
{
  "message_type": "info",
  "template": {
    "title": "🔄 Mudança de Estado - {project_name}",
    "format": "**Estado Anterior**: {old_state}\n**Novo Estado**: {new_state}\n**Motivo**: {reason}\n**Impacto**: {impact}\n\n📋 [Ver Projeto]({project_link})"
  }
}
```

### 3.4 Pedidos de Confirmação
```json
{
  "message_type": "approval_request",
  "template": {
    "title": "🤔 Aprovação Necessária - {request_title}",
    "format": "**Solicitação**: {request_description}\n**Contexto**: {context}\n**Opções**:\n{options_list}\n**Prazo**: {deadline}\n\n@{approvers} - Aguardando sua decisão\n\n🔗 [Contexto Completo]({context_link})"
  }
}
```

## 4. Formato Mínimo das Mensagens

### 4.1 Estrutura Base
```json
{
  "message_id": "uuid",
  "timestamp": "ISO-8601",
  "channel_id": "string",
  "message_type": "enum",
  "priority": "enum",
  "title": "string (max 100 chars)",
  "content": {
    "sections": [
      {
        "type": "decision|information|action_item",
        "title": "string",
        "content": "string"
      }
    ]
  },
  "metadata": {
    "task_links": ["url"],
    "doc_links": ["url"],
    "responsible_users": ["user_id"],
    "tags": ["string"]
  },
  "tracking": {
    "correlation_id": "string",
    "session_id": "string",
    "project_id": "string"
  }
}
```

### 4.2 Regras de Formatação

**Clareza e Rastreabilidade**:
- Título curto e descritivo (máx. 100 caracteres)
- Seções claramente separadas entre decisões e informações
- Links obrigatórios para tasks/docs relacionados
- Identificação clara do responsável por cada ação
- Timestamp e ID de correlação para rastreamento

**Consistência Visual**:
- Emojis padronizados por tipo de mensagem
- Formatação markdown consistente
- Estrutura de seções previsível
- Cores/prioridades visuais definidas

## 5. Casos de Uso Prioritários para o Piloto

### 5.1 Notificação de Falha de Pipeline
```yaml
Cenário: Pipeline de CI/CD falha
Trigger: Webhook do GitHub Actions
Ação: 
  - Enviar alerta crítico no chat
  - Mencionar responsável pelo commit
  - Criar thread para troubleshooting
  - Linkar logs e task relacionada
Formato: Alert template com prioridade crítica
```

### 5.2 Fechamento de Ciclo Kaizen
```yaml
Cenário: Conclusão de retrospectiva/melhoria
Trigger: Finalização de task tipo "Kaizen"
Ação:
  - Resumo estruturado das melhorias
  - Decisões de implementação
  - Próximos passos definidos
  - Agendamento de revisão
Formato: Summary template com seções de decisão
```

### 5.3 Anúncio de Conclusão de Etapa do Blueprint
```yaml
Cenário: Marco importante do projeto concluído
Trigger: Mudança de status de épico/milestone
Ação:
  - Comunicado de progresso
  - Impacto nas próximas etapas
  - Reconhecimento da equipe
  - Links para entregáveis
Formato: Info template com celebração
```

### 5.4 Pedido de Aprovação de Arquitetura
```yaml
Cenário: Decisão arquitetural requer consenso
Trigger: Task marcada como "Needs Approval"
Ação:
  - Apresentar opções estruturadas
  - Contexto e trade-offs
  - Prazo para decisão
  - Coleta de votos/feedback
Formato: Approval request template interativo
```

### 5.5 Alerta de Dependência Bloqueada
```yaml
Cenário: Task dependente está bloqueada
Trigger: Análise de dependências do orquestrador
Ação:
  - Identificar bloqueio
  - Sugerir alternativas
  - Escalar para responsáveis
  - Propor replanning
Formato: Alert template com ações sugeridas
```

## 6. Implementação Técnica

### 6.1 Arquitetura do Conector
```
Orquestrador → MCP Chat Server → ClickUp API → Chat Interface
                     ↓
              Message Queue (Redis)
                     ↓
              Webhook Handlers
```

### 6.2 Configuração de Segurança
- Autenticação via API tokens do ClickUp
- Rate limiting por canal/usuário
- Validação de permissões por workspace
- Logs de auditoria de todas as mensagens

### 6.3 Monitoramento
- Métricas de entrega de mensagens
- Tempo de resposta do conector
- Taxa de engajamento por tipo de mensagem
- Alertas de falha de conectividade

## 7. Critérios de Sucesso

### 7.1 Métricas Quantitativas
- **Disponibilidade**: >99% uptime do conector
- **Latência**: <2s para entrega de mensagens
- **Taxa de Entrega**: >95% de mensagens entregues
- **Engajamento**: >80% de mensagens lidas em 1h

### 7.2 Métricas Qualitativas
- Redução de reuniões desnecessárias
- Melhoria na transparência de decisões
- Aumento na velocidade de aprovações
- Satisfação da equipe com comunicação

## 8. Roadmap de Expansão

### Fase 2: Integração Multi-Canal
- Suporte simultâneo a Slack e Teams
- Roteamento inteligente por tipo de mensagem
- Sincronização cross-platform

### Fase 3: IA Conversacional
- Chatbot integrado para consultas
- Processamento de linguagem natural
- Respostas automáticas contextuais

### Fase 4: Analytics Avançado
- Dashboard de comunicação
- Análise de sentimento
- Otimização de padrões de mensagem

---

**Documento**: Especificação Conector MCP Chat v1.0  
**Data**: 2026-01-19  
**Responsável**: Codegen  
**Status**: Especificação Completa  
**Próximos Passos**: Implementação técnica do conector ClickUp

