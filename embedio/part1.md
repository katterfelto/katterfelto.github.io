# EmbedIO - Our First 'Webpage'

## Overview

[EmbedIO](https://github.com/unosquare/embedio) is a .NET asssembly which makes it possible to host a mini webserver in your application, whether it's a console, desktop (Winforms, WPF or UWP) or Windows Service application; the later is where my interest lies. 

At work we have some Windows Services for which we provide Winforms based control panels that communicate with the servce using WCF. It is looking like the future of WCF is not very rosey so we are exploring alternatives, build a web based control panel into the service is one strategy we are considering.

## Set up a Project

Firstly we need a host application, a .NET Core console app will do. I'm using linux and VS Code but this works just as well in Visual Studio on Windows (and I guess Mac but I don't have access to try it).
```
mkdir embedtest
cd embedtest
dotnet new console
```

Once that's finished and we have our prompt back we need to add the EmbedIO assembly from Nuget:
```
dotnet add package EmbedIO
```

Now everything is prepared we can edit some code.

## Create the WebServer

Open `program.cs` in your favourite editor/IDE. First we need to add a couple of using statements to the top of the file:
``` csharp
using EmbedIO;
using EmbedIO.Actions;
```

Next add the function to create our web server:
``` csharp
private static WebServer CreateWebServer(string url)
{
    return new WebServer(o => o
        .WithUrlPrefix(url))
        .WithModule(new ActionModule("/",
            HttpVerbs.Any, 
            ctx => ctx.SendStringAsync(
                    "<h1>Hello EmbedIO!</h1>", 
                    "text/html", 
                    System.Text.Encoding.UTF8
                    )));
}
```

The above function creates and returns a `WebServer` object. As part of the constructor we access the options object and include the url we want to list for calls on using the `WithUrlPrefix()` function.

We then chain a call to the `WebServer` objects `WithModule()` function. This adds an `ActionModule` to handle calls to the "/" route of the URL. We can chain any number of modules for different routes and http verbs. The web server object processes it's chain of modules until it finds the first one in the list that is configured to handle the route and verb defined in the call. The selected module then handles the call and provides the required response.

## Starting the Server
Finally we need to call the above function and then start the webserver, lets replace the contents of the `main` function with the following.

``` csharp
var url = "http://localhost:9696/";
using (var server = CreateWebServer(url))
{
    // Once we've registered our modules and configured them, we call the RunAsync() method.
    server.RunAsync();

    var browser = new System.Diagnostics.Process()
    {
        StartInfo = new System.Diagnostics.ProcessStartInfo(url) { UseShellExecute = true }
    };
    browser.Start();
    // Wait for any key to be pressed before disposing of our web server.
    // In a service, we'd manage the lifecycle of our web server using
    // something like a BackgroundWorker or a ManualResetEvent.
    Console.ReadKey(true);
}
```

We call the function we created in the previous section and pass it onto the `using` block we defined. At the start of the block we start the server object as a background task.

The next couple of lines starts our favourite web browser in a new process and calls our base URL.

Finally we wait for a key press in the console window before we continue. Once a key is pressed we exit the using block causing our web server object to go out of scope, stop listening and be disposed of.

And that's it, compile and run the application. It should open your favourite browser and display the stunning web page we designed.

## Finishing Up

There are some things to consider here. Firstly, the URL we provided to listen on is localhost, this means we can't connect to our server from another PC on the network. On Windows replacing `localhost` with `+` or `*` will make the server listen on all network interfaces. Replacing `localhost` with one of your NIC IP Addresses will make the server listen on that IP Address only.

You will notice log messages appearing in the console window. It's very helpful for testing and debugging but in production we might not want this to happen. To disable logging completely we need a using statement and then to add a line of code in `CreateWebServer` before we create the `WebServer` object.

``` csharp
using Swan.Logging;
```

``` csharp
Logger.UnregisterLogger<ConsoleLogger>();
```

The web content we are serving is very simple but at least we have something running. In the next step we will serve multiple files, intially from a local folder, then from a zip file and finally from embedded resources within our application assembly.

## Finally

I can't take credit for the vast majority of the code in this tutorial. Most of it can be found in the EmbedIO githb repository. And special thanks goes to geoperez for responding to the issue I generated with helpful, freindly feedback.