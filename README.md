# Git Actions Course

## Git Command Summary

- **git init:** Initializes a new Git repository.
- **git config:** Sets the username and email for commits.
- **git clone:** Copies a remote repository to your local system.
- **git remote:** Views or modifies the URL of the remote repository.
- **git add:** Adds files to the staging area.
- **git commit:** Bundles changes into a snapshot with a descriptive message.
- **git branch:** Creates, lists, or deletes branches.
- **git checkout:** Switches between branches.
- **git merge:** Combines changes from one branch into another.
- **git fetch:** Downloads latest commits without merging.
- **git pull:** Fetches and merges commits from the remote repository.
- **git push:** Uploads local commits to the remote repository.

## GitHub's Main Features

- **Repository Hosting:** GitHub hosts Git repositories, allowing you to view and create branches, commits, tags, and files.
- **Collaboration:** Features like forking repositories, starring, and watching repositories for updates facilitate collaboration.
- **Issues and Pull Requests:** Issues are action items or tasks, while pull requests are used for code review and merging branches.
- **GitHub Actions:** This feature allows you to create workflows to automate tasks such as build, release, and deploy.
- **Project Management and Documentation:** GitHub offers project management tools like boards and roadmaps, as well as a Wiki for documentation.
- **Security and Insights:** Provides security reports and alerts for vulnerabilities, and insights for tracking repository activity.
- **Settings:** You can configure various repository-related features, including visibility, environments, secrets, and variables.

## Explore the GitHub CLI

- **GitHub CLI Overview:** The GitHub Command Line Interface (CLI) allows you to interact with GitHub directly from your terminal.
- **Authentication:** Use the `gh auth` command to log in with your GitHub credentials.
- **Configuration:** The `gh config` command lets you configure settings like Git protocol, browser for URLs, and text editor.
- **Available Commands:** Offers commands for interacting with repositories, pull requests, issues, releases, and more, including `gh workflow` and `gh run`.
- **API Command:** The `gh api` command enables HTTP requests to GitHub API endpoints for automation and data retrieval.

## Understand Semantic Versioning

- **Semantic Versioning:** A versioning scheme that clarifies the changes in software releases with three parts: major, minor, and patch.
  - **Major version:** Changes that break backwards compatibility.
  - **Minor version:** New features or functionality that are backwards compatible.
  - **Patch version:** Bug fixes or minor changes that do not alter functionality.
- **Dependency Management:** Using tools like PIP (Python), NPM (JavaScript), and Maven (Java) to manage software dependencies.
## 6. Control Job Execution

### 6. Control Job Execution
Learn how to control job execution by setting up dependencies, outputs, and job concurrency.

#### 6.1 Explore job concurrency, outputs, and dependencies
- **Job Dependencies**: Jobs can be configured to run sequentially using the `needs` keyword, ensuring one job completes before another starts.
                      example  Imagine you have three tasks: Task A, Task B, and Task C. By default, all tasks start at the same time. But you can set it up so that Task B waits for Task A to finish before starting, and Task                                  C waits for Task B. This is done using the "needs" keyword. If Task A fails, Task B won't start unless you specify it should always run.
  
- **Job Outputs**: Outputs allow data sharing between jobs, but are not meant for artifacts like binaries or reports.
                   example Think of outputs as messages passed between tasks. For example, Task A can produce a result (like a number or text) that Task B will need. Task B can then use this result to complete its work. This                             ensures data flows correctly between tasks.
- **Concurrency Groups**: Concurrency groups ensure only one job or workflow in the group runs at a time, preventing resource contention and improving performance.
                    example Sometimes, you might want to ensure that only one task runs at a time to avoid overloading the system. You can group tasks together and set rules so that only one task from the group runs at any                                 given time. This is useful when tasks require a lot of resources.


#### 6.3 Run jobs within containers
- **Container Usage**: Running jobs within containers allows you to start with the necessary environment and tools, making the process faster and more efficient.
- **Container Configuration**: You can specify the container image within the container map, and pull images from different container registries, including Docker Hub and GitHub Container Registry.
- **Additional Settings**: You can configure settings like credentials for private registries, environment variables, ports, and mount volumes to enhance the container's functionality.

**refer Git Action-Workflows: .github/workflows/needs_outputs_concurrency_containers.yml**

### 6.5 Explore matrix strategies
- **Matrix Strategies**: This allows you to run the same job in different environments or configurations. For example, you can test your code on multiple versions of a programming language or different operating systems.
- **Configuration**: Define a strategy map and within it, a matrix map with variables to create different job combinations. Use the `include` and `exclude` lists to manage these combinations.
- **Strategy Settings**: Customize matrix job behavior with settings like `fail-fast` to cancel jobs on failure and `max-parallel` to define the number of jobs running simultaneously.

