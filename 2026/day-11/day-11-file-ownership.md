# Day 11 – File Ownership Challenge (chown & chgrp)

## Task

Master file and directory ownership in Linux.

- Understand file ownership (user and group)
- Change file owner using `chown`
- Change file group using `chgrp`
- Apply ownership changes recursively

---

## Expected Output

- A markdown file: `day-11-file-ownership.md`
- Screenshots showing ownership changes

---

## Challenge Tasks

### Task 1: Understanding Ownership (10 minutes)

1. Run `ls -l` in your home directory
2. Identify the **owner** and **group** columns
3. Check who owns your files

**Format:** `-rw-r--r-- 1 owner group size date filename`

Document: What's the difference between owner and group?

owner:

-Owner is the specific user who created or currently own a file, typically own highest authority (RWX).
-Owner can typically change permission and file ownership.

Group:

-Group is the collection of user assigned to the file, allowing a shared access for colaboration.
-Define the permission for specific group of users to the file.
-Shared acces to file without making it public

- ***

### Task 2: Basic chown Operations (20 minutes)

1. Create file `devops-file.txt`
2. Check current owner: `ls -l devops-file.txt`
3. Change owner to `tokyo` (create user if needed)
4. Change owner to `berlin`
5. Verify the changes

**Try:**

```bash
sudo chown tokyo devops-file.txt
```

ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ ls -l
total 0
-rw-r--r-- 1 takyo ubuntu 0 Feb 23 13:48 devops-file.txt

ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ sudo chown berlinn devops-file.txt

ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ ls -l
total 0
-rw-r--r-- 1 berlinn ubuntu 0 Feb 23 13:48 devops-file.txt
ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$

---

### Task 3: Basic chgrp Operations (15 minutes)

1. Create file `team-notes.txt`
2. Check current group: `ls -l team-notes.txt`
3. Create group: `sudo groupadd heist-team`
4. Change file group to `heist-team`
5. Verify the change

ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ ls -l
total 0
-rw-r--r-- 1 berlinn ubuntu 0 Feb 23 13:48 devops-file.txt
-rw-rw-r-- 1 ubuntu heist-team 0 Feb 23 13:58 team-notes.txt
ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$

---

### Task 4: Combined Owner & Group Change (15 minutes)

Using `chown` you can change both owner and group together:

1. Create file `project-config.yaml`
2. Change owner to `professor` AND group to `heist-team` (one command)
3. Create directory `app-logs/`
4. Change its owner to `berlin` and group to `heist-team`

**Syntax:** `sudo chown owner:group filename`

Answer:

      ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ mkdir app-logs
      ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ ls
      app-logs  devops-file.txt  project-config.yaml  team-notes.txt
      ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ sudo chown berlinn app-logs/
      ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ sudo chgrp heist-team app-logs/
      ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ ls -l
      total 4
      drwxrwxr-x 2 berlinn   heist-team 4096 Feb 23 16:19 app-logs
      -rw-r--r-- 1 berlinn   ubuntu        0 Feb 23 13:48 devops-file.txt
      -rw-rw-r-- 1 professor ubuntu        0 Feb 23 14:07 project-config.yaml
      -rw-rw-r-- 1 ubuntu    heist-team    0 Feb 23 13:58 team-notes.txt
      ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$

---

### Task 5: Recursive Ownership (20 minutes)

1. Create directory structure:

   ```
   mkdir -p heist-project/vault
   mkdir -p heist-project/plans
   touch heist-project/vault/gold.txt
   touch heist-project/plans/strategy.conf
   ```

2. Create group `planners`: `sudo groupadd planners`

3. Change ownership of entire `heist-project/` directory:
   - Owner: `professor`
   - Group: `planners`
   - Use recursive flag (`-R`)

     ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ sudo chown -R professor heist-project/ && sudo chgrp -R planners heist-project/

     ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ ls -l
     total 8
     drwxrwxr-x 2 berlinn heist-team 4096 Feb 23 16:19 app-logs
     -rw-r--r-- 1 berlinn ubuntu 0 Feb 23 13:48 devops-file.txt
     drwxrwxr-x 4 professor planners 4096 Feb 23 16:22 heist-project
     -rw-rw-r-- 1 professor ubuntu 0 Feb 23 14:07 project-config.yaml
     -rw-rw-r-- 1 ubuntu heist-team 0 Feb 23 13:58 team-notes.txt
     ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$

