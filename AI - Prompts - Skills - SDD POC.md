# Goal
Create a template for extraction to generalize a set of already setup steps (read sprocs to generate code in C# in .NET 8 with best practices) and on top of that to add structured logging, and be able to create a CURL of the specified sproc to test it in Postman

# Topics to include in AI automation

- SKILLS -> How viable and in which cases it would make sense to move the current step process into different skills (to reduce the context window)
- Agent orchestrator -> How to make sub-agents to execute specific tasks and report back to the orchestrator (which should have the architecture skill to act upon reports) which  should NEVER code, to avoid compression.
- SDD -> How can it be implemented into the current approach