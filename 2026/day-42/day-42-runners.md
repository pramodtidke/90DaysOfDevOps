# Day 42 – Runners: GitHub-Hosted & Self-Hosted

## Task Completed

Every job needs a machine to run on. Today you understand **runners** — GitHub's hosted ones and how to set up your own self-hosted runner on a real server.

---

## Challenge Tasks

### Task 1: GitHub-Hosted Runners

1. Create a workflow with 3 jobs, each on a different OS:
   - `ubuntu-latest`
   - `windows-latest`
   - `macos-latest`
2. In each job, print:
   - The OS name
   - The runner's hostname
   - The current user running the job
3. Watch all 3 run in parallel

Write in your notes: What is a GitHub-hosted runner? Who manages it?

A GitHub-hosted runner is a temporary virtual machine provided by GitHub that runs the jobs defined in a GitHub Actions workflow. When a workflow starts, GitHub automatically creates the runner, executes the job, and then destroys the machine after the job finishes.

The GitHub infrastructure team manages GitHub-hosted runners.

---

### Task 2: Explore What's Pre-installed

1. On the `ubuntu-latest` runner, run a step that prints:
   - Docker version
   - Python version
   - Node version
   - Git version
     - name: check version
       run: |
       echo "Checking versions of installed software..."
       echo "Git version: $(git --version)"
       echo "Node.js version: $(node --version)"
       echo "Python version: $(python --version)"
       echo "Java version: $(java -version 2>&1)"
       echo "Docker version: $(docker --version)"

2. Look up the GitHub docs for the full list of pre-installed software on `ubuntu-latest`
   yes i checked the github docs of preinstalled softwares: https://docs.github.com/en/actions/concepts/runners/github-hosted-runners

Write in your notes: Why does it matter that runners come with tools pre-installed?
Yes the runner comes with self hosted preinstalled tools which actually commonly used by each team.

---

### Task 3: Set Up a Self-Hosted Runner

1. Go to your GitHub repo → Settings → Actions → Runners → **New self-hosted runner**
2. Choose Linux as the OS
3. Follow the instructions to download and configure the runner on:
   - Your local machine, OR
   - A cloud VM (EC2, Utho, or any VPS)
4. Start the runner — verify it shows as **Idle** in GitHub

**Verify:** Your runner appears in the Runners list with a green dot.

I have added Screenshot of this task.

---

### Task 4: Use Your Self-Hosted Runner

1. Create `.github/workflows/self-hosted.yml`
2. Set `runs-on: self-hosted`
3. Add steps that:
   - Print the hostname of the machine (it should be YOUR machine/VM)
   - Print the working directory
   - Create a file and verify it exists on your machine after the run
4. Trigger it and watch it run on your own hardware

**Verify:** Check your machine — is the file there?

ubuntu@ip-172-31-20-48:~/actions-runner$ ./run.sh

√ Connected to GitHub

Current runner version: '2.332.0'
2026-03-12 12:06:36Z: Listening for Jobs
2026-03-12 12:06:39Z: Running job: self-hosted-runner
2026-03-12 12:06:46Z: Job self-hosted-runner completed with result: Succeeded

---

### Task 5: Labels

1. Add a **label** to your self-hosted runner (e.g., `my-linux-runner`)
2. Update your workflow to use `runs-on: [self-hosted, my-linux-runner]`
3. Trigger it — does it still pick up the job?
   No it does not pick up jobs because added extra runer which is mylinuxrunner

Write in your notes: Why are labels useful when you have multiple self-hosted runners?
label help us to identify the runner

---

### Task 6: GitHub-Hosted vs Self-Hosted

Fill this in your notes:

---

+----------------------+---------------------------+------------------------------+
| | GitHub-Hosted | Self-Hosted |
+----------------------+---------------------------+------------------------------+
| Who manages it? | GitHub | Your team / organization |
+----------------------+---------------------------+------------------------------+
| Cost | Free or limited minutes | You pay for your server/VM |
+----------------------+---------------------------+------------------------------+
| Pre-installed tools | Many tools already there | You install tools yourself |
+----------------------+---------------------------+------------------------------+
| Good for | Quick setup, small CI/CD | Custom environments, large |
| | pipelines | enterprise systems |
+----------------------+---------------------------+------------------------------+
| Security concern | Less control over system | Full control but you manage |
| | | security |
+----------------------+---------------------------+------------------------------+
