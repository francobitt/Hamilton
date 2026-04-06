# Hamilton

A Claude Code writing pipeline that uses an orchestrator + subagent architecture to process writing assignments in batch.

## How it works

The orchestrator (defined in `CLAUDE.md`) scans the `assignments/` directory and dispatches each valid assignment to a `writer` subagent. The subagent reads the prompt and context files, generates a written response, and saves it to `responses/`. Assignments are processed sequentially.

## Project structure

```
assignments/
  <assignment_name>/
    prompt.md          # Task instructions (tone, format, length, etc.)
    context/
      <file>           # Source material — any format, any number of files
responses/
  <assignment_name>/
    output.md          # Generated response
.claude/
  agents/
    writer.md          # Writer subagent definition
CLAUDE.md              # Orchestrator instructions
```

## Adding an assignment

1. Create a new subdirectory under `assignments/`:
   ```
   assignments/my_assignment/
   ```
2. Add a `prompt.md` describing the writing task.
3. Create a `context/` subfolder and drop in any source files (PDFs, text files, etc.).
4. Run the orchestrator — it will pick up the new folder automatically.

## Running the pipeline

Open Claude Code in this directory and run the orchestrator:

```
claude
```

Claude Code will read `CLAUDE.md` and begin processing all assignment folders. Each folder is dispatched to the `writer` subagent in order. Results appear in `responses/` under a matching subfolder name.

## Validation

The orchestrator skips any assignment folder that is missing `prompt.md` or an non-empty `context/` subdirectory and logs a warning:

```
SKIP: <folder> — missing context/ directory
```

## Dependencies

None. PDF reading is handled natively by the Claude Code `Read` tool.
