# Claude Code Slash Commands

A collection of custom slash commands for [Claude Code](https://claude.com/claude-code) to streamline development workflows and enhance productivity.

## 📋 Available Commands

### `/code-reviewer`
Expert code review with comprehensive analysis covering:
- Security vulnerabilities and attack vectors
- Performance bottlenecks and optimization opportunities
- Logic errors and edge cases
- Code maintainability and technical debt
- Best practices and conventions

### `/fix-jira`
Automated Jira ticket resolution workflow:
1. Fetches and analyzes Jira ticket details
2. Understands the problem and locates affected code
3. Generates technical solution design
4. Implements fixes using best practices

**Usage**: `/fix-jira TICKET-123`

### `/git-commit-pusher`
Git workflow automation specialist that:
- Reviews staged and unstaged changes
- Generates conventional commit messages
- Verifies changes before committing
- Handles push operations safely

### `/golang-backend-engineer`
Expert Go backend development assistance for:
- Google Cloud Spanner queries and transactions
- Pub/Sub message handling
- Redis caching strategies
- Elasticsearch operations
- Follows systematic analysis → planning → implementation workflow

### `/pr`
GitHub Pull Request creation with:
- Automatic PR template population
- Jira ticket integration (AFD-* pattern)
- Smart upstream branch targeting

**Usage**: `/pr [target-branch]` (defaults to `master`)

### `/repo-analyze`
AI-powered repository analysis using Gemini CLI + Claude collaboration:
- Architecture and design pattern analysis
- Code quality and maintainability assessment
- Security vulnerability scanning
- Performance bottleneck identification
- Automatic documentation generation

**Analysis types**: `architecture`, `functions`, `quality`, `security`, `performance`, `documentation`, `full`

**Usage**: `/repo-analyze [analysis-type] [repo-path] [output-path]`

### `/technical-solution-architect`
Transforms PRDs and requirements into comprehensive technical solutions:
- Requirement analysis and system design
- Architecture diagrams (Mermaid)
- Interface and API specifications
- Integration planning
- Implementation roadmap

## 🚀 Installation

1. Clone this repository:
```bash
git clone https://github.com/virgoC0der/claude-code-slash-commands.git
```

2. Copy the command files to your Claude Code commands directory:
```bash
cp commands/*.md ~/.claude/commands/
```

3. Restart Claude Code or reload commands.

## 📖 Usage

Simply type the slash command in Claude Code:

```
/code-reviewer
/fix-jira AFD-1234
/pr master
/repo-analyze full ./my-project
```

## 🛠️ Customization

Each command is defined in a Markdown file in the `commands/` directory. You can:
- Modify existing commands to fit your workflow
- Create new commands by adding `.md` files
- Adjust prompts and behavior to match your team's standards

## 📚 Requirements

Some commands have external dependencies:
- `/repo-analyze`: Requires [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- `/fix-jira`: Requires Atlassian MCP integration
- `/pr`: Requires GitHub CLI (`gh`)

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Submit bug reports or feature requests
- Create pull requests with improvements
- Share your custom commands

## 📄 License

MIT License - feel free to use and modify these commands for your needs.

## 🔗 Links

- [Claude Code Documentation](https://docs.claude.com/en/docs/claude-code)
- [Slash Commands Guide](https://docs.claude.com/en/docs/claude-code/slash-commands)