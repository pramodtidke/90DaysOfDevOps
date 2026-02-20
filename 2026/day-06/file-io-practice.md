# Day 06 – Linux Fundamentals: Read and Write Text Files

## Task: Completed

Today’s goal is to **practice basic file read/write** using only fundamental commands.

You create a small text file and completed practice:

- Creating a file
- Writing text to a file
- Appending new lines
- Reading the file back

      ubuntu@ip-172-31-18-13:~/practice$ touch note.txt
      ubuntu@ip-172-31-18-13:~/practice$ echo "Hello we are learning the devops"
      Hello we are learning the devops
      ubuntu@ip-172-31-18-13:~/practice$ cat note.txt
      ubuntu@ip-172-31-18-13:~/practice$ echo "Hello we are learning the devops" > note.txt
      ubuntu@ip-172-31-18-13:~/practice$ cat note.txt
      Hello we are learning the devops
      ubuntu@ip-172-31-18-13:~/practice$ vim note.txt
      ubuntu@ip-172-31-18-13:~/practice$ echo "This is day 6 practice session " >> note.txt
      ubuntu@ip-172-31-18-13:~/practice$ cat note.txt
      Hello we are learning the devops


      This is day 6 practice session
      ubuntu@ip-172-31-18-13:~/practice$ echo "we learn about devops from basics " >> note.txt
      ubuntu@ip-172-31-18-13:~/practice$ cat note.txt
      Hello we are learning the devops


      This is day 6 practice session
      we learn about devops from basics
      ubuntu@ip-172-31-18-13:~/practice$ vim note.txt
      ubuntu@ip-172-31-18-13:~/practice$ cat note.txt
      Hello we are learning the devops
      This is day 6 practice session
      we learn about devops from basics
      ubuntu@ip-172-31-18-13:~/practice$ ls
      note.txt
      ubuntu@ip-172-31-18-13:~/practice$ echo "hello i am pramod" | tee -a note.txt
      hello i am pramod
      ubuntu@ip-172-31-18-13:~/practice$ cat note.txt
      Hello we are learning the devops
      This is day 6 practice session
      we learn about devops from basics
      hello i am pramod
      ubuntu@ip-172-31-18-13:~/practice$
      ubuntu@ip-172-31-18-13:~/practice$ head -n 1 note.txt
      Hello we are learning the devops
      ubuntu@ip-172-31-18-13:~/practice$ tail -n 2 note.txt
      we learn about devops from basics
      hello i am pramod
      ubuntu@ip-172-31-18-13:~/practice$

we used this resources:

`touch` (create an empty file)

- `cat` (read full file)
- `head` and `tail` (read parts of a file)
- `tee` (write and display at the same time)
