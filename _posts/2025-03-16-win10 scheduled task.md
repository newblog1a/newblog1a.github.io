---
published: true
---
## What is the CreateExplorerShellUnelevatedTask scheduled task?

Explorer needs to run non-elevated because it needs to be available to other applications which aren’t necessarily elevated. To ensure that it can do that, Explorer detects that it is running elevated and de-elevates itself by using a scheduled task called Create­Explorer­Shell­Unelevated­Task which does what it says on the tin: It launches Explorer un-elevated. The elevated version of Explorer exits after asking the scheduled task to run.

So the elevated Explorer exits, and is replaced by an un-elevated Explorer.

A customer wanted to know why, when they looked at several of the servers they had deployed, some of them have the Create­Explorer­Shell­Unelevated­Task and some don’t. Does this mean that the systems which don’t have the special task have been corrupted or compromised?

No need to worry.

The Create­Explorer­Shell­Unelevated­Task is created on demand when Explorer is first launched elevated. If your system doesn’t have this task, then pat yourself on the back. It means that your system hasn’t ever needed it.

  https://devblogs.microsoft.com/oldnewthing/20220524-00/?p=106682