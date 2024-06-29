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

# Workflows
- A workflow is a configurable automated process that will run one or more jobs
- Workflows are defined in the .github/workflows directory in a repository, and a repository can have multiple workflows, each of which can perform a different set of tasks.
- A workflow must contain the following basic components:
   - One or more events that will trigger the workflow.
   - One or more jobs, each of which will execute on a runner machine and run a series of one or more steps.
   - Each step can either run a script that you define or run an action, which is a reusable extension that can simplify your workflow.
- Workflow triggers are events that cause a workflow to run. These events can be:
   - Events that occur in your workflow's repository
   - Events that occur outside of GitHub and trigger a `repository_dispatch` event on GitHub
   - Scheduled times
   - Manual
- By default, the jobs in your workflow all run in parallel at the same time. If you have a job that must only run after another job has completed, you can use the `needs` keyword to create this dependency. If one of the jobs fails, all dependent jobs are skipped; however, if you need the jobs to continue, you can define this using the `if` conditional statement
- If your job requires a database or cache service, you can use the `services` keyword to create an ephemeral container to host the service; the resulting container is then available to all steps in that job and is removed when the job has completed
- Each workflow run will use the version of the workflow that is present in the associated commit SHA or Git ref of the event. When a workflow runs, GitHub sets the `GITHUB_SHA` (commit SHA) and `GITHUB_REF` (Git ref) environment variables in the runner environment
- When you use the repository's `GITHUB_TOKEN` to perform tasks, events triggered by the `GITHUB_TOKEN`, with the exception of `workflow_dispatch` and `repository_dispatch`, will not create a new workflow run. This prevents you from accidentally creating recursive workflow runs.
- If you specify multiple activity types, only one of those event activity types needs to occur to trigger your workflow. If multiple triggering event activity types for your workflow occur at the same time, multiple workflow runs will be triggered.
- Use the `branches` filter when you want to include branch name patterns or when you want to both include and exclude branch names patterns. Use the `branches-ignore` filter when you only want to exclude branch name patterns. You cannot use both the `branches` and `branches-ignore` filters for the same event in a workflow
- If you define both `branches/branches-ignore` and `paths/paths-ignore`, the workflow will only run when both filters are satisfied.
- You cannot use `branches` and `branches-ignore` to filter the same event in a single workflow. If you want to both include and exclude branch patterns for a single event, use the branches filter along with the `!` character to indicate which branches should be excluded.
 ```yaml
  on:
  pull_request:
    branches:
      - 'releases/**'
      - '!releases/**-alpha'
  ```
 - Use the `tags` filter when you want to include tag name patterns or when you want to both include and exclude tag names patterns. Use the `tags-ignore` filter when you only want to exclude tag name patterns. You cannot use both the `tags` and `tags-ignore` filters for the same event in a workflow.
 - The patterns defined in `branches` and `tags` are evaluated against the Git ref's name
 - The filter determines if a workflow should run by evaluating the changed files and running them against the `paths-ignore` or `paths` list. If there are no files changed, the workflow will not run.
 - GitHub generates the list of changed files using `two-dot diffs` for `pushes` and `three-dot diffs` for `pull requests`
   - `Pull requests`: Three-dot diffs are a comparison between the most recent version of the topic branch and the commit where the topic branch was last synced with the base branch.
   - `Pushes to existing branches`: A two-dot diff compares the head and base SHAs directly with each other.
   - `Pushes to new branches`: A two-dot diff against the parent of the ancestor of the deepest commit pushed.
  
 - Diffs are limited to 300 files. If there are files changed that aren't matched in the first 300 files returned by the filter, the workflow will not run. You may need to create more specific filters so that the workflow will run automatically.
 - When using the `workflow_run` event, you can specify what branches the triggering workflow must run on in order to trigger your workflow.
 - **[Events that Trigger Workflows](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)**
 - **[WorkFlow Syntax for workflows](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)**
 - You can use workflow commands when running shell commands in a workflow or in an action's code
   ```bash
   echo "::workflow-command parameter1={data},parameter2={data}::{command value}"
   ::debug::{message}
   echo "::debug::Set the Octocat variable"
   ::notice file={name},line={line},endLine={endLine},title={title}::{message}
   ```
 - A workflow that uses another workflow is referred to as a `caller` workflow. The reusable workflow is a `called` workflow. One `caller` workflow can use multiple `called` workflows. Each `called` workflow is referenced in a single line. The result is that the `caller` workflow file may contain just a few lines of YAML, but may perform a large number of tasks when it's run. When you reuse a workflow, the entire `called` workflow is used, just as if it was part of the `caller` workflow
 - To cache dependencies for a job, you can use GitHub's `cache` action. The action creates and restores a cache identified by a unique key. Alternatively, if you are caching the package managers listed below, using their respective `setup-*` actions requires minimal configuration and will create and restore dependency caches for you.
 - GitHub CLI is preinstalled on all GitHub-hosted runners. For each step that uses GitHub CLI, you must set an environment variable called GH_TOKEN to a token with the required scopes

