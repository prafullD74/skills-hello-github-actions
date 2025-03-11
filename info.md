What is GitHub Actions?: GitHub Actions is a flexible way to automate nearly every aspect of your team's software workflow. You can automate testing, continuously deploy, review code, manage issues and pull requests, and much more. The best part, these workflows are stored as code in your repository and easily shared and reused across teams.
The GitHub Actions feature page, see [GitHub Actions](https://github.com/features/actions).
The "GitHub Actions" user documentation, see [GitHub Actions](https://docs.github.com/actions).

What is a workflow?: A workflow is a configurable automated process that will run one or more jobs. Workflows are defined in special files in the .github/workflows directory and they execute based on your chosen event.
["Understanding GitHub Actions"](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
[pull_request event](https://docs.github.com/en/developers/webhooks-and-events/webhooks/webhook-events-and-payloads#pull_request)

What is a job?: A job is a set of steps in a workflow that execute on the same runner (a runner is a server that runs your workflows when triggered). Workflows have jobs, and jobs have steps. Steps are executed in order and are dependent on each other. You'll add steps to your workflow later in the course. To read more about jobs, see ["Jobs"](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#jobs).

["Understanding the workflow file"](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#understanding-the-workflow-file) article.
```yaml
name: Post welcome comment
on:
  pull_request:
    types: [opened]
permissions:
  pull-requests: write
jobs:
  build:
    name: Post welcome comment
    runs-on: ubuntu-latest
```

What are steps?: Actions steps run - in the order they are specified, from the top down - when a workflow job is processed. Each step must pass for the next step to run. Each step consists of either a shell script that's executed, or a reference to an action that's run. When we talk about an action (with a lowercase "a") in this context, we mean a reusable unit of code. You can find out about actions in ["Finding and customizing actions"](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions).

```yaml
name: Post welcome comment
on:
  pull_request:
    types: [opened]
permissions:
  pull-requests: write
jobs:
  build:
    name: Post welcome comment
    runs-on: ubuntu-latest
    steps:
      - run: gh pr comment $PR_URL --body "Welcome to the repository!"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          PR_URL: ${{ github.event.pull_request.html_url }}
```

Uses GitHub CLI (gh) to add a comment when a pull request is opened. To allow GitHub CLI to post a comment, we set the GITHUB_TOKEN environment variable to the value of the GITHUB_TOKEN secret, which is an installation access token, created when the workflow runs. For more information, see ["Automatic token authentication"](https://docs.github.com/en/actions/security-guides/automatic-token-authentication). We set the PR_URL environment variable to the URL of the newly created pull request, and we use this in the gh command.
