# Logging

This code snippet shows how to set up the bootstrap host builder with platform services. 

> remember: include [Serilog package](https://github.com/serilog/serilog-extensions-logging-file)

```dotnet
    public static IHostBuilder CreateHostBuilder(
        string[] args
    ) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration(builder =>
            {
                builder.ConfigureBuilder();
            })
            .ConfigureLogging(builder =>
            {
                builder.AddConsole(o => o.IncludeScopes = true);
                builder.AddDebug();
                builder.AddFile(
                    builder
                        .Services
                        .BuildServiceProvider()
                        .GetRequiredService<IConfiguration>()
                        .GetSection("Logging")
                    );
            })
            .ConfigureWebHostDefaults(webBuilder => 
            { 
                webBuilder.ConfigureKestrel((context, options) =>
                    {
                        options.Limits.MaxConcurrentConnections = 100;
                     })
                .UseStartup<Startup>();
            });
    }
```


Sample logging configuration section:

```json
"Logging": {
    "PathFormat": "c:/logs/log-{Date}.txt",
    "OutputTemplate": "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} [{Level}] [{SourceContext}] [{EventId}] {Message}{NewLine}{Exception}",
    "LogLevel": {
    "Default": "Information",
    "Microsoft": "Warning"
    }
},
```

[[dotnet.md]]
