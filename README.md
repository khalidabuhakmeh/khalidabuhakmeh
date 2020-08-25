<a href="https://khalidabuhakmeh.com">
  <img width="100%" src="https://khalidabuhakmeh.com/assets/images/posts/logos/vhs-logo.png" alt="abuhakmeh logo" />
</a>

<hr/>

<div style="width:"100%; text-align:center">
    <a href="https://khalidabuhakmeh.com" style="display: inline-block; margin: auto; width: 50px;">
      <img src="https://user-images.githubusercontent.com/228256/87049842-71345b80-c1cb-11ea-8a67-b002bcfde0a8.png" alt="abuhakmeh blog" width="32%" />
    </a>
    <a href="https://twitter.com/buhakmeh" style="display: inline-block; margin: auto; width: 250px;">
      <img src="https://user-images.githubusercontent.com/228256/87049848-72658880-c1cb-11ea-9adb-0fc1eabe9052.png" alt="twitter @buhakmeh" width="32%" />
    </a>
    <a href="https://blog.jetbrains.com/dotnet" style="display: inline-block; margin: auto; width: 250px;">
      <img src="https://user-images.githubusercontent.com/228256/87049854-742f4c00-c1cb-11ea-891f-daea7cf57573.png" alt="jetbrains .NET Blog" width="32%" />
    </a>    
</div>

<hr/>

### Hi there 👋

My name is Khalid Abuhakmeh ([@buhakmeh][twitter] on Twitter). These days I'm a developer advocate at [JetBrains][jetbrains] primarily focusing on [.NET][dotnet] technologies using tools like [Rider][rider] and [ReSharper][resharper]. I also create content for JetBrains at the [official blog][jb_blog].

I'm also proud of my personal blog at [khalidabuhakmeh.com][blog], where I write mostly about .NET. My posts focus on learning in the open and try to help readers solve problems or grasp ideas. I'm on a tech journey and I hope you come along with me on the ride.

Some of my favorite posts include:

