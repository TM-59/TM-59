# Skill collection specification

This repository holds Agent Skills for the languages, technologies and certifications used across this account.
A skill is a folder with one `SKILL.md`: YAML front matter for discovery, and a Markdown body that an agent (or a person) reads when the task matches.

<sub>このリポジトリの skill 集の仕様。各言語・技術・資格を「使いこなす」ための手順・判断基準・落とし穴を、エージェントにも人にも読める形で置く。</sub>

## 1. Scope

| Category | Folder | What a skill covers |
|---|---|---|
| Languages | `skills/languages/<slug>/` | Toolchain, project layout, idioms that matter in practice, testing, pitfalls |
| Technologies | `skills/tech/<slug>/` | Frameworks, platforms and tools: when to choose them, setup, operation, verification, pitfalls |
| Certifications | `skills/certs/<slug>/` | Exam structure, study method, how to practise, common traps, where to confirm current rules |

The inventory is taken from real usage, not from a wish list: a language is listed only when repositories contain code in it, a technology only when it is used in a build, deployment or operation, and a certification only when study material for it has been built and checked.

## 2. File format

Each `SKILL.md` follows the open Agent Skills format (agentskills.io).

```yaml
---
name: <slug>            # lowercase letters, digits and hyphens; equals the folder name; 64 chars max
description: <text>     # what it does AND when to use it; 1,024 chars max; never empty
---
```

Body rules:

1. **Write what a capable model or engineer would get wrong without it.** Skip textbook material that any reference already explains.
2. **Lead with a default.** Give one recommended way, then the escape hatch, instead of a menu of options.
3. **Include a verification loop.** Every skill says how to check that the work is right (a command, a test, a measurable threshold).
4. **Keep a "Pitfalls" section.** It is usually the most valuable part.
5. **Explain reasons instead of shouting.** Avoid all-caps MUST/NEVER; state why.
6. **Keep time-sensitive facts out of the main path.** Versions, fees, dates and pass rates change; point to the official page to confirm them.
7. **Stay under 500 lines.** Split long reference material into a separate file only when it is actually needed.

## 3. Description style

The description is the only thing an agent sees before deciding to load a skill, so it states both the task and the trigger words, e.g. "Use when writing, testing or packaging Rust command-line tools with cargo and clap".
Descriptions are written in English. Japanese exam names are added in parentheses where a Japanese speaker would search for them.

## 4. Language of the body

- Languages, technologies and international certifications: English.
- Japanese national and domestic certifications: Japanese, because the official material, the exam and the candidates are Japanese.

## 5. Content policy

- Experience is described in general terms. Public repositories may be named; private projects are not named or described.
- No credentials, host names, account identifiers, personal data, internal paths or cost figures.
- Certification skills summarise structure and study approach in original wording. Exam questions and official texts are not reproduced.
- Facts that can change are linked to their official source rather than copied.

## 6. Change process

- One Markdown file per commit and per pull request, so each change can be reviewed on its own.
- The README link section at the bottom of the profile is updated in separate README-only pull requests.
- Before each commit and again before each merge, the text, the commit message and the pull request description are checked for private information with a local scanner and by reading the diff.

## 7. Using these skills

- **Claude Code:** copy a skill folder into `~/.claude/skills/` (personal) or `.claude/skills/` (project).
- **Other agents** that read the Agent Skills format (for example `.agents/skills/`): copy the folder unchanged.
- **People:** read the `SKILL.md` directly; each one is self-contained.
