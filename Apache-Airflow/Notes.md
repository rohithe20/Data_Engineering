








## How does Airflow work 

- "DAG" files are added to the "DAGS" directory (this directory is named differently depending on what airflow setup is chosen).
- By default, the Airflow scheduler scans the "DAGS" directory every 5 minutes. The scheduler then serializes the "DAG" into a metadata database so that the web server and other airflow components can access it.
- Scheduler checks if the "DAG" is ready to run. If so, a "DAG run" is created, an instance of a "DAG" at a specific time. A "DAG" run can have various states (note that the state here can be thought of as a property of the DAG run).
- Scheduler then checks to see if there are tasks to run. If so, a "task instance" is created for the task which is an instance of the task at a specific time. The task instances also have different states like:
  - none: The task has not yet been queued for execution(its dependencies are not yet met)
  - scheduled: The scheduler has determined the Task's dependencies are met and it should run
  - queued: The task has been assigned to an Executor and is awaiting a worker
  - running: The task is running on a Worker 
  - success: The task finished running without errors
  - restarting: The task was externally requested to restart when it was running
  - failed: The task had an error during execution and failed to run

- When the task instance is in the "scheduled" state, it is sent to the Executor. The Executor is responsible for defining how and on which system the task is to be executed. The Executor does NOT execute tasks. Rather, it pushes the task instance into a queue. The executor then updates the state of the task instance to "queued".
- When a Worker is available and the task instance is at the front of the queue, the task instance is polled from the queue and is executed on the worker which updates the state of the task instance to "running"



## Airflow CLI  

A non-exhaustive list of important airflow CLI commands can be found below: 

- airflow db check: checks if metadata database can be reached
- airflow db clean - moves old records in database tables to archive tables
- airflow db init - used to initialize the metadata database
- airflow dags backfill - allows user to run or rerun a dag for a specified date range
- airflow dags reserialize - reserializes data 
