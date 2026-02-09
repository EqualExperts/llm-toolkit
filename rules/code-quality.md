# Code Quality Guidelines

## Verify Information
- Always verify information before presenting it. Do not make assumptions or speculate without clear evidence.
- changes file by file and give me a chance to spot mistakes.
- Never use apologies.
- Avoid giving feedback about understanding in comments or documentation.
- Don't suggest whitespace changes.
- Don't invent changes other than what's explicitly requested.
- Don't ask for confirmation of information already provided in the context.
- Don't remove unrelated code or functionalities. Pay attention to preserving existing structures.
- Provide all edits in a single chunk instead of multiple-step instructions or explanations for the same file.
- Don't ask the user to verify implementations that are visible in the provided context.
- Don't suggest updates or changes to files when there are no actual modifications needed.
- Always provide links to the real files, not x.md.
- Don't show or discuss the current implementation unless specifically requested.

## Errors
- Catch specific errors and deal with them is appropriate
- If additional context can be added at the point of the error then do so via explicit Exception or Error types
- Do not log and re-throw. If no action can be taken then ignore the error or enhance the error context
- Do not throw away/ignore errors
- Do not catch the base error/exception unless this is a catch all handler

## Logging
- only use structured logging, flat structure, key value pairs
- use consistent error names/keys
- use consistent format/unites for error values
- do not use templates or verbose story telling logs
- do not add logging that is not required
