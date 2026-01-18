# CodeReview-Pro


# CodeReviewer Pro

**AI-Powered Code Review & Quality Assistant** right inside VS Code  
Automate reviews, catch issues early, refactor smarter, and visualize code health — without leaving your editor.

[![Marketplace Version](https://vsmarketplacebadges.dev/version/pritam.codereviewer-pro.svg)](https://marketplace.visualstudio.com/items?itemName=pritam.codereviewer-pro)
[![Installs](https://vsmarketplacebadges.dev/installs/pritam.codereviewer-pro.svg)](https://marketplace.visualstudio.com/items?itemName=pritam.codereviewer-pro)
[![Rating](https://vsmarketplacebadges.dev/rating/pritam.codereviewer-pro.svg)](https://marketplace.visualstudio.com/items?itemName=pritam.codereviewer-pro)

## ✨ Features

### Core Review Engine
- **Inline analysis**: Real-time linter + AI suggestions (bugs, smells, performance, security)
- **GitHub / GitLab / Bitbucket PR review**: View diffs, add inline comments, sync threads — all from VS Code
- **Editor decorations**: Gutter icons, line highlights, CodeLens metrics, hover explanations

### AI Superpowers
- Contextual **refactoring agent** — select code → propose + apply changes
- **Predictive bug risk** — forecast issues from commit history & change impact
- **Auto-generated tests** + coverage overlays
- **Comment tone & rephrasing** — keep feedback constructive

### Visualizations
- **Interactive dashboard** (webview panel/sidebar): charts, dependency graphs (Mermaid), trends
- **Multi-repo impact maps** — see cross-project ripple effects

### Unique Differentiators
- **Gamified quality challenges** — badges, points, team leaderboards
- **Green code score** — energy-efficiency tips & optimizations
- **Community rules marketplace** — share & discover custom rules/prompts

## 📸 Screenshots

*(Add screenshots here later — e.g. inline decorations, dashboard, refactoring preview)*

![Inline suggestions and gutter icons](images/screenshot-inline.png)
![Review dashboard with charts](images/screenshot-dashboard.png)
![Refactoring agent preview](images/screenshot-refactor.png)

## 🚀 Getting Started

1. Install **CodeReviewer Pro** from the VS Code Marketplace
2. Open a project with Git repository
3. Run command: `CodeReviewer: Analyze Current File` (⇧⌘P → type "CodeReviewer")
4. Or open dashboard: `CodeReviewer: Show Dashboard`

First-time setup may prompt for:
- GitHub/GitLab token (for PR features)
- Optional OpenAI / local LLM API key (for advanced AI)

## ⚙️ Configuration

Open Settings (`⌘,`) → search for `codereviewer`

Key settings:
```json
"codereviewer.aiProvider": "openai" | "local" | "none",
"codereviewer.enableGamification": true,
"codereviewer.showGreenScore": true,
"codereviewer.inlineDecorations.enabled": true
```

## 🛠️ Commands

- `CodeReviewer: Analyze Current File`  
- `CodeReviewer: Analyze Changed Files`  
- `CodeReviewer: Show Dashboard`  
- `CodeReviewer: Refactor Selection`  
- `CodeReviewer: Generate Tests`  
- `CodeReviewer: View PR Comments`

## Contributing

We welcome contributions!

1. Fork & clone the repo
2. `npm install`
3. `npm run watch` or `F5` to debug
4. Submit PRs to `main`

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## 📄 License

MIT © 2026 Pritam

---

Made with ❤️ in Kallakurichi  
Star ⭐ the repo if this saves you review time!
```

### Folder Structure to Start With (Recommended for 2025/2026 VS Code extensions)

```
codereviewer-pro/
├── .vscode/                  # debug & recommended settings
│   ├── launch.json
│   └── settings.json
├── images/                   # screenshots for README + marketplace
├── src/
│   ├── extension.ts          # main entry point (activate/deactivate)
│   ├── commands/             # command handlers
│   ├── providers/            # CodeLensProvider, HoverProvider, etc.
│   ├── webviews/             # dashboard HTML/JS/CSS
│   ├── analyzers/            # linter wrappers, AI calls, metrics
│   └── utils/                # helpers (git, config, etc.)
├── media/                    # icons, bundled assets
│   └── icon.png
├── package.json
├── tsconfig.json
├── README.md                 # ← the one above
├── CHANGELOG.md
├── .gitignore
└── .vscodeignore
```

### Starting Code – Minimal Working `package.json` + `src/extension.ts`

**package.json**

```json
{
  "name": "codereviewer-pro",
  "displayName": "CodeReviewer Pro",
  "description": "AI-powered code review, refactoring, visualizations & quality dashboard in VS Code",
  "version": "0.1.0",
  "publisher": "pritam",
  "engines": {
    "vscode": "^1.85.0"
  },
  "categories": [
    "Other",
    "Programming Languages",
    "Linters",
    "SCM Providers"
  ],
  "keywords": [
    "code review",
    "AI review",
    "refactoring",
    "pull request",
    "code quality",
    "dashboard"
  ],
  "activationEvents": [
    "onStartupFinished",
    "workspaceContains:.git"
  ],
  "main": "./out/extension.js",
  "contributes": {
    "commands": [
      {
        "command": "codereviewer.analyzeFile",
        "title": "CodeReviewer: Analyze Current File"
      },
      {
        "command": "codereviewer.showDashboard",
        "title": "CodeReviewer: Show Dashboard"
      }
    ],
    "configuration": {
      "title": "CodeReviewer Pro",
      "properties": {
        "codereviewer.aiProvider": {
          "type": "string",
          "default": "none",
          "enum": ["none", "openai", "local"],
          "description": "AI provider for suggestions & refactoring"
        }
      }
    }
  },
  "scripts": {
    "vscode:prepublish": "npm run compile",
    "compile": "tsc -p ./",
    "watch": "tsc -watch -p ./",
    "pretest": "npm run compile && npm run lint",
    "lint": "eslint src --ext ts"
  },
  "devDependencies": {
    "@types/vscode": "^1.85.0",
    "@types/node": "20.x",
    "@typescript-eslint/eslint-plugin": "^7.0.0",
    "@typescript-eslint/parser": "^7.0.0",
    "eslint": "^8.56.0",
    "typescript": "^5.3.3"
  }
}
```

**src/extension.ts** (minimal bootstrap)

```ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  console.log('CodeReviewer Pro is now active!');

  // Register basic command
  let analyzeDisposable = vscode.commands.registerCommand('codereviewer.analyzeFile', () => {
    const editor = vscode.window.activeTextEditor;
    if (!editor) {
      vscode.window.showInformationMessage('No active editor!');
      return;
    }

    vscode.window.showInformationMessage(`Analyzing ${editor.document.fileName}... (placeholder)`);
    // → Later: run linters, AI, decorations here
  });

  let dashboardDisposable = vscode.commands.registerCommand('codereviewer.showDashboard', () => {
    const panel = vscode.window.createWebviewPanel(
      'codereviewerDashboard',
      'CodeReviewer Dashboard',
      vscode.ViewColumn.Beside,
      { enableScripts: true }
    );

    panel.webview.html = getDashboardWebviewContent();
  });

  context.subscriptions.push(analyzeDisposable, dashboardDisposable);
}

function getDashboardWebviewContent() {
  return `<!DOCTYPE html>
  <html lang="en">
  <head><meta charset="UTF-8"><title>Dashboard</title></head>
  <body>
    <h1>CodeReviewer Pro Dashboard</h1>
    <p>Coming soon: charts, metrics, graphs...</p>
  </body>
  </html>`;
}

export function deactivate() {}
```

Next steps you can take:
- `npm init -y` → overwrite with the package.json above
- `npm install --save-dev typescript @types/vscode eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin`
- Create `tsconfig.json` (standard VS Code one)
- Press F5 → debug the extension in a new VS Code window

Let me know which part to expand first:
- Add decorations & CodeLens
- Create real webview dashboard (with Chart.js or Mermaid)
- Integrate simple ESLint wrapper
- Add Git/PR commands
- AI prompt scaffolding (OpenAI / local)

Happy coding! 🚀
