# 50005Lab1
## TODO#4 Notes:
### Worker Revival Process
To look for a process to allocate a job, we loop through all the processes until we find a process that is `alive and task_status == 0` or if we find a process that is dead. If we find a process that is `alive and task_status == 0`, we will allocate the job to this process. Otherwise, we will count the number of processes which are busy or dead. If all the processes are either 1) alive and running a task; or 2) dead, one of the dead processes (if more than 1) will be revived and the job will be dispatched to the same buffer.
```
while (file still has lines to read) {
  // init not_done = 1 to run while loop until job has been allocated
  // init no_of_busy = 0 to count the number of processes checked
  while (not_done) {
    for each process {
      // init alive = waitpid(...)
      if (task_status == 0 && alive == 0) {
        // allocate job to process
        not_done = 0;
        break;
      } else if (status == 9) {
        if (no_of_busy >= number_of_processes) {
          // fork new children; child is added to children_processes, parent adds job to buffer
        } else {
          no_of_busy++;
        }
      } else {
        no_of_busy++;
      }
    }
  }
}
```

### Termination
To terminate, a while loop is used to keep running a chunk of code until all the processes have been terminated (ie `while (terminated_children < number of processes)`). Within the while loop, a for loop is used to loop through the different children processes. In the for loop, if the child process is `alive and task_status == 0`, send a termination signal and increment the `terminated_children` count; otherwise if the child is dead, simply increment the `terminated_children` count. This ensures that all children that are not already dead are killed.

```
// init terminated_children = 0
while (terminated_children < number_of_processes) {
  for (int i = 0; i<number_of_processes; i++) {
    // init alive = waitpid(...)
    if (task_status == 0 && alive == 0) {
      // send termination signal
      terminated_children++;
    } else if (status == 9) {
      terminated_children++;
    }
  }
}
```
