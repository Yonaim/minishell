# minishell

"yonazang & meyun's 금쪽이(Gold Baby Shell)"
![screenshot](/screenshot.png)

A minimal Unix shell implemented in C for the 42 Seoul curriculum.


## Project Overview

`minishell` is a lightweight Unix shell that interprets and executes user commands in a POSIX-like environment.
The project provides experience with parsing, process control, inter-process communication, and Unix system calls.

---

## Core Features

* **Lexical Analysis and Parsing**

  * Handles quoting (`'`, `"`), escaping (`\`), and environment variable expansion (`$VAR`, `$?`)
  * Supports command separation with semicolons (`;`) and pipelines (`|`)
* **Built-in Commands**

  * Implements standard shell built-ins:

    * `cd`, `echo`, `pwd`, `export`, `unset`, `env`, `exit`
  * Built-ins are executed in the main shell process where required
* **Process Management**

  * Forks and executes external programs located in the system `$PATH`
  * Handles proper parent/child process control for pipelines and redirections
* **Redirection and Pipelines**

  * Supports input/output redirection (`>`, `>>`, `<`)
  * Handles chained pipelines (e.g., `ls | grep foo | wc -l`)
* **Signal Handling**

  * Custom handling for `SIGINT` (`Ctrl+C`), `SIGQUIT` (`Ctrl+\`), and end-of-file (`Ctrl+D`)
* **Environment Variable Management**

  * Supports environment variable assignment, export, and expansion
  * Maintains a shell-local environment

---

## Getting Started

### Build

```sh
make
```

### Run

```sh
./minishell
```

You will see a prompt. Enter commands as you would in Bash.

### Example

```sh
echo Hello, World!
export NAME=Yona
echo $NAME
cat < infile | grep pattern > outfile
ls -l | wc -l
```

---

## Project Structure

```
.
├── src/        # Source files
├── include/    # Header files
├── Makefile
└── README.md
```

---

## Major Modules and Functions

* **main.c**:
  Shell entry point. Initializes the environment, signal handlers, and the main input loop.
* **parser/**:
  Implements lexical analysis and parsing.

  * `lexer.c`: Tokenizes the input line (recognizes commands, arguments, operators).
  * `parser.c`: Constructs command structures (AST or linked list) for execution.
* **executor/**:
  Handles command execution logic.

  * `execute.c`: Manages built-in command dispatch and process creation.
  * `pipeline.c`: Sets up and executes pipelines using `fork()` and `pipe()`.
  * `redirect.c`: Handles input/output redirection (`dup2`, file open/close).
* **env/**:
  Environment variable storage and manipulation.

  * `env.c`: Provides API for `getenv`, `setenv`, `unsetenv`, and environment expansion.
* **builtin/**:
  Built-in command implementations (e.g., `cd.c`, `echo.c`, etc.).
* **utils/**:
  Helper functions for string handling, memory management, and error reporting.

---

## Parsing Logic

Parsing is performed in multiple stages:

1. **Lexical Analysis (Tokenization)**

   * The input line is split into tokens: words, operators (`|`, `;`, `>`, `>>`, `<`), and special symbols.
   * Handles quotes (single/double), escaping, and splitting rules according to shell syntax.
   * Recognizes and expands environment variables during this stage.

2. **Syntax Parsing**

   * Tokens are organized into a command structure (e.g., linked list or tree).
   * Each node represents a command, its arguments, and its I/O redirections.
   * Pipelines and command sequences are constructed by linking nodes.

3. **Execution Preparation**

   * Built-ins are identified and routed to internal handlers.
   * External commands are resolved using `$PATH`.
   * Redirections and pipelines are prepared before process creation.

---

## Error Handling and Limitations

### Error Handling

* **Syntax Errors**

  * Detects and reports basic syntax errors, such as unmatched quotes or invalid redirection.
* **Execution Errors**

  * Reports errors for command not found, permission denied, failed redirection, etc.
* **Signal Handling**

  * Handles interrupts gracefully without terminating the shell (e.g., `Ctrl+C` cancels current input, not the shell).
* **Memory Management**

  * Attempts to free all dynamically allocated resources to prevent leaks.

### Limitations

* Does **not** support advanced Bash features such as:

  * Job control (background processes, `&`)
  * Command substitution (`` `cmd` ``, `$(cmd)`)
  * Shell scripting (no function or loop syntax)
  * Command history or line editing (no readline integration)
* Error messages may be minimal compared to a full shell.
* Does not implement all edge cases from the POSIX shell specification.

## References

- POSIX Shell & Utilities: Detailed ToC (IEEE)
	-> https://pubs.opengroup.org/onlinepubs/9699919799/utilities/contents.html
- Bash Reference Manual (GNU)
	-> https://www.gnu.org/software/bash/manual/bash.html
