# Introduction

When it comes to operating a computer, it is essential for the user (or, in modern terms, the machine) to keep track of any ongoing processes. In the first part of the lab, I will be demonstrating the use of the process, or “ps”, command to show processes and their status in addition to the relationship between a parent and child process. Then, in the second part, I will be working through some examples of process scheduling to show the results of different schedulers and how some schedulers are better than others in some instances.

# Process Management Practice

## Installing the Program and Compiling It for Use on CentOS

The first thing I do in this lab is install the program “git” so that I can bring the necessary files into my virtual machine. To do so, I will once again be using “yum” with the command “yum install git”. After a brief prompt, the necessary files load up and git is installed on my machine. With git installed, the next step is to download the necessary code for the lab. To do so, I will be “cloning” the destination file onto my machine. As such, the command will be “git clone” in addition to the URL for the code folder. After typing the command out, the folder is successfully loaded into my “root” directory. Within the folder is the file “lab03.c” which is the file I need. Also located in the folder is an extraneous “stack” file. However, the lab file is only source code for the program. In order to run it on Linux, I must first compile the code into a useable program. This will be done with Linux’s local C compiler, GCC. Using yum again, I install the compiler. Next, with the command “gcc lab03.c -o lab03”, I compile the program. The “-o” option sets a name for the compiled program. In this case, the name will be “lab03” (Geeks for Geeks). With this in place, I use “cd” to change the current directory to the “IS-3033-Downloads” folder. After listing the files within it, I notice that my desired program is printed in green text. This means that the file is an executable program. Therefore, the compilation was a success.

## Running the Program and Discussing the Relationships between a Parent and Child Process

With the program ready to run, I type in the command “./lab03”. The “./” can be thought of as a pointer to the current directory. This is necessary to run programs that are not located in Linux’s “bin” or “sbin” folders. From there, the program runs, and the output is displayed. Using “ctrl + z”, I push the command to the background. Next, I list the processes in the terminal with “ps -l” which will print all running processes in the terminal along with names and labels (Tech on the Net). In doing so, I can verify that the PID of the parent and child processes match those that are mentioned in the program. With the information verified, I am ready to test the relations of the parent and child processes.

### Investigation 1

After using the command “kill -9 3553”, the parent process of the program is killed. The “-9” option is the “SIGKILL” signal that the “kill” command sends to kill the program. As a result, the child process is also killed. As the following figure will show, in the first process list, there are four processes. In the second, there are two processes. This means that when the parent process was killed, so too was the child. This is the expected result of killing a parent process.

- _Figure 1_: Steps 1-4 of the lab in addition to the current investigation. Notice how after entering the “kill” command for parent process 3553, the parent and child process disappear from the list when it is called forth again.

<p align="center">
  <img width="608" height="375" src="assets/fig1.png">
</p>

### Investigation 2

For this investigation, running the program again indicates that the PIDs for the parent and child processes have changed. Nevertheless, entering “kill -9 3575” (3575 being the PID for the child process), keeps the parent process alive, but labels the child process as “defunct” despite still being listed with the other processes. This means that the child process has become a zombie process which implies that the parent process has failed to recycle the “body” of the child process.

- _Figure 2_: At the halfway point, this section of the lab is executed. The program is called again, and the command is passed to kill the child process. Notice how in the final process list, process 3575, the child process, is labeled as “defunct.” This means that the process has become a zombie.

<p align="center">
  <img width="608" height="375" src="assets/fig2.png">
</p>

### Investigation 3

Leaving the processes as they are, I enter the command ‘ps -u”. This command will list all the ongoing processes under a specific user (CyberITHub). I suspect that the lack of a specific username after the option implies the current user. Going down by the columns in the bottom table of Figure 3:

| Column Label        | Definition         |
|:-------------|:------------------|
| PID           | The process ID number. The parent process is 3575, and the child zombie process is 3575. |
| %CPU | The amount of CPU usage from each program. Notice how the zombie program is still taking up processing space.|
| %MEM           | The amount of memory being used. Neither process appears to be taking up memory.      |
| VSZ           | “Virtual Memory Size.” The size of memory that Linux has given to a process (LinuxConfig). This does not necessarily mean that all this memory will be used. The parent process has a fair amount of memory allocated to it whereas the child zombie process has no memory available. |
| RSS           | “Resident Set Size.” The size of memory that a process has used to load all its pages (LinuxConfig). Once again, the parent process is using a decent amount of memory whereas the child zombie process is not using any memory. |
| TTY           | Prints the name of the terminal I am using (How-To Geek). In this case, the processes are taking place on terminal 1. |
| STAT           | Shows the statuses of processes. In this case, the “T” of the parent process means that the process has been stopped (moved to the background), and the “Z” of the child process identifies it as a zombie (Die.net). |
| START           | The start time of the processes. The times for the parent and child processes are the same.  |
| TIME           | The amount of time the CPU has used for a process (Die.net). Both the parent and child zombie process take one second. |
| COMMAND           | The command that commences the process. The parent process is labeled with the right command, “./lab03”, but the child zombie process states that the command has to do with lab03 but is defunct. |

