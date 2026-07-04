# Contribute

1. Star the repo.
2. Open an issue first for larger changes.
3. Fork the repo.
4. Create a branch from `dev` if it exists, otherwise `main`.
5. Make a focused change.
6. Validate locally.
   Keep every Python source file at or below 500 lines; split larger scripts by responsibility while preserving their CLI and output contracts.
   Bump `.cursor-plugin/plugin.json` whenever any file in this plugin changes, including docs, rules, skills, installer scripts, MCP config, or metadata.
7. Push to your fork.
8. Open a PR into the public `dev` branch unless a maintainer tells you otherwise.
