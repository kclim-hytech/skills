# Custom Skills Guide

This document summarizes how to structure and maintain custom skills in a repository-friendly format for team use. It is based on Anthropic's guidance on creating custom skills, rewritten as an internal reference.

## What a custom skill is

A custom skill is a folder that contains at minimum a `skill.md` file, with optional supporting files such as references, resources, or executable scripts. Skills can range from a short instruction file to a more advanced multi-file package.

Good custom skills usually:
- Solve one specific, repeatable task.
- Have clear instructions that Claude can follow.
- Include examples when helpful.
- Define when they should be used.
- Stay focused on one workflow instead of trying to do everything.

## Minimum structure

Each skill should live in its own directory.

```text
skills/
└── my-skill/
    └── skill.md
```

The `skill.md` file should begin with YAML frontmatter and must include:
- `name` — human-friendly skill name, up to 64 characters.
- `description` — short explanation of what the skill does and when to use it, up to 200 characters.

The description matters a lot because Claude uses it to decide when the skill should be invoked.

## Example skill.md

```md
---
name: Brand Guidelines
description: Apply Acme Corp brand guidelines to presentations and documents.
---

## Overview

Use this skill when creating materials that must follow Acme Corp brand standards.

## Brand Colors

- Primary: #FF6B35
- Secondary: #004E89
- Accent: #F7B801

## Typography

- Headers: Montserrat Bold
- Body: Open Sans Regular

## When to apply

- Presentations
- Client reports
- Marketing materials
```

## Adding extra files

If one `skill.md` becomes too large, add supporting files inside the same skill folder, such as `REFERENCE.md` for supplemental information. Referencing those files in `skill.md` helps Claude decide whether to access them when executing the skill.

Example:

```text
skills/
└── brand-guidelines/
    ├── skill.md
    ├── REFERENCE.md
    └── resources/
```

## Adding scripts

Advanced skills can include executable code files. Use scripts carefully:
- Do not hardcode API keys or passwords.
- Keep dependencies explicit.
- Review downloaded skills before enabling them.

## Packaging rules

When packaging a skill for upload, create a ZIP file that contains the skill folder as the root of the archive. Ensure the folder name matches the skill name.

Correct:

```text
my-skill.zip
└── my-skill/
    ├── skill.md
    └── resources/
```

Incorrect:

```text
my-skill.zip
└── skill.md
```

## Testing checklist

Before upload:
1. Review `skill.md` for clarity.
2. Verify the description matches the intended trigger conditions.
3. Confirm all referenced files exist in the expected paths.
4. Test with prompts that should cause the skill to be used.

After upload:
1. Enable the skill in Claude's Skills settings.
2. Try several prompts that should trigger it.
3. Check whether Claude is loading it as expected.
4. Refine the description if invocation is inconsistent.

## Best practices

- Keep each skill focused on one workflow.
- Start simple in Markdown before adding scripts.
- Use examples to show successful behavior.
- Test incrementally after each major change.
- Design skills so they can work alongside other skills when needed.

Anthropic also recommends following the open Agent Skills specification at `agentskills.io` for broader compatibility.

## Suggested repo layout

A practical structure for this repository:

```text
README.md
skills/
├── custom-skills-guide.md
├── brand-guidelines/
│   ├── skill.md
│   └── REFERENCE.md
├── report-writer/
│   └── skill.md
└── security-review/
    └── skill.md
```

## Security notes

When maintaining skills in a shared repository:
- Avoid storing secrets in files.
- Review scripts before execution.
- Use approved external integrations only.
- Treat third-party skills as untrusted until reviewed.
