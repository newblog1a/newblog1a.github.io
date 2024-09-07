---
published: true
---
## NeeView 

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net8.0-windows</TargetFramework>
    <Nullable>enable</Nullable>
    <UseWPF>true</UseWPF>
    <UseWindowsForms>true</UseWindowsForms>
```

https://bitbucket.org/neelabo/neeview/src/master/NeeView/NeeView.csproj

Is it OK to enable both Winforms and WPF for a .NET 6.0 application?
  https://stackoverflow.com/questions/71374091/is-it-ok-to-enable-both-winforms-and-wpf-for-a-net-6-0-application

## ImageGlass
```xml
﻿<Project Sdk="Microsoft.NET.Sdk">

    <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>net8.0-windows10.0.17763.0</TargetFramework>
        <Nullable>enable</Nullable>
        <UseWindowsForms>true</UseWindowsForms>
```

https://github.com/d2phap/ImageGlass/blob/develop/Source/ImageGlass/ImageGlass.csproj

### relative: webview

Webview2 is the UI for the Settings, Quick Setup, About, Check for Update dialog. You can use ImageGlass 9 without Webview2 (need to bypass the Quick Setup dialog), or feel free to use the version 8 or other apps.
  https://github.com/d2phap/ImageGlass/discussions/1700