#### 6.6 Implement matrix strategies
- **Creating a Repository**: Set up a new repository with a README file and create a workflow file in the `.github/workflows/` directory.
- **Defining Matrix Strategy**: Defining a Matrix: You create a strategy map and within it, a matrix map. The matrix map includes variables that define the different combinations you want to test. 
"""
For example:
yaml
strategy:
matrix:
os: [ubuntu-latest, windows-latest]
node: [12, 14]
"""
This will run the job on both Ubuntu and Windows, with Node.js versions 12 and 14.

- **Include and Exclude List**: You can add more combinations with the **include** list or remove some with the **exclude** list. This helps you customize which combinations to run.
- **Settings**:
    **fail-fast**: If set to true, it stops all jobs if one fails.
    **max-parallel**: Limits the number of jobs running at the same time.

**refer Git Action-Workflows: .github/workflows/matrix.yml**

## 7 Integrate GitHub Actions, Create Custom Actions, Optimize Workflows

### 7.1 Understand actions in GitHub Actions
**Actions in GitHub Actions**: Think of actions like functions in programming. They are reusable components that perform specific tasks. You can create custom actions or use ones shared by the GitHub community.

**Types of Actions**: There are three main types:
**1)Composite Actions**: Combine multiple steps into one action.
**2)JavaScript Actions**: Use JavaScript to define the action.
**3)Docker Container Actions:** Run the action inside a Docker container.

**Using Actions**: In your workflow, use the uses keyword to specify an action. For example:
yaml

name: Hello World
uses: actions/hello-world-javascript-action@v1.1

This specifies the action to use and its version.

Parameters: Customize actions with parameters using the **with** keyword. For Docker actions, use **args** within **with** to pass arguments.

**refer Git Action-Workflows: .github/workflows/actions.yml**


### 7.2 Composite Actions ###
**Composite Actions**: These are reusable actions in GitHub that bundle multiple workflow steps into a single action. This helps in organizing and reusing code efficiently.

**Metadata File (action.yaml)**
action.yaml: This file, also known as the metadata file, defines the configuration of the composite action. It includes inputs, outputs, and the steps to be executed.

Key Sections of action.yaml
**Inputs:**
Definition: Inputs are parameters that you can pass to your action. They are optional.
Keys: Each input has a key (ID), description, default value (optional), and a required flag (boolean) to specify if the input is mandatory.
Usage: Inputs are accessed using the inputs context followed by the input ID.

**Outputs**:
Definition: Outputs are the data that your action can produce and pass to other steps.
Keys: Each output has an ID, description, and a value which can be a string or an expression.
Usage: Outputs are defined in the outputs map and accessed using the steps context.

**Runs Section**:
Definition: This section specifies the type of action and the steps to be executed.
Using Key: For composite actions, this is set to composite.
Steps: The steps are similar to those in a workflow file and include uses and run keys.
**
Example Breakdown**
Input Section:
yaml
inputs:
example_input:
description: 'An example input'
required: true
default: 'default_value'

Here, example_input is the key, with a description, a required flag set to true, and a default value.

Output Section:
yaml
outputs:
example_output:
description: 'An example output'
value: ${{ steps.step_id.outputs.some_output }}

example_output is the key, with a description and a value that references an output from a specific step.

Runs Section:
yaml
runs:
using: 'composite'
steps:
- run: echo "Hello, world!"

Specifies that the action is composite and includes a step that runs a simple echo command.

## 7.3. JavaScript Actions ###
**JavaScript Actions**:
These are actions you can create in GitHub Actions using JavaScript. They allow you to automate parts of your CI/CD pipeline with custom code.

**Metadata File**:
Just like composite actions, JavaScript actions require a metadata file (action.yml). This file includes inputs, outputs, and a runs section that specifies the runtime and the main script file.

**Required Packages:**
You need to have Node.js installed on your system.
Install the @actions/core and @actions/github packages from the GitHub Actions toolkit using npm. These packages provide functions to interact with GitHub Actions workflows and the GitHub API.

**Compiling with Vercel NCC:**
The @vercel/ncc package is useful for compiling your Node.js modules into a single file. This simplifies the deployment process as you don't need to upload the entire node_modules directory.

**Creating the Action**:
In the action.yml file, specify the runtime (e.g., node16) and the main script file (e.g., index.js).
You can also define pre and post-scripts that run before or after the main action.

**Example Structure**:
The JavaScript file imports the necessary packages and uses a try-catch block to execute the action's code.
If an error occurs, it is caught and logged, and the action sets a failure exit code.

## 7.4.Docker Container Action ##:
-  This type of GitHub action builds an image from your Dockerfile, specifying the operating system, tools, and dependencies needed for your task.
- **Metadata File Configuration**: You need a metadata file to configure inputs, outputs, and action settings. The `using` key must be set to Docker, and you can specify environment variables, arguments, entry points, and pre/post-entry points.
- **File Access**: Outputs can be accessed through the GitHub output environment file, and files generated in the container can be accessed via the `GitHub/workspace` path.
- **Dockerfile Requirements**: The Dockerfile name must start with a capital "D," and the first instruction should be the `FROM` instruction.

**Step to implement docker action**: 
**1. Select the Right Runner**: Use a Linux runner with Docker installed, such as GitHub-hosted runners.
**2. Environment Variables and Arguments**: Use the env keyword for environment variables and define arguments to pass parameters to the Docker container.
**3. Entry Points**: Configure the entry point, pre-entrypoint, and post-entrypoint as needed for setup or cleanup scripts.
**4. Accessing Outputs**: Access output data through the GitHub output environment file or the GitHub.workspace context.
**5. Dockerfile Naming**: Ensure your Dockerfile starts with a capital "D" and the first instruction is the FROM instruction.

### 7.5 Learn about artifacts and cache ###
**Artifacts**: These are files generated by your jobs, like logs, test results, and binaries. They are stored for 90 days by default and can be uploaded or downloaded using specific GitHub actions (upload-artifact and download-artifact).

**refer Git Action-Workflows: .github/workflows/artifact.yml**

**Cache**: This is used to store files that don't change often, like dependencies or packages. Caching these files speeds up your workflows because you don't need to download them every time. The cache is limited to 10GB and is automatically removed if not accessed for over 7 days.

Both artifacts and cache help in making your CI/CD pipelines more efficient by managing files generated or needed during your workflows. This can be particularly useful in your role when dealing with large projects and frequent deployments.

**refer Git Action-Workflows: .github/workflows/chache.yml**
- **Management**: Both artifacts and cache can be managed via the GitHub web interface, REST API, and GitHub CLI.


## 8 Continuous Integration ##

**Continuous Integration (CI):** This is a process where you regularly test your code to make sure it works correctly. Think of it as a way to catch mistakes early.

- **Unit Testing and Code Coverage:** These are methods to check if small parts of your code work as expected and how much of your code is tested.
- **Code Scanning:** This helps find errors and security issues in your code automatically.
- **Dependabot and Secret Scanning:** Dependabot helps keep your code's dependencies up to date, and secret scanning checks for sensitive information in your code.
- **Code Owners and Branch Protection:** These features help manage who is responsible for different parts of the code and protect important parts of your code from unwanted changes.

## 8.1 Set up unit testing and code coverage, part 1

**1.Unit Testing**
**Purpose**: Unit testing helps you verify that individual parts (units) of your code work as expected.
**Benefits**: It catches bugs early, ensures code stability, and makes the codebase more maintainable.
**Tools**: Depending on the programming language, you can use tools like Jest (JavaScript), PyTest (Python), or JUnit (Java).

**1.1Code Coverage**
**Purpose**: Code coverage measures how much of your code is tested by your unit tests.
**Benefits**: It helps identify untested parts of your code where bugs might hide.
**Tools**: Tools like Istanbul (JavaScript), Coverage.py (Python), and JaCoCo (Java) can be integrated into your workflow.

**Why They Matter**
**Early Bug Detection:** By committing code frequently and running tests, you can detect and fix bugs early.
**Reduced Technical Debt:**e Ensuring your code is well-tested and covered reduces future maintenance and refactoring needs.

## 8.2 Set up unit testing and code coverage, part 2

- **Create a Repository:** Go to GitHub, create a new public repository with a README file and a .gitignore file for Node.js.
- **Clone the Repository:** Copy the repository URL and clone it using Visual Studio Code.
- **Set Up the Project:** Open the terminal in VS Code and initialize the project with `npm init -y`. Install Jest for unit testing with `npm install --save-dev jest`.
- **Write Code and Tests:** Create a folder named `src` and add a file `appOperations.js` with a function to multiply two numbers. Create a `tests` folder with a file `appOperations.test.js` to write test cases for your function.
- **Run Tests:** Configure Jest in `package.json` and run `npm test` to execute the tests.
- **Add Code Coverage:** Add the `--coverage` flag to the test command to generate a code coverage report.
- **Automate with GitHub Actions:** Create a `.github/workflows` directory with a YAML file to define a workflow for running tests and generating code coverage reports automatically on pull requests.
- **Enhance Workflow:** Use a GitHub Action to publish the code coverage report to pull requests and set a coverage threshold.

## 8.3 Code Scanning

**Purpose:** Code scanning helps identify security vulnerabilities and coding errors in your GitHub repository.

**How It Works:**
- **Queries and CodeQL:** Code scanning uses queries to detect specific vulnerabilities. CodeQL is the tool that generates a database representing your codebase and runs these queries.
- **Custom Queries:** You can use pre-written queries by GitHub experts or write your own to tailor the scanning to your needs.

**Setting Up Code Scanning:**
- **Enabling Feature:** You need to enable code scanning in the repository settings.
- **Default Setup:** By default, code scanning runs on each push to the default branch, on pull requests, and on a weekly schedule.
- **Advanced Setup:** For more customization, you can configure advanced settings or run CodeQL CLI in an external CI system and upload results to GitHub.

**Alerts and Visualization:**
- **Alerts:** When a vulnerability or error is found, an alert is created.
- **Security Tab:** You can view these alerts in the Security tab of your GitHub repository.

**Supported Languages:** Languages that CodeQL supports include C, C++, C#, Go, Java, JavaScript, Python, Ruby, and Swift.

**Practical Application Integration:** You can integrate third-party code scanning tools within GitHub Actions or external CI systems, making it flexible and adaptable to your workflow.

## 8.4 Dependabot

**Purpose:** Dependabot helps keep your code's dependencies up to date, ensuring your code is secure and runs smoothly.

**How It Works:** Dependabot scans your repository for outdated dependencies and creates pull requests to update them automatically.

**Settings:** You can configure it using a `dependabot.yml` file.

**Updates:**
- **Version Updates:** Keeps all dependencies up to date, even if there are no known vulnerabilities.
- **Security Updates:** Specifically targets dependencies with known vulnerabilities and updates them to secure versions.

## 8.5 Secret Scanning

**Purpose:** Secret scanning identifies sensitive information that might have been accidentally committed to your repository.

**How It Works:** It scans the entire Git history for patterns matching known secrets and creates alerts in the Security tab.

## 8.6 CODEOWNERS File

**Purpose:** The CODEOWNERS file defines the users or teams responsible for specific sections of code.

**Location:** The CODEOWNERS file can be placed in the root, `.github`, or `docs` directory.

**Structure and Rules:**
- **File Naming:** Must be named CODEOWNERS in capital letters.
- **Ownership Rules:** Each line specifies a file path or pattern followed by owners.

**Example Patterns:**
- `/` - The user `repo-owner` is the default owner of everything in the repository.
- `/scripts/` - The users `scripts-owner` and `anotheruser@email.com` are the owners of any file in the scripts directory.
- `/apps/*` - The user `someuser` is the owner of all files in the apps directory, but not its subdirectories.
- `*.js` - The user `js-owner` is the owner of all JavaScript files in the repository.
- `logs/` - The user `log-owner` is the owner of any directory named logs throughout the whole repository.

**Precedence of Rules:** The last pattern matching a file or directory takes precedence.

**Permissions:** Users or teams must have write access to the repository.

**Set up code owners:**
1. **Create a New Branch:** Name it `add-codeowners`.
2. **Add CODEOWNERS File:** Define ownership rules.
3. **Commit and Push Changes:** Commit the changes, push the branch to GitHub, and create a pull request.
4. **Review and Merge:** Ensure the correct users or teams have write access, and merge the changes.

## 8.7 Branch Protection

**Overview:** Branch protection enforces workflows and requirements before changes can be pushed or merged.

**Creating Branch Protection Rules:**
- **Location:** In the repository settings.
- **Pattern Matching:** Use `fnmatch` syntax for branch name patterns.

**Branch Protection Settings:**
1. **Require Pull Request Reviews:** Prevents unapproved pull requests from being merged.
2. **Require Status Checks:** Ensures checks pass before merging.
3. **Require Conversation Resolution:** All comments must be addressed before merging.
4. **Require Signed Commits:** Enforces the use of cryptographic keys for verification.
5. **Require Linear History:** Maintains a linear history with squash or rebase merges.
6. **Require Merge Queues:** Automates merges and ensures compatibility.
7. **Require Successful Deployments:** Changes must be successful in test environments before merging.
8. **Lock Branch:** Makes the branch read-only.
9. **Prevent Bypassing Settings:** Stops administrators from bypassing settings.
10. **Restrict Who Can Push:** Defines who can push to a protected branch.
11. **Allow Force Pushes:** (Disabled by default).
12. **Allow Deletions:** (Disabled by default).

**Configure branch protection:**
- **Setting Up:** Navigate to repository settings, branches, and add a rule for the main branch.
- **Protection Rules:** You can set various requirements like pull request reviews and status checks.
- **Testing and Merging:** Observe how rules enforce workflow during pull request creation.
