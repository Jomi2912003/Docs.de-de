---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Datenbindung an Accordion (c#) | Microsoft Docs
author: wenz
description: "Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel w deklariert..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a8250f58655b8fe8638d8e7a7b084ee9c33fe986
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-c"></a>Datenbindung an Accordion (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber die Bindung an eine Datenquelle bietet mehr Flexibilität.


## <a name="overview"></a>Übersicht

Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel auf der Seite selbst deklariert, aber die Bindung an eine Datenquelle bietet mehr Flexibilität.

## <a name="steps"></a>Schritte

Erstens ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([Https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)), und fügen die `AdventureWorks.mdf` Datenbankdatei.

In diesem Beispiel gehen wir davon aus, dass die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch die Standardeinstellungen. Wenn Ihr Setup abweicht, müssen Sie passen Sie die Verbindungsinformationen für die Datenbank.

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Fügen Sie dann eine Datenquelle auf der Seite "ein. Um eine begrenzte Menge von Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank. Wenn Sie die Visual Studio-Assistent zum Erstellen der Datenquelle verwenden, beachten Sie, dass ein Fehler in der aktuellen Version nicht den Tabellennamen Präfixlänge ist (`Vendor`) mit `Purchasing`. Das folgende Markup zeigt die richtige Syntax:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Beachten Sie den Namen (ID) der Datenquelle aus. Diese sehr Identifikation muss dann verwendet werden, der `DataSourceID` Eigenschaft des Steuerelements ' Accordion ':

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

Innerhalb des Steuerelements ' Accordion ' können Sie Vorlagen für verschiedene Teile des Steuerelements, einschließlich des Headers angeben (`<HeaderTemplate>`) und den Inhalt (`<ContentTemplate>`). In den folgenden Elementen Ausgabe nur die Daten aus der Datenquelle mithilfe der `DataBinder.Eval()` Methode:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Wenn die Seite geladen wird, muss die Datenquelle an die ' Accordion ' mit dem folgenden serverseitigen Code gebunden werden:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Um dieses Beispiel zu beenden, müssen Sie die beiden CSS-Klassen, auf die verwiesen wird, werden in das Steuerelement ' Accordion ' definieren (in ihren Eigenschaften `HeaderCssClass` und `ContentCssClass`). Fügen Sie das folgende Markup in der `<head>` Abschnitt der Seite:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Die Daten in der ' Accordion ' stammen, direkt aus der Datenquelle](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Die Daten in der ' Accordion ' stammen, direkt aus der Datenquelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](databinding-to-an-accordion-cs/_static/image3.png))

>[!div class="step-by-step"]
[Nächste](dynamically-adding-an-accordion-pane-cs.md)
