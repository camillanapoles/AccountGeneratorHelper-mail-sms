# MCP Tools Specification for ClickUp Connector

## Overview
This document defines the complete set of MCP tools for the ClickUp connector, following MCP protocol conventions and providing standardized interfaces for multi-agent orchestrator interaction.

## Tool Naming Convention
All tools follow the pattern: `clickup_{action}_{resource}` or `clickup_{action}` for general operations.

## Core MCP Tools

### Authentication Tools

#### clickup_authenticate
**Description**: Authenticate with ClickUp API using personal token
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "token": {
      "type": "string",
      "description": "ClickUp personal API token (pk_*)"
    },
    "workspace_filter": {
      "type": "array",
      "items": {"type": "string"},
      "description": "Optional workspace ID filter"
    }
  },
  "required": ["token"]
}
```

### Discovery Tools

#### clickup_get_workspaces
**Description**: List all accessible workspaces (teams)
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "archived": {
      "type": "boolean",
      "default": false,
      "description": "Include archived workspaces"
    }
  }
}
```

#### clickup_get_spaces
**Description**: List spaces in a workspace
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "workspace_id": {
      "type": "string",
      "description": "Workspace (team) ID"
    },
    "archived": {
      "type": "boolean",
      "default": false,
      "description": "Include archived spaces"
    }
  },
  "required": ["workspace_id"]
}
```

#### clickup_get_lists
**Description**: Get lists from a folder or space
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "folder_id": {
      "type": "string",
      "description": "Folder ID (for foldered lists)"
    },
    "space_id": {
      "type": "string", 
      "description": "Space ID (for folderless lists)"
    },
    "archived": {
      "type": "boolean",
      "default": false,
      "description": "Include archived lists"
    }
  }
}
```

### Task Management Tools (Core)

#### clickup_get_tasks
**Description**: Get tasks from a list with filtering and pagination
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "list_id": {
      "type": "string",
      "description": "List ID to get tasks from"
    },
    "archived": {
      "type": "boolean",
      "default": false,
      "description": "Include archived tasks"
    },
    "page": {
      "type": "integer",
      "default": 0,
      "description": "Page number for pagination"
    },
    "statuses": {
      "type": "array",
      "items": {"type": "string"},
      "description": "Filter by status names"
    },
    "assignees": {
      "type": "array", 
      "items": {"type": "string"},
      "description": "Filter by assignee IDs"
    },
    "tags": {
      "type": "array",
      "items": {"type": "string"}, 
      "description": "Filter by tag names"
    }
  },
  "required": ["list_id"]
}
```

#### clickup_get_task
**Description**: Get detailed information about a specific task
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "task_id": {
      "type": "string",
      "description": "Task ID"
    },
    "custom_task_ids": {
      "type": "boolean",
      "default": false,
      "description": "Use custom task IDs"
    },
    "team_id": {
      "type": "string",
      "description": "Team ID for context (required if using custom task IDs)"
    }
  },
  "required": ["task_id"]
}
```

#### clickup_create_task
**Description**: Create a new task in a list
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "list_id": {
      "type": "string",
      "description": "List ID to create task in"
    },
    "name": {
      "type": "string",
      "description": "Task name"
    },
    "description": {
      "type": "string",
      "description": "Task description (markdown supported)"
    },
    "assignees": {
      "type": "array",
      "items": {"type": "integer"},
      "description": "Array of assignee user IDs"
    },
    "tags": {
      "type": "array",
      "items": {"type": "string"},
      "description": "Array of tag names"
    },
    "status": {
      "type": "string",
      "description": "Task status"
    },
    "priority": {
      "type": "integer",
      "enum": [1, 2, 3, 4],
      "description": "Priority level (1=urgent, 2=high, 3=normal, 4=low)"
    },
    "due_date": {
      "type": "integer",
      "description": "Due date timestamp (milliseconds)"
    },
    "parent": {
      "type": "string",
      "description": "Parent task ID (for subtasks)"
    }
  },
  "required": ["list_id", "name"]
}
```

#### clickup_update_task
**Description**: Update an existing task's properties
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "task_id": {
      "type": "string",
      "description": "Task ID to update"
    },
    "name": {
      "type": "string",
      "description": "New task name"
    },
    "description": {
      "type": "string",
      "description": "New task description"
    },
    "status": {
      "type": "string",
      "description": "New task status"
    },
    "priority": {
      "type": "integer",
      "enum": [1, 2, 3, 4],
      "description": "New priority level"
    },
    "assignees": {
      "type": "object",
      "properties": {
        "add": {
          "type": "array",
          "items": {"type": "integer"},
          "description": "User IDs to add as assignees"
        },
        "rem": {
          "type": "array", 
          "items": {"type": "integer"},
          "description": "User IDs to remove as assignees"
        }
      }
    }
  },
  "required": ["task_id"]
}
```

