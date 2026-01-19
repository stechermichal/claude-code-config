Run fast TypeScript type checking on modified files only:

1. Get all modified files (staged + unstaged): `git diff HEAD --name-only`
2. Filter for TypeScript files (.ts, .tsx)
3. If no TypeScript files modified, report "No TypeScript files to check" and stop
4. Run: `tsc --noEmit [file1] [file2] [file3]` with only the modified TypeScript files
5. Parse the tsc output and group errors by file
6. Format output readably:
   - Show filename, line number, and error message for each error
   - For common errors (like type assignment issues), suggest quick fixes
   - Use clear visual formatting with line breaks between files
7. Provide summary: "Checked X files, found Y errors"
8. Add note: "This checks only modified files. Run `yarn lint:types` for full project-wide type checking."

This is a fast local check for quick feedback during development. It checks full TypeScript type safety (types, interfaces, assignments, signatures) but only on files you're actively working on.
