---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Erstellen der JavaScript-Client | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b397c5a413ae213c9b79da1c0e0626efe21c7e21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="create-the-javascript-client"></a><span data-ttu-id="48f6f-102">Erstellen der JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="48f6f-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="48f6f-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="48f6f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="48f6f-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="48f6f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="48f6f-105">In diesem Abschnitt erstellen Sie den Client für die Anwendung mit HTML, JavaScript, und die [Knockout.js](http://knockoutjs.com/) Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="48f6f-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="48f6f-106">Wir müssen die Client-app in Phasen entwerfen:</span><span class="sxs-lookup"><span data-stu-id="48f6f-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="48f6f-107">Zeigt eine Liste von Büchern.</span><span class="sxs-lookup"><span data-stu-id="48f6f-107">Showing a list of books.</span></span>
- <span data-ttu-id="48f6f-108">Ein Buch Details angezeigt.</span><span class="sxs-lookup"><span data-stu-id="48f6f-108">Showing a book detail.</span></span>
- <span data-ttu-id="48f6f-109">Hinzufügen eines neuen Buchs an.</span><span class="sxs-lookup"><span data-stu-id="48f6f-109">Adding a new book.</span></span>

<span data-ttu-id="48f6f-110">Die Bibliothek Knockout verwendet das Model-View-ViewModel (MVVM)-Muster:</span><span class="sxs-lookup"><span data-stu-id="48f6f-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="48f6f-111">Die **Modell** ist die serverseitige Darstellung der Daten in der Business-Domäne (in unserem Groß-/Kleinschreibung, Bücher und Autoren).</span><span class="sxs-lookup"><span data-stu-id="48f6f-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="48f6f-112">Die **Ansicht** ist die Darstellungsschicht (HTML).</span><span class="sxs-lookup"><span data-stu-id="48f6f-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="48f6f-113">Die **Ansichtsmodell** ist ein JavaScript-Objekt, das die Modelle enthält.</span><span class="sxs-lookup"><span data-stu-id="48f6f-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="48f6f-114">Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="48f6f-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="48f6f-115">Sie hat keine Kenntnis der HTML-Darstellung.</span><span class="sxs-lookup"><span data-stu-id="48f6f-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="48f6f-116">Stattdessen dar abstrakte Funktionen in der Ansicht, z. B. &quot;eine Liste von Büchern&quot;.</span><span class="sxs-lookup"><span data-stu-id="48f6f-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="48f6f-117">Die Ansicht für das Ansichtsmodell datengebunden ist.</span><span class="sxs-lookup"><span data-stu-id="48f6f-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="48f6f-118">Updates für das ViewModel werden automatisch in der Sicht widergespiegelt.</span><span class="sxs-lookup"><span data-stu-id="48f6f-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="48f6f-119">Das Ansichtsmodell ruft auch Ereignisse aus der Ansicht ab, wie z. B. Schaltfläche klickt.</span><span class="sxs-lookup"><span data-stu-id="48f6f-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="48f6f-120">Dieser Ansatz erleichtert so ändern Sie das Layout und die Benutzeroberfläche der app, da Sie die Bindungen ändern können, ohne Code umzuschreiben.</span><span class="sxs-lookup"><span data-stu-id="48f6f-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="48f6f-121">Sie können z. B. eine Liste von Elementen als Anzeigen einer `<ul>`, ändern Sie ihn später in einer Tabelle.</span><span class="sxs-lookup"><span data-stu-id="48f6f-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="48f6f-122">Fügen Sie der Bibliothek Knockout hinzu</span><span class="sxs-lookup"><span data-stu-id="48f6f-122">Add the Knockout Library</span></span>

<span data-ttu-id="48f6f-123">In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**.</span><span class="sxs-lookup"><span data-stu-id="48f6f-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="48f6f-124">Wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="48f6f-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="48f6f-125">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="48f6f-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="48f6f-126">Mit diesem Befehl wird der Ordner "Skripts" Knockout Dateien hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="48f6f-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="48f6f-127">Erstellen des Modells anzeigen</span><span class="sxs-lookup"><span data-stu-id="48f6f-127">Create the View Model</span></span>

<span data-ttu-id="48f6f-128">Fügen Sie eine JavaScript-Datei mit dem Namen "App.js", um den Ordner "Skripts" hinzu.</span><span class="sxs-lookup"><span data-stu-id="48f6f-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="48f6f-129">(Im Projektmappen-Explorer mit der Maustaste den Ordner "Skripts", wählen Sie **hinzufügen**, und wählen Sie dann **JavaScript-Datei**.) Fügen Sie in den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="48f6f-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="48f6f-130">In Knockout die `observable` Klasse ermöglicht die Datenbindung.</span><span class="sxs-lookup"><span data-stu-id="48f6f-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="48f6f-131">Wenn der Inhalt der Observable-Objekt ändern, benachrichtigt der Observable-Objekt alle datengebundenen Steuerelemente, damit selbst aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="48f6f-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="48f6f-132">(Die `observableArray` Klasse ist die Array-Version des *Observable*.) Zunächst weist unsere Ansichtsmodell zwei Wahrnehmbare Elemente:</span><span class="sxs-lookup"><span data-stu-id="48f6f-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="48f6f-133">`books`enthält die Liste der Bücher an.</span><span class="sxs-lookup"><span data-stu-id="48f6f-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="48f6f-134">`error`enthält eine Fehlermeldung, wenn ein AJAX-Aufruf ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="48f6f-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="48f6f-135">Die `getAllBooks` Methode ist einen AJAX-Aufruf zum Abrufen der Liste von Büchern.</span><span class="sxs-lookup"><span data-stu-id="48f6f-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="48f6f-136">Und es sich um das Ergebnis auf verlagert die `books` Array.</span><span class="sxs-lookup"><span data-stu-id="48f6f-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="48f6f-137">Die `ko.applyBindings` Methode ist Teil der Knockout-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="48f6f-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="48f6f-138">Wird das Ansichtsmodell als Parameter, die dem Datenbindung eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="48f6f-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="48f6f-139">Fügen Sie ein Skript-Paket hinzu</span><span class="sxs-lookup"><span data-stu-id="48f6f-139">Add a Script Bundle</span></span>

<span data-ttu-id="48f6f-140">Bundling ist ein Feature in ASP.NET 4.5, die ganz einfach kombinieren oder mehrere Dateien in einer einzelnen Datei bündeln.</span><span class="sxs-lookup"><span data-stu-id="48f6f-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="48f6f-141">Bundling reduziert die Anzahl der Anforderungen an den Server, der Seitenladezeit verbessert werden kann.</span><span class="sxs-lookup"><span data-stu-id="48f6f-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="48f6f-142">Öffnen Sie die Datei App\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="48f6f-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="48f6f-143">Fügen Sie den folgenden Code, um die RegisterBundles-Methode.</span><span class="sxs-lookup"><span data-stu-id="48f6f-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

>[!div class="step-by-step"]
<span data-ttu-id="48f6f-144">[Zurück](part-5.md)
[Weiter](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="48f6f-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>