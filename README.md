# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
```
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <stdlib.h>

int main()
{
    int fd[2];
    pid_t pid;
    char write_msg[] = "Hello from Parent Process";
    char read_msg[100];

    // Create pipe
    if(pipe(fd) == -1)
    {
        printf("Pipe failed\n");
        return 1;
    }

    // Create child process
    pid = fork();

    if(pid < 0)
    {
        printf("Fork failed\n");
        return 1;
    }

    // Parent Process
    if(pid > 0)
    {
        close(fd[0]); // Close read end

        write(fd[1], write_msg, strlen(write_msg)+1);

        close(fd[1]); // Close write end
    }

    // Child Process
    else
    {
        close(fd[1]); // Close write end

        read(fd[0], read_msg, sizeof(read_msg));

        printf("Child received message: %s\n", read_msg);

        close(fd[0]); // Close read end
    }

    return 0;
}
```


## OUTPUT
<img width="592" height="465" alt="image" src="https://github.com/user-attachments/assets/9eeb1645-86be-48a9-8795-a27ea675af11" />


## C Program that illustrate communication between two process using named pipes using Linux API system calls

```
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

int main()
{
    int fd;
    char *myfifo = "/tmp/myfifo";
    char msg[] = "Hello from Writer Process";

    mkfifo(myfifo, 0666);

    fd = open(myfifo, O_WRONLY);

    write(fd, msg, strlen(msg) + 1);

    printf("Message Sent\n");

    close(fd);

    return 0;
}
```


## OUTPUT
<img width="530" height="387" alt="image" src="https://github.com/user-attachments/assets/91dc1d12-ccd1-450e-afe5-1f03fd36aebf" />


# RESULT:
The program is executed successfully.
