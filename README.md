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
#### [Dispose Resources Asynchronously With IAsyncDisposable](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;iasyncdisposable-dispose-resources-asynchronously) 
*It’s no secret that JetBrains Rider is my IDE of choice. The JetBrains team’s ability to stay current with language additions has helped me keep my C# knowledge relevant in a fast-paced world. It’s always exciting to see refactoring recommendations, and while it’s easy to hit Alt+Enter, I like to understand why I’m about to apply a change.

In this post, we’ll be looking at a relatively new keyword combination of await using and what it means for our codebase.



IDisposable

The IDisposable interface has been around since .NET Framework 1.0 and gives us the ability to release internal resources. Experienced .NET Developers are likely familiar with the disposing pattern.

using (var disposable &#x3D; new Disposable()) {
  &#x2F;&#x2F; do your thing here...
}


This interface is fundamental to .NET, with the documentation page for IDisposable listing over 500 implementations of the interface in the framework alone. The count doesn’t even include the use of IDisposable in the open-source ecosystem and commercial products.

There is a flaw with IDisposable, and it’s become more apparent with the introduction of Task. The freeing of resources is synchronous and can block threads. Even worse, incorrect disposable of an asynchronous resource can lead to dead-locks.

IAsyncDisposable

In .NET Core 3.0, we see the introduction of an IAsyncDisposable interface, which is currently derived by a handful of concepts as of writing this post: Microsoft.Extensions.DependencyInjection.ServiceProvider, System.Collections.Generic.IAsyncEnumerator&lt;T&gt;, System.IO.Stream, System.Threading.Timer,  and System.Text.Json.Utf8JsonWriter. The interface allows us to free up asynchronous resources by implementing a DisposeAsync method that returns a ValueTask.

public interface IAsyncDisposable
{
  ValueTask DisposeAsync();
}


Note, that IDisposable and IAsyncDisposable are two independent interfaces and can be implemented exclusively of each other. We can imagine that IAsyncDisposable does not implement IDisposable because it does not want to give developers the false impression that there is a synchronous option by default in all cases.

Using IAsyncDisposable In Our Code

Let’s write a simple example that will allow C# to use the await using construct. Our first step is to create a class that implements the IAsyncDisposable interface.

public class HotGarbage : IAsyncDisposable
{
    public async ValueTask DisposeAsync()
    {
        Console.WriteLine(&quot;What&#39;s that smell? *sniff sniff*&quot;);
        await Task.Delay(TimeSpan.FromSeconds(3));
        Console.WriteLine(&quot;Disposing of Hot Garbage&quot;);
    }

    public void Look()
    {
        Console.WriteLine(&quot;Hmmm...&quot;);
    }
}


Again, we should take note that our class only implements IAsyncDisposable, which means our class likely has dependencies that require asynchronous disposal.

Let’s use our new class.

class Program
{
    static async Task Main(string[] args)
    {
        async Task Run()
        {
            await using var hotGarbage &#x3D; new HotGarbage();
            hotGarbage.Look();                
        }

        await Run();
    }
}


We are using a local function to create a scope that ends at the end of our code block. The await using keywords are useful as we can reduce the number of braces in our code. Additionally, it reduces the likelihood that a refactor may introduce issues of incorrect disposal.

Running the code, we see the results of our asynchronous disposal.

Hmmm...
What&#39;s that smell? *sniff sniff*
Disposing of Hot Garbage


Conclusion

While our IDEs can make great suggestions, it is up to us to understand and implement those suggestions. The IAsyncDisposable interface should make it easier for folks writing libraries that deal with I&#x2F;O operations. Authors of database access libraries, image processing, and internet protocols should be able to dispose of managed resources gracefully without fear of dead-locks. For us consumers of libraries, we now get some sweet syntactic sugar in the await using keyword, further reducing our codebase’s visual footprint.

What do you think? Do you utilize the await using keywords combo in your code? Why or Why not? Please leave a comment below.*
#### [My Favorite JetBrains Rider Themes](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;my-favorite-jetbrains-rider-themes) 
*Integrated development environments (IDEs) are constant, logical, and efficient executors of our technological whims. They do what we tell them when we tell them (most of the time). That’s great because, as developers working within an ecosystem, we can rely upon our experience being the same as we jump from our work and home devices. Even better, the experience is the same when we pair with our favorite co-workers. JetBrains Rider prides itself on providing a consistent and industry-leading user experience, but don’t confuse consistency with being boring.

In this post, we’ll explore some of my favorite themes found in the JetBrains Plugin marketplace that allow us to express our individuality. These themes are in no particular order, and is not meant to be an exhaustive list.



XCode Dark



Regardless of your thoughts on XCode, what is undeniable is Apple’s ability to put together an aesthetically appealing product. XCode ships with a beautiful dark theme punctuated by blues, pinks, and oranges. It’s a stunning theme and is currently my favorite daily-driver IDE dressing.

Get XCode Dark here.

Dracula Theme



Rider ships with a Dracula theme, but there is also an especially appealing version in the plugins marketplace. A grey theme with very vibrant colors reminiscent of a lite brite toy set. Colors like green, blue, pink, and purple pop, but are easy on the eyes. The border highlights around the basic Rider chrome add an extra layer of appeal that is super fun.

Get Dracula Theme here.

Trash Panda



Trash Panda is a theme for our inner-hacker. The color scheme is cyberpunk AF with an “I mean business” dark theme. Developers will love the blue highlighted icons, and the muted colors found in the text editor. Also, let’s face it; it is fun to say Trash Panda.

Get Trash Panda here.

Nord



A theme that’s beautifully muted and chill, yet somehow aggressive, you won’t be able to help but feel like a Viking. The color scheme reminds me of glaciers, fog, moss, and an eeriness only found near the poles of the earth. The dark theme is easy on the eyes and provides the right balance between contrast and style.

Get Nord here.

Hiberbee



Hiberbee lives up to the name, with a visual aesthetic that positively stings the senses. Another dark theme with colors that remind you of fall next to a meadow: blues, oranges, yellows, and greens. Hiberbee sets the text on a black background with a greyish shell.

Get Hiberbee here

Lotus Light



Lotus is a zen-like programming experience, with subtle hues of pink. The theme also comes with support for Rainbow braces and brackets to help us keep track of formatting. It’s a cute programming experience that is executed professionally by the creator.

Get Lotus theme here.

Doki Theme : KilLaKill Ryoku



Doki Theme is an anime-inspired theme set, with a library of containing a theme and sticker set for popular anime characters. My favorite is Ryoku from KillLaKill. The color is so anime it hurts, with a sharp grey background and awesome shades of red. Also, there are toggles to turn off an on the anime stickers, but who would want to do that? If you’re into Kawaii, this is the only theme you need.

Get Doki theme here.

Helsing



Helsing is a dark theme designed to work in direct sunlight. A disclaimer for all vampire developers out there, this may not be the theme for you. The color scheme is a grey shell, with text having shades of white, grey, and light green. Catch some sun rays, compile some code, and save humanity from Dracula himself.

Get Helsing here.

Metal Gear Solid V5



Feel like Solid Snake himself as you code your way through a Metal Gear inspired IDE experience. Blacks, reds, and muted browns give this theme a real military aesthetic. Sneak up on those bugs in your cardboard box camouflage.


