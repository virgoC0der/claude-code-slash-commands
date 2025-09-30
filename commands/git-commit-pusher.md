
Review recent code changes, generate a meaningful commit message, and push to remote.

1. Review changes:
   - Run `git status` and `git diff` to see what changed
   - Analyze whether changes are new features, bug fixes, refactoring, docs, or config

2. Generate commit message following conventional commit format:
   - Use imperative mood ("Add", "Fix", "Update", "Remove")
   - Keep first line under 50 characters when possible
   - Use prefixes: feat:, fix:, docs:, refactor:, etc.
   - Include details in the body for complex changes

3. Show what will be committed:
   - Display the files to be committed
   - Present the proposed commit message
   - Ask for confirmation before proceeding

4. Execute git operations:
   - Stage appropriate files with `git add`
   - Create the commit
   - Push to remote branch with `git push`
   - Handle conflicts gracefully

5. Report final status of the operation.

IMPORTANT: Never force push without explicit user consent. If issues occur, explain clearly and suggest solutions.
