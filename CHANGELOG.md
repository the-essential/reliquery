# Changelog

All notable changes to Reliquery will be documented in this file.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), versioned per [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-05-05

### Added
- **Chronicle** skill — intake pipeline for processing raw content (manuscripts, session logs, brain dumps) into structured vault relics with staging, review, backup, and commit workflows
- **Memorize** skill — memory pipeline for indexing committed relics into MemPalace as searchable drawers with section-level chunking, duplicate detection, and verification
- **Forget** skill — controlled deletion pipeline for removing stale or incorrect palace entries, with optional Erase mode for vault file cleanup
- **Cartograph** skill — relational mapping pipeline for translating vault relationships into temporal knowledge graph triples
- **Study** skill — master orchestrator that chains Chronicle → Memorize → Cartograph (with Forget as needed) in a single pipeline
- **Reliquery Help** skill — orientation guide covering system overview, MCP tool reference, and setup/troubleshooting
- Plugin packaging with `.mcp.json` for MemPalace server integration
- Example vault with sample relics demonstrating frontmatter schema and file structure
- Full documentation: README, contributing guide, and MIT license
