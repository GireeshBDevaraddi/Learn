# Overview
- GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production
- You need only GitHub repository to create and run the GitHub Actions
- Each workflow must be written in YAML and have a `.yml` extension. They also need a corresponding `.properties.json` file that contains extra metadata about the workflow (this is displayed in the GitHub.com UI)
	- For example: `ci/django.yml` and `ci/properties/django.properties.json`
- Your workflow contains one or more _jobs_ which can run in sequential order or in parallel. Each job will run inside its own virtual machine _runner_, or inside a container, and has one or more _steps_ that either run a script that you define or run an _action_, which is a reusable extension that can simplify your workflow.

1. **What is Workflow?**
	-  A workflow is a configurable automated process that will run one or more jobs. Workflows are defined by a YAML file checked in to your repository and will run when triggered by an event in your repository, or they can be triggered manually, or at a defined schedule
	- Create a `.github/workflows` directory in your repository on GitHub if this directory does not already exist. In the `.github/workflows` directory, create a file with the `.yml` or `.yaml` extension
2. **What is Event?**
	- An event is a specific activity in a repository that triggers a workflow run. For example, activity can originate from GitHub when someone creates a pull request, opens an issue, or pushes a commit to a repository
	- For a complete list of events that can be used to trigger workflows, see [Events that trigger workflows](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows).
3. **What is Job?**
	- A job is a set of _steps_ in a workflow that is executed on the same runner. Each step is either a shell script that will be executed, or an _action_ that will be run. Steps are executed in order and are dependent on each other. Since each step is executed on the same runner, you can share data from one step to another.
	- You can configure a job's dependencies with other jobs; by default, jobs have no dependencies and run in parallel with each other
4. **What is Action?**
	- An _action_ is a custom application for the GitHub Actions platform that performs a complex but frequently repeated task. Use an action to help reduce the amount of repetitive code that you write in your workflow files
	- An action can pull your git repository from GitHub, set up the correct toolchain for your build environment, or set up the authentication to your cloud provider. You can write your own actions, or you can find actions to use in your workflows in the GitHub Marketplace.
5. **What is Runner?**
	- A runner is a server that runs your workflows when they're triggered. Each runner can run a single job at a time. each workflow run executes in a fresh, newly-provisioned virtual machine
#

