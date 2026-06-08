# AI Agent

Educational Python project for a small AI coding agent built with the Gemini API.
The agent accepts a prompt, can call a limited set of local tools, and currently
operates inside the `calculator/` example project.

> Warning: do not use this project with sensitive information. It can read,
> execute, and overwrite files inside its configured working directory.

## What it can do

The agent exposes these tool functions to Gemini:

- list files and directories
- read file contents, capped at `MAX_CHARS`
- run Python files with optional arguments
- write or overwrite files

The working directory is set in [config.py](config.py) and currently points to
`./calculator`. Each tool validates paths so requests outside that directory are
rejected.

## Requirements

- Python 3.14 or newer
- `uv`
- Gemini API key

Dependencies are declared in [pyproject.toml](pyproject.toml):

- `google-genai`
- `python-dotenv`

## Setup

Create a local `.env` file:

```sh
GEMINI_API_KEY=your_api_key_here
```

Install dependencies:

```sh
uv sync
```

## Usage

Run the agent with a prompt:

```sh
uv run python main.py "read the calculator README and summarize it"
```

Verbose mode prints token usage and tool calls:

```sh
uv run python main.py --verbose "run the calculator tests"
```

## Project Layout

```text
.
|-- main.py                  # CLI entry point and Gemini conversation loop
|-- call_function.py         # Tool declaration and dispatch
|-- config.py                # Tool limits and working-directory settings
|-- prompts.py               # System prompt
|-- functions/
|   |-- get_file_content.py
|   |-- get_files_info.py
|   |-- run_python_file.py
|   `-- write_file.py
`-- calculator/              # Example project the agent is allowed to modify
```

## Local Checks

The repository includes small script-style checks for each tool:

```sh
uv run python test_get_files_info.py
uv run python test_get_file_content.py
uv run python test_run_python_file.py
uv run python test_write_file.py
```

The calculator example also has its own test script:

```sh
uv run python calculator/tests.py
```

## Safety Notes

This is intended for educational use only. The safeguards are intentionally
simple and are not a security boundary for untrusted prompts, untrusted code, or
sensitive files. Keep the working directory narrow, review generated changes,
and avoid placing secrets in files the agent can access.
