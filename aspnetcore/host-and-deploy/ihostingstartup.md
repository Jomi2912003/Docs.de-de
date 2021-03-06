---
title: "Hinzufügen von app-Funktionen aus einer externen Assembly IHostingStartup in ASP.NET Core"
author: guardrex
description: "Zum Hinzufügen von Funktionen zu einer ASP.NET Core-app aus einer externen Assembly mit einer Implementierung IHostingStartup zu ermitteln."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: e4b6293aff9fa39b70af40507a2cf5b7efcb295b
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a>Hinzufügen von app-Funktionen aus einer externen Assembly IHostingStartup in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

Ein [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) Implementierung ermöglicht das Hinzufügen von Funktionen zu einer app beim Start von außerhalb der app `Startup` Klasse. Eine Bibliothek von externen Tools können z. B. eine `IHostingStartup` Implementierung, um zusätzliche Konfigurationsanbieter oder Dienste für eine app bereitzustellen. `IHostingStartup`*in ASP.NET Core 2.0 und höher verfügbar ist.*

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Ermitteln der geladenen Startassemblys für hosting

Um zu ermitteln, ob von der app oder von Bibliotheken hosting Startassemblys geladen werden, aktivieren Sie die Protokollierung, und überprüfen Sie die Anwendungsprotokolle. Fehler beim Laden von Assemblys werden protokolliert. Geladene hosting Autostart-Assemblys werden auf der Debug-Ebene protokolliert, und alle Fehler werden protokolliert.

Die Beispiel-app lautet der [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) in einem `string` array erstellt und das Ergebnis wird in der app-Indexseite angezeigt:

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Deaktivieren Sie automatische Laden von hosting Autostart-Assemblys

Es gibt zwei Möglichkeiten, um das automatische Laden von hosting Startassemblys zu deaktivieren:

