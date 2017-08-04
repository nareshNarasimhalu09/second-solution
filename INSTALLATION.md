## Prerequisite
You’ll need to setup your machine to run .NET Core. You can find the installation instructions on the [.NET Core](https://www.microsoft.com/net/core) page and then you'll need to download .NET Core 1.1 from [.NET Downloads] (https://www.microsoft.com/net/download/core#/current/runtime) page as well.

## Installation Steps
Follow the below mentioned steps to get started in development.

Clone the repository

    git clone https://cagit.careerbuilder.com/CorpAppsCB/cost_centers_api.git

Switch to the project's directory

    cd cost_centers_api\src\CostCentersApi

Then restore Nuget Packages

    dotnet restore

Create the local database and migration scripts

    dotnet ef database update –context CostCenterDbContext

Then build and run

    dotnet build
    dotnet run --environment "development"
    
## Testing in Postman

```http
GET http://localhost:5000/cost_centers
```
