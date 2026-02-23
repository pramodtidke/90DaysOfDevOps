# Day 09 – Linux User & Group Management Challenge

## Task

- Create users and set passwords

  User Created Succesfully:

          takyo:x:1001:1001::/home/takyo:/bin/bash
          professor:x:1003:1003::/home/professor:/bin/bash
          berlinn:x:1004:1004::/home/berlinn:/bin/bash
          nairobi:x:1005:1007::/home/nairobi:/bin/bash

          set password and switch user in bash shell succesfully:

          ubuntu@ip-172-31-18-13:~$ su takyo
          Password:
          takyo@ip-172-31-18-13:/home/ubuntu$

- Create groups and assign users
  I have successfull create a group using a " sudo usergoup <group_name>" and " sudo usermod -aG <group-name> <user-name>"

        developers:x:1005:takyo,berlinn
        admins:x:1006:professor
        nairobi:x:1007:
        project-team:x:1008:nairobi,takyo

- Set up shared directories with group permissions

  I have succesfully setup shared directory with permission

      ubuntu@ip-172-31-18-13:~/opt$ ls -l
      total 8
      drwxr-xr-x 2 ubuntu developers   4096 Feb 23 06:25 dev-project
      drwxr-xr-x 2 ubuntu project-team 4096 Feb 23 06:36 team-workspace
      ubuntu@ip-172-31-18-13:~/opt$

Use what you learned from Days 1-7 to find the right commands!

---
