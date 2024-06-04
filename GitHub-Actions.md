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

# Expressions
- Use expressions to programmatically set environment variables in workflow files and access contexts. An expression can be any combination of literal values, references to a context, or functions. You can combine literals, context references, and functions using operators
- You need to use specific syntax to tell GitHub to evaluate an expression rather than treat it as a string. `${{ <expression> }}`
- Note that in conditionals, falsy values (false, 0, -0, "", '', null) are coerced to false and truthy (true and other non-falsy values) are coerced to true.
> - GitHub ignores case when comparing strings.
> - For numerical comparison, the `fromJSON()` function can be used to convert a string to a number.
> - GitHub performs loose equality comparisons
> - If the types do not match, GitHub coerces the type to a number. GitHub casts data types to a number
> - When NaN is one of the operands of any relational comparison (>, <, >=, <=), the result is always false
> - Objects and arrays are only considered equal when they are the same instance
 <img width="720" alt="image" src="https://github.com/GireeshBDevaraddi/Learn/assets/135546164/e5311596-91bd-4472-9e0a-86f9f0ff6bbe">

# Functions
- GitHub offers a set of built-in functions that you can use in expressions. Some functions cast values to a string to perform comparisons
1. `contains( search, item )`
2. `startsWith( searchString, searchValue )`
3. `endsWith( searchString, searchValue )`
4. `format( string, replaceValue0, replaceValue1, ..., replaceValueN)`
    <img width="718" alt="image" src="https://github.com/GireeshBDevaraddi/Learn/assets/135546164/b8f52b86-4beb-411d-83f2-eb6cdd866e0c">
5. `join( array, optionalSeparator )`
6. `fromJSON(value)`
7. `hashFiles(path)`

## Status Check Functions
- Use status check functions as expressions in if conditionals. A default status check of `success()` is applied unless you include one of these functions
1. `success()`
   - Returns `true` when all previous steps have succeeded.
2. `always()`
   - Causes the step to always execute, and returns `true`, even when canceled. The always expression is best used at the step level or on tasks that you expect to run even when a job is canceled
3. `cancelled()`
   - Returns `true` if the workflow was canceled.
4. `failure()`
   - Returns `true` when any previous step of a job fails. If you have a chain of dependent jobs, `failure()` returns `true` if any ancestor job fails.

- You can use the `*` syntax to apply a filter and select matching items in a collection.



