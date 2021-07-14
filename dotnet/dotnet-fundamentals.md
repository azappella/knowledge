# .net core fundamentals

[Source](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/?view=aspnetcore-3.1&tabs=windows)

## The Startup class

The Startup class is where:

    Services required by the app are configured.
    The app's request handling pipeline is defined, as a series of middleware components.

## Host

On startup, an ASP.NET Core app builds a host. The host encapsulates all of the app's resources, such as:

    An HTTP server implementation
    Middleware components
    Logging
    Dependency injection (DI) services
    Configuration

There are two different hosts:

    .NET Generic Host
    ASP.NET Core Web Host



# .NET Core Web Application Lifecycle

Program.cs
Startup.cs

```
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
```


Default builder settings

The CreateDefaultBuilder method:

    Sets the content root to the path returned by GetCurrentDirectory.
    Loads host configuration from:
        Environment variables prefixed with DOTNET_.
        Command-line arguments.
    Loads app configuration from:
        appsettings.json.
        appsettings.{Environment}.json.
        Secret Manager when the app runs in the Development environment.
        Environment variables.
        Command-line arguments.
    Adds the following logging providers:
        Console
        Debug
        EventSource
        EventLog (only when running on Windows)
    Enables scope validation and dependency validation when the environment is Development.

The ConfigureWebHostDefaults method:

    Loads host configuration from environment variables prefixed with ASPNETCORE_.
    Sets Kestrel server as the web server and configures it using the app's hosting configuration providers. For the Kestrel server's default options, see Kestrel web server implementation in ASP.NET Core.
    Adds Host Filtering middleware.
    Adds Forwarded Headers middleware if ASPNETCORE_FORWARDEDHEADERS_ENABLED equals true.
    Enables IIS integration. For the IIS default options, see Host ASP.NET Core on Windows with IIS.


https://docs.microsoft.com/en-us/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.1