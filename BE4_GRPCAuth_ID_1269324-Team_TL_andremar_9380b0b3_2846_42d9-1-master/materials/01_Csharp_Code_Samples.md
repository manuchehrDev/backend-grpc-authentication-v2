#### **RBAC**
Let's look at an example in C# of what access to methods based on roles might look like.

```csharp
[Authorize(Roles="Cashier")]
public ActionResult SellItem(...)
{
    ...
}

[Authorize(Roles="Customer")]
public ActionResult AddItemToShoppingCart(...)
{
    ...
}
```

#### **CBAC**

Let's look at an example in C#:

```csharp
[ClaimsAuthorize(Claims="Adult-older-than-18, Trust-worthy-client")]
public ActionResult RequestLoan(...)
{
    ...
}

[ClaimsAuthorize(Claims="Child")]
public ActionResult AddParent(...)
{
    ...
}
```

### Dependency Injection, IoC-containers
Let's look at an example of how dependency implementation works in C#.

First, we create an interface of some class and its implementation.

```csharp
public interface ISomeClass
{
    Task WriteMessage(string message);
}

public class SomeClass: ISomeClass
{
    private readonly ILogger<SomeClass> _logger;

    public SomeClass(ILogger<SomeClass> logger)
    {
        _logger = logger;
    }

    public Task WriteMessage(string message)
    {
        _logger.LogInformation("...");
        return Task.FromResult(0);
    }
}
```

Now let's register the dependency in the Program.cs (.net6+) file.

```csharp
...
builder.Services.AddTransient<ISomeClass, SomeClass>();
...
```

Finally, you can inject a dependency into the desired class.

```csharp
public class IndexModel : PageModel
{
    private readonly ISomeClass _someClass;

    public IndexModel(ISomeClass someClass)
    {
        _someClass = someClass;
    }

    public async Task OnGetAsync()
    {
        await _someClass.WriteMessage("Hooray!");
    }
}
```

Yay! We learned how to add dependencies without explicitly calling the object constructor.