# Jobs
-  A job is a set of _steps_ in a workflow that is executed on the same runner. Each job runs in a runner environment specified by `runs-on`
-  Use `jobs.<job_id>` to give your job a unique identifier. The key `job_id` is a string and its value is a map of the job's configuration data. You must replace `<job_id>` with a string that is unique to the jobs object. The `<job_id>` must start with a letter or _ and contain only alphanumeric characters, -, or _.
-  Use `jobs.<job_id>.name` to set a name for the job, which is displayed in the GitHub UI.
-  Use `jobs.<job_id>.runs-on` to define the type of machine to run the job on.
-  If you would like to run your workflow on multiple machines, use `jobs.<job_id>.strategy`.
-  Quotation marks are not required around simple strings like `self-hosted`, but they are required for expressions like  `"${{ inputs.chosen-os }}"`
-  You can use `runs-on` to target runner groups, so that the job will execute on any runner that is a member of that group
-  You can use the `jobs.<job_id>.if` conditional to prevent a job from running unless a condition is met. You can use any supported context and expression to create a conditional
-  The `jobs.<job_id>.if` condition is evaluated before `jobs.<job_id>.strategy.matrix` is applied.
-  When you use expressions in an `if` conditional, you can, optionally, omit the `${{ }}` expression syntax because GitHub Actions automatically evaluates the `if` conditional as an expression. However, this exception does not apply everywhere.
-  You must always use the `${{ }}` expression syntax or escape with `'', "", or ()` when the expression starts with `!`, since `!` is reserved notation in YAML format
  ```yaml
	if: ${{ ! startsWith(github.ref, 'refs/tags/') }}
```
 - A matrix strategy lets you use variables in a single job definition to automatically create multiple job runs that are based on the combinations of the variables.
 - A matrix will generate a maximum of 256 jobs per workflow run. This limit applies to both GitHub-hosted and self-hosted runners.
 - Use `jobs.<job_id>.strategy.matrix.include` to expand existing matrix configurations or to add new configurations. The value of `include` is a list of objects.
 - To remove specific configurations defined in the matrix, use `jobs.<job_id>.strategy.matrix.exclude`. An excluded configuration only has to be a partial match for it to be excluded
 -  All `include` combinations are processed after `exclude`. This allows you to use `include` to add back combinations that were previously excluded.
 -  You can control how job failures are handled with `jobs.<job_id>.strategy.fail-fast` and `jobs.<job_id>.continue-on-error`
 -  `jobs.<job_id>.strategy.fail-fast` applies to the entire matrix. If `jobs.<job_id>.strategy.fail-fast` is set to `true` or its expression evaluates to `true`, GitHub will cancel all in-progress and queued jobs in the matrix if any job in the matrix fails. This property defaults to `true`
 -  `jobs.<job_id>.continue-on-error` applies to a single job. If `jobs.<job_id>.continue-on-error` is `true`, other jobs in the matrix will continue running even if the job with `jobs.<job_id>.continue-on-error: true` fails.
 -  You can use `jobs.<job_id>.strategy.fail-fast` and `jobs.<job_id>.continue-on-error` together.
 -  By default, GitHub will maximize the number of jobs run in parallel depending on runner availability. To set the maximum number of jobs that can run simultaneously when using a matrix job strategy, use `jobs.<job_id>.strategy.max-parallel`
 -  By default, GitHub Actions allows multiple jobs within the same workflow, multiple workflow runs within the same repository, and multiple workflow runs across a repository owner's account to run concurrently. This means that multiple workflow runs, jobs, or steps can run at the same time
 -  You can use `jobs.<job_id>.concurrency` to ensure that only a single job or workflow using the same concurrency group will run at a time. A concurrency group can be any string or expression. Allowed expression contexts: `github`, `inputs`, `vars`, `needs`, `strategy`, and `matrix`.
 -  To also cancel any currently running job or workflow in the same concurrency group, specify `cancel-in-progress: true`. To conditionally cancel currently running jobs or workflows in the same concurrency group, you can specify `cancel-in-progress` as an expression with any of the allowed expression contexts.
 -  The concurrency group name is case insensitive
 -  Ordering is not guaranteed for jobs or workflow runs using concurrency groups. Jobs or workflow runs in the same concurrency group are handled in an arbitrary order.
 -  You can also limit the concurrency of jobs within a workflow by using the `concurrency` keyword at the job level
 -  The `concurrency` key is used to group workflows or jobs together into a concurrency group. When you define a `concurrency` key, GitHub Actions ensures that only one workflow or job with that key runs at any given time
 -  Use `jobs.<job_id>.container` to create a container to run any steps in a job that don't already specify a container. If you have steps that use both script and container actions, the container actions will run as sibling containers on the same network with the same volume mounts
 -  If you do not set a `container`, all steps will run directly on the host specified by `runs-on` unless a step refers to an action configured to run in a container.
 -  The default shell for `run` steps inside a container is `sh` instead of `bash`. This can be overridden with `jobs.<job_id>.defaults.run` or `jobs.<job_id>.steps[*].shell`.
 -  Use `jobs.<job_id>.container.image` to define the Docker image to use as the container to run the action. The value can be the Docker Hub image name or a registry name
 -  If the image's container registry requires authentication to pull the image, you can use `jobs.<job_id>.container.credentials` to set a map of the `username` and `password`. The credentials are the same values that you would provide to the `docker login` command.
 -  Use `jobs.<job_id>.container.env` to set a `map` of environment variables in the container.
 -  Use `jobs.<job_id>.container.ports` to set an `array` of ports to expose on the container.
 -  Use `jobs.<job_id>.container.volumes` to set an `array` of volumes for the container to use.
 -  To specify a volume, you specify the source and destination path:
	- <source>:<destinationPath>.
	- <source> is a volume name or an absolute path on the host machine. 
 	- <destinationPath> is an absolute path in the container.
   ```yaml
	volumes:
	  - my_docker_volume:/volume_mount
	  - /data/my_data
	  - /source/directory:/destination/directory
   ```
 - Use `jobs.<job_id>.container.options` to configure additional Docker container resource options
 - 
