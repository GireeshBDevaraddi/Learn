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

## Contexts
- Contexts are a way to access information about workflow runs, variables, runner environments, jobs, and steps. Each context is an object that contains properties, which can be strings or other objects.
- You can access contexts using the expression syntax `${{ <context> }}`
 1. `github` context
    - The github context contains information about the workflow run and the event that triggered the run. You can also read most of the github context data in environment variables
    - [Github Context](https://docs.github.com/en/actions/learn-github-actions/contexts#github-context)
   
 2. `env` context
    - The env context contains variables that have been set in a workflow, job, or step. It does not contain variables inherited by the runner process
    - You can retrieve the values of variables stored in `env` context and use these values in your workflow file. You can use the `env` context in any key in a workflow step except for the `id` and `uses` keys
    - [env Context](https://docs.github.com/en/actions/learn-github-actions/contexts#env-context)
   
 3. `vars` context
    - The vars context contains custom configuration variables set at the organization, repository, and environment levels
    - If a configuration variable has not been set, the return value of a context referencing the variable will be an empty string.
    - [vars context](https://docs.github.com/en/actions/learn-github-actions/contexts#vars-context)
      
4. `job` context
   - The job context contains information about the currently running job.
   - [job context](https://docs.github.com/en/actions/learn-github-actions/contexts#job-context)

5. `jobs` context
   - The jobs context is only available in reusable workflows, and can only be used to set outputs for a reusable workflow
   - [jobs context](https://docs.github.com/en/actions/learn-github-actions/contexts#jobs-context)

6. `steps` context
   - The steps context contains information about the steps in the current job that have an id specified and have already run
   - [steps context](https://docs.github.com/en/actions/learn-github-actions/contexts#steps-context)
  
7. `runner` context
   - The runner context contains information about the runner that is executing the current job
   - [runner context](https://docs.github.com/en/actions/learn-github-actions/contexts#runner-context)
  
8. `secrets` context
   - The secrets context contains the names and values of secrets that are available to a workflow run.
   - The secrets context is not available for composite actions due to security reasons. If you want to pass a secret to a composite action, you need to do it explicitly as an input
   - `GITHUB_TOKEN` is a secret that is automatically created for every workflow run, and is always included in the `secrets` context
   - [secrets context](https://docs.github.com/en/actions/learn-github-actions/contexts#secrets-context)
  
9. `strategy` context
   - For workflows with a matrix, the strategy context contains information about the matrix execution strategy for the current job
   - [strategy context](https://docs.github.com/en/actions/learn-github-actions/contexts#strategy-context)

10. `matrix` context
    - For workflows with a matrix, the matrix context contains the matrix properties defined in the workflow file that apply to the current job
    - There are no standard properties in the matrix context, only those which are defined in the workflow file
    - [matrix context](https://docs.github.com/en/actions/learn-github-actions/contexts#matrix-context)

11. `needs` context
    - The needs context contains outputs from all jobs that are defined as a direct dependency of the current job. Note that this doesn't include implicitly dependent jobs (for example, dependent jobs of a dependent job)
    - [needs context](https://docs.github.com/en/actions/learn-github-actions/contexts#needs-context)

12. `inputs` context
    - The inputs context contains input properties passed to an action, to a reusable workflow, or to a manually triggered workflow.
    -  For reusable workflows, the input names and types are defined in the `workflow_call` event configuration of a reusable workflow, and the input values are passed from `jobs.<job_id>.with` in an external workflow that calls the reusable workflow
    -  For manually triggered workflows, the inputs are defined in the `workflow_dispatch` event configuration of a workflow.
    -  The properties in the inputs context are defined in the workflow file. They are only available in a reusable workflow or in a workflow triggered by the workflow_dispatch event
    -  [inputs context](https://docs.github.com/en/actions/learn-github-actions/contexts#inputs-context)

## Variables
 - Variables provide a way to store and reuse non-sensitive configuration information. You can store any configuration data such as compiler flags, usernames, or server names as variables. Variables are interpolated on the runner machine that runs your workflow. Commands that run in actions or workflow steps can create, read, and modify variables
 - You can set a custom variable in two ways.
   1. To define an environment variable for use in a single workflow, you can use the `env` key in the workflow file
      - The scope of a custom variable set by this method is limited to the element in which it is defined
      - The entire workflow, by using `env` at the top level of the workflow file.
      - The contents of a job within a workflow, by using `jobs.<job_id>.env`.
      - A specific step within a job, by using `jobs.<job_id>.steps[*].env`.
   3. To define a configuration variable across multiple workflows, you can define it at the organization, repository, or environment level
      - When you define configuration variables, they are automatically available in the vars context
      - ou can use configuration variables to set default values for parameters passed to build tools at an organization level, but then allow repository owners to override these parameters on a case-by-case basis.

- **Configuration variable precedence**
  - If a variable with the same name exists at multiple levels, the variable at the lowest level takes precedence.
  - The following rules apply to configuration variable names:
     - Names can only contain alphanumeric characters ([a-z], [A-Z], [0-9]) or underscores (_). Spaces are not allowed.
     - Names must not start with the GITHUB_ prefix.
     - Names must not start with a number.
     - Names are case insensitive.
     - Names must be unique at the level they are created at.
  - Individual variables are limited to 48 KB in size.
  - You can store up to 1,000 organization variables, 500 variables per repository, and 100 variables per environment.
  - A workflow created in a repository can access the following number of variables:
	- Up to 500 repository variables, if the total size of repository variables is less than 256 KB. If the total size of repository variables exceeds 256 KB, only the repository variables that fall below the limit will be available (as sorted alphabetically by variable name).
	- Up to 1,000 organization variables, if the total combined size of repository and organization variables is less than 256 KB. If the total combined size of organization and repository variables exceeds 256 KB, only the organization variables that fall below that limit will be available (after accounting for repository variables and as sorted alphabetically by variable name).
	- Up to 100 environment-level variables
  - Environment-level variables do not count toward the 256 KB total size limit. If you exceed the combined size limit for repository and organization variables and still need additional variables, you can use an environment and define additional variables in the environment.

- **[Workflow Billing and Limits](https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#about-billing-for-github-actions)**
- 