* Legen Sie die [Hosting-Start zu verhindern, dass](xref:fundamentals/hosting#prevent-hosting-startup) Konfigurationseinstellung hosten.
* Legen Sie die `ASPNETCORE_preventHostingStartup` -Umgebungsvariablen angegeben.

Wenn die Host-Einstellung oder die Umgebungsvariable auf festgelegt ist `true` oder `1`, hosting Autostart-Assemblys werden nicht automatisch geladen. Wenn beide festgelegt sind, steuert die hosteinstellung das Verhalten an.

Deaktivieren hosting Startassemblys, die mit der Einstellung oder Umgebung Hostvariable sie global deaktiviert und kann mehrere Funktionen eine app zu deaktivieren. Es ist nicht aktuell möglich, einem hosting Startassembly, die von einer Bibliothek hinzugefügt werden, es sei denn, die Bibliothek einen eigenen Konfigurationsoption bietet selektiv zu deaktivieren. Eine zukünftige Version wird bieten die Möglichkeit, hosting Startassemblys selektiv zu deaktivieren (siehe [GitHub ausstellen Aspnet/Hosting-1243](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup-features"></a>Implementieren Sie IHostingStartup-Funktionen

### <a name="create-the-assembly"></a>Erstellen Sie die assembly

Ein `IHostingStartup` Funktion wird als eine Assembly basierend auf einer Konsolen-app ohne einen Einstiegspunkt bereitgestellt. Die Assemblyverweise der [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) Paket:

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

Ein [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) Attribut identifiziert eine Klasse als eine Implementierung der `IHostingStartup` für das Laden und ausführen, die beim Erstellen der [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost). Im folgenden Beispiel wird der Namespace `StartupFeature`, und die Klasse ist `StartupFeatureHostingStartup`:

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

Eine Klasse implementiert `IHostingStartup`. Der Klasse [konfigurieren](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) -Methode verwendet ein [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) Funktionen eine app hinzufügen:

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

Beim Erstellen eine `IHostingStartup` -Projekt die Abhängigkeitsdatei (*\*. deps.json*) legt die `runtime` Speicherort der Assembly, die die *"bin"* Ordner:

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

Es wird nur ein Teil der Datei angezeigt. Der Name der Assembly im Beispiel wird `StartupFeature`.

### <a name="update-the-dependencies-file"></a>Aktualisieren Sie die Abhängigkeitsdatei

Die Common Language Runtime-Speicherort angegeben, der  *\*. deps.json* Datei. Aktiviert die Funktion der `runtime` Element muss den Speicherort des Features Common Language Runtime-Assembly angeben. Präfix der `runtime` Speicherort mit `lib/netcoreapp2.0/`:

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

In der Beispiel-app, Änderung von der  *\*. deps.json* Datei erfolgt durch eine [PowerShell](/powershell/scripting/powershell-scripting) Skript. Das PowerShell-Skript wird automatisch durch einen Build-Ziel in der Projektdatei ausgelöst.

### <a name="feature-activation"></a>Die Aktivierung der Funktion

**Legen Sie die Assemblydatei**

Die `IHostingStartup` der Implementierung Assemblydatei muss *"bin"*-in die app bereitgestellt oder in der [Common Language Runtime Speicher](/dotnet/core/deploying/runtime-store):

Platzieren Sie für pro-Benutzer verwenden die Assembly in das Benutzerprofil Common Language Runtime Speicher an:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Platzieren Sie die Assembly in der .NET Core-Installation Common Language Runtime Speicher, für die globale Verwendung:

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

Beim Bereitstellen der Assemblys auf die Common Language Runtime Speicher wird die Symboldatei möglicherweise ebenfalls bereitgestellt werden, jedoch nicht erforderlich, damit die Funktion ordnungsgemäß arbeitet.

**Legen Sie die Abhängigkeitsdatei**

Der Implementierung  *\*. deps.json* Datei muss unter einem zugänglichen Pfad sein.

Legen Sie für die Verwendung von pro Benutzer, die Datei in die `additonalDeps` Ordner, der des Benutzerprofils `.dotnet` Einstellungen: 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Für globale verwenden, legen Sie die Datei in die `additonalDeps` Ordner der .NET Core-Installation:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

Beachten Sie die Version `2.0.0`, gibt die Version der freigegebenen Runtime, die die Ziel-app verwendet. Die freigegebene Runtime wird angezeigt, der  *\*. runtimeconfig.json* Datei. In der Beispiel-app-freigegebene Runtime wird angegeben, der *HostingStartupSample.runtimeconfig.json* Datei.

**Umgebungsvariablen festlegen**

Legen Sie die folgenden Umgebungsvariablen im Kontext der app, die die Funktion verwendet.

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

Nur hosting Autostart-Assemblys werden gescannt, für die `HostingStartupAttribute`. Der Assemblyname der Implementierung wird in dieser Umgebungsvariable bereitgestellt. Die Beispiel-app wird dieser Wert auf `StartupDiagnostics`.

Der Wert kann auch festgelegt werden mithilfe der [Hosting Startassemblys](xref:fundamentals/hosting#hosting-startup-assemblies) Konfigurationseinstellung hosten.

DOTNET\_ZUSÄTZLICHE\_EINZAHLGN

Der Speicherort für der Implementierung  *\*. deps.json* Datei.

Wenn die Datei in des Benutzerprofils befindet *.dotnet* Ordners zur Verwendung von pro Benutzer:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Wenn die Datei in der .NET Core-Installation für die Verwendung von globalen platziert wird, geben Sie den vollständigen Pfad zur Datei:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

Die Beispiel-app wird dieser Wert auf:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Beispiele zum Festlegen von Umgebungsvariablen für verschiedene Betriebssysteme finden Sie in [arbeiten mit mehreren Umgebungen](xref:fundamentals/environments).

## <a name="sample-app"></a>Beispiel-app

Die [Beispielapp](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)) verwendet `IHostingStartup` So erstellen ein Diagnosetool. Das Tool hinzugefügt die app starten, die Diagnoseinformationen bieten zwei Middlewares:

* Registrierte Dienste
* Adresse: Schema, Host, Pfad Basis, Pfad, einer Abfragezeichenfolge
* Verbindung: remote-IP-, der Remoteport, lokale IP-Adresse Lokaler Port, Clientzertifikat
* Anforderungsheader
* Umgebungsvariablen

Um das Beispiel auszuführen:

1. Das Start-Diagnose Projekt verwendet [PowerShell](/powershell/scripting/powershell-scripting) so ändern Sie die *StartupDiagnostics.deps.json* Datei. PowerShell ist standardmäßig auf Windows-Betriebssystemen ab Windows 7 SP1 und Windows Server 2008 R2 SP1 installiert. Um PowerShell auf anderen Plattformen zu erhalten, finden Sie unter [Installieren von Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).
2. Buildprojekt Diagnose starten. Ein Build-Ziel in der Projektdatei:
   * Verschiebt die Assembly und Symboldateien das Benutzerprofil Common Language Runtime Speicher.
   * Löst das PowerShell-Skript zum Ändern der *StartupDiagnostics.deps.json* Datei.
   * Verschiebt die *StartupDiagnostics.deps.json* Datei am Benutzerprofil `additionalDeps` Ordner.
3. Festlegen der Umgebungsvariablen:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Führen Sie die Beispiel-app.
5. Anfordern der `/services` finden in der app-Endpunkt registriert Dienste. Anfordern der `/diag` Endpunkt, um die Diagnoseinformationen anzuzeigen.
