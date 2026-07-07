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

Review the output for any warnings related to package compatibility or version conflicts.

### 2. Build the Solution

Perform a full solution build to confirm the absence of errors in a clean build environment:

```bash
dotnet build --configuration Release
```

### 3. Run Unit Tests

If the solution contains test projects, execute them to verify that existing functionality behaves as expected after the migration:

```bash
dotnet test --configuration Release --logger "console;verbosity=detailed"
```

Review any failing tests and determine whether they are caused by behavioral differences between the legacy .NET Framework and the new cross-platform .NET runtime.

### 4. Check for Runtime Compatibility Issues

Some APIs that compiled successfully may behave differently or throw exceptions at runtime. Pay particular attention to:

- **File system paths**: Ensure no hardcoded Windows-style paths (e.g., `C:\`) exist in configuration files or code.
- **Windows-only APIs**: Check for any usage of APIs such as `System.Drawing`, `Microsoft.Win32.Registry`, or COM interop, which may not be supported on non-Windows platforms without additional packages.
- **Configuration files**: Verify that `appsettings.json` and any environment-specific configuration files are present and correctly structured.
- **Connection strings**: Confirm that database or service connection strings are valid and accessible in the new environment.

### 5. Run the Application Locally

Start the web application locally and navigate through its core functionality:

```bash
dotnet run --project src/DocumentProcessor.Web/DocumentProcessor.Web.csproj --configuration Release
```

Verify that:
- The application starts without exceptions.
- Key routes and endpoints respond correctly.
- Any document processing workflows complete as expected.

### 6. Review Target Framework

Open `DocumentProcessor.Web.csproj` and confirm the `TargetFramework` is set to the intended version, for example:

```xml
<TargetFramework>net8.0</TargetFramework>
```

Ensure all dependent projects in the solution target a compatible framework version.

### 7. Review Nullable Reference Type Warnings

If the project has nullable reference types enabled (`<Nullable>enable</Nullable>`), review any compiler warnings related to nullability. While these are not build errors, they can indicate potential null reference exceptions at runtime.

### 8. Validate Publishing

Publish the application to confirm the output is complete and self-contained if required:

```bash
dotnet publish src/DocumentProcessor.Web/DocumentProcessor.Web.csproj --configuration Release --output ./publish
```

Inspect the `./publish` directory to ensure all necessary files, static assets, and configuration files are present.