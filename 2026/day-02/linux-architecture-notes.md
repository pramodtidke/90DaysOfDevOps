# Day 02 – Linux Architecture, Processes, and systemd

## Task:- Completed

1.  The core components of Linux (kernel, user space, init/systemd)?

         Answer: The hardware(CPU, RAM, etc) is managed by the kernel, via shell(UI/CLI) to run applications.
         we have the ASK Model (Application, Shell, Kernel) in linux OS

         Kernel:-
         Kernel is the heart of the linux OS.
         The Core of Linux OS that interact directly with hardware, prefered memory management, process, etc.
         It Acts as bridge between hardware and Applications.

         Shell: User Interface(CLI or GUI) that sits beetween the user and kernel interpreter user command and pass them to kernel.

         Applications: Upper level programm that runs on to top of shell, kernel rely on the system call to request hardware resourcess.

         UserSpace:-
         User space is where everyday life happens. This is where your applications run — your browser, text editor, terminal commands like bash and ls, and background services. Programs here can do useful work, but they don’t have free access to everything. They operate with limited permissions so that if something goes wrong, it doesn’t bring the whole system down.

         Kernel space, on the other hand, is the city’s control center. It’s where the core of the operating system lives and where hardware is managed. Code running here has full access to memory, CPU, and devices — so it has to be extremely careful and tightly controlled.

         This separation exists for one simple reason: safety.
         If an app crashes in user space, the system stays alive. If anything corrupts kernel space, the entire system can crash. Keeping these two worlds apart is what makes Linux stable, secure, and reliable.

         init / systemd:-

         systemd is the most widely used init system in modern Linux.
         It is the first user-space process that runs (process ID 1), and from that moment on, it becomes the manager of the entire system.

         In simple terms:

         👉 systemd decides what starts, when it starts, and how it’s monitored.

2.  How processes are created and managed

         A process is the programm that is currently running.

    how process are created & Managed:-

         In Linux, a process is a running instance of a program.

         When a user runs a command, the shell creates a new process by calling fork(), which creates a copy of itself.

         The child process then uses exec() to replace its memory with the new program. Each process is assigned a unique Process ID (PID).

         The Linux kernel scheduler manages processes by allocating CPU time using scheduling algorithms,
         and processes can be controlled using signals like SIGTERM and SIGKILL. Tools like ps, top, and systemctl help monitor and manage them.

3.  What systemd does and why it matters

         systemd is the init system and service manager in modern Linux distributions. It is the first process started by the kernel (PID 1) during boot and is responsible for initializing the system. It starts and manages system services, handles dependencies between services, monitors them, and restarts them if they fail. It also manages logging, devices, mounts, timers, and system states.”

    Why it matters:

         systemd improves boot performance by starting services in parallel, ensures better reliability by automatically restarting failed services, and provides a unified way to manage system processes using tools like systemctl.