4. Verify all files and subdirectories changed: `ls -lR heist-project/`

ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ sudo chown -R professor heist-project/ && sudo chgrp -R planners heist-project/
ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$ ls -lR heist-project/
heist-project/:
total 8
drwxrwxr-x 2 professor planners 4096 Feb 23 16:22 plans
drwxrwxr-x 2 professor planners 4096 Feb 23 16:22 vault

heist-project/plans:
total 0
-rw-rw-r-- 1 professor planners 0 Feb 23 16:22 strategy.conf

heist-project/vault:
total 0
-rw-rw-r-- 1 professor planners 0 Feb 23 16:22 gold.txt
ubuntu@ip-172-31-18-13:~/Devops_challenge/day11$

---

### Task 6: Practice Challenge (20 minutes)

1. Create users: `tokyo`, `berlin`, `nairobi` (if not already created)
2. Create groups: `vault-team`, `tech-team`
3. Create directory: `bank-heist/`
4. Create 3 files inside:

   ```
   touch bank-heist/access-codes.txt
   touch bank-heist/blueprints.pdf
   touch bank-heist/escape-plan.txt
   ```

5. Set different ownership:
   - `access-codes.txt` → owner: `tokyo`, group: `vault-team`
   - `blueprints.pdf` → owner: `berlin`, group: `tech-team`
   - `escape-plan.txt` → owner: `nairobi`, group: `vault-team`

**Verify:** `ls -l bank-heist/`

answer:

ubuntu@ip-172-31-18-13:~/Devops_challenge/day11/bank-heist$ sudo chown takyo access-codes.txt && sudo chgrp vault-team access-codes.txt
ubuntu@ip-172-31-18-13:~/Devops_challenge/day11/bank-heist$ sudo chown berlinn blueprints.pdf && sudo chgrp tech-team blueprints.pdf
ubuntu@ip-172-31-18-13:~/Devops_challenge/day11/bank-heist$ sudo chown nairobi escape-plan.txt && sudo chgrp vault-team escape-plan.txt
ubuntu@ip-172-31-18-13:~/Devops_challenge/day11/bank-heist$ ls -l
total 0
-rw-rw-r-- 1 takyo vault-team 0 Feb 23 16:36 access-codes.txt
-rw-rw-r-- 1 berlinn tech-team 0 Feb 23 16:36 blueprints.pdf
-rw-rw-r-- 1 nairobi vault-team 0 Feb 23 16:36 escape-plan.txt
ubuntu@ip-172-31-18-13:~/Devops_challenge/day11/bank-heist$

---

## Key Commands Reference

```bash
# View ownership
ls -l filename

# Change owner only
sudo chown newowner filename

# Change group only
sudo chgrp newgroup filename

# Change both owner and group
sudo chown owner:group filename

# Recursive change (directories)
sudo chown -R owner:group directory/

# Change only group with chown
sudo chown :groupname filename
```

---

## Hints

- Most `chown`/`chgrp` operations need `sudo`
- Use `-R` flag for recursive directory changes
- Always verify with `ls -l` after changes
- User must exist before using in `chown`
- Group must exist before using in `chgrp`/`chown`

---

## Documentation

Create `day-11-file-ownership.md`:

```markdown
# Day 11 Challenge

## Files & Directories Created

[list all files/directories]

## Ownership Changes

[before/after for each file]

Example:

- devops-file.txt: user:user → tokyo:heist-team

## Commands Used

[your commands here]

## What I Learned

[3 key points about file ownership]
```

---

## Troubleshooting

**Permission denied?**

- Use `sudo` for chown/chgrp operations

**Group doesn't exist?**

- Create it first: `sudo groupadd groupname`

**User doesn't exist?**

- Create it first: `sudo useradd username`

---

## Why This Matters for DevOps

In real DevOps scenarios, you need proper file ownership for:

- Application deployments
- Shared team directories
- Container file permissions
- CI/CD pipeline artifacts
- Log file management

---

## Submission

1. Navigate to `2026/day-11/` folder
2. Add `day-11-file-ownership.md` with screenshots
3. Commit and push to your fork

---

## Learn in Public

Share on LinkedIn about mastering file ownership.

Use hashtags:

```
#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
```

Happy Learning
**TrainWithShubham**
