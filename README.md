# HR Assist using MCP

An AI-powered HR assistant built on the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/). It exposes HR management operations as MCP tools, allowing an AI assistant (like Claude) to manage employees, leaves, meetings, and IT tickets through natural language.

## Features

- **Employee Management**: add employees, search by name, view details, get manager/direct reports
- **Leave Management**: check leave balance, apply for leave, view leave history
- **Meeting Management**: schedule and cancel meetings
- **IT Ticket Management**: raise and track equipment/procurement requests
- **Email**: send real emails via Gmail SMTP (welcome emails, notifications)
- **Onboarding Prompt**: built-in prompt that chains all tools to onboard a new employee end-to-end

## Setup

### 1. Clone the repo

```bash
git clone <repo-url>
cd hr-assist-using-mcp
```

### 2. Install dependencies

Requires Python 3.13+. Uses [uv](https://github.com/astral-sh/uv) for package management.

```bash
uv sync
```

### 3. Configure environment variables

Copy `sample.env` to `.env` and fill in your Gmail credentials:

```bash
cp sample.env .env
```

```env
CB_EMAIL=your_gmail@gmail.com
CB_EMAIL_PWD=your_app_password
```

> Use a [Gmail App Password](https://support.google.com/accounts/answer/185833), not your regular Gmail password.

### 4. Connect to Claude Desktop

Add the following to your Claude Desktop MCP config (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "hr-assist": {
      "command": "uv",
      "args": ["run", "python", "server.py"],
      "cwd": "/path/to/hr-assist-using-mcp"
    }
  }
}
```

## Project Structure

```
hr-assist-using-mcp/
├── server.py          # MCP server — registers all tools and prompts
├── utils.py           # Seeds in-memory stores with dummy data
├── emails.py          # Gmail SMTP email sender
├── hrms/
│   ├── schemas.py         # Pydantic models
│   ├── employee_manager.py
│   ├── leave_manager.py
│   ├── meeting_manager.py
│   └── ticket_manager.py
├── sample.env         # Template for environment variables
└── pyproject.toml
```

## Available MCP Tools

| Tool | Description |
|------|-------------|
| `add_employee` | Add a new employee |
| `get_employee_details` | Look up employee by name |
| `get_employee_leave_balance` | Check remaining leave days |
| `apply_leave` | Apply for leave on given dates |
| `get_leave_history` | View past leave records |
| `schedule_meeting` | Schedule a meeting |
| `get_meetings` | List upcoming meetings |
| `cancel_meeting` | Cancel a scheduled meeting |
| `create_ticket` | Raise an IT/equipment ticket |
| `update_ticket_status` | Update ticket status |
| `list_tickets` | List tickets with optional filters |
| `send_email` | Send an email via Gmail |
