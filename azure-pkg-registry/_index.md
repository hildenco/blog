# Using Azure Package Registry as private NuGet Repository


# Creating a NuGet Package
Creating a NuGet package is simple when we know clearly how to do it and use the right tools.

In short, we will need:
* Visual Studio Developer Command Prompt
* msbuild
* know a little how NuGet packages work

# Setting Package Properties
In Visual Studio, you can set these values in the project properties (right-click the project in Solution Explorer, choose Properties, and select the Package tab). You can also set these properties directly in the project files (.csproj).

We need two things in order to be able to produce a NuGet package from our project:
Add a package reference to our csproj file
Invoke msbuild with the -t:

Add a package reference to our project file
Open the csproj file and add the following <PropertyGroup> element inside the <ItemGroup> section:
<ItemGroup>
  <!-- ... -->
  <PackageReference Include="NuGet.Build.Tasks.Pack" Version="5.2.0"/>
  <!-- ... -->
</ItemGroup>

Now run the msbuild command below to restore the dependencies:
msbuild -t:restore

The command below 
msbuild <YOUR.sln> -t:pack

For more information, check this page / https://docs.microsoft.com/en-us/nuget/create-packages/creating-a-package-msbuild.

# Building
I recommend testing our build is by using msbuild. And the best way to do that is by using the Visual Studio command prompt. To open VS's command prompt, type developer on the start menu and open the developer command prompt.