- [Conditionally Apply LINQ Clauses](https://khalidabuhakmeh.com/conditionally-apply-linq-clauses)
- [Use Neo4J to Find The Shortest Path](https://khalidabuhakmeh.com/use-neo4j-to-find-the-shortest-path)
- [Writing .NET Database Integration Tests](https://khalidabuhakmeh.com/dotnet-database-integration-tests)
- [SQL Polling Listener for Azure SQL and Other SQL Databases](https://khalidabuhakmeh.com/sql-polling-listener-for-azure-sql-and-other-sql-databases)

### 📙 Blog Posts

<!--START_SECTION:feed-->
- **[Dispose Resources Asynchronously With IAsyncDisposable](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;iasyncdisposable-dispose-resources-asynchronously)**
- **[My Favorite JetBrains Rider Themes](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;my-favorite-jetbrains-rider-themes)**
- **[Use Select Dropdown In ASP.NET Razor](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;use-select-dropdown-in-aspdotnet-razor)**
- **[Building HTML with C#](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;building-html-with-csharp)**
- **[Secrets of a .NET Professional](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;secrets-of-a-dotnet-professional)**
- **[Use C# Preprocessor Directives](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;csharp-preprocessor-directives)**
- **[Handle HTTP Status Codes With Razor Pages](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;handle-http-status-codes-with-razor-pages)**
- **[More HTTP Methods In ASP.NET Core HTML Forms](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;more-http-methods-aspnet-core-html-forms)**
- **[Sentiment Analysis With C#, ML.NET, and Oakton Commands](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;sentiment-analysis-with-csharp-mldotnet-and-oakton-commands)**
- **[Write Xamarin.Mac Apps With JetBrains Rider](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;write-xamarin-mac-apps-with-jetbrains-rider)**
<!--END_SECTION:feed-->quot;&lt;h1&gt;{Content}&lt;&#x2F;h1&gt;&quot;);
        }
    }
}


We get the same expected output.

&lt;html&gt;&lt;h1&gt;Hello, World!&lt;&#x2F;h1&gt;&lt;&#x2F;html&gt;


Conclusion

Using the classes of HtmlContentBuilder, HtmlString, and HtmlFormattableString give us the ability to build and manipulate HTML structures in C#. It’s important that these classes are meant to build new HTML structures, and cannot parse existing HTML. Leveraging these classes, we could build a custom DSL that lets us write HTML using C# syntax while getting all the benefits of encoding.

Please let me know what you think by leaving a comment below.*
#### [Secrets of a .NET Professional](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;secrets-of-a-dotnet-professional) 
*Working as a .NET Professional is a tumultuous rollercoaster ride of emotional highs and crushing lows. It’s likely the same for other communities, with different flavors of success and failures. I have over a decade of .NET development work, and I am here to share some general mantras that have served me well. I hate to call it adivce because all advice is context-specific. Readers should take any general counsel with a healthy dose of skepticism and critical thinking. That said, I hope folks reading this will find value through the prism of my experiences.

In this post, we’ll go through technical and non-technical ideas that have helped me through some of my toughest projects.



Strings, Strings, Strings

At its lowest level, we can boil programming to  1’s and 0’s. A few levels up from that is our human abstraction comprising mainly of string values. The string class in .NET is arguably the most used in all of .NET.  While we can utilize it like a primitive type, it does not behave like a primitive during runtime. Most memory leaks I’ve encountered were due to the creation of unique strings. We can see this happen in some of the following situations:


  Logging with custom messages
  HTTP request building
  Exception messages with custom messages
  String concatenation (using + instead of StringBuilder)
  ORM SQL generation out of control


The CLR will store string constants in an in-memory data structure called the intern pool.


  The common language runtime conserves string storage by maintaining a table, called the intern pool, that contains a single reference to each unique literal string declared or created programmatically in your program. –Microsoft


The behavior usually is something we want in our applications, but we need to be mindful of it as we hit any meaningful scale. It’s a good idea to follow these guidelines:


  Use StringBuilder when building large strings.
  Use structured logging techniques and avoid “custom messages.”
  Use string constants when possible.


If we’re experiencing memory leaks, it is a good idea to look through our dependencies to see what could be generating strings. Here is a past experience with a leak in Windows Azure Application Insights.

Decompiling Dependencies

Adding a dependency to a project is a hopeful act. We hope that the package will solve our problems with little or no effort. Often that’s the case, but sometimes it is a complete disaster. A decade ago, it would have been a frustrating experience, not understanding what the heck is happening.

With today’s tools, we can step into our dependencies to understand the underlying behavior, especially in those situations where it’s all gone wrong. Productivity tools like ReSharper and Rider have helped millions of developers better understand their dependencies. Visual Studio has also added this feature recently. Debugging into a third-party package can help expedite a solution, allowing developers to solve their issues.

There are cases where the decompilation loses some clarity. In these cases, when possible, we can go to the authors’ GitHub repository. Reading more code, especially other folks’ code, makes us better developers.

          Reading more code makes us better developers. Tweet To Share   

Nothing Is Free

The following statement may seem obvious on face value, but many developers still fall into this trap.

          Nothing is free, and it always costs us something. Tweet To Share   

In the case of third-party packages, they are abstracting away a problem for our convenience. Let’s take an absurd example of an ultra-convenient abstraction.

public static void Main() {
  DoEverything();
}


How expensive should we expect DoEverything to be? Is it making any network calls? Is it bound to I&#x2F;O? Let’s take a less absurd example using Entity Framework.

var results &#x3D; db.Products.ToList();


Most developers would expect this to “work,” and it might work for a bit. Then the product table grows from 100 to 100,000, and now we’re in trouble. Abstractions are convenient, but we still have to understand the power we are wielding in many cases. Pausing and asking simple questions can save a developer, team, and company days of debugging under pressure.

Task.Awesome

Asynchronous programming is pretty commonplace in the .NET ecosystem. We’ve recently been able to make everything async, and that’s awesome for everyone.

public class Program
{
    public async Task Main(string[] args)
    {
        await CreateHostBuilder(args).Build().RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) &#x3D;&gt;
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder &#x3D;&gt; { webBuilder.UseStartup&lt;Startup&gt;(); });
}


It’s essential to at least have a shallow under the class at the center of all of it, and that’s Task. The asynchronous programming model in .NET is a state machine where every call to await creates a new step in state-machine. For the most part, the async&#x2F;await pair work, but understanding how the abstraction works helps us make decisions around cancellation tokens and proper edge-case handling.

Learn LINQ

The Language Integrated Query language is an elegant addition to any .NET developers toolkit. I could wax poetic about all the ways I love LINQ, but that is time wasted not learning LINQ.

Take It Easy On The Patterns

Impostor syndrome is a difficult struggle for many of us. It’s easy to fall into the trap of adding more abstractions into our codebase to compensate for our internal conflicts. We should consider each pattern appropriately and understand what it brings to a project and what burden it may introduce on ourselves and team members. In the majority of my experience, adding more abstractions was a way to delay the need to solve the real problems facing us.

One Project Is Fine*

Likely, the most controversial opinion on the list, but commonly, one project (excluding a test project), is fine for many solutions. .NET allows us to have a mind-boggling number of projects in a solution. Many of us see that as a challenge to pump our solutions with as many assemblies as humanly possible. Starting with one project gets us out of practicing Project Feng Shui and back to solving the problem at hand. Projects make sense for teams delivering assemblies, less so for teams iterating on a service.

*Developers should use their judgment to make the ultimate decision.

Integration Tests &gt; Unit Tests

The systems we build have many complex external dependencies. The hubris of thinking we could mock away decades of investment in a database, web service, or any other technology has been the source of many frustrating arguments. In my opinion, one good integration test is worth 1,000 unit tests. Here is a previous post describing how to write database integration tests. With the rise of containerization, the pain of writing integration tests has never been lower.

          one good integration test is worth 1,000 unit tests Tweet To Share   

Unit tests still have their place in our testing insfrastructure, but in my experience, tests that rely heavily on mocking external behaviors will erode and become unmanageable over time.

Use Tools Other Than .NET

I love what .NET allows developers to accomplish, and with its ability to be cross-platform, its never been a better time to be a .NET developer. The power of .NET may tempt us into bending everything we do towards our preferences, but that means we’re closing out a world of wonderful possibilities.

By exploring other ecosystems, we might find more fitting solutions to our problems. The JavaScript community has many great projects to offer, and so does the Ruby and Python communities. We limit ourselves when we only look in one place for the solutions to our problems.

This blog is written primarily using Jekyll and Ruby, and I’ve written a few posts to help folks looking to get into blogging.

People &gt; Technology

Technology can help us overcome many technical hurdles, but many of the problems teams face are not technological: vision, communication, vision, and planning. A cohesive team with dated technology will outperform a dysfunctional team with the latest tech every time. Nothing beats working with folks we respect and that we can have spirited and productive debates with for the ultimate goal of building cool things.

          A cohesive team with dated technology will outperform a dysfunctional team with the latest tech every time Tweet To Share   

What’s The Community Think

I mentioned that I was writing this post to the broader community, and the response was overwhelmingly positive.

I&#39;m writing a post titled &quot;Secrets of a .NET Professional&quot;. What are some of your greatest &quot;secrets&quot; that you would share with the world?&amp;mdash; Space Cowboy Khalid 👨‍🚀 (@buhakmeh) May 27, 2020


I suggest reading the tweets if you’re interested in adding to the conversation or seeing what other folks think. Here are some of my favorites.

Greatest secret: consult the documentation on @docsmsft! You&#39;ll find a lot of useful information. If something&#39;s missing or incorrect, provide feedback and&#x2F;or submit changes to help fix it :)&amp;mdash; Shahed Chowdhuri @ Microsoft (@shahedC) May 27, 2020

Understand what your code for data access does when it runs on the database engine. Don&#39;t assume it will just work and be performant. Learn to debug in those places that go beyond .NET.&amp;mdash; Chris Woodruff &quot;Woody&quot; 🏠🥃🍺 (@cwoodruff) May 27, 2020

The big secret is that everyone just googles everything. So just writing &quot;Google&quot; is going to make it a really short post.&amp;mdash; Rachel Appel (@RachelAppel) May 27, 2020

1. You don&#39;t need to follow every trend, especially if you only hear it from MS.2. Spend some time in other ecosystems and tool chains. This way you know what you have in .NET and what you&#39;re missing out on.3. Don&#39;t worry about whether you&#39;re professional. Just do your thing.&amp;mdash; Immo Landwerth (@terrajobst) May 27, 2020

First rule. Just because you can, doesn&#39;t mean that you should. No language or framework can protect you from dumb mistakes. Second, learn what the dumb mistakes are for a variety of languages and platforms. Third, learn what immutable means and watch those string concatenations.&amp;mdash; Tyler Jensen (@tyler6502) May 28, 2020


There are more 💎gems in the thread, and definitely worth a read and follow.

Conclusion

Being a professional is a moving target at best, and we are all trying to do our best. Approaching our careers with humility and a passion for learning and getting better will put us in a position to advance towards our goals. We need to understand that the hurdles we’ll face on our journey will be a mixture of technical and non-technical and that finding balance is critical for our success. The technical advice I gave will likely change over time, but the non-technical information is timeless. Ultimately, we’ve chosen a profession designed to help folks solve their problems. To do that, we first need to understand and solve our problems.

I offer office hours above if you’re interested in having a chat or debating any points in this post.

Thank You.*
#### [Use C# Preprocessor Directives](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;csharp-preprocessor-directives) 
*Run configurations are one of those things that many .NET developers take for granted. Whenever we start a new project, we have two basic configurations: Debug and Release. For most of us, that’s more than enough to deliver a capable production application. For those of us targeting multiple platforms, we may need a configuration for each destination platform.

In this post, we’ll walk through the basics of C# preprocessor directives. We’ll use keywords like define, if, else, elif, and undef to change our application’s behavior. We’ll also look at some of the predefined preprocessor directives that come with the .NET Framework. Finally, we’ll see how we can alter the preprocessor directives on our run configurations.



What is a Preprocessor Directive

A preprocessor directive gives us the ability to define blocks of code that only get compiled if we meet criteria. Previously we mentioned that most C# projects start with two different run configurations of Debug and Release. If we look at the preprocessor directives defined for each run configuration, we’ll see they differ.


  
    
      Run Configuration
      Preprocessor Directives
    
  
  
    
      Debug
      DEBUG, TRACE
    
    
      Release
      TRACE
    
  


The Debug run configuration has two preprocessors, while Release only has one preprocessor. In Debug, we can define additional code blocks that increase our ability to diagnose and fix issues.

internal class Program
{
    public static void Main(string[] args)
    {
    #if DEBUG
        Console.WriteLine(&quot;Before the hello, World!&quot;);
    #endif
        
        Console.WriteLine(&quot;Hello, World!&quot;);
    }
}


How To Use Preprocessor Directives

There are several ways to introduce preprocessor directives into our projects.


  Predefined by the .NET Framework
  Run Configuration Compile Constants
  Using the define keyword in code
  Using the -define option with the compiler.


Predefined Preprocessor Directives

C# has many predefined preprocessor symbols representing all the available target frameworks found in our development environment. These preprocessor directives can help us transition legacy code to newer platforms, or target future platforms while maintaining our codebase for the present. Here is a list of known predefined directives, and we can assume the .NET team will continue the pattern for future iterations of SDK releases.


  
    
      Target Frameworks
      Preprocessor Directives
    
  
  
    
      .NET Framework
      NETFRAMEWORK, NET20, NET35, NET40, NET45,NET451, NET452, NET46, NET461, NET462, NET47, NET471, NET472, NET48
    
    
      .NET Standard
      NETSTANDARD, NETSTANDARD1_0, NETSTANDARD1_1,NETSTANDARD1_2, NETSTANDARD1_3, NETSTANDARD1_4, NETSTANDARD1_5,NETSTANDARD1_6, NETSTANDARD2_0, NETSTANDARD2_1
    
    
      .NET Core
      NETCOREAPP, NETCOREAPP1_0, NETCOREAPP1_1, NETCOREAPP2_0, NETCOREAPP2_1, NETCOREAPP2_2, NETCOREAPP3_0, NETCOREAPP3_1
    
  


Other development platforms may also introduce a set of specific preprocessor directives. For example, the Unity Game Engine presents an abundant list of preprocessor directives for each target platform from Android, iOS, WebGL, and more.

Run Configuration Constants

One of the most straightforward approaches to adding additional compile constants is by adding them to a specific run configuration. We can modify our run configuration by right-clicking the properties of our project in our IDE. Here is a screenshot from Rider.



We can add additional preprocessor directives or remove them from our compilation.

Using the Define Keyword In Code

We can leverage the define keyword to create preprocessor directives inside of our C# files. There are caveats to this approach.


  The symbol must appear at the top of the file before any instructions.
  The new symbol does not conflict with another preprocessor directive.
  The scope of the symbol is the file in which we define it.


Let’s have a look at this use case:

#define Hello
using System;

namespace Changes
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            #if Hello
            Console.WriteLine(&quot;Hello, World!&quot;);
            #endif
        }
    }
}


Using the Define Compiler Option

Instead of modifying our run configuration, we can also make compile-time decisions about our preprocessor directives. When calling the C# compiler, we can pass in our directives using the -define option.

&gt; csc HelloWorld.cs -define:DEBUG;Hello


This approach works, but it is a little less convenient than modifying our run configurations. This approach may be useful in continuous integration environments that may be looking at environment variables. That said, C# projects support multiple run configurations, and that approach may be more deterministic.

Code Example

We can think of preprocessor directives as boolean values, and so does C#. Because of their logical nature, we get to use boolean operators to make preprocessor decisions. Take the following example.

#define FIZZ
#define BUZZ

using System;

namespace Changes
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            #if (FIZZ &amp;&amp; BUZZ)
                Console.WriteLine(&quot;FizzBuzz&quot;);
            #elif (FIZZ)
                Console.WriteLine(&quot;Fizz&quot;);
            #elif (BUZZ)
                Console.WriteLine(&quot;FizzBuzz&quot;);
            #else
                Console.WriteLine(&quot;Hello, World!&quot;);
            #endif
        }
    }
}


