# github actions

GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production.

## Components of github actions

You can configure a GitHub Actions workflow to be triggered when an event occurs in your repository, such as a pull request being opened or an issue being created. Your workflow contains one or more jobs which can run in sequential order or in parallel. Each job will run inside its own virtual machine runner, or inside a container, and has one or more steps that either run a script that you define or run an action, which is a reusable extension that can simplify your workflow.

## example

```yaml
name: GitHub Actions Demo
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v2
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```

## workflow file

name: learn-github-actions

  Optional - The name of the workflow as it will appear in the Actions tab of the GitHub repository.

on: [push]

  Specifies the trigger for this workflow. This example uses the push event, so a workflow run is triggered every time someone pushes a change to the repository or merges a pull request. This is triggered by a push to every branch; for examples of syntax that runs only on pushes to specific branches, paths, or tags, see "Workflow syntax for GitHub Actions."

jobs:

  Groups together all the jobs that run in the learn-github-actions workflow.

check-bats-version:

  Defines a job named check-bats-version. The child keys will define properties of the job.

  runs-on: ubuntu-latest

  Configures the job to run on the latest version of an Ubuntu Linux runner. This means that the job will execute on a fresh virtual machine hosted by GitHub. For syntax examples using other runners, see "Workflow syntax for GitHub Actions."

  steps:

  Groups together all the steps that run in the check-bats-version job. Each item nested under this section is a separate action or shell script.

    - uses: actions/checkout@v2

  The uses keyword specifies that this step will run v2 of the actions/checkout action. This is an action that checks out your repository onto the runner, allowing you to run scripts or other actions against your code (such as build and test tools). You should use the checkout action any time your workflow will run against the repository's code.

    - uses: actions/setup-node@v2
      with:
        node-version: '14'

  This step uses the actions/setup-node@v2 action to install the specified version of the Node.js (this example uses v14). This puts both the node and npm commands in your PATH.

    - run: npm install -g bats

  The run keyword tells the job to execute a command on the runner. In this case, you are using npm to install the bats software testing package.

    - run: bats -v

  Finally, you'll run the bats command with a parameter that outputs the software version.