- _Figure 3_: The bottom table shows the output for “ps -u”. Notice how this process list is different from the previous ones. Notice how the child process, despite being defunct, is still taking up resources on the processor. Each column regarding the parent and child processes is covered in-depth above.

<p align="center">
  <img width="608" height="375" src="assets/fig3.png">
</p>

# Process Scheduling

This section is a hands-on demonstration of process scheduling. Given information about each process' burst times, I will create a Gantt chart for each process list, explain their completion order, and calculate the total turnaround times and average turnaround times.


| Process        | Burst Time (in ms)         |
|:-------------|:------------------|
| P1           | 3 |
| P2           | 11   |
| P3           | 16      |
| P4           | 6 |
| P5           | 2 |

The processes all arrive at the same time (that is, Arrival Time = 0).

### FIFO

First, we will use FIFO (First in, Last out) scheduling. Since the processes all arrived at the same time, they will be executed in a random order.

![Problem1A](assets/p1a.png)

Turnaround Time = Completion Time - Arrival Time

P1: 3 - 0 = 3

P2: 14 - 0 = 14

P3: 30 - 0 = 30

P4: 36 - 0 = 36

P5: 38 - 0 = 38

- Total Turnaround Time = 121 ms
- Average Turnaround Time = 24.2 ms

### SJF

Now we want to use SJF (Shortest Job First) scheduling. Therefore, the order of the processes depends on their time to completion. In this case, the order will be P5, P1, P4, P2, P3.

![Problem1B](assets/p1b.png)

Turnaround Time = Completion Time - Arrival Time

P1: 5 - 0 = 5

P2: 22 - 0 = 22

P3: 38 - 0 = 38

P4: 11 - 0 = 11

P5: 2 - 0 = 2

- Total Turnaround Time = 78 ms
- Average Turnaround Time = 15.6 ms

### Discussion

As the data shows, the SJF scheduler will have a faster turnaround time than FIFO. Since the process order of FIFO will be totally random, it is likely that the largest jobs could come first and take up processing time. Whereas SJF will complete the shortest processes first and leave the longer ones for last. This means that SJF will have a shorter total turnaround time and average turnaround time.

# Conlcusion

This lab did well to show how exactly a parent process can affect a child process. Moreover, it shows the creation of a zombie process and how this can take up resources on a computer despite no longer working. This highlights the importance of good programming. Had the program been properly coded, the parent process would have recycled the “body” of the killed child process so that it would no longer use resources on the computer. Finally, I felt the example problems of process scheduling does well to show the strengths and weaknesses of different kinds of schedulers and that there is no such thing as a “perfect” scheduler. Overall, I felt this lab provided good insight regarding the intricacies and relations of computer processes.

# References

- <a href="https://www.cyberithub.com/ps-command-in-linux-unix-with-examples/#Example_3_How_to_show_all_the_processes_running_with_a_specific_userps_-u_in_LinuxUnix" target="_blank">31 Popular Ps Command in Linux/Unix with Examples (Monitor Linux Processes).</a> CyberITHub

- <a href="https://www.geeksforgeeks.org/gcc-command-in-linux-with-examples/" target="_blank">Gcc Command in Linux with Examples.</a> GeeksforGeeks

- <a href="https://www.howtogeek.com/428174/what-is-a-tty-on-linux-and-how-to-use-the-tty-command/" target="_blank">What Is a TTY on Linux? (and How to Use the Tty Command).</a> McKay, Dave

- <a href="https://linux.die.net/man/1/ps" target="_blank">Ps(1): Report Snapshot of Current Processes - Linux Man Page.</a> Ps(1) - Linux Man Page

- <a href="https://linuxconfig.org/ps-output-difference-between-vsz-vs-rss-memory-usage" target="_blank">Ps Output - Difference between VSZ vs RSS Memory Usage.</a> Reynolds, Luke. Linux Tutorials - Learn Linux Configuration

- <a href="https://www.techonthenet.com/unix/basic/ps.php" target="_blank">UNIX Basic Commands: Ps.</a> techonthenet