  Fortunately, coding is one of those things, that gets easier the more you do it. – Solid Snake (maybe)


Get MGSV5 here.

P.S. The Konami code doesn’t work here… or does it?

Godot Theme



Godot is a powerful game engine that has a beautiful in-engine editor. This theme takes the Godot aesthetic and makes it a first-class Rider experience. A mix of muted pascal colors and vibrant pops, make this a visually appealing coding experience. Easily in my top three themes, I find myself falling in love with it every time I start up Rider. For Godot game engine developers, this is a must have as it makes context switching between Godot and Rider seamless.

Get the Godot theme here.

Honorable Mention : Defaults



Rider ships with many great default themes like Rider (Dark&#x2F;Light), Rider Melon (Dark&#x2F;Light), ReSharper (Dark&#x2F;Light), and Visual Studio themes. These themes offer a stable and reliable experience right out of the box. The Rider themes also use JetBrains’ own Mono font, which is just beautiful.

Get Rider here.

Conclusion

We don’t have to compromise between choosing the best IDE, and our need to express our individuality. JetBrains Rider has some fantastic themes in the plugin marketplace, and this list barely scratches the surface. On top of the visual enhancements, there is a broad set of plugins that allow us to fine-tune or code experience.

Let me know if any of these themes are those you use, send me a tweet with a screenshot of your Rider IDE, and leave a comment for any themes not featured on this list.*
#### [Use Select Dropdown In ASP.NET Razor](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;use-select-dropdown-in-aspdotnet-razor) 
*ASP.NET’s Razor engine comes with an assortment of HTML tag helpers. Some of the helpers are straightforward, while others may be challenging for folks to grasp. One of the more powerful HTML elements is select, allowing us to select a value from a set of options.

In this post, we’ll see how we can structure our ASP.NET models and views to get the most out of the select HTML element.



Getting Started

In most cases, we’ll have a model that has a property that can contain a single value for a property. The values for this property may come from an enum or a list of string values. In this case, let’s look at an Order class which has an OrderStatus property named Status.

public class Order
{
    public int Id { get; set; }
    public DateTime OrderedAt { get; set; }
    public string Username { get; set; }
    public OrderStatus Status { get; set; } &#x3D; OrderStatus.New;
}

public enum OrderStatus
{
    New,
    Pending,
    Shipped,
    Delivered
}


The Order class is a typical model we may have in our application’s domain model. Let’s move on to setting up our ViewModel.

ViewModel and PageModel

Let’s clarify what a ViewModel is for those of us not familiar with the concept. A ViewModel is a surrogate between our backend services and the view in our application. These C# classes provide commands, values, and valuable context for our views. When using MVC, we need to create our own ViewModels, but when using Razor Pages, we can utilize the PageModel. In this example, we’ll be using Razor Pages.

Our PageModel will have a single Order instance that we’ll hardcode into our class for the sake of this example.

public class IndexModel : PageModel
{
    static Order State { get; } &#x3D; new Order
    {
        Id &#x3D; 1,
        Username &#x3D; &quot;Khalid.Abuhakmeh&quot;,
        OrderedAt &#x3D; DateTime.Now,
        Status &#x3D; OrderStatus.New
    };
    
    public Order Current { get; set; }
    
    public void OnGet()
    {
        Current &#x3D; State;
    }
}


When the page loads on a GET request, we will pass the state of our order to our Order property. Let’s move on to adding the options of statuses to our page model.

SelectListItem

The HTML select tag helpers for Razor use an abstraction model called SelectListItem to create a collection of suitable values. In our page model, we’ll be creating a collection of order statuses.

For reusability, I’ve created this static helper to work with the Enum types found in our example.

public static class SelectList
{
    public static List&lt;SelectListItem&gt; Create&lt;TEnum&gt;
        (bool includeEmptyOption &#x3D; false)
    {
        var type &#x3D; typeof(TEnum);
        if (!type.IsEnum)
        {
            throw new ArgumentException(
                &quot;type must be an enum&quot;,
                nameof(type)
            );
        }

        var result &#x3D;
            Enum
                .GetValues(type)
                .Cast&lt;TEnum&gt;()
                .Select(v &#x3D;&gt; 
                    new SelectListItem(
                    v.ToString(),
                    v.ToString()
                    )
                )
                .ToList();

        if (includeEmptyOption)
        {
            &#x2F;&#x2F; Insert the empty option
            &#x2F;&#x2F; at the beginning
            result.Insert(
                0, 
                new SelectListItem(&quot;Pick An Option&quot;, &quot;&quot;)
            );
        }

        return result;
    }
}


We can now add the collection of SelectListItem to our page model. We will also need to add the BindProperty attribute to our Current property to allow us to post and bind values to it.

public class IndexModel : PageModel
{
    static Order State { get; } &#x3D; new Order
    {
        Id &#x3D; 1,
        Username &#x3D; &quot;Khalid.Abuhakmeh&quot;,
        OrderedAt &#x3D; DateTime.Now,
        Status &#x3D; OrderStatus.New
    };
    
    [BindProperty] public Order Current { get; set; }
    public List&lt;SelectListItem&gt; Statuses { get; } 
        &#x3D; SelectList.Create&lt;OrderStatus&gt;();
    
    public void OnGet()
    {
        Current &#x3D; State;
    }
}


In the next section, we’ll see how we utilize our page model to bind to the Razor HTML tag helpers.

HTML &amp; Razor

In this section, we’ll be using some tag helpers to create a select dropdown with our known OrderStatus values.

&lt;form action&#x3D;&quot;&quot; method&#x3D;&quot;post&quot;&gt;
    &lt;label asp-for&#x3D;&quot;Current.Id&quot;&gt;
        Order #@Model.Current.Id (@Model.Current.OrderedAt.ToShortDateString())
        &lt;input type&#x3D;&quot;hidden&quot; asp-for&#x3D;&quot;Current.Id&quot; &#x2F;&gt;
        &lt;input type&#x3D;&quot;hidden&quot; asp-for&#x3D;&quot;Current.OrderedAt&quot; &#x2F;&gt;
    &lt;&#x2F;label&gt;
    &lt;label asp-for&#x3D;&quot;Current.Username&quot;&gt;
        &lt;input type&#x3D;&quot;hidden&quot; asp-for&#x3D;&quot;Current.Username&quot;&#x2F;&gt;
    &lt;&#x2F;label&gt;
    
    &lt;label asp-for&#x3D;&quot;Current.Status&quot;&gt;
    &lt;&#x2F;label&gt;
    &lt;select asp-for&#x3D;&quot;Current.Status&quot; asp-items&#x3D;&quot;Model.Statuses&quot;&gt;
    &lt;&#x2F;select&gt;
    &lt;button type&#x3D;&quot;submit&quot;&gt;Save Order&lt;&#x2F;button&gt;
&lt;&#x2F;form&gt;


We’ll also need to add an OnPost method to our Razor Page. Our updated page model will look like this.

public class IndexModel : PageModel
{
    private static Order State { get; } &#x3D; new Order
    {
        Id &#x3D; 1,
        Username &#x3D; &quot;Khalid.Abuhakmeh&quot;,
        OrderedAt &#x3D; DateTime.Now,
        Status &#x3D; OrderStatus.New
    };

    [BindProperty] public Order Current { get; set; }
    public List&lt;SelectListItem&gt; Statuses { get; } 
        &#x3D; SelectList.Create&lt;OrderStatus&gt;();

    public void OnGet()
    {
        Current &#x3D; State;
    }

    public void OnPost()
    {
        State.Status &#x3D; Current.Status;
        Current &#x3D; State;
    }
}


Now we have a functioning select HTML element that updates the state of our Current order on the backend. Cool! The form works but could use some design flourishes. Here it is in action on the front end.



And our breakpoint shows we’re getting our status value from the form.



Gotchas

Here are a few mistakes some folks may run into and may want to keep in mind when working with these tag helpers.


  Do NOT add any additional HTML tags inside your select HTML element. ASP.NET will not function because you are overriding its natural behavior.
  SelectListItem has more properties, and we can utilize them to make for more user-friendly UIs.
  Remember to use BindProperty, or your Razor Page will not function.
  name attributes are important. Without them, your data will not make it to the server.
  The enum type might not be structurally sufficient to provide us with the metadata we want, like display names, groups, etc.
  Our form needs a submit input type, or nothing will happen when we click our button.


Conclusion

HTML forms are not magic; they work in predictable ways. Razor syntax builds on top of the predictable approaches designed into the HTML specification. By understanding how HTML works, we can shape our backend code to provide the most straightforward and cleanest approach needed to build our Razor views. I hope this post was helpful and please leave a comment or questions below.*
#### [Building HTML with C#](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;building-html-with-csharp) 
*Hypertext Markup Language is the first programming language many of us learn. Like chess, it’s easy to learn but can take a lifetime to master. Its declarative syntax is ideal and ubiquitous across many different ecosystems, and C# embraces it with ASP.NET. In this post, we’ll investigateHtmlContentBuilder, look at its API, and consider a potential DSL built on top of the builder.



What Is HtmlContentBuilder

Building HTML in code has always been tricky. How do we distinguish a tag from an intended literal? Improper handling of user input can lead to incorrect syntax, and at its worst, it could lead to security vulnerabilities. Let’s take a look at an example string.

&lt;h1&gt;Hello, Developers!&lt;&#x2F;h1&gt;


Experienced developers will understand that we have a string value inside of an h1 tag. What about the following example?

&lt;code&gt;&lt;h1&gt;Hello, Code!&lt;&#x2F;h1&gt;&lt;&#x2F;code&gt;

The problem starts to get a little more difficult because in this case, everything inside of our code tag should have been encoded.

&lt;code&gt;&amp;lt;h1&amp;gt;Hello, World!&amp;lt;&#x2F;h1&amp;gt;&lt;&#x2F;code&gt;


ASP.NET  has introduced a set of classes that allow us to work with HTML content in C#. These classes include HtmlContentBuilder, HtmlString,  HtmlFormattableString. All of them implement the IHtmlContent interface. The most interesting class is HtmlContentBuilder, which gives us the ability to work with HTML structures.

The HtmlContentBuilder class allows us to Append, Clear, CopyTo, MoveTo, and WriteTo efficiently.

Appending

HtmlContentBuilder provides multiple Append methods. They primarily differ in whether they encode the content passed into them.

IHtmlContentBuilder Append(string unencoded)
IHtmlContentBuilder AppendHtml(IHtmlContent htmlContent)
IHtmlContentBuilder AppendHtml(string encoded)


It’s important to know that appending to the builder is not finalized immediately. Let’s take a look at the internal implementation of Append.

public IHtmlContentBuilder Append(string unencoded)
{
    if (!string.IsNullOrEmpty(unencoded))
    {
        Entries.Add(unencoded);
    }
    return this;
}


HtmlContentBuilder stores values in an Entries collection. Storage of entries will become clearer when we look at Move and Copy functions.

Usage of Append methods is straightforward, with more fine-grained functionality coming in the form of extension methods found in HtmlContentBuilderExtensions.

var builder &#x3D; new HtmlContentBuilder();
&#x2F;&#x2F; AppendFormat is from HtmlContentBuilderExtensions
builder.AppendFormat(&quot;&lt;html&gt;&lt;h1&gt;{0}&lt;&#x2F;h1&gt;&lt;&#x2F;html&gt;&quot;, &quot;Hello, Khalid!&quot;);


Moving Elements

Entries in an HtmlContentBuilder instance need to be some flavor of IHtmlContent.  The storage of these entries allows us to move elements in and out of HTML containers.

public void MoveTo(IHtmlContentBuilder destination)
{
    if (destination &#x3D;&#x3D; null)
    {
        throw new ArgumentNullException(nameof(destination));
    }

    for (var i &#x3D; 0; i &lt; Entries.Count; i++)
    {
        var entry &#x3D; Entries[i];

        string entryAsString;
        IHtmlContentContainer entryAsContainer;
        if ((entryAsString &#x3D; entry as string) !&#x3D; null)
        {
            destination.Append(entryAsString);
        }
        else if ((entryAsContainer &#x3D; entry as IHtmlContentContainer) !&#x3D; null)
        {
            &#x2F;&#x2F; Since we&#39;re moving, do a deep flatten.
            entryAsContainer.MoveTo(destination);
        }
        else
        {
            &#x2F;&#x2F; Only string, IHtmlContent values can be added to the buffer.
            destination.AppendHtml((IHtmlContent)entry);
        }
    }

    Entries.Clear();
}


Copying Elements

Copying is a necessary function for any templating approach. Looking at the implementation, we can see how a copy happens.

public void CopyTo(IHtmlContentBuilder destination)
{
    if (destination &#x3D;&#x3D; null)
    {
        throw new ArgumentNullException(nameof(destination));
    }

    for (var i &#x3D; 0; i &lt; Entries.Count; i++)
    {
        var entry &#x3D; Entries[i];

        string entryAsString;
        IHtmlContentContainer entryAsContainer;
        if ((entryAsString &#x3D; entry as string)  !&#x3D; null)
        {
            destination.Append(entryAsString);
        }
        else if ((entryAsContainer &#x3D; entry as IHtmlContentContainer) !&#x3D; null)
        {
            &#x2F;&#x2F; Since we&#39;re copying, do a deep flatten.
            entryAsContainer.CopyTo(destination);
        }
        else
        {
            &#x2F;&#x2F; Only string, IHtmlContent values can be added to the buffer.
            destination.AppendHtml((IHtmlContent)entry);
        }
    }
}


Using HtmlContentBuilder

For all intents and purposes, HtmlContentBuilder is an internal implementation detail for ASP.NET MVC, Razor Pages, and Blazor. We likely will interact with HtmlString more often than HtmlContentBuilder. That doesn’t mean we are restricted to using this class in an ASP.NET application exclusively. Here is an example in a console application.

using Microsoft.AspNetCore.Html;
using static System.Console;
using static System.Text.Encodings.Web.HtmlEncoder;

namespace ConsoleApp16
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder &#x3D; new HtmlContentBuilder();
            builder.AppendFormat(&quot;&lt;html&gt;&lt;h1&gt;{0}&lt;&#x2F;h1&gt;&lt;&#x2F;html&gt;&quot;, &quot;Hello, Khalid!&quot;);
            builder.WriteTo(Out, Default);
        }
    }
}


To get our results, we need a TextWriter instance and the encoder for our HTML. Running the WriteTo method, we see the expected output of HTML.

&lt;html&gt;&lt;h1&gt;Hello, Khalid!&lt;&#x2F;h1&gt;&lt;&#x2F;html&gt;


Not that exciting, but what we can do is wrap HTML elements in a custom domain-specific language (DSL). Here is the same output but with the HTML encapsulated in classes.

using System.IO;
using System.Text.Encodings.Web;
using Microsoft.AspNetCore.Html;
using static System.Console;
using static System.Text.Encodings.Web.HtmlEncoder;

namespace ConsoleApp16
{
    class Program
    {
        static void Main(string[] args)
        {
            new Html(
                new H1(&quot;Hello, World!&quot;)
            )
            .Write(Out, Default);
        }
    }

    public class Html
    {
        private readonly H1 h1;

        public Html(H1 h1)
        {
            this.h1 &#x3D; h1;
        }

        public void Write(TextWriter writer, HtmlEncoder encoder)
        {
            var builder &#x3D; new HtmlContentBuilder();
            builder.AppendFormat(&quot;&lt;html&gt;{0}&lt;&#x2F;html&gt;&quot;, h1.ToString());
            builder.WriteTo(writer, encoder);
        }
    }

    public class H1
    {
        public H1(string content)
        {
            Content &#x3D; content;
        }

        private string Content { get; set; }

        public IHtmlContent ToString()
        {
            return new HtmlString(<!--START_SECTION:feed-->
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


There is a problem. When we run our application, we get the default browser error page.



What’s the deal?

StatusCodePagesMiddleware

Well, it turns out we need an additional middleware known as the StatusCodePagesMiddleware.  This middleware gives us the ability to either redirect or execute a route in our web application. Let’s take a look at the internal implementation of this middleware.

var statusCodeFeature &#x3D; new StatusCodePagesFeature();
context.Features.Set&lt;IStatusCodePagesFeature&gt;(statusCodeFeature);

await _next(context);

if (!statusCodeFeature.Enabled)
{
    &#x2F;&#x2F; Check if the feature is still available because other middleware (such as a web API written in MVC) could
    &#x2F;&#x2F; have disabled the feature to prevent HTML status code responses from showing up to an API client.
    return;
}

&#x2F;&#x2F; Do nothing if a response body has already been provided.
if (context.Response.HasStarted
    || context.Response.StatusCode &lt; 400
    || context.Response.StatusCode &gt;&#x3D; 600
    || context.Response.ContentLength.HasValue
    || !string.IsNullOrEmpty(context.Response.ContentType))
{
    return;
}

var statusCodeContext &#x3D; new StatusCodeContext(context, _options, _next);
await _options.HandleAsync(statusCodeContext);


We can see that any status code above 400 and below 600 will cause the middleware to jump into action. Additionally, we can turn the feature off for any reason. In the code comments, its mentioned that it might be essential to turn this feature off for API based responses.

To add this middleware, we need to call UseStatusCodePagesWithReExecutein our Startup.Configure method.

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler(&quot;&#x2F;Error&quot;);
        &#x2F;&#x2F; The default HSTS value is 30 days. You may want to change this for production scenarios, see https:&#x2F;&#x2F;aka.ms&#x2F;aspnetcore-hsts.
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();

    &#x2F;&#x2F; *** Here is our middleware registration ***
    app.UseStatusCodePagesWithReExecute(&quot;&#x2F;Error&quot;);

    app.UseRouting();

    app.UseAuthorization();

    app.UseEndpoints(endpoints &#x3D;&gt; { endpoints.MapRazorPages(); });
}


Now, when we run our previous NotFound request, we’ll see our error page.



Take note, to preserve the URI but still return a response to the client, we want to use ReExecute and not Redirect.

Conclusion

While Razor Pages allows our OnGet and OnPost methods to be void, we should consider HTTP semantics and always return an IActionResult. The different return type allows us to handle error modes more gracefully, and leverage our pre-packaged Error page. The attention to detail should keep both humans and our robotic overlords happy.

I hope you found this post helpful, and please leave a comment.*
#### [More HTTP Methods In ASP.NET Core HTML Forms](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;more-http-methods-aspnet-core-html-forms) 
*Many developers are familiar with your most common HTTP methods of GET and POST. Many of us do not realize that there is a world of other HTTP methods that can we can utilize for additional semantic meaning for our web requests. HTTP methods include GET, PUT, POST, DELETE, HEAD, OPTIONS, TRACE, PATCH, and CONNECT. We can use HTTP methods to include additional endpoints in our applications without bloating the URI interface we provide our users.

In this post, we’ll see how we can leverage the spectrum of HTTP methods in our ASP.NET Core MVC applications. We’ll be enhancing our HTML forms to support all possible HTTP methods.



HttpMethodOverrideMiddleware

All HTML forms (&lt;form&gt;) have an action attribute, with only two valid values: GET and POST. The restricted action values limit the possible ways our front end can communicate with our back end. Luckily for us, ASP.NET Core ships with HttpMethodOverrideMiddleware, which allows our application to route a GET or a POST to an ASP.NET Core endpoint that has a different HTTP method.

For example, let’s take an endpoint with an HttpDelete attribute.

[HttpDelete, Route(&quot;{id:int}&quot;)]
public async Task&lt;IActionResult&gt; Delete(int id)
{
    var script &#x3D; await db.Scripts.FindAsync(id);
    
    if (script !&#x3D; null)
    {
        db.Scripts.Remove(script);
        await db.SaveChangesAsync();
    }

    return RedirectToPage(&quot;&#x2F;Index&quot;);
}


Technically, an HTML form could never call this endpoint, since the DELETE method is not supported. To get it working, we’ll need to register the HttpMethodOverrideMiddleware int our Startup class.

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler(&quot;&#x2F;Error&quot;);
        app.UseHsts();
    }

    &#x2F;&#x2F; allow the frontend to call with more HTTP methods
    app.UseHttpMethodOverride(new HttpMethodOverrideOptions
    {
        FormFieldName &#x3D; 
            HtmlHelperExtensions.HttpMethodOverrideFormName
    });
    
    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseAuthorization();

    app.UseEndpoints(endpoints &#x3D;&gt;
    {
        endpoints.MapControllers();
        endpoints.MapRazorPages();
    });
}


Note, that the middleware requires an HttpMethodOverrideOptions instance that sets the FormFieldName. We’ll use the name later in our extension method. First, let’s look at the implementation HttpMethodOverrideMiddleware.

&#x2F;&#x2F; HttpMethodOverrideMiddleware
public async Task Invoke(HttpContext context)
{
    if (string.Equals(context.Request.Method, &quot;POST&quot;, StringComparison.OrdinalIgnoreCase))
    {
        if (_options.FormFieldName !&#x3D; null)
        {
            if (context.Request.HasFormContentType)
            {
                var form &#x3D; await context.Request.ReadFormAsync();
                var methodType &#x3D; form[_options.FormFieldName];
                if (!string.IsNullOrEmpty(methodType))
                {
                    context.Request.Method &#x3D; methodType;
                }
            }
        }
        else
        {
            var xHttpMethodOverrideValue &#x3D; context.Request.Headers[xHttpMethodOverride];
            if (!string.IsNullOrEmpty(xHttpMethodOverrideValue))
            {
                context.Request.Method &#x3D; xHttpMethodOverrideValue;
            }
        }
    }
    await _next(context);
}


We notice that the middleware will only use our form name when the request meets two criteria:


  The HTTP request has an original HTTP Method of POST.
  That we set the form name in the registration of the middleware.
  The HTTP request has a form content type, which most POST requests do.


If our request does not meet those criteria, then the middleware will fallback to looking for a request header of X-Http-Method-Override.

HtmlHelper Extension For Overrides

To take advantage of the middleware, we need to create an HTML helper extension.

using Microsoft.AspNetCore.Mvc.Rendering;
using Microsoft.AspNetCore.Html;
public static class HtmlHelperExtensions
{
    public const string HttpMethodOverrideFormName &#x3D; &quot;X-HTTP-Method-Override&quot;;
    
    public static IHtmlContent HttpMethodOverride(
        this IHtmlHelper helper,
        System.Net.Http.HttpMethod method, 
        string name &#x3D; HttpMethodOverrideFormName)
    {
        return helper.Raw(<!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;&lt;input type&#x3D;\&quot;hidden\&quot; name&#x3D;\&quot;{name}\&quot; value&#x3D;\&quot;{method}\&quot; &#x2F;&gt;&quot;);
    }
}


Our Razor views can take advantage of this extension method.

&lt;form method&#x3D;&quot;post&quot; 
    action&#x3D;&quot;@Url.Action(&quot;Delete&quot;, &quot;Scripts&quot;, new {@script.Id})&quot;&gt;
    @Html.HttpMethodOverride(HttpMethod.Delete)
    &lt;button type&#x3D;&quot;submit&quot;&gt;Delete&lt;&#x2F;button&gt;
&lt;&#x2F;form&gt;


Now, whenever the UI submits the HTML form, we will hit our ASP.NET Core controller action from previously. Our extension method creates a hidden input with the HTTP method we want ASP.NET Core to handle when routing our request. We use the System.Net.HttpMethods class to restrict our parameter choices.

Conclusion

HTML semantics are essential, and HTTP methods allow us to express intent without jamming more information into our requests. With the HttpMethodOverrideMiddleware we can use all the HTTP methods available to us with ease.

I hope this post is helpful, and please leave a comment.*
#### [Sentiment Analysis With C#, ML.NET, and Oakton Commands](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;sentiment-analysis-with-csharp-mldotnet-and-oakton-commands) 
*Machine learning is a hot topic in modern development. We can find the fruits of data science labors in our digital personal assistants, streaming recommendations, fraud detection, and cancer research. The application of machine learning is boundless and can bring new solutions to long-standing issues. For .NET developers, its never been easier to take advantage of machine learning with the arrival of ML.NET.

In this post, we’ll break down one of the ML.NET samples into its two critical components: Training and Prediction. We’ll be building a console application that allows us to train and store a model. The prediction component will allow us to recall a trained model and predict the classification of new data. Let’s get started!



Machine Learning Basics

For those with limited or no experience with machine learning, we can think of it as a mechanism to categorize any dataset. When we look at data, each data point will have features and labels. A feature is an attribute of the current data instance. For example, we may describe a dog as an animal with four legs. The animal’s four legs are a feature. A label is a known fact of the data instance that categorizes it. For example, in our previous case, we label the data instance as a dog.

Using features and labels, we can build a dataset of known instances. We use this dataset to create a Model. We train the model with a certain percentage of our dataset while withholding a certain portion to test and verify our new model’s accuracy. Once we have a model, we can use it to predict the categorization of any further input with a certain probability. The more training data we have, the more accurate our predictions will be.

There are various types of models we can train:


  Binary Classification: True or False
  Multi-class Classification: What type is this thing?
  Recommendation: Given a set of data, what other things might fit into it.
  Regression: Predicting a future value based on the features.
  Time Series forecasting: Similar to regression but with time as the main component.
  Anomaly Detection: Is a data point an outlier given our current understanding.
  Clustering: Categorization based on features.
  Ranking: Ordering by “importance”
  Computer Vision: detection and classification of objects


In this post, we’ll be building a binary classification engine. It will determine if a particular sentiment falls into the category of Toxic or Non-toxic. Let’s start with training our model.

We’ll be using an adapted version of the sample found at the official ML.NET repository. To download this solution, you can go to my GitHub repository.

Training Our Sentiment Analysis Model

To train a model, we need a dataset. In this example, we’ll be using a dataset of sentiment pulled from Wikipedia moderators. Be warned, some of the data can be a little nasty. The two most essential columns in our dataset include label and comment. Before we load this dataset, we need to create a data object.

public class SentimentIssue
{
    [LoadColumn(0)]
    public bool Label { get; set; }
    [LoadColumn(2)]
    public string Text { get; set; }
}


This object allows ML.NET to parse the tab-separated value file. We also need our output class, named SentimentPrediction. We’ll be using our prediction class later in this post.

public class SentimentPrediction
{
    &#x2F;&#x2F; ColumnName attribute is used to change the column name from
    &#x2F;&#x2F; its default value, which is the name of the field.
    [ColumnName(&quot;PredictedLabel&quot;)]
    public bool Prediction { get; set; }

    &#x2F;&#x2F; No need to specify ColumnName attribute, because the field
    &#x2F;&#x2F; name &quot;Probability&quot; is the column name we want.
    public float Probability { get; set; }

    public float Score { get; set; }
}


We can load our training data with the following code.

&#x2F;&#x2F; Create MLContext to be shared across the model creation workflow objects 
&#x2F;&#x2F; Set a random seed for repeatable&#x2F;deterministic results across multiple trainings.
var mlContext &#x3D; new MLContext(seed: 1);

&#x2F;&#x2F; STEP 1: Common data loading configuration
var dataView &#x3D; mlContext.Data.LoadFromTextFile&lt;SentimentIssue&gt;(input.DatasetPath, hasHeader: true);


After loading our data, we need to split our data into two parts: The training and the testing sets.

var trainTestSplit &#x3D; mlContext.Data.TrainTestSplit(dataView, testFraction: 0.2);
var trainingData &#x3D; trainTestSplit.TrainSet;
var testData &#x3D; trainTestSplit.TestSet;


Before we can begin training our model, we need to transform our data. ML.NET has built-in transformers. We need to turn our text input into a feature.

&#x2F;&#x2F; STEP 2: Common data process configuration with pipeline data transformations          
var dataProcessPipeline &#x3D; mlContext.Transforms.Text.FeaturizeText(outputColumnName: &quot;Features&quot;, inputColumnName: nameof(SentimentIssue.Text));


The next two lines of code allow us to use the label in the dataset to create a classification trainer.

&#x2F;&#x2F; STEP 3: Set the training algorithm, then create and config the modelBuilder   
var trainer &#x3D; mlContext.BinaryClassification.Trainers.SdcaLogisticRegression(labelColumnName: &quot;Label&quot;, featureColumnName: &quot;Features&quot;);
var trainingPipeline &#x3D; dataProcessPipeline.Append(trainer);


Finally, we can train our model.

&#x2F;&#x2F; STEP 4: Train the model fitting to the DataSet
ITransformer trainedModel &#x3D; trainingPipeline.Fit(trainingData);


Our next optional step sees us verifying our newly trained model.

&#x2F;&#x2F; STEP 5: Evaluate the model and show accuracy stats
var predictions &#x3D; trainedModel.Transform(testData);
var metrics &#x3D; mlContext.BinaryClassification.Evaluate(data: predictions, labelColumnName: &quot;Label&quot;, scoreColumnName: &quot;Score&quot;);


We can use a console helper in the sample project to write out the results of our test data against our model.

*       Accuracy: 94.69 %
*       Area Under Curve:      94.07 %
*       Area under Precision recall Curve:  77.40 %
*       F1Score:  63.95 %
*       LogLoss:  .21
*       LogLossReduction:  .52
*       PositivePrecision:  .88
*       PositiveRecall:  .5
*       NegativePrecision:  .95
*       NegativeRecall:  99.32 %


We can see that our model shows a 94.69% accuracy rate against our test data. That’s pretty decent. We only need to train the model once, and we can now save it to disk.

&#x2F;&#x2F; STEP 6: Save&#x2F;persist the trained model to a .ZIP file
mlContext.Model.Save(trainedModel, trainingData.Schema, outputPath);


We will be using this trained model in the next section.

Using Our Trained Model

We may have noticed that training our model can take a bit of time. We don’t want to retrain our model every time we need a prediction. Luckily ML.NET allows us the ability to store and load existing trained models.

Given the path to a saved model, we can load it into memory.

var mlContext &#x3D; new MLContext(seed: 1);
var transformer &#x3D; mlContext.Model.Load(modelPath, out _);
var engine &#x3D;
    mlContext.Model.CreatePredictionEngine&lt;SentimentIssue, SentimentPrediction&gt;(transformer);


Once loaded, we can use our prediction engine.

var loop &#x3D; true;
Console.CancelKeyPress +&#x3D; (sender, args) &#x3D;&gt; loop &#x3D; false;
while (loop)
{
    Console.Write(&quot;<!--START_SECTION:feed-->
<!--END_SECTION:feed-->gt; &quot;);
    var line &#x3D; Console.ReadLine();
    var example &#x3D; new SentimentIssue {Text &#x3D; line };
    var prediction &#x3D; engine.Predict(example);
    var result &#x3D; prediction.Prediction ? &quot;Toxic&quot; : &quot;Non Toxic&quot;;

    Console.WriteLine(<!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D; Single Prediction  &#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&quot;);
    Console.WriteLine(<!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;Text: {example.Text} \n&quot; +
                      <!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;Prediction: {result} ({prediction.Probability})&quot;);
}


Running our engine shows our prediction model in action.

<!--START_SECTION:feed-->
<!--END_SECTION:feed-->gt; this is awesome!
&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D; Single Prediction  &#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;
Text: this is awesome! 
Prediction: Non Toxic (0.18389213)
<!--START_SECTION:feed-->
<!--END_SECTION:feed-->gt; this sucks
&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D; Single Prediction  &#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;
Text: this sucks 
Prediction: Toxic (0.97032255)


Note the probability next to the prediction. Our engine scores each outcome. We can think of it as confidence. In the toxic forecast, we can see that our engine is 97% the text is toxic. In the non-toxic prediction, we can see that our model is 82% (100 - 18) the result is not toxic. Remember that the confidence is answering the question, “How confident are we that this sentiment is toxic?”

Oakton For A Better ML Experience

Oakton is a library for parsing command-line arguments and building command-line actions. It’s a perfect companion for ML.NET, as it allows us to separate the training of a model, create clean up commands, and expose a testing fixture.

To get started, we first need to create a console application project. In the Main method, we want to scan and register all new Oakton commands.

class Program
{
    private static async Task&lt;int&gt; Main(string[] args)
    {
        var executor &#x3D; CommandExecutor.For(_ &#x3D;&gt;
        {
            _.RegisterCommands(typeof(Program).GetTypeInfo().Assembly);
        });

        return await executor.ExecuteAsync(args);
    }
}


Next, we can create two commands: TrainingCommand and CheckCommand. Each one has its own input as well. Let’s look at the TrainingCommand first.

using System;
using System.IO;
using Common;
using MachineLearningHelloWorld.Structures;
using Microsoft.ML;
using Oakton;

namespace MachineLearningHelloWorld
{
    public class TrainingInput
    {
        [FlagAlias(&#39;i&#39;)]
        [Description(&quot;training dataset path&quot;, Name &#x3D; &quot;data&quot;)]
        public string DatasetPath { get; set; }
        
        [FlagAlias(&#39;o&#39;)]
        [Description(&quot;training model output (zip file)&quot;, Name &#x3D; &quot;output&quot;)]
        public string OutputPath { get; set; }
    }
    
    public class TrainingCommand : OaktonCommand&lt;TrainingInput&gt;
    {
        public const string DefaultModelPath &#x3D; &quot;.&#x2F;&quot;;

        public static string GetModelPath(string path)
        {
            return Path.Combine(path, &quot;model.zip&quot;);
        }

        public TrainingCommand()
        {
            Usage(&quot;Default output&quot;).Arguments(x &#x3D;&gt; x.DatasetPath);
            Usage(&quot;Override output&quot;).Arguments(x &#x3D;&gt; x.DatasetPath, x &#x3D;&gt; x.OutputPath);
        }
        
        public override bool Execute(TrainingInput input)
        {
            if (string.IsNullOrEmpty(input.DatasetPath))
            {
                ConsoleHelper.ConsoleWriteException(&quot;a dataset is required to train the model.&quot;);
                return false;
            }

            var outputPath &#x3D; GetModelPath(
                string.IsNullOrEmpty(input.OutputPath)
                ? DefaultModelPath
                : input.OutputPath
            );
            
            &#x2F;&#x2F; Create MLContext to be shared across the model creation workflow objects 
            &#x2F;&#x2F; Set a random seed for repeatable&#x2F;deterministic results across multiple trainings.
            var mlContext &#x3D; new MLContext(seed: 1);

            &#x2F;&#x2F; STEP 1: Common data loading configuration
            var dataView &#x3D; mlContext.Data.LoadFromTextFile&lt;SentimentIssue&gt;(input.DatasetPath, hasHeader: true);

            var trainTestSplit &#x3D; mlContext.Data.TrainTestSplit(dataView, testFraction: 0.2);
            var trainingData &#x3D; trainTestSplit.TrainSet;
            var testData &#x3D; trainTestSplit.TestSet;

            &#x2F;&#x2F; STEP 2: Common data process configuration with pipeline data transformations          
            var dataProcessPipeline &#x3D; mlContext.Transforms.Text.FeaturizeText(outputColumnName: &quot;Features&quot;, inputColumnName: nameof(SentimentIssue.Text));

            &#x2F;&#x2F; STEP 3: Set the training algorithm, then create and config the modelBuilder                            
            var trainer &#x3D; mlContext.BinaryClassification.Trainers.SdcaLogisticRegression(labelColumnName: &quot;Label&quot;, featureColumnName: &quot;Features&quot;);
            var trainingPipeline &#x3D; dataProcessPipeline.Append(trainer);

            &#x2F;&#x2F; STEP 4: Train the model fitting to the DataSet
            ITransformer trainedModel &#x3D; trainingPipeline.Fit(trainingData);

            &#x2F;&#x2F; STEP 5: Evaluate the model and show accuracy stats
            var predictions &#x3D; trainedModel.Transform(testData);
            var metrics &#x3D; mlContext.BinaryClassification.Evaluate(data: predictions, labelColumnName: &quot;Label&quot;, scoreColumnName: &quot;Score&quot;);

            ConsoleHelper.PrintBinaryClassificationMetrics(trainer.ToString(), metrics);

            &#x2F;&#x2F; STEP 6: Save&#x2F;persist the trained model to a .ZIP file
            mlContext.Model.Save(trainedModel, trainingData.Schema, outputPath);

            Console.WriteLine(&quot;The model is saved to {0}&quot;, outputPath);

            return true;
        }
    }
}


We can use Oakton to parse our input arguments. We can run the training command with the following:

dotnet run training data.tsv


The command trains our model using the passed in data set, as in the previous section. The advantage of using Oakton is we can define various usages for our newly created commands. In this case, we have a default output for our trained model.

Let’s take a look at the CheckCommand.

public class CheckInput
{
    [FlagAlias(&#39;m&#39;)]
    [Description(&quot;path of the trained model&quot;)]
    public string ModelPath { get; set; }
}

public class CheckCommand : OaktonCommand&lt;CheckInput&gt;
{
    public CheckCommand()
    {
        Usage(default).Arguments();
        Usage(&quot;with model&quot;).Arguments(x &#x3D;&gt; x.ModelPath);
    }
    
    public override bool Execute(CheckInput input)
    {
        var mlContext &#x3D; new MLContext(seed: 1);

        var modelPath &#x3D; input.ModelPath.IsEmpty()
            ? TrainingCommand.GetModelPath(TrainingCommand.DefaultModelPath)
            : input.ModelPath.EndsWith(&quot;.zip&quot;)
                ? input.ModelPath
                : TrainingCommand.GetModelPath(input.ModelPath);
        
        var transformer &#x3D; mlContext.Model.Load(modelPath, out _);
        var engine &#x3D;
            mlContext.Model.CreatePredictionEngine&lt;SentimentIssue, SentimentPrediction&gt;(transformer);

        &#x2F;&#x2F; User has to Ctrl+C out
        Console.WriteLine(<!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D; Zoltar Of Sentiment  &#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&quot;);

        var loop &#x3D; true;
        Console.CancelKeyPress +&#x3D; (sender, args) &#x3D;&gt; loop &#x3D; false;
        while (loop)
        {
            Console.Write(&quot;<!--START_SECTION:feed-->
<!--END_SECTION:feed-->gt; &quot;);
            var line &#x3D; Console.ReadLine();
            var example &#x3D; new SentimentIssue {Text &#x3D; line };
            var prediction &#x3D; engine.Predict(example);
            var result &#x3D; prediction.Prediction ? &quot;Toxic&quot; : &quot;Non Toxic&quot;;

            Console.WriteLine(<!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D; Single Prediction  &#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&#x3D;&quot;);
            Console.WriteLine(<!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;Text: {example.Text} \n&quot; +
                              <!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;Prediction: {result} ({prediction.Probability})&quot;);
        }

        return loop;
    }
}


We can pass in an entirely different trained model, or use the default model. We will also let a user type as many lines of sentiment as they want.

dotnet run check


Using Oakton, we can continue to evolve our ML.NET app with new commands.

Conclusion

ML.NET has been on my list of technology for a while now. Once we break down the sample project into its two components, it is clear that it is easy to consume trained models. I expect to look at the other classification models, and see where we could use it in our existing applications. The addition of Oakton makes it easy to focus on the different parts of data science and leaves open the possibility of future enhancements.

I hope you found this post helpful, and thanks to the ML.NET team for making such a great library and a vast library of samples.

Remember you can download this sample project from my GitHub repository.*
#### [Write Xamarin.Mac Apps With JetBrains Rider](https:&#x2F;&#x2F;khalidabuhakmeh.com&#x2F;write-xamarin-mac-apps-with-jetbrains-rider) 
*macOS is my operating system of choice, but oddly I do most of my professional programming with the .NET Framework. Historically, the choice of Macs paired with .NET would raise eyebrows, but not anymore. The release of .NET Core has made the .NET ecosystem more welcoming of folks with other operating systems.

It’s undeniable that Xamarin helped pave the way for technological diversity in the .NET space. While we’re reaping the benefits now, the idea of developing native macOS applications has never really piqued my interest until now.

In this post, we’ll be following Microsoft’s guide to building a Hello, Mac native mac application using Xamarin.Mac tweaked for JetBrains Rider.



Why Use Xamarin.Mac

There are multiple cross-platform frameworks for building native applications: Xamarin.Forms, UnoPlatform, Electron, and more. So why choose Xamarin.Mac?

Xamarin.Mac gives us the ability to write truly native apps for macOS. We get to use macOS’ buttons, labels, windows, and the entire UI toolkit. It’s an ideal choice for folks looking to target macOS only. Native macOS components fulfill the needs of our UI, while we can lean on the power of the .NET Framework to handle backend functionality.

macOS applications utilize the Model-View-Controller pattern, which makes it possible for other technologies to step into the backend role. In this demo, we’ll be using C#.

Getting Started

Before we get started writing the demo, we’ll need all of our dependencies installed.


  XCode
  Xamarin iOS &amp; Mac
  Mono


We can download XCode from the Apple AppStore. Installation of the other two dependencies can happen from inside of JetBrains Rider. In the Preferences window, we can select Environment and select Mono and Xamarin iOS &amp; Mac.



Warning to the uninitiated, installing XCode is going to take a while. Now is a good time to get a snack, hug loved ones, and pet the family dog.

Hello Demo

From the JetBrains Rider welcome dialog, we start by locating the Xamarin category on the left. From there, the dialog updates allowing us to choose the Platform. In our case, we want to target macOS. For the sake of this demo, the Target macOS API is irrelevant, but we can pick one that sparks joy. We can not create the project.



Noticing the layout of our project, we see a few native macOS files mixed with C# files. In this demo, we’ll be focusing primarily on ViewController as it will hold our logic.



Before we can start writing any C# code, we need to create outlets on our controller so that the UI can communicate with the backend. We can do this by right-clicking the Main.storyboard file and selecting Open in XCode.



With XCode open, we can change some settings. Let’s add a few controls and link them back to our controller. To open the library, we’ll use the shortcut Shift+Command+L (⇧⌘L).



From here, we can drag a Push Button and a Label to our view. We can play with the layout settings for each control, but it is unnecessary for this tutorial. We could spend forever making the UI “perfect”.



From here, let’s create a few outlets. From XCode, we need to open the ViewController.h file, which holds our ViewController interface. We’ll want to have two editors side-by-side, we can create the layout by clicking a button in the top right of our current editor. We’ll highlight the button below in a purple box.



The next step is where the magic happens. We will create the outlets in our ViewController. Holding down the Ctrl (⌃) key, we’ll click the button and drag it right below the close } and between @end.

We want to choose Action from the Connection dropdown and give the Action a name. In this case, we give it the name of ClickedButton. We also want the Type to be NSButton.

Next, let’s add the label to our ViewController. Holding Ctrl (⌃), we need to drag an outlet in between the {} of ViewController. We should see an Insert Outlet label. We can now give our new outlet a name, in this instance, we can use CountLabel. If its unclear, please watch the video below.



Our ViewController.h file should now contain the following code.

#import &lt;Foundation&#x2F;Foundation.h&gt;
#import &lt;AppKit&#x2F;AppKit.h&gt;

@interface ViewController : NSViewController {
  IBOutlet NSTextFieldCell *CountLabel;
}
- (IBAction)ClickedButton:(NSButton *)sender;

@end


We can save the file now and go back to Rider. When we look at ViewController.Designer.cs we’ll notice that Rider generated the outlets necessary to interact with our macOS UI.

using Foundation;
using System.CodeDom.Compiler;

namespace CountDracula
{
   [Register (&quot;ViewController&quot;)]
   partial class ViewController
   {
      [Outlet]
      AppKit.NSTextFieldCell CountLabel { get; set; }

      [Action (&quot;ClickedButton:&quot;)]
      partial void ClickedButton (AppKit.NSButton sender);

      void ReleaseDesignerOutlets ()
      {
         if (CountLabel !&#x3D; null) {
            CountLabel.Dispose ();
            CountLabel &#x3D; null;
         }

      }
   }
}


We’ll need to add the following code to our ViewController class.

using System;
using AppKit;

namespace CountDracula
{
    public partial class ViewController : NSViewController
    {
        private int Count { get; set; }
        
        public ViewController(IntPtr handle) : base(handle)
        {
        }

        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            CountLabel.StringValue &#x3D; &quot;No Count!&quot;;
        }

        partial void ClickedButton(AppKit.NSButton sender)
        {
            CountLabel.StringValue &#x3D; <!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;Count: {++Count}&quot;;
            View.Window.Title &#x3D; <!--START_SECTION:feed-->
<!--END_SECTION:feed-->quot;🧛‍️ {Count}... Muhahaha!&quot;;
        }
    }
}


When we run the application, we can increment the counter by clicking the button.



Hooray! We have our first native macOS application. If your clicks aren’t going through, be sure that the connection between the button and the ViewController is active in XCode. You can do this by right-clicking the button in XCode.



Conclusion

I love macOS but never thought of writing any apps for it, which seems like an oversight on my part. Using JetBrains Rider, we can use our favorite IDE while developing apps for our operating system of choice. With Xamarin.Mac we get a native app experience with the ability to use .NET to handle the backend. We also get to use native .NET debugging tools to diagnose issues.

I hope you found this tutorial helpful. Please leave a comment below if you have any questions.*
<!--END_SECTION:feed-->

### Office Hours ⏱

![office hours](https://res.cloudinary.com/abuhakmeh/image/fetch/c_limit,f_auto,q_auto,w_1200/https://khalidabuhakmeh.com/assets/images/posts/misc/office_hours.jpg)

As a developer advocate, I'm here to help by offering [Office Hours](https://khalidabuhakmeh.com/office-hours/). These are anywhere from 30-60 minute sessions where we can discuss tech, problem solving, or general career advice. I've held every position from junior developer to director of software development.

### 🗃 OSS

I also [do OSS](https://www.nuget.org/profiles/khalidabuhakmeh) when the time allows. Some of my most popular NuGet packages include:

- [ConsoleTables](https://github.com/khalidabuhakmeh/consoletables)
- [Codez](https://github.com/khalidabuhakmeh/codez)

That's over **1.2 Million downloads!**

I have also contributed logos to multiple .NET ecosystem projects like [Marten](https://github.com/JasperFx/marten), [DDay.iCal](https://github.com/rianjs/ical.net), and [Buildalyzer](https://github.com/daveaglick/Buildalyzer).

### Personal 😜

Fun Facts: 

- My wife, [Nicole](https://wanderinglush.com) and I love traveling.
- I'm currently trying to learn Japanese. おはようございます。(Good Morining!).
- I have two dogs named Samson and Guinness
- I have **[KhalidForAmerica.com](https://khalidforamerica.com)** which points to my blog, and other domains.
- I enjoy Synthwave music.

If you would like to reach me, the best way is on Twitter via [@buhakmeh][twitter].

[twitter]: https://twitter.com/buhakmeh
[jetbrains]: https://jetbrains.com
[rider]: https://jetbrains.com/rider
[resharper]: https://jetbrains.com/resharper
[blog]: https://khalidabuhakmeh.com
[jb_blog]: https://blog.jetbrains.com/dotnet
[dotnet]: https://dot.net


<p align="center">
  <img alt="Visitor count" src="https://profile-counter.glitch.me/khalidabuhakmeh/count.svg" style="display: inline-block; margin: auto;" />  
</p>

<!--
**khalidabuhakmeh/khalidabuhakmeh** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
