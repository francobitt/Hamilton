# Hamilton

A Claude Code writing pipeline that uses an orchestrator + subagent architecture to process writing assignments in batch.

## How it works

The orchestrator (defined in `CLAUDE.md`) scans the `assignments/` directory and dispatches each valid assignment to a `writer` subagent. The subagent reads the prompt, resolves context file paths from `context.md`, reads the referenced files from the central `/context` pool, generates a written response, and saves it to `responses/`. Assignments are processed sequentially.

## Project structure

```
context/
  <file>                 # Shared source material — any format, any number of files
assignments/
  <assignment_name>/
    prompt.md            # Task instructions (tone, format, length, etc.)
    context.md           # List of file paths into /context to use as source material
responses/
  <assignment_name>/
    output.md            # Generated response
.claude/
  agents/
    writer.md            # Writer subagent definition
CLAUDE.md                # Orchestrator instructions
```

## Adding an assignment

1. Add any source files to the central `/context` directory.
2. Create a new subdirectory under `assignments/`:
   ```
   assignments/my_assignment/
   ```
3. Add a `prompt.md` describing the writing task.
4. Add a `context.md` listing the paths to the relevant files from `/context`:
   ```markdown
   - /path/to/context/my_file.pdf
   ```
5. Run the orchestrator — it will pick up the new folder automatically.

## Running the pipeline

Open Claude Code in this directory and run the orchestrator:

```
claude
```

Claude Code will read `CLAUDE.md` and begin processing all assignment folders. Each folder is dispatched to the `writer` subagent in order. Results appear in `responses/` under a matching subfolder name.

## Validation

The orchestrator skips any assignment folder that is missing `prompt.md` or `context.md` and logs a warning:

```
SKIP: <folder> — missing context.md
```

## Dependencies

None. PDF reading is handled natively by the Claude Code `Read` tool.
