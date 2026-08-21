# Definition

This is a runtime component in charge of **identifying** and **freeing** memory from unused managed resources -- It helps to prevent memory leaks by doing this.

# Personal definition

Garbage collector is a background process which runs alongside our .NET applications, this process ensures the **sweet spot** for memory usage required for the current processes being run.

GC would try to deallocate as much as possible those resources that are not longer being used for the current scope. 

>[!important]
>Re-gaining those spaces of memory depends on the traceability of references or values in the current scope.
## Key concepts

- **Root**: This refers to variables on the stack.
# Generations

There are different phases or generations for the garbage collector depending on the time that manage resources will live in an application. 

Those are:
- **Gen 0 (Marking phase):** short-lived objects (local variables, temp objects).
- **Gen 1:** intermediate between short-lived and long-lived objects.
- **Gen 2:** long-lived objects (static data, caches, singletons).

>[!INFO]
>Younger generations are collected more often since it's faster.
# Finalizers

This is a special method in .NET which is **called before and object is destroyed**. 

It's whole use is to release unmanaged resources (like database connections, COM components, etc.)

It has a destructor like syntax -> `~MethodName() { //dispose unmanage resources logic }`
