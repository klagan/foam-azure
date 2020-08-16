# Nuget 

[(source)](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file#packagesourcecredentials)

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

### Package fee credentials in Docker 

[(source)](https://github.com/dotnet/dotnet-docker/blob/master/documentation/scenarios/nuget-credentials.md)

There is an additional complexity when configuring a build process in a docker environment.  THe environment has no access to your local nuget.config so it cant reach the credentials we set up on the local host machine (above).

What we need to do is add this configuration into the image `nuget.config` and pass the user name and password as parameters.

First, we edit our `nuget.config` associated with the project:

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
  <packageSourceCredentials>
    <my-custom-feed>
        <add key="Username" value="%nugetUid%" />
        <add key="ClearTextPassword" value="%nugetPwd%" />
    </my-custom-feed>
  </packageSourceCredentials>
</configuration>
```
Second, edit the `dockerfile` to accept the arguments from the command line:

```
ARG nugetUid
ARG nugetPwd
```

Thirdly, we set up the local host credentials in environment variables (Im an Apple Mac user so you will need to look up the Windows equivalents):

```
export nugetUid=someone@somwehere.com
export nugetPwd=my-secret-password/PAT
```

This now means we are in a position to call the `docker build` command which will pull the nuget values in from the local host environment variables and push them into the dockerfile which will replace the placeholders we put in the `nuget.config` file!

`docker build -t my_image:version --build-arg nugetUid --build-arg nugetPwd .`



[[dotnet.md]]
