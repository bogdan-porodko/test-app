# quiz-cli

An interactive command-line quiz game for learning JavaScript.

## Description

quiz-cli is a small, dependency-free CLI quiz application written in modern JavaScript (ES Modules). It presents categorized multiple-choice questions (JavaScript, Node.js, General), shows immediate feedback with explanations, tracks progress with a simple progress bar, and prints a review of incorrect answers at the end. It's ideal for quick practice and learning via terminal.

## Key Features

- Categorized multiple-choice quizzes (javascript, nodejs, general)
- Configurable number of questions per run
- Immediate feedback with colorized output and explanations
- Progress bar and final score summary
- Review of incorrect answers after the quiz
- No external dependencies — uses Node's built-in `readline` and `fs/promises`
- Easy to extend by editing `data/questions.json`

## Tech Stack

- JavaScript (ES Modules)
- Node.js (engines: >= 18.0.0)
- Built-in Node modules: `readline`, `fs/promises`
- ANSI escape codes for terminal colors

## Quick Start

### Prerequisites

- Node.js 18.0.0 or later
- A POSIX-like terminal (ANSI colors recommended). Works on macOS, Linux, and modern Windows terminals.

### Installation

Clone the repository and run:

```bash
git clone https://github.com/bogdan-porodko/test-app.git
cd test-app
# Run the app
node index.js
# or using the npm script
npm start
```

Note: There are no external npm dependencies, so no `npm install` is required. The project uses the `type: "module"` setting in `package.json`.

## Usage

Run the app:

```bash
node index.js
```

Or:

```bash
npm start
```

Example session flow (interactive):

1. Banner is shown.
2. Prompt to choose a category (e.g., javascript).
3. Prompt to choose the number of questions.
4. Questions are presented one by one with numbered choices.
5. After selecting an option, the CLI shows whether the answer was correct or not, displays an explanation, and updates the progress bar.
6. After the quiz completes, a final score and a review of incorrect answers are shown.
7. Optionally prompt to play again.

Example (ASCII/annotated):

```
===========================
   QUIZ-CLI — JavaScript
===========================
Choose a category:
  1) javascript
  2) nodejs
  3) general

How many questions? (1-5) 3

Q1/3 - What is a closure?
  1) A function bundled with its scope
  2) An I/O operation
  3) A syntax error
  4) A Node.js API
> 1

✅ Correct!
Explanation: A closure is a function that retains access to variables from its lexical scope.

Progress: [######----] 60%

... (more questions)

Final score: 2 / 3 (66%)
Review:
- Q2: ... (your answer) | Correct: ... | Explanation: ...
```

Colors are used to highlight success (green), errors (red), warnings (yellow), info (cyan), etc.

## Project Structure

- index.js — CLI entrypoint and main flow (reads questions, prompts user, starts Quiz)
- package.json — project metadata and scripts
- data/questions.json — question bank organized by category
- src/
  - colors.js — ANSI color helpers and convenience methods
  - input.js — readline helpers: prompt, select, confirm, pressEnter
  - quiz.js — Quiz class: shuffling, asking questions, scoring, progress bar, review
- .DS_Store — macOS metadata (can be ignored or removed)

## Configuration & Extensibility

### Adding/Updating Questions

Questions live in `data/questions.json`. Each category is an array of question objects. The expected shape:

```json
{
  "javascript": [
    {
      "question": "What is a closure?",
      "options": [
        "A function bundled with its scope",
        "An I/O operation",
        "A syntax error",
        "A Node.js API"
      ],
      "answer": 0,
      "explanation": "A closure is a function that retains access to variables from its lexical scope."
    }
    // ...
  ],
  "nodejs": [
    // ...
  ],
  "general": [
    // ...
  ]
}
```

- `question` (string): the question text.
- `options` (array of strings): numbered choices presented to the user.
- `answer` (integer): index of the correct option (0-based).
- `explanation` (string): short explanation shown after answering.

To add a new category, add a new key at the top level (e.g., `"web"`), and provide an array of questions following the same shape.

### Customization Ideas

- Load additional question files and merge them at runtime.
- Add difficulty levels or tagging (e.g., beginner/intermediate/advanced).
- Export results to a file or support a scoreboard.
- Internationalization (support multiple languages for prompts/questions).

## Development Notes

- Node version: The project requires Node >= 18.0.0 (see `package.json` -> `engines`).
- ES Modules: The repo uses `type: "module"` — use `import`/`export` syntax.
- No external dependencies: The project relies only on built-in Node APIs.
- Scripts:
  - `npm start` — runs `node index.js`
  - `npm test` — runs `node --test` (no tests included by default)

Shebang: `index.js` contains an executable shebang so it can be made executable and run directly on UNIX-like systems:

```bash
chmod +x index.js
./index.js
```

## Testing

There are no tests included in the repository. The `test` script is a placeholder that runs Node's built-in test runner:

```bash
# Run the (currently empty) test suite
npm test
# or directly
node --test
```

Suggestions for testing:

- Add unit tests for `src/quiz.js` (scoring, shuffling, progress bar rendering).
- Test interactive helpers in `src/input.js` using dependency injection or by abstracting the readline interface.
- Use Node's `assert` or a test framework (Node built-in test runner is fine for small projects).

## Roadmap & Ideas

- Persist high scores to a local JSON file
- Add timed quizzes and per-question time limits
- Add difficulty/power-ups and achievements
- Create a web UI or TUI (text-based UI with blessed / ink)
- Add CI workflow and automated tests
- Add a LICENSE file (MIT) at repo root if not present

## Contributing

Contributions are welcome! Suggested workflow:

1. Fork the repository
2. Create a branch: `git checkout -b feat/add-questions`
3. Make changes, add tests, and run locally
4. Commit and push: `git push origin feat/add-questions`
5. Open a pull request with a description of your changes

Please follow basic guidelines:
- Keep changes focused
- Update or add tests if applicable
- Document new features in this README

## Troubleshooting

- "Command not found" or permission errors when running `./index.js`:
  - Ensure the file is executable: `chmod +x index.js`
  - Or run via `node index.js`
- Colors not showing correctly:
  - Some terminals may not support ANSI colors. Try a different terminal or set `TERM` correctly.
- Node errors about modules or import:
  - Confirm Node >= 18: `node -v`
  - Project uses ESM (`type: "module"`), so CommonJS `require()` won't work in project files.
- Readline issues on Windows:
  - Use Windows Terminal, PowerShell, or enable VT100 support. Older cmd.exe may not handle some ANSI sequences well.
- Questions not found or JSON parse error:
  - Ensure `data/questions.json` is valid JSON and follows the expected structure.

## Example Banner & Progress Snippets

Banner (example):

```
===========================
   QUIZ-CLI — JavaScript
===========================
```

Progress bar example:

```
Progress: [##########] 100%
Progress: [#####-----] 50%
```

Colors are applied via `src/colors.js` to emphasize correct/incorrect output and important messages.

## License

This project is released under the MIT License (see `package.json`). You may add a `LICENSE` file with the following contents:

```
MIT License

Copyright (c) YEAR

Permission is hereby granted, free of charge, to any person obtaining a copy
...
```

(Insert the standard MIT license text and replace YEAR and copyright holder.)

---

Enjoy practicing JavaScript — happy quizzing!
