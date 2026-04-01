
## Folder Structure
Current path uner /Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch/Communications/ or obsidian vault: Kodama/Projects/patchwatch/Communications

`CLAUDE.md`: This file
`comm.md`: The main communication between user and Kodama(claude).
`Log.md`: When a topic is resolved, add a short summary and remove corresponding conversation in `comm.md`.
[otherfiles].md: Can be details of a single problem, will be referred by comm.md






## comm.md: The main communication between user and Claude.
### comm.md Format
- User writes requests or questions at the bottom of comm.md WITHOUT any prefix
- Kodama (Claude AI) always appends a response in this format:

```
[YYYY-MM-DD] Kodama: ...
```

- If a task is added to WorkLog/kodama-todo.md, confirm it:
```
[YYYY-MM-DD] Kodama: Added to kodama-todo.md — "-[ ]xxx"
```

- If a decision is made, confirm it and create/append to a doc under `../Frameworks/`
```
[YYYY-MM-DD] Kodama: Decision logged to Frameworks/xxx.md — "topic"
```



## Log.md

Summary Format:
Example:
```
[YYYY-MM-DD]Explained How to set up for deployment 
```


```
[YYYY-MM-DD]Change Request: xxxxx.  Result: fix in commit xxxx 
```



## Otherfiles.md
* Remove if