![](https://teamcity.cb.com/app/rest/builds/buildType:AuthenticationCore_DevelopBranchBuild/statusIcon)

# Authentication.Core
This is a .Net Core package you can consume in your Api authenticate requests. Currently supports only Partner Authentication.  

## Getting Started
As a prerequisite, you’ll need to setup your machine to run .NET Core. You can find the installation instructions on the [.NET Core](https://www.microsoft.com/net/core) page and then you'll need to download .NET Core 1.1 from [.NET Downloads] (https://www.microsoft.com/net/download/core#/current/runtime) page as well. You can also refer to [CA WIKI](https://cagit.careerbuilder.com/CorpAppsCB/wiki/wiki) page for more .NET related resources.  

## Package Installation
First, [install NuGet](http://docs.nuget.org/docs/start-here/installing-nuget). Then using the **Enterprise Software** NuGet Packages Source: http://svrus4teamcity1.cb.careerbuilder.com:8080/NugetServer/nuget, install Authentication.Core from the package manager console:

    PM> Install-Package Authentication.Core

## Configuration
The example below is configured with SqlServer adapter `.UseSqlServer`. It also can be configured using other db adapters supported by EntityFrameworkCore.  

`Startup.cs`
```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    
    services.AddDbContext<PartnerDbContext>(options => options
        .UseSqlServer(Configuration.GetConnectionString("PartnerAuthenticationConnection")));
    services.AddMvc(options => options.Filters.Add(typeof(PartnerAuthenticationFilter)));         
    
    ...
}
```

In the method below the database for Authentication is created and seeded automatically if not there already, no migrations needed. The database is created with some sample partner data. You could use the following combination for your testing:  
**PartnerId: TestPartner**  
**Password: TestPassword**  
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory, PartnerDbContext partnerDbContext)
{
    ...
    
    if (env.IsDevelopment())
        AuthenticationSeedData.Initialize(partnerDbContext);
        
    ...
}
```

In the configuration below the format used is SqlServer, but you can use any other db format, depending on the option used in the ConfigureServices method. Please make sure you have a local db instance up and running, which in this case is **(localdb)\mssqllocaldb**.  

`appsettings.development.json`
```json
{

  "ConnectionStrings": {
    "PartnerAuthenticationConnection":
      "Server=(localdb)\\mssqllocaldb;Database=PartnerAuthentication;Trusted_Connection=True;MultipleActiveResultSets=true"
  }

}
```

In the configuration for Staging and Production the keys are needed for teamcity to substitute the actual values, So you can copy and paste the way they are.  

`appsettings.staging.json`
```json
{

  "ConnectionStrings": {
    "PartnerAuthenticationConnection":
      "Server=<DATASERVERGOESHERE>;Database=<DATABASEGOESHERE2>;User=<USERGOESHERE>;Password='<PASSWORDGOESHERE>';"
  }

}
```

`appsettings.production.json`
```json
{

  "ConnectionStrings": {
    "PartnerAuthenticationConnection":
      "Server=<DATASERVERGOESHERE>;Database=<DATABASEGOESHERE2>;User=<USERGOESHERE>;Password='<PASSWORDGOESHERE>';"
  }

}
```

## RESTful calls
To call into your api once you have installed and configured Authentication.Core, you should pass the appropriate headers to avoid a 401 response.  
`Authorization: Partner TestPartner:TestPassword`

## Contributing
We encourage you to contribute to this repo by following the [CONTRIBUTING](CONTRIBUTING.md) instructions.
