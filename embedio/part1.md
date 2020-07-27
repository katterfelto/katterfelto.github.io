# EmbedIO - Our First 'Webpage'

## Overview

EmbedIO is a .NET asssembly which makes it possible to host a mini webserver in your application, whether it's a console, desktop (Winforms, WPF or UWP) or Windows Service application; the later is where my interest lies. 

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

## Write Some Code

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

Finally we need to call the new function and then start the webserver, lets replace the contents of the `main` function with the following.

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

And that's it, compile and run the application. It should open your favourite browser and display the stunning web page we designed.

## What did we do?

In the `CreateWebServer` function we:

1. Created a new instance of the WebServer class.
2. The `WithUrlPrefix` function defines the network interface and port number the web server will listen on.
3. We added a module to handle requests to the root object.
4. We added an action in the module which triggered on any type of HTTP request to return our desired HTML string.

In the `main` function we:
1. Created the web server using our funtion.
2. Started the web server running in the background.
3. Started a new process to connect a browser to our web server's URL.

## Finishing Up

There are some thigs to consider here. Firstly, the URL we provided to listen on is localhost when , we can't connect to our server from another PC on the network. On Windows replacing `localhost` with `+` will make the server listen on all network interfaces. Replacing `localhost` with one of your NIC IP Addresses will make the server listen on that IP Address only.

The web content we are serving is very simple but at least we have something running. In the next step we will serve multiple files, intially from a local folder, then from a zip file and finally from embedded resources within our application assembly.

## Finally

I can't take credit for the vast majority of the code in this tutorial. Most of it can be found in the EmbedIO githb repository. And special thanks goes to geoperez for responding to the issue I generated with helpful, freindly feedback.