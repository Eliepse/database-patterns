# Tasks

Store tasks (or "jobs") that is planned, running or finished/failed.

## Structure

| name        | type        | nullable | description                                             |
| ----------- | ----------- | -------- | ------------------------------------------------------- |
| id          | int         | no       | Primary key                                             |
| type        | enum/string | no       | type of the task, allow to match the right task handler |
| payload     | json        | yes      | holds data useful for the handler to run the task       |
| started_at  | datetime    | yes      | When the task has started being handled                 |
| finished_at | datetime    | yes      | When the task has been successfully completed           |
| failed_at   | datetime    | yes      | When the task has failed                                |
| created_at  | datetime    | no       | When task has been created                              |
| created_by  | datetime    | yes      | Who created the task                                    |

## Features

- Addaptable to any task with type and payload
- Can be created from an operator, or from the system (`created_by IS NULL`)
- 'status' without a dedicated column:
  - pending: `started_at IS NULL`
  - running: `started_at IS NOT NULL AND finished_at IS NULL AND failed_at IS NULL`
  - done: `finished_at IS NOT NULL`
  - failed: `failed_at IS NOT NULL`
- Analytics that can be computed:
  - pending until being handled: `started_at - created_at`
  - processing time: `finished_at - started_at`
  - failure rate by type
  - processing duration by type
  - number of parrallel tasks being processed
  - queue size
