# Repository guidance for future sessions

This repository contains portable coding-agent skills, not an application to reorganize with FSD. Each skill lives at `skills/<name>/SKILL.md`; put focused, task-specific guidance in `skills/<name>/rules/` and link to it from the skill's rule index. Keep descriptions discoverable, advice actionable, and examples consistent with current framework conventions. Update the skills table in `README.md` when adding a skill.

The `feature-sliced-design` skill is intended first for **Next.js and TanStack Start application repositories**. Apply FSD to a target application's structure only when it fits the task and its existing conventions; do not impose a wholesale migration. Preserve framework-owned route files, server/client boundaries, and existing routing behavior. For detailed React, TanStack Router, Start, or Query implementation advice, use the corresponding skill rather than duplicating it here.

Run `pnpm format:check` after editing Markdown (and `pnpm format` if formatting needs correction). Review the diff and run any relevant checks before finishing.
