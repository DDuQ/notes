There are a couple of ways to define a Value Object, any of the following can apply:

- Represents something in the domain and it's defined solely by its properties. 
- Combination of discrete properties. 
- Part of an entity.

Value Objects are **immutable**, every time that we retrieve an instance of a Value Object, we should be safe to say that ==its properties are unique and won’t change in the future==.