### Task Relationship Tools

#### clickup_add_dependency
**Description**: Add a dependency relationship between tasks
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "task_id": {
      "type": "string",
      "description": "Task ID that depends on another"
    },
    "depends_on": {
      "type": "string", 
      "description": "Task ID that this task depends on"
    }
  },
  "required": ["task_id", "depends_on"]
}
```

### Comment Management Tools

#### clickup_get_task_comments
**Description**: Get comments from a task
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "task_id": {
      "type": "string",
      "description": "Task ID to get comments from"
    },
    "start": {
      "type": "integer",
      "description": "Pagination start index"
    }
  },
  "required": ["task_id"]
}
```

#### clickup_create_task_comment
**Description**: Create a comment on a task
**Input Schema**:
```json
{
  "type": "object",
  "properties": {
    "task_id": {
      "type": "string",
      "description": "Task ID to comment on"
    },
    "comment_text": {
      "type": "string",
      "description": "Comment text content"
    },
    "assignee": {
      "type": "integer",
      "description": "User ID to assign comment to"
    },
    "notify_all": {
      "type": "boolean",
      "default": true,
      "description": "Notify all task members"
    }
  },
  "required": ["task_id", "comment_text"]
}
```

## Usage Examples

### Example 1: Create Task with Dependencies
```json
// 1. Create main task
{
  "tool": "clickup_create_task",
  "arguments": {
    "list_id": "123456789",
    "name": "Implement user authentication",
    "description": "Add OAuth2 authentication to the application",
    "assignees": [12345],
    "tags": ["backend", "security"],
    "priority": 2
  }
}

// 2. Add dependency
{
  "tool": "clickup_add_dependency",
  "arguments": {
    "task_id": "main_task_id",
    "depends_on": "prerequisite_task_id"
  }
}
```

### Example 2: Task Status Workflow
```json
// 1. Get tasks in "To Do" status
{
  "tool": "clickup_get_tasks",
  "arguments": {
    "list_id": "123456789",
    "statuses": ["to do"],
    "assignees": ["12345"]
  }
}

// 2. Update task status to "In Progress"
{
  "tool": "clickup_update_task",
  "arguments": {
    "task_id": "task_id",
    "status": "in progress"
  }
}

// 3. Add progress comment
{
  "tool": "clickup_create_task_comment",
  "arguments": {
    "task_id": "task_id",
    "comment_text": "Started working on this task. ETA: 2 days.",
    "notify_all": true
  }
}
```

## Error Handling

### Standard Error Response
```json
{
  "isError": true,
  "content": [{
    "type": "text",
    "text": "Error: {error_type} - {error_message}"
  }]
}
```

### Common Error Types
- `AUTHENTICATION_ERROR`: Invalid or expired token
- `PERMISSION_ERROR`: Insufficient permissions for operation
- `NOT_FOUND_ERROR`: Resource not found
- `RATE_LIMIT_ERROR`: API rate limit exceeded
- `VALIDATION_ERROR`: Invalid input parameters

## Implementation Notes

### Authentication Requirements
- Personal API tokens (pk_*) for MVP
- OAuth2 support for future enhancement
- Token validation on startup and runtime

### Rate Limiting
- ClickUp API: 100 requests per minute per token
- Connector: 90 requests per minute (safety buffer)
- Exponential backoff for rate limit errors

### Caching Strategy
- Workspace/space/list metadata: 10 minutes TTL
- Task data: 2 minutes TTL
- User permissions: 5 minutes TTL
