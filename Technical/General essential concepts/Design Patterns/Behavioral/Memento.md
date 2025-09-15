# Definition

Allows an object to capture and store its internal state at a particular moment in time, so it can be restored later without exposing its internal implementation.

>[!IMPORTANT]
This is a valuable when requiring an undo feature.

>[!WARNING]
As the past changes start to grow, we will have to keep track of all past fields for an object.

# Key roles

## Originator
Object whose state is required to be saved and stored.
```csharp
// ----- Originator -----
public class Editor
{
    private string _content;

    public void Type(string words)
    {
        _content += words;
    }

    public string GetContent()
    {
        return _content;
    }

    public EditorMemento Save()
    {
        return new EditorMemento(_content);
    }

    public void Restore(EditorMemento memento)
    {
        _content = memento.Content;
    }
}
```
## Memento
Object that stores internal state of the [[Memento#Originator|originator]].
``` csharp
// ----- Memento -----
public class EditorMemento
{
    public string Content { get; }

    public EditorMemento(string content)
    {
        Content = content;
    }
}
```
## Caretaker
Manager object that requests saves/restores, but does not modify [[Memento#Memento|memento]]. 

```csharp
// ----- Caretaker -----
public class History
{
    private readonly Stack<EditorMemento> _history = new();

    public void Save(Editor editor)
    {
        _history.Push(editor.Save());
    }

    public void Undo(Editor editor)
    {
        if (_history.Count > 0)
        {
            var memento = _history.Pop();
            editor.Restore(memento);
        }
    }
}
```

# Demo

```csharp
// ----- Usage -----
class Program
{
    static void Main()
    {
        var editor = new Editor();
        var history = new History();

        editor.Type("Hello ");
        history.Save(editor);

        editor.Type("World!");
        history.Save(editor);

        Console.WriteLine(editor.GetContent()); // Output: Hello World!

        history.Undo(editor);
        Console.WriteLine(editor.GetContent()); // Output: Hello 

        history.Undo(editor);
        Console.WriteLine(editor.GetContent()); // Output: (empty)
    }
}
```