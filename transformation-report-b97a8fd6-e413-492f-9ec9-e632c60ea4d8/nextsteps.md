# Next Steps

## Issues resolved
- Transformed DocumentProcessor.Web.csproj to net8.0

## Summary

The transformation appears to have completed successfully. No build errors were detected across any of the projects in the solution, including the most independent project `DocumentProcessor.Web`.

## Validation Steps

### 1. Restore Dependencies

Run the following command from the solution root to ensure all NuGet packages are restored correctly:

```bash
dotnet restore
```

Review the output for any warnings related to package compatibility or deprecated packages targeting older frameworks.

### 2. Build the Solution

Perform a full solution build to confirm the error-free state is consistent across all configurations:

```bash
dotnet build --configuration Debug
dotnet build --configuration Release
```

### 3. Run Unit Tests

If the solution contains test projects, execute them to verify that existing functionality has not regressed during the transformation:

```bash
dotnet test --configuration Release --logger "console;verbosity=detailed"
```

Review test output carefully for any failures or skipped tests that may indicate compatibility issues introduced during migration.

### 4. Review Target Framework

Open `DocumentProcessor.Web.csproj` and confirm the `<TargetFramework>` element is set to the intended .NET version, for example:

```xml
<TargetFramework>net8.0</TargetFramework>
```

Ensure all other projects in the solution target a compatible framework version.

### 5. Review Removed Windows-Specific APIs

Check the codebase for any usage of APIs that were available in .NET Framework but have limited or no support in cross-platform .NET, such as:

- `System.Web` namespaces
- Windows Registry access
- `HttpContext` usage patterns specific to `System.Web`
- `AppDomain` features that are no longer supported

Use the [.NET Upgrade Assistant compatibility analyzer](https://learn.microsoft.com/en-us/dotnet/core/porting/upgrade-assistant-overview) or the `Microsoft.DotNet.PlatformAbstractions` tooling to identify remaining compatibility concerns.

### 6. Run the Application Locally

Start the application and verify it runs as expected:

```bash
dotnet run --project src/DocumentProcessor.Web/DocumentProcessor.Web.csproj --configuration Release
```

Navigate through the application's primary workflows to confirm runtime behavior matches the legacy version.

### 7. Verify Configuration Files

Confirm that `appsettings.json` (and environment-specific variants such as `appsettings.Production.json`) contain all settings that were previously held in `Web.config` or `App.config`. Pay particular attention to:

- Connection strings
- Application-specific keys
- Logging configuration

### 8. Publish the Application

Once local validation is complete, produce a published output to verify the deployment artifact builds correctly:

```bash
dotnet publish src/DocumentProcessor.Web/DocumentProcessor.Web.csproj --configuration Release --output ./publish
```

Review the contents of the `./publish` directory to confirm all expected files, static assets, and dependencies are present before deploying to the target environment.