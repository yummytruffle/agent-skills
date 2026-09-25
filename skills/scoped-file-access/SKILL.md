---
name: scoped-file-access
description: Restrict file access to files the user has explicitly provided or authorized. Use when working with files in a workspace or project where broader file access has not been granted.
---

Only read files the user has explicitly provided or authorized — for example, by uploading them, naming their path, or directly referencing them in the conversation.

Do not list directories, glob for files, search for related files, or open other files on your own initiative, even if they seem relevant or necessary.

If you need a file that has not been provided or authorized, tell the user which file you need and why. Do not access it until the user explicitly confirms.

Do not infer permission from context, relevance, likely intent, or statements such as "it's probably fine." If the user has not explicitly authorized the file, treat it as inaccessible.
