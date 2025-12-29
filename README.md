# bash-notes

A concise, practical collection of Bash and shell-scripting notes, examples, and hands-on exercises. This repository documents core concepts, common patterns, and useful recipes for writing shell scripts and automating tasks on Unix-like systems.

Why this project exists

- Helps students and practitioners quickly find clear explanations and working examples for Bash topics.
- Collects short, focused guides and reference material you can read, adapt, and run.

Intended audience

- Beginners learning shell scripting and command-line tools.
- Developers and system administrators who want quick reference snippets and tips.

Badges

- License: MIT — [LICENSE](LICENSE)

Getting started

Prerequisites

- A Unix-like environment (Linux, macOS, WSL) with `bash` installed (Bash 4+ recommended).
- `git` to clone the repository.

Clone the repository

```bash
git clone <your-repo-url> bash-notes
cd bash-notes
```

Explore the notes

- The content is stored as Markdown files. Open them in your editor or view them directly in the terminal:

```bash
ls -R
# View a note in the terminal
bat BASH/Intro\ to\ Bash\ Shell\ Scripting/01-Intro\ to\ Bash\ Shell\ Scripting.md
```

Run example scripts

- Many notes include runnable scripts. Make a script executable and run it:

```bash
chmod +x examples/hello.sh
./examples/hello.sh
```

If a script requires a specific interpreter line, run it explicitly:

```bash
bash examples/hello.sh
```

Quick usage snippets

- Export an environment variable:

```bash
export MY_VAR="hello"
echo "$MY_VAR"
```

- Positional parameters in a script (save as `args.sh`):

```bash
#!/usr/bin/env bash
echo "Arg1: $1"
echo "All: $@"
```

Project structure

- `BASH/` — The main learning material and tutorials.
  - `Intro to Bash Shell Scripting/` — Fundamentals: shebang, comments, running scripts, simple examples.
  - `Variables/` — Detailed notes on variables, expansion, quoting, positional parameters, and environment vs shell variables.
- `LICENSE` — Project license.
- `README.md` — This file.

Examples of files

- [BASH/Intro to Bash Shell Scripting/01-Intro to Bash Shell Scripting.md](BASH/Intro%20to%20Bash%20Shell%20Scripting/01-Intro%20to%20Bash%20Shell%20Scripting.md)
- [BASH/Variables/01-Bash Variables.md](BASH/Variables/01-Bash%20Variables.md)

What problems this repository solves

- Provides a curated, searchable set of notes and examples for learning and referencing Bash scripting patterns.
- Reduces time spent searching for correct quoting, parameter handling, and script-running idioms.

Contributing

- Contributions are welcome. Preferred workflow:
  1.  Fork the repository.

2.  Create a descriptive branch: `git checkout -b fix/clear-variable-examples`.
3.  Make small, focused changes to Markdown files or add example scripts.
4.  Commit with clear messages and open a pull request.

Guidelines

- Keep notes concise and focused on a single concept per file where possible.
- Include runnable examples when practical and mark required shell versions or external commands.
- Preserve existing file naming conventions and directory layout.

Reporting issues

- Open an issue in the repository to report typos, broken examples, or propose new topics.

License

- This project is licensed under the MIT License. See the full text in [LICENSE](LICENSE).

Acknowledgements

- Personal learning notes inspired by community tutorials, man pages, and shell reference guides.

If you'd like, I can also draft a CONTRIBUTING.md with templates and a short checklist for reviewers.
