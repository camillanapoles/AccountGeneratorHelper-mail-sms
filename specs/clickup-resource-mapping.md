# ClickUp Resource Mapping for MCP Connector

## Overview
This document maps ClickUp API v2 resources and operations required by the multi-agent orchestrator, defining the minimum viable set of operations for the MCP connector.

## ClickUp Hierarchy Structure
```
Workspace (Team in v2)
├── Space
    ├── Folder (optional)
        ├── List
            ├── Task
                ├── Subtask (task with parent)
                ├── Comment
                ├── Attachment
                └── Custom Fields
```

## Core Resources and Operations

### 1. Workspaces/Teams
**Purpose**: Top-level organization container
**Required Operations**:
- `clickup_get_workspaces` - List accessible workspaces
- `clickup_get_workspace` - Get workspace details

**API Endpoints**:
- `GET /api/v2/team` - Get teams (workspaces)
- `GET /api/v2/team/{team_id}` - Get team details

### 2. Spaces
**Purpose**: Project-level organization within workspaces
**Required Operations**:
- `clickup_get_spaces` - List spaces in workspace
- `clickup_get_space` - Get space details

**API Endpoints**:
- `GET /api/v2/team/{team_id}/space` - Get spaces
- `GET /api/v2/space/{space_id}` - Get space details

### 3. Lists
**Purpose**: Container for tasks, equivalent to project boards
**Required Operations**:
- `clickup_get_lists` - List all lists (foldered and folderless)
- `clickup_get_list` - Get list details
- `clickup_get_list_members` - Get list access permissions

**API Endpoints**:
- `GET /api/v2/folder/{folder_id}/list` - Get foldered lists
- `GET /api/v2/space/{space_id}/list` - Get folderless lists
- `GET /api/v2/list/{list_id}` - Get list details

### 4. Tasks (Core Resource)
**Purpose**: Primary work items with rich metadata
**Required Operations**:
- `clickup_get_tasks` - List tasks with filtering
- `clickup_get_task` - Get task details
- `clickup_create_task` - Create new task
- `clickup_update_task` - Update task properties
- `clickup_delete_task` - Delete task
- `clickup_assign_task` - Manage task assignees
- `clickup_add_dependency` - Add task dependencies

**API Endpoints**:
- `GET /api/v2/list/{list_id}/task` - Get tasks in list
- `GET /api/v2/task/{task_id}` - Get task details
- `POST /api/v2/list/{list_id}/task` - Create task
- `PUT /api/v2/task/{task_id}` - Update task

### 5. Comments
**Purpose**: Communication and documentation on tasks/lists
**Required Operations**:
- `clickup_get_task_comments` - Get task comments
- `clickup_create_task_comment` - Add task comment
- `clickup_update_comment` - Update comment
- `clickup_delete_comment` - Delete comment

**API Endpoints**:
- `GET /api/v2/task/{task_id}/comment` - Get task comments
- `POST /api/v2/task/{task_id}/comment` - Create task comment

## Key Findings

### ClickUp Docs Limitation
**Critical Discovery**: ClickUp Docs/Pages are **NOT available** in the public API v2. This impacts the original specification scope and requires alternative approaches for documentation integration.

### LLM Projects Space Conventions
Based on task analysis, the LLM Projects space uses:
- **agentic** tag for multi-agent related tasks
- Task linking via mentions (task_mention format)
- Hierarchical project organization
- Status-based workflow management

## Implementation Priorities

### MVP Scope (Phase 1)
- Task CRUD operations
- Comment management
- List and workspace navigation
- Basic custom field support
- Task relationships and dependencies

### Future Scope (Phase 2)
- Advanced custom field types
- Time tracking integration
- Goals and milestones
- Advanced reporting
- Alternative documentation solutions
