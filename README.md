# test-app (quiz-cli)

An interactive command-line quiz game for learning JavaScript, Node.js and general programming topics. Run in your terminal to practice questions, track your score, and review explanations for missed items.

## Key Features

- Multiple categories (javascript, nodejs, general)
- Numbered selection prompts and yes/no confirmations
- Shuffled questions and per-run progress bar
- Score tracking and end-of-quiz review of incorrect answers with explanations
- Small, dependency-free Node.js ES module codebase

## Tech Stack

- Node.js (ES Modules)
- Built-in `readline` (wrapped in `src/input.js`)
- ANSI terminal coloring helpers (`src/colors.js`)
- JSON-based question data (`data/questions.json`)

## Prerequisites

- Node.js v18.0.0 or newer (package.json `engines` requires >=18.0.0)
- Git (for cloning)

The project uses ES modules (`"type": "module"` in package.json), so ensure your Node version supports that (Node ≥ 14 with flags, recommended ≥ 18).

## Installation

Clone the repository and install (there are no external runtime dependencies, but installing dev tools or running `npm` scripts uses npm):

```bash
git clone https://github.com/bogdan-porodko/test-app.git
cd test-app
npm install
```

Note: `npm install` is a no-op for dependencies here (the project has none), but it ensures local npm metadata is initialized and lets you run npm scripts.

## Running the CLI

You can run the quiz using npm or directly with node:

```bash
# Using npm
npm start

# Or directly
node index.js
```

There is also a test script configured (no tests included by default):

```bash
npm test
```

## Usage Examples

- Start the app:
  - `npm start` or `node index.js`
- Select a category from the numbered list (use the number keys + Enter)
- Answer each multiple-choice question by typing the number of the option and pressing Enter
- Confirm prompts with `y` or `n` when requested
- Press Enter when prompted to continue between screens

Typical session flow:
1. Banner and welcome
2. Pick a category (javascript / nodejs / general)
3. Quiz starts — questions are shown one at a time
4. Progress bar updates as you answer
5. Final score and a review of incorrect answers with explanations

## How the Quiz Works

- Questions are loaded from `data/questions.json`.
- Each category contains an array of question objects with the following shape:
  - `question` (string)
  - `options` (array of strings)
  - `answer` (number) — index of the correct option (0-based)
  - `explanation` (string)
- The `Quiz` class (in `src/quiz.js`) handles:
  - Shuffling questions each run
  - Presenting questions and collecting answers
  - Tracking the number of correct answers
  - Rendering a simple progress bar during the run
  - Showing results and reviewing incorrect answers with explanations

Controls provided by `src/input.js`:
- `select` — choose from a numbered list
- `prompt` — free text input
- `confirm` — yes/no prompts
- `pressEnter` — proceed on Enter

Terminal text coloring and emphasis are provided by `src/colors.js`.

## Project Structure

Root layout:

.
- .DS_Store
- index.js
- package.json
- data/
  - questions.json
- src/
  - colors.js
  - input.js
  - quiz.js

You can visualize it like:

```
.
├─ index.js                 # ES module CLI entry point
├─ package.json             # npm metadata, scripts, engines
├─ data/
│  └─ questions.json        # categories and questions
└─ src/
   ├─ colors.js             # ANSI color helpers
   ├─ input.js              # readline wrapper (prompts, select, confirm)
   └─ quiz.js               # Quiz class, logic, progress, review
```

## Scripts (package.json)

- `start` — node index.js (run the CLI)
- `test` — `node --test` (placeholder / no tests included)

Example:

```json
"scripts": {
  "start": "node index.js",
  "test": "node --test"
}
```

## Configuration & Data

- Questions are stored in `data/questions.json`.
- To add or modify questions, follow the existing structure: group by category, each question object with `question`, `options`, `answer` (0-based), and `explanation`.

Example question entry:
```json
{
  "question": "What is the typeof null in JavaScript?",
  "options": ["object", "null", "undefined", "number"],
  "answer": 0,
  "explanation": "In JavaScript, typeof null returns 'object' (this is a historic bug)."
}
```

## Development Notes

- Node version: >= 18.0.0 is recommended (package.json `engines`).
- ES Modules: `package.json` contains `"type": "module"`. Files use `import`/`export`.
- No external dependencies are used — this is intentionally lightweight and portable.
- If you add dependencies, update `package.json` and run `npm install`.

## Troubleshooting

- "Cannot use import statement outside a module" — ensure `"type": "module"` is present in package.json and use Node ≥ 18.
- "Error: ENOENT: no such file or directory, open 'data/questions.json'" — run commands from the repository root or ensure the data file exists.
- Wrong Node version — check with `node -v`. If below required version, upgrade Node.js.
- Terminal color codes look raw — your terminal might not support ANSI colors; try a modern terminal emulator.

Recommended .gitignore additions (not present in repo):
```
node_modules/
.DS_Store
```

## Contributing

Contributions, issues and feature requests are welcome.

- Fork the repo
- Create a branch: `git checkout -b feat/my-feature`
- Make changes and tests
- Submit a pull request

Please add new questions by editing `data/questions.json` and keep each category balanced.

## License

This project is published with the MIT license (see `package.json` license field). Add a `LICENSE` file to the repository for full license text.

---

If you'd like, I can provide a sample `.gitignore`, add a CONTRIBUTING.md template, or generate additional categories/examples for `data/questions.json`.
