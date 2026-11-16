>[!IMPORTANT] Any of the following:
>- Classes
>- Records
>- Objects
>- Delegates
>- Arrays

>[!IMPORTANT]
>Not all reference types are stored in the heap. There are exceptions in which a ref type inside a method passes the escape analysis and is marked as stack allocated by the JIT compiler.
>
>Sources: 
>- [.NET Performance improvements: Object Stack Allocation](https://devblogs.microsoft.com/dotnet/performance-improvements-in-net-10/#deabstraction:~:text=Object%20Stack%20Allocation)
>- [Microsoft Docs: Stack allocation](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10/runtime#stack-allocation)

# Indirection
Reference types would hold their ref in the [[Stack|stack]] while the values would be stored in the [[Heap|managed heap]].

>[!NOTE] Visual reference
>![[Reference Types-1.png]]