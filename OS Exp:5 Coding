#include<stdio.h>
#include<stdlib.h>
#include<sys/shm.h>
#include<sys/ipc.h>
#include<sys/types.h>
#include<sys/wait.h>
#include<unistd.h>

int main()
{
    int child, shmid, i;
    char *shmptr;

    child = fork();

    if (child > 0)   // Parent process
    {
        shmid = shmget(2041, 32, 0666 | IPC_CREAT);
        shmptr = shmat(shmid, 0, 0);

        printf("\n PARENT PROCESS WRITING \n");
        for(i = 0; i < 10; i++)
        {
            shmptr[i] = 'a' + i;
            putchar(shmptr[i]);
        }

        shmptr[10] = '\0';   // Null terminate

        printf("\n\n %s", shmptr);

        wait(NULL);          // Wait for child

        shmdt(shmptr);       // Detach
    }
    else   // Child process
    {
        sleep(1);   // Ensure parent writes first

        shmid = shmget(2041, 32, 0666);
        shmptr = shmat(shmid, 0, 0);

        printf("\n CHILD PROCESS READING \n");
        for(i = 0; i < 10; i++)
            putchar(shmptr[i]);

        shmdt(shmptr);                 // Correct detach
        shmctl(shmid, IPC_RMID, NULL); // Remove shared memory
    }

    return 0;
}