We can use keywords like if, elif, and endif to create a logical structure for compilation. We can also use operators like and (&amp;&amp;), or (||), and not (!) to create intricate scenarios.

Not commonly used, but we can also use the undef keyword to unregister a preprocessor directive.

#define FIZZ
#define BUZZ
#undef FIZZ
#undef BUZZ

using System;

namespace Changes
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            #if (FIZZ &amp;&amp; BUZZ)
                Console.WriteLine(&quot;FizzBuzz&quot;);
            #elif (FIZZ)
                Console.WriteLine(&quot;Fizz&quot;);
            #elif (BUZZ)
                Console.WriteLine(&quot;FizzBuzz&quot;);
            #else
                Console.WriteLine(&quot;Hello, World!&quot;);
            #endif
        }
    }
}


In the above example, its as if our symbols of FIZZ and BUZZ never existed. The undef keyword may be helpful to opt file out of a directive for debugging purposes.

Conclusion

Preprocessor directives are a powerful tool for removing code blocks from our compilation step. It can help us target multiple platforms and share a majority of our code between different audiences. Development platforms like Xamarin, Mono, and even .NET itself have used preprocessor directives to a high degree of success. The most recommended approach is to alter our project run configurations, but we have many options, as we can see. Finally, while this approach is powerful, many developers should consider a strategy outside of compilation, like feature flags, if they intend to toggle behavior during runtime.*
#### [Handle HTTP Status Codes With Razor Pages](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;handle-http-status-codes-with-razor-pages) 
*HTTP semantics might not be vital to us humans, but they are the essential glue that holds the internet together. HTTP status codes allow clients to interpret the response they are about to receive and make processing accommodations. We may all be familiar with 200 OK, but there is a stable of other status codes that can indicate redirects, missing content, and catastrophic events.

In this post, we’ll see how we can use a middleware to process status codes respectfully so that both humans and our web clients get the necessary information necessary to take the next step.



Error Page

When we create a new Razor Pages project, we immediately have an Error page created. Let’s take a look at the implementation.

[ResponseCache(Duration &#x3D; 0, Location &#x3D; ResponseCacheLocation.None, NoStore &#x3D; true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId &#x3D;&gt; !string.IsNullOrEmpty(RequestId);

    private readonly ILogger&lt;ErrorModel&gt; _logger;

    public ErrorModel(ILogger&lt;ErrorModel&gt; logger)
    {
        _logger &#x3D; logger;
    }

    public void OnGet()
    {
        RequestId &#x3D; Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}


When we run into the error, we see the following page.



Let’s say we go to edit a resource, only to find it does not exist. Most applications will return a 404 (Not Found) result. Let’s see how to do that in our Razor Pages OnGet method.

&#x2F;&#x2F; the route is &#x2F;{id:int}
public IActionResult OnGet(int id)
{
    var result &#x3D; GetResource(id);
    if (result &#x3D;&#x3D; null)
    {
        return NotFound();
    }

    return Page();
}


There is a problem. When we run our application, w