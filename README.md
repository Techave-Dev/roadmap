# Techave Roadmap

> Your path to becoming a tech expert

A comprehensive learning roadmap platform built with [Docusaurus](https://docusaurus.io/) to guide developers through their tech journey.

## 🚀 Quick Start

### Prerequisites

- Node.js >= 20.0
- pnpm (recommended) or npm

### Installation

```bash
pnpm install
```

### Local Development

```bash
pnpm start
```

This command starts a local development server and opens up a browser window. Most changes are reflected live without having to restart the server.

### Build

```bash
pnpm build
```

This command generates static content into the `build` directory and can be served using any static hosting service.

### Serve Production Build Locally

```bash
pnpm serve
```

## 📚 Available Roadmaps

- **Backend Development** - Master server-side programming, databases, APIs, authentication, architecture patterns, and deployment strategies

## 🛠️ Adding New Roadmaps

1. Create a new folder in `roadmaps/` (e.g., `roadmaps/frontend/`)
2. Add `_category_.json` to configure the roadmap
3. Add markdown files for each topic
4. Docusaurus will automatically generate navigation

Example structure:
```
roadmaps/
└── your-roadmap/
    ├── _category_.json
    ├── fundamentals.md
    ├── advanced.md
    └── projects.md
```

## 📝 Scripts

- `pnpm start` - Start development server
- `pnpm build` - Build for production
- `pnpm serve` - Serve production build locally
- `pnpm clear` - Clear Docusaurus cache
- `pnpm typecheck` - Run TypeScript type checking

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Add new roadmaps
- Improve existing content
- Fix typos or errors
- Suggest new topics

## 📄 License

Copyright © 2026 Techave. Built with Docusaurus.

## 🔗 Links

- Website: [techave.dev](https://techave.dev)
- GitHub: [@techave-dev](https://github.com/techave-dev)
- Repository: [techave-dev/roadmap](https://github.com/techave-dev/roadmap)
