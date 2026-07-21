# Codex Ask Mode

This build modifies the Codex CLI to operate as an **ask/chatbot mode only**. It is intended for users who want a simple question-answering assistant without code execution, file modifications, or tool use.

## What Changed

### Default Behavior
- The base instructions have been changed from coding agent instructions to a **helpful assistant persona**.
- The agent **cannot** create, modify, delete, or execute files, commands, or code.
- The agent **cannot** access the file system, shell, or any external tools.
- The agent **only** answers questions, explains concepts, and provides information.

### Files Modified
1. `models-manager/prompt.md` — Updated default base instructions to ask-mode-only persona.
2. `protocol/src/prompts/base_instructions/default.md` — Updated protocol-wide default instructions to match.

### How It Works
When you run `codex exec --ask`, the agent receives the new instructions and responds **only in plain text**. No tool calls are made. This is enforced by the `--ask` flag, which suppresses all tools from model requests.

## How to Use

### Build
```bash
cd codex-rs
cargo build --release -p codex-exec --target x86_64-unknown-linux-gnu
```

### Run
```bash
./target/x86_64-unknown-linux-gnu/release/codex-exec --ask "your question here"
```

### Examples
```bash
./target/x86_64-unknown-linux-gnu/release/codex-exec --ask "What is the difference between TCP and UDP?"

./target/x86_64-unknown-linux-gnu/release/codex-exec --ask "Explain how DNS works"

./target/x86_64-unknown-linux-gnu/release/codex-exec --ask "Can you help me brainstorm names for my project?"
```

## What You Can Ask
- Factual questions
- Explanations of concepts
- Code understanding and debugging help (read-only)
- Brainstorming and idea generation
- Learning and educational questions
- General knowledge questions

## What You Cannot Do
- Ask the agent to write, edit, or delete files
- Ask the agent to run shell commands
- Ask the agent to execute code
- Ask the agent to modify your system

## Distribution

### Binary Size
- Release binary: ~250 MB
- Compressed tar.gz: ~94 MB

### Send to Friends/Users
```bash
# Create compressed archive
tar -czf codex-exec-x86_64-linux.tar.gz -C codex-rs/target/x86_64-unknown-linux-gnu/release codex-exec

# Upload to file sharing service
curl --upload-file codex-exec-x86_64-linux.tar.gz https://transfer.sh/codex-exec.tar.gz
```

### How Users Run It
```bash
tar -xzf codex-exec-x86_64-linux.tar.gz
chmod +x codex-exec
./codex-exec --ask "your question"
```

## Important Notes

### Platform
This build is for **Linux x86_64** only. If you need other platforms, rebuild with the appropriate target triple:
- macOS Intel: `x86_64-apple-darwin`
- macOS Apple Silicon: `aarch64-apple-darwin`
- Windows: `x86_64-pc-windows-msvc`

### AGENTS.md Override
Users can still override instructions by placing custom text in:
- `$CODEX_HOME/AGENTS.md`
- `$CODEX_HOME/AGENTS.override.md`
- `<workspace>/AGENTS.md`

These files take precedence over the built-in instructions.

### Not for Coding Tasks
This build is specifically for **question-answering mode only**. If you want the full coding agent capabilities, use the original Codex CLI without `--ask`.

## Troubleshooting

### Binary won't run
- Ensure you're on Linux x86_64
- Make it executable: `chmod +x codex-exec`
- Check that you extracted it correctly from the tar.gz

### Agent still tries to use tools
- Ensure you're using the `--ask` flag
- Ensure you're running the rebuilt binary, not a system-installed version
- Check: `which codex-exec` to verify the path

### Build fails
- Ensure you have Rust installed: `rustc --version`
- Try: `cargo build --release -p codex-exec` without target specification first

## License
This project is based on OpenAI's Codex CLI. See the original repository for license details.
