# About

🔖 My curated list of useful resources for **dotnet** development.

----

## 🏆 Great NuGet packages

Noteworthy packages to consider picking up as a developer.

### OneOf

[This package](https://www.nuget.org/packages/OneOf) enables you to write clean and reliable code. It ships with the ``Match`` method that is used to map outcomes.

Some of the benefits:

* Method signature describes all method outcomes:

    ````csharp
    OneOf<User, InvalidName, NameTaken> CreateUser(string username)
    ````

* Consumer must always handle all outcomes:

    ````csharp
    [HttpPost]
    public IActionResult Register(string username)
    {
        OneOf<User, InvalidName, NameTaken> createUserResult = CreateUser(username);
        return createUserResult.Match(
            user => new RedirectResult("/dashboard"),
            invalidName => {
                ModelState.AddModelError(nameof(username), $"Sorry, that is not a valid username.");
                return View("Register");
            },
            nameTaken => {
                ModelState.AddModelError(nameof(username), "Sorry, that name is already in use.");
                return View("Register");
            }
        );
    }
    ````
----

### MediatR

[MediatR](https://www.nuget.org/packages/MediatR/) is another tool that can be used to write clear and efficient code.

It is the most popular C# implementation [of the mediator](https://en.wikipedia.org/wiki/Mediator_pattern) pattern.

It supports request/response, commands, queries, notifications and events.

I've used only used it in the Request/Response paradigm. I find that it makes the code much cleaner and easier to understand. It also helps massively with testing.

You can create *Handlers* for commands as follows:

````csharp
public class PingHandler : IRequestHandler<Ping, string>
{
    public Task<string> Handle(Ping request, CancellationToken cancellationToken)
    {
        return Task.FromResult("Pong");
    }
}
````

You can then issue commands to the **mediator** and have them handled as follows:

````csharp
var response = await mediator.Send(new Ping());
Debug.WriteLine(response); // "Pong"
````
----
### FluentAssertions

We all love code that *ANYBODY* could understand, and [FluentAssertions](https://www.nuget.org/packages/FluentAssertions/) definitely helps with that!

It lets you write assertion statements in human readable format. This is usually the first package I install when creating a new project. A **must have** for your testing side.

To get an idea of how this looks like consider the following: 

````csharp
string actual = "ABCDEFGHI";
actual.Should().StartWith("AB").And.EndWith("HI").And.Contain("EF").And.HaveLength(9);

IEnumerable numbers = new[] { 1, 2, 3 };

numbers.Should().OnlyContain(n => n > 0);
numbers.Should().HaveCount(4, "because we thought we put four items in the collection");
````



