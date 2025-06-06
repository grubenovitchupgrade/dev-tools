# Local LLM Git Tools

A collection of tools that use local LLM (via Ollama) to improve Git workflows without sending code to external services.

## Features

- Generate meaningful Git commit messages from diffs
- Create high-quality PR descriptions from commit history
- Works completely offline with local LLM models
- Multiple style options for commit messages
- Automatic staging of all changes (no need for git add)
- Interactive commit workflow with confirmation prompts
- One-step commit and push functionality
- Branch creation and switching capabilities
- End-to-end workflow from branch creation to PR submission
- Integration with GitHub CLI for PR creation

## Requirements

- Git
- Python 3.6+
- [Ollama](https://ollama.ai/) for local LLM inference
- GitHub CLI (`gh`) for PR creation
- Python requests library (`pip install requests`)

## Installation

1. Make sure you have Ollama installed and running:
   ```
   # Install Ollama (macOS/Linux)
   curl -fsSL https://ollama.ai/install.sh | sh
   
   # Start Ollama
   ollama serve
   ```

2. Run the setup script:
   ```
   ./llm_setup
   ```
   
3. The setup script will check dependencies and download the default model (phi3:mini).

## Usage

### All-in-One Git Workflow

The `llm_commit` tool now supports a complete git workflow from branch creation to PR submission:

```bash
# End-to-end workflow: create branch, commit, push, and create PR
llm_commit --branch feature/new-feature --push --pr

# Workflow with branch creation, custom commit style, and PR with editing
llm_commit --branch bugfix/issue-42 --style compact --pr --pr-edit

# Branch from current code, prefix ticket ID, auto-push, and create PR
llm_commit --branch feature/TICKET-123-awesome-feature --prefix --push --pr

# Shortcut commands:
llm_commit s                   # Short for --style compact
llm_commit pr                  # Short for --push --pr 
llm_commit branch new-feature  # Short for --branch new-feature --push --pr
```

### Generate Commit Messages

```bash
# Basic usage - generates a commit message and creates a commit
llm_commit

# Preview the generated message without committing
llm_commit --dry-run

# Edit the message before committing
llm_commit --edit

# Use a different Ollama model
llm_commit --model codellama:7b

# Choose a different commit message style
llm_commit --style detailed

# Generate ultra-compact commit messages (max 30 chars)
llm_commit --style compact
# Or use the shortcut
llm_commit s

# Add prefix with ticket ID from branch name
llm_commit --prefix

# Skip confirmation prompts and commit directly
llm_commit --yes

# Automatically push changes after commit
llm_commit --push

# Skip confirmations and auto-push (for quick CI fixes)
llm_commit --yes --push

# Create or switch to a branch before committing
llm_commit --branch feature/new-feature

# Create a branch, commit changes, and create a PR in one command
llm_commit --branch feature/new-feature --pr

# Create a branch, commit, and push with a specific base branch for PR
llm_commit --branch bugfix/issue-123 --pr --base develop

# Complete workflow: create branch, commit with compact message, push and create PR
llm_commit --branch feature/awesome --style compact --push --pr

# See all options
llm_commit --help
```

### Generate PR Descriptions

```bash
# Basic usage - generates a PR description and creates a PR
llm_pr

# Preview the generated description without creating a PR
llm_pr --dry-run

# Edit the description before creating a PR
llm_pr --edit

# Use a different Ollama model
llm_pr --model codellama:7b

# Compare against a different base branch
llm_pr --base develop

# Specify a custom PR title
llm_pr --title "My custom PR title"

# See all options
llm_pr --help
```

## Configuration

The tools use default settings that work well for most cases:

- Default model: `phi3:mini` (requires ~4GB RAM)
- Default commit style: `conventional`
- Default PR base branch: `main`

You can override these defaults with command-line arguments as shown in the usage examples.

### Customizing Prompts

You can customize the LLM prompts used to generate commit messages and PR descriptions by editing the template files:

- Commit message prompts: `cli/prompts/commit_prompts.py`
- PR description prompts: `cli/prompts/pr_prompts.py`

These files contain the prompt templates with placeholders for the git diff information. Modify these files to change the style, format, or instructions used to generate content.

For example, to customize the conventional commit style, edit the `CONVENTIONAL_PROMPT` variable in `commit_prompts.py`.

## Alternative Models

Depending on your hardware and requirements, you may want to use different models:

- **phi3:mini** - Default, balanced quality and performance (~4GB RAM)
- **llama3:8b** - Higher quality output, requires more resources (~8GB RAM)
- **codellama:7b** - Specialized for code, good performance (~8GB RAM)
- **mistral:7b** - Good balance of quality and speed (~8GB RAM)

To use a different model, use the `--model` flag or download models with:

```bash
ollama pull llama3:8b
ollama pull codellama:7b
ollama pull mistral:7b
```

## Troubleshooting

1. **Ollama not running**: Make sure Ollama is running with `ollama serve`
2. **Model not found**: Pull the model with `ollama pull <model-name>`
3. **Too slow**: Try a smaller model like `phi3:mini` or `tinyllama`
4. **PR creation fails**: Ensure you have GitHub CLI installed and authenticated

## License

MIT