
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
