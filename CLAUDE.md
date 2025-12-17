# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

ClaudeKit-Skills is a collection of **Agent Skills** for Claude Code. Skills are folders containing instructions, scripts, and resources that Claude loads dynamically to perform specialized tasks. They follow a progressive disclosure architecture where Claude loads information in stages as needed.

## Skill Structure

Every skill requires a `SKILL.md` file with YAML frontmatter:

```
skill-name/
├── SKILL.md              # Required entry point with name/description frontmatter
├── scripts/              # Executable code (Python/JS preferred over bash)
├── references/           # Documentation loaded into context as needed
└── assets/               # Files used in output (templates, icons, fonts)
```

**Key constraints:**
- `SKILL.md` must be under 200 lines
- Reference/script files also under 200 lines each (split as needed)
- Skill name must match directory name (hyphen-case)
- Description must include concrete use cases for automatic activation

## Creating Skills

Use the `/skill:create [prompt]` command or invoke the `skill-creator` skill directly.

Initialization script: `.claude/skills/skill-creator/scripts/init_skill.py <skill-name> --path <output-directory>`

Packaging script: `.claude/skills/skill-creator/scripts/package_skill.py <path/to/skill-folder>`

## Directory Layout

```
.claude/
├── agents/           # Subagent definitions (e.g., mcp-manager)
├── commands/         # Slash commands (/git:cm, /git:pr, /skill:create, /use-mcp)
└── skills/           # All agent skills organized by domain
```

## Available Slash Commands

- `/skill:create [prompt]` - Create a new agent skill
- `/use-mcp [task]` - Execute MCP server tools via mcp-manager subagent
- `/git:cm` - Stage all files and create a commit
- `/git:pr [to-branch] [from-branch]` - Create a pull request
- `/git:cp` - Stage, commit and push all code

## Skill Categories

- **Authentication**: better-auth
- **AI/ML**: ai-multimodal, google-adk-python
- **Development**: backend-development, frontend-design, frontend-development
- **Web**: web-frameworks, ui-styling
- **Browser**: chrome-devtools
- **DevOps**: devops, databases
- **Tools**: claude-code, mcp-builder, mcp-management, repomix, media-processing
- **Docs**: docs-seeker
- **Quality**: code-review, debugging/*
- **Documents**: document-skills/* (docx, pdf, pptx, xlsx)
- **E-commerce**: shopify
- **Problem-solving**: problem-solving/*, sequential-thinking
- **Meta**: skill-creator, template-skill

## Writing Style for Skills

- Use **imperative/infinitive form** (verb-first instructions)
- Write in third person ("This skill should be used when..." not "Use this skill when...")
- Sacrifice grammar for concision in reference files
- Avoid duplication between SKILL.md and references

## Script Requirements

- Prefer Node.js or Python over bash (Windows compatibility)
- Python scripts need `requirements.txt`
- Scripts must respect `.env` hierarchy: `process.env` > `skill/.env` > `skills/.env` > `.claude/.env`
- Include `.env.example` for required variables
- Write tests for scripts
