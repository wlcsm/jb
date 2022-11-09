# jb - simple job manager

I have a problem where I want to run commands in the background and be able view the status and output from anywhere. Kind of like `jobs` but not bound to a specific process and a little friendlier.

My personal use cases include:

* run `godoc` on Go projects to view the documentation
* File watcher for hot updating in Svelete development
* Long running (but eventually ending) operations e.g. system backups

`jb` is a simple script to manage background jobs. It does only four things:

1. Run jobs in background
2. Print logs
3. Lists all recorded jobs
4. Clear stopped jobs from record

## Example

I'm going to start on a new Go project, so I want to be able to view the documentation for the standard libraries in the browser with `godoc`.

I first use `jb list` to check if the `godoc` documentation server is still running from last time.

```
$ jb list
JOB ID  STATUS  PID     COMMAND
1       STOPPED 8370    godoc
```

Seems like it has stopped since my computer has rebooted since then.
So lets start it again

```
$ jb run godoc
2
```

`jb` returns the job id so that could be used in scripts.
We can check it is indeed running with

```
$ jb list
JOB ID  STATUS  PID     COMMAND
1       STOPPED 8370    godoc
2       RUNNING 969753  godoc
```

Though we can see that our previous command is still displayed there. We can clear it with `jb clear`

```
$ jb clear
$ jb list
JOB ID  STATUS  PID     COMMAND
2       RUNNING 969753  godoc
```

I can then inspect the output of the command by providing its job id to `jb log`

```
~ $ jb log 2
using module mode; GOMOD=/dev/null
```

If I wanted to kill the process, I would simply issue a `kill` command with the PID given by `jb list`

```
$ kill 969753
$ jb list
JOB ID  STATUS  PID     COMMAND
2       STOPPED 969753  godoc
```

we can see that indeed the process has stopped

