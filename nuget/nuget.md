# Nuget

In a dot net project, we can store the package feeds in the solution by using a `nuget.config` file like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <config>
    <add key="repositoryPath" value=".\packages" />
  </config>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
    <add key="dotnet.myget.org roslyn" value="https://dotnet.myget.org/F/roslyn/api/v3/index.json" />
    <add key="my-custom-feed" value="https://pkgs.dev.laganlabs.it/examples/_packaging/my-custom-feed/nuget/v3/index.json" />
  </packageSources>
</configuration>
```

The `nuget.config` file carries the package feed dependencies of the solution and allows an IDE to load them for package resolution.  This helps define the solution as a completely self sufficient entity describing all requirements and dependencies.

The `nuget.org` and `dotnet.myget.org` package feeds are publicly accessible and do not have any credential requirements.  However, the `my-custom-feed` is a protected package feed hosted in Azure Devops and requires user credentials to access them.  How do we provide credentials without accidentally storing them to the repository?

## Package feed credentials

One way of providing package feed credentials is to use another `nuget.config`.  The `nuget.config` we shall use is the one located on the local machine and **not** in the code repository.

I work on an Apple Mac which means the location of my machine level `nuget.config` is `~/.nuget/nuget/nuget.config`  This file merges with the solution level `nuget.config`.  Here we can add our credentials 
assured they will be used **in** the solution but not committed to the repository.

```xml
~/.nuget/nuget/nuget.config
  <packageSourceCredentials>
    <my-custom-feed>
        <add key="Username" value="someone@somewhere.com" />
         <add key="ClearTextPassword" value="<PAT>" />
    </my-custom-feed>
</packageSourceCredentials>
```

[[dotnet.md]]