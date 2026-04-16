---
name: skill-hello
description: This is a sample skill that activates when the user says "hello"
metadata:
  version: 1.1.0
---

# skill-hello

This is a sample skill that activates when the user says "hello". It responds with a structured yaml response containing a greeting message in English, the current time retrieved from the system, and the version of the skill. The format must match the following schema:

```yaml
response:
  message: string
  time: string
  version: string
```
