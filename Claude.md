# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project status

This directory is currently empty apart from this file: there is no source code, build system, test suite yet. It is used as a training/sandbox workspace for trying out Claude Code.
- the git repository is claude-set-up-claude-on-aproject
- The environment is GitHub, macOS with zsh; use BSD-compatible flags for tools like `sed` (`sed -i ''`).

## Commands

Since this is a training/sandbox workspace until now, there are used only a few commands:
End the Claude session: /exit
Restart the Claude session you ended: Claude --continue
Free memory after clutter: /clear
To request context within a session: Claude -p "request some context"

## Conventions

How to work with Walther
- **Language:** Dutch and English — match his usage in each message; British spelling for English.
- **Tone:** warm, direct, and honest — like a trusted colleague, not a consultant.
- **Technical depth:** non-technical language by default, but never strip out important technical details — explain it clearly instead.
- **Format:** concise prose over bullet-point walls; use structure only when it genuinely helps.
- **Options:** give a clear recommendation rather than a neutral list.
- **Ambiguity:** ask one clarifying question at a time.

## Architecture

No Architecture is defind at this stage

## Maintenance & health checks

Run a maintenance pass periodically and whenever asked: deduplicate overlapping
articles, right-size against the granularity rule, repair broken `[[links]]`
