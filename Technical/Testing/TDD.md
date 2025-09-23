# Definition

Stands for **Test Driven Development** and it is a practice where tests are created first before coding features, in this way we prevent bugs by having a guide based on tests.

# How it works

It has a repetitive cycle: **Red -> Green -> Refactor**.

![[Excalidraw/TDD.excalidraw.md#^frame=nb2bl3I5ci6HyTewlUgx8|600]]
## Red
Write a failing test (since feature don't exists yet).
## Green
Write the minimum code necessary to make the test pass.
## Refactor
Clean up the code while keeping tests green.

>[!NOTE]
>In paper those are the essential factors for implementing TDD, but there is a missing key piece which allows us to keep in track for incoming tests for future functionalities.
>
>That's when `List Tests` come into the picture.

![[Excalidraw/TDD.excalidraw.md#^frame=uhAP1ct73JJp_2V6UrVqN|600]]

## List Tests
This is meant for the thinking phase; offload everything that comes to mind.

We're in the middle of writing code and ideas comes to mind to improve test cases or we notice there are edge cases that need to be covered, etc. 

In such cases to track those improvements it will clarify and provide guide on what to prioritize by writing those ideas (into a backlog for instance).