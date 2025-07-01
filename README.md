# AI Code Reviewer

This GitHub Action performs code reviews on pull requests using the OpenAI API. It analyzes the diff of the pull request and posts comments with suggestions for improvement.

## Features

-   **Automated Code Reviews**: Get instant feedback on your pull requests.
-   **Customizable Rules**: Define your own review rules in a separate file.
-   **Multi-language Support**: Configure the language of the review comments.
-   **File Exclusion**: Exclude specific files or directories from the review using glob patterns.

## Inputs

| Name                  | Description                                                                                         | Required | Default   |
| --------------------- | --------------------------------------------------------------------------------------------------- | -------- | --------- |
| `GITHUB_TOKEN`        | Your GitHub token to interact with the repository. Usually `${{ secrets.GITHUB_TOKEN }}`.            | `true`   |           |
| `OPENAI_API_KEY`      | Your OpenAI API key.                                                                                | `true`   |           |
| `OPENAI_API_MODEL`    | The OpenAI model to use for the review.                                                             | `false`  | `gpt-4`   |
| `language`            | The language for the AI's review comments. e.g., 'Traditional Chinese', 'English', 'Japanese'.        | `false`  | `English` |
| `custom_rules_path`   | Path to a `.md` file with custom review rules to override the default instructions.                   | `false`  | `''`      |
| `exclude`             | A comma-separated list of glob patterns to exclude files from the review. e.g., `dist,**/*.test.js`. | `false`  | `''`      |

## Usage

Create a workflow file (e.g., `.github/workflows/ai-review.yml`) in your repository with the following content:

```yaml
name: AI Code Review

on:
  pull_request:
    types: [opened, synchronize]

permissions:
  pull-requests: write
  contents: read

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@v3

      - name: AI Code Reviewer
        uses: oscar3x39/ai-codereviewer@main
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          OPENAI_API_MODEL: 'gpt-4'
          language: 'Traditional Chinese'
          exclude: "**/*.json, **/*.md"
          custom_rules_path: '.github/ai-review-rules.md'
```

## Custom Rules Example

You can provide a custom rules file to the AI to guide its review process. Here is an example of what you could put in your `.github/ai-review-rules.md` file:

```markdown
# AI Review Rules

You are a senior software engineer responsible for maintaining high code quality.

## Core Directives
- **All review comments MUST be written in Traditional Chinese.**
- Your tone should be constructive, professional, and helpful.
- For every piece of feedback, briefly explain the "why" behind your suggestion.

## Technical Checks
- **Security**: Look for hardcoded API keys, secrets, or potential injection vulnerabilities.
- **Readability**: Identify overly complex logic that could be simplified.
- **Best Practices**: Ensure the code follows common design patterns and best practices for the language.
- **No Magic Numbers**: All constant values should be defined as named constants.
```

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.
