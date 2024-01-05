## Tailwind CSS in Blazor
This project uses Tailwind CSS instead of Bootstrap. It's based on the .NET 8 Blazor Web App project template.

:tv: Watch the YouTube tutorial:  
[How to Use Tailwind CSS in Blazor | Quick Start](https://youtu.be/put2m4xTJ30).

## Installation Instructions
### Download the standalone Tailwind CSS CLI
Download the standalone Tailwind CSS CLI and move it into a new top-level *Tailwind* folder.
Download here: https://github.com/tailwindlabs/tailwindcss/releases/

## Manual Setup
The following steps are only required when not using this project. They are already performed in the source code in this repository.

### Create and Configure the Tailwind CSS Config File
```bash
./../../Tailwind/tailwindcss.exe init
```

```csharp
/** @type {import('tailwindcss').Config} */
module.exports = {
    content: [
        "./../**/*.{razor,html,cshtml}"
    ],
    theme: {
        extend: {},
    },
    plugins: [],
}
```

### Create the Source Tailwind CSS File
Create Source File *Styles/tailwind-app.css*:
```csharp
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Load the Generated Tailwind CSS
Add the compiled Tailwind CSS stylesheet to App.razor:
```csharp
<link rel="stylesheet" href="tailwind-app.css" />
```

### Use Tailwind CSS in Code
```csharp
<div class="text-4xl font-bold underline">
    Welcome to your new Tailwind CSS styled Blazor app.
</div>
```

### Run the Tailwind CSS CLI
```bash
./../../Tailwind/tailwindcss.exe -i ./Styles/tailwind-app.css -o ./wwwroot/tailwind-app.css --watch
```

## Optional Prefixing
If you want to gradually introduce Tailwind CSS into a project that uses a different CSS library such as Bootstrap, you can configure Tailwind CSS to use a prefix.

```csharp
/** @type {import('tailwindcss').Config} */
module.exports = {
    content: [
        "./../**/*.{razor,html,cshtml}"
    ],
    theme: {
        extend: {},
    },
    plugins: [],
    prefix: "tw-",
}
```

However, when we do so, we also need to use that prefix on all CSS classes we add to our components. For example, "text-4xl" is now "tw-text-4xl".

## MSBuild After Build Script (Powershell)
Instead of running the Tailwind CSS CLI in a terminal, you can also set up a PowerShell script.

You need to have a policy in place that allows script execution. I don't want/have that on my system but it might be helpful for developers who want a fully integrated solution.

Put the following target in your .csproj file:

```csharp
  <Target Name="PostBuild" AfterTargets="PostBuildEvent">
    <Exec Command="powershell .\tailwindwatch.ps1" />
  </Target>
```

And the following PowerShell code into your *tailwindwatch.ps1* file:
```bash
TASKLIST | FINDSTR tailwindcss.exe || .\..\..\Tailwind\tailwindcss.exe -i .\Styles\tailwind-app.css -o .\wwwroot\tailwind-app.css --watch
```
