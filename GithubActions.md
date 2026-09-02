
### Soft Development Pipeline Steps:

General steps include:
```
Code -> Test -> Build -> Package (Dockerize) -> Push -> Deploy -> Verify
```

### What is CI?

CI stands for **Continuous Integration.** It is the process in which every code change is automatically built and tested as soon as it is pushed to the repository.
```
CI: Code -> Test -> Build -> Package
```

CI means that whenever developers push/merge code, the pipeline automatically validates the code by running things like:

- Tests
- Linting
- Build
- Type checking
- Docker image creation

One important distinction: pushing th

### What is CD?

CD can mean **Continuous Delivery** or **Continuous Deployment.**

**Continuous Delivery:**
The application is automatically built and packaged into a
deployable artifact and kept ready for deployment.

**Continuous Deployment:**
The deployable artifact produced by the pipeline is
automatically deployed to the target environment.

Example:

CI:
Code → Test → Build → Package → Push

CD:
Deploy → Verify

The complete mental model
```
Developer
   ↓
Code
   ↓
Test
   ↓
Build
   ↓
Package / Dockerize
   ↓
Push artifact/image
   ↓
Deploy
   ↓
Verify
```

### CI/CD Terminology

- **Workflow:**
  A workflow is the full automation process.

- **Job:**
  A job is a major task inside a workflow.

- **Steps:**
  Steps are the individual commands inside a job.

- **Runner:**
  A runner is the computer where your job runs.

  It is a temporrary machine provisioned to run the entire pipeline.

  It can be:
    - Github hosted
    - Self hosted

Example Workflow:

```
Workflow
  - Job (CI)
    - Step 1: Checkout code
    - Step 2: Run tests
    - Step 3: Build app
  - Job (CD)
    - Step 1: Deploy
    - Step 2: Verify
```

- **Trigger:**
  A trigger is basically what starts the pipeline.

  When event happens then:
  -> Workflow selected -> Job is scheduled -> Runner is provisioned -> Steps execute -> Job Finishes -> Runner is destroyed


### Github Actions

GitHub Actions is GitHub's built-in CI/CD (Continuous Integration and Continuous Deployment) and automation platform. It lets you automate tasks in your GitHub repository whenever certain events occur.

Key concepts
1. Workflow
   - A workflow is an automated process defined in a YAML file.

   - Workflow files are stored in:

      `.github/workflows/`

2. Event (Trigger)
   - An event starts a workflow.
   - Common events:
      - push
      - pull_request
      - schedule
      - workflow_dispatch (manual trigger) [This displays a Run Workflow button in the Actions Tab on Github, and we can manually trigger it from here]

3. Job
   - A workflow contains one or more jobs.
   - Jobs run independently (or in sequence if dependencies are defined).
   - Each job runs on a virtual machine (runner).

3. Runner
   - A runner is the machine that executes the job.
   - GitHub provides hosted runners like:
      - ubuntu-latest
      - windows-latest
      - macos-latest

4. Step
   - Each job consists of multiple steps.
   - A step can:
      - Run shell commands
      - Use an existing GitHub Action

5. Action
   - An action is a reusable piece of automation.
   - Instead of writing everything from scratch, you can reuse actions published by GitHub or the community.

   Example:
   ```
   - uses: actions/checkout@v4
   ```
   This action checks out your repository's code onto the runner.



Example Workflow:
```
name: Build Project

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run a one-line script
        run: echo Hello, world
      
      - name: Run a multi-line script
        run: |
         echo first line
         echo second line

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

What happens here?
1. Someone pushes code to the main branch.
2. GitHub starts the Build Project workflow.
3. It creates an Ubuntu virtual machine (runner).
4. It checks out the repository's code.
5. It runs the echo Hello, world command.
6. It runs the two multi-line echo commands.
7. It sets up Node.js 20.
8. It installs the project dependencies using npm install.
9. It runs the tests using npm test.