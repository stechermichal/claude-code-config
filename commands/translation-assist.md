Run `yarn extract-translations` to extract all translation keys from the codebase.

After extraction completes, scan the English translation files (`apps/app/public/locales/en/**/*.json`) to find keys that need English translations (keys with empty string values `""`, indicating they haven't been translated yet).

For each untranslated key found:
1. Analyze the Czech text to understand the context and meaning
2. Generate 3 different English translation suggestions that are:
   - Natural and idiomatic in English
   - Appropriate for an investment platform context
   - Professional and clear
3. Use the AskUserQuestion tool to present the 3 suggestions to the user, allowing them to:
   - Select one of the 3 suggestions (Option A, B, or C)
   - Choose "Other" to type their own custom translation
4. Update the corresponding English JSON file with the user's chosen translation

Process translations interactively, one key at a time, until all untranslated keys are handled.

Focus on `apps/app/public/locales/en/translation.json` first, then handle other translation files (faq.json, reports/*.json, etc.) if they have untranslated keys.

Important notes:
- Preserve any HTML tags, React components (like `<0>`, `<1>`), and interpolation variables (like `{{variable}}`) exactly as they appear in the Czech text
- Maintain the same JSON formatting and structure
- Run prettier on the updated files after making changes
