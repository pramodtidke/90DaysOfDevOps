# Day 38 – YAML Basics

## Task

Before writing a single CI/CD pipeline, you need to get comfortable with **YAML** — the language every pipeline is written in.

You will:

- Understand YAML syntax and rules
- Write YAML files by hand
- Validate them

---

---

## Challenge Tasks

### Task 1: Key-Value Pairs

Create `person.yaml` that describes yourself with:

- `name`
- `role`
- `experience_years`
- `learning` (a boolean)

**Verify:** Run `cat person.yaml` — does it look clean? No tabs?

Code: person.yml

name: personal information
on:
workflow_dispatch:
jobs:
details:
runs-on: ubuntu-latest

    steps:
      - name: My name
        run: echo "Pramod Tidke"
      - name: experience
        run: echo "2 year of experience"
      - name: role is
        run: echo "software engineer"
      - name: learning
        run: echo "true"

---

### Task 2: Lists

Add to `person.yaml`:

- `tools` — a list of 5 DevOps tools you know or are learning
- `hobbies` — a list using the inline format `[item1, item2]`

Write in your notes: What are the two ways to write a list in YAML?

code:

      - name: tools
        run: echo "git, docker, kubernetes, ansible, terraform"
      - name: hobbie
        run: echo [coding, music, gaming]

---

### Task 3: Nested Objects

Create `server.yaml` that describes a server:

- `server` with nested keys: `name`, `ip`, `port`
- `database` with nested keys: `host`, `name`, `credentials` (nested further: `user`, `password`)

**Verify:** Try adding a tab instead of spaces — what happens when you validate it?

code:
name: server informatin

on:
push:
branches: [main]

jobs:
server:
runs-on: ubuntu-latest

    steps:
      - name: server name
        run: echo "ubuntu server"
      - name: ip address
        run: echo "192.168.1.1"
      - name: ports
        run: echo "80, 443, 22"

---

### Task 4: Multi-line Strings

In `server.yaml`, add a `startup_script` field using:

1. The `|` block style (preserves newlines)

Code:

- name: data information
  run: |
  echo "database:"
  echo " host: localhost"
  echo " port: 3306"
  echo " credential:"
  echo " username: root"
  echo " password: password"

GitHUbAction:

Run echo "database:"
echo "database:"
echo " host: localhost"
echo " port: 3306"
echo " credential:"
echo " username: root"
echo " password: password"
shell: /usr/bin/bash -e {0}
database:
host: localhost
port: 3306
credential:
username: root
password: password

2. The `>` fold style (folds into one line)
   code:

- name: services of database
  run: >
  echo "services:"
  echo " - mysql"
  echo " - postgresql"
  echo " - mongodb"

Github Action:
Run echo "services:" echo " - mysql" echo " - postgresql" echo " - mongodb"
echo "services:" echo " - mysql" echo " - postgresql" echo " - mongodb"
shell: /usr/bin/bash -e {0}
services: echo - mysql echo - postgresql echo - mongodb

Write in your notes: When would you use `|` vs `>`?

"|" use for scripting, code blocks because pip maintain the format as it is

">" use for long notes, paragraphs, text because it folds into a single line after execution.

---

### Task 5: Validate Your YAML

1. Install `yamllint` or use an online validator
2. Validate both your YAML files
3. Intentionally break the indentation — what error do you get?
4. Fix it and validate again

Ans: Yes i do check with the yamllint and validate the code, got some indentation error at yaml code and then fix it and validate it.

---

### Task 6: Spot the Difference

Read both blocks and write what's wrong with the second one:

```yaml
# Block 1 - correct because the both item list is indentation level is correct
name: devops
tools:
  - docker
  - kubernetes
```

```yaml
# Block 2 - broken because the both item list is incorrect in indentation level
name: devops
tools:
  - docker
    - kubernetes
```
