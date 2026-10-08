# Interactive Grader Pipelines

Interactive problems involve bidirectional communication between a contestant's program and an interactor daemon.

## Architecture

```
+---------------------+     Standard Out / Pipe 1     +---------------------+
|                     | ----------------------------> |                     |
|  Contestant Binary  |                               |  Interactor Binary  |
|                     | <---------------------------- |                     |
+---------------------+     Standard In / Pipe 2      +---------------------+
```

- **Pipe Duplexing:** Two synchronized anonymous UNIX pipes connect contestant `stdout` to interactor `stdin`, and interactor `stdout` to contestant `stdin`.
- **Verdict Determination:** The interactor determines the final exit code and diagnostic message.
