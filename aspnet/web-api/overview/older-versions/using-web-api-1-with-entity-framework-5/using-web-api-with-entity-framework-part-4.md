---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: "Teil 4: Hinzufügen der Ansicht für ein Admin | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2960eee37201655a9e4632bf0196ba18a0e2e82a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="c458e-102">Teil 4: Hinzufügen einer-Administratoransicht</span><span class="sxs-lookup"><span data-stu-id="c458e-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="c458e-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c458e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c458e-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="c458e-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="c458e-105">Fügen Sie eine Administrator-Ansicht</span><span class="sxs-lookup"><span data-stu-id="c458e-105">Add an Admin View</span></span>

<span data-ttu-id="c458e-106">Nun wir aktivieren Sie auf der Clientseite und fügen Sie eine Seite, die Daten aus den Admin-Controller verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="c458e-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="c458e-107">Die Seite ermöglichen es Benutzern zu erstellen, bearbeiten oder Löschen von Produkten, per AJAX-Anforderungen an den Controller.</span><span class="sxs-lookup"><span data-stu-id="c458e-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="c458e-108">Im Projektmappen-Explorer erweitern Sie den Ordner Controller, und öffnen Sie die Datei mit dem Namen HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="c458e-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="c458e-109">Diese Datei enthält einen MVC-Controller.</span><span class="sxs-lookup"><span data-stu-id="c458e-109">This file contains an MVC controller.</span></span> <span data-ttu-id="c458e-110">Fügen Sie eine Methode mit dem Namen `Admin`:</span><span class="sxs-lookup"><span data-stu-id="c458e-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="c458e-111">Die **HttpRouteUrl** Methode erstellt den URI der Web-API, und wir dies in den ansichtsbehälter für die spätere speichern.</span><span class="sxs-lookup"><span data-stu-id="c458e-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="c458e-112">Als Nächstes positionieren Sie den Textcursor innerhalb der `Admin` Aktionsmethode, und klicken Sie dann mit der rechten Maustaste und wählen Sie **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="c458e-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="c458e-113">Hierdurch erscheint der **Ansicht hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="c458e-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="c458e-114">In der **Ansicht hinzufügen** Dialogfeld Name der Ansicht "Admin".</span><span class="sxs-lookup"><span data-stu-id="c458e-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="c458e-115">Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**.</span><span class="sxs-lookup"><span data-stu-id="c458e-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="c458e-116">Klicken Sie unter **Modellklasse**, wählen Sie "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="c458e-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="c458e-117">Behalten Sie alle anderen Optionen die Standardwerte bei.</span><span class="sxs-lookup"><span data-stu-id="c458e-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="c458e-118">Auf **hinzufügen** Fügt eine Datei mit dem Namen Admin.cshtml unter Ansichten/Start hinzu.</span><span class="sxs-lookup"><span data-stu-id="c458e-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="c458e-119">Öffnen Sie diese Datei, und fügen Sie folgenden HTML-Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="c458e-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="c458e-120">Folgenden HTML-Code definiert die Struktur der Seite, jedoch keine Funktionalität läuft noch.</span><span class="sxs-lookup"><span data-stu-id="c458e-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="c458e-121">Erstellen Sie eine Verknüpfung zur Seite "Administrator"</span><span class="sxs-lookup"><span data-stu-id="c458e-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="c458e-122">Klicken Sie im Projektmappen-Explorer erweitern Sie den Ordner Views, und erweitern Sie dann den Ordner Shared.</span><span class="sxs-lookup"><span data-stu-id="c458e-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="c458e-123">Öffnen Sie die Datei mit dem Namen \_Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="c458e-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="c458e-124">Suchen Sie die **Ul** Element mit Id = "im Menü" und einen Aktionslink für die Admin-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="c458e-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="c458e-125">In das Beispielprojekt vorgenommen ich einigen andere kosmetischen Änderungen, z. B. die Zeichenfolge "Ihr Logo hier einfügen" ersetzt.</span><span class="sxs-lookup"><span data-stu-id="c458e-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="c458e-126">Diese nicht auf die Funktionalität der Anwendung auswirken.</span><span class="sxs-lookup"><span data-stu-id="c458e-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="c458e-127">Sie können das Projekt herunterladen und vergleichen Sie die Dateien.</span><span class="sxs-lookup"><span data-stu-id="c458e-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="c458e-128">Führen Sie die Anwendung, und klicken Sie auf den Link "Admin", der am oberen Rand auf der Startseite angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="c458e-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="c458e-129">Die Seite "Administrator" sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="c458e-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="c458e-130">Rechts jetzt die Seite keine Auswirkung.</span><span class="sxs-lookup"><span data-stu-id="c458e-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="c458e-131">Im nächsten Abschnitt werden wir Sie Knockout.js verwenden, um einer dynamischen Benutzeroberfläche zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c458e-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="c458e-132">Hinzufügen von Autorisierung</span><span class="sxs-lookup"><span data-stu-id="c458e-132">Add Authorization</span></span>

<span data-ttu-id="c458e-133">Die Seite "Administrator" wird aktuell für Personen, die auf der Website zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="c458e-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="c458e-134">Ändern Sie diese Option, um die Berechtigungen für Administratoren einschränken.</span><span class="sxs-lookup"><span data-stu-id="c458e-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="c458e-135">Durch Hinzufügen einer Rolle "Administrator" und einen Benutzer mit Administratorrechten starten.</span><span class="sxs-lookup"><span data-stu-id="c458e-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="c458e-136">Im Projektmappen-Explorer erweitern Sie den Filter-Ordner, und öffnen Sie die Datei mit dem Namen InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="c458e-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="c458e-137">Suchen Sie die `SimpleMembershipInitializer` Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="c458e-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="c458e-138">Nach dem Aufruf von **WebSecurity.InitializeDatabaseConnection**, fügen Sie den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="c458e-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="c458e-139">Dies ist eine schnelle Möglichkeit zum Hinzufügen der Rolle "Administrator", und erstellen Sie einen Benutzer für die Rolle.</span><span class="sxs-lookup"><span data-stu-id="c458e-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="c458e-140">Im Projektmappen-Explorer erweitern Sie den Ordner Controller, und öffnen Sie die Datei "HomeController.cs".</span><span class="sxs-lookup"><span data-stu-id="c458e-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="c458e-141">Hinzufügen der **autorisieren** -Attribut auf die `Admin` Methode.</span><span class="sxs-lookup"><span data-stu-id="c458e-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="c458e-142">Öffnen Sie die Datei AdminController.cs und Hinzufügen der **autorisieren** -Attribut auf die gesamte `AdminController` Klasse.</span><span class="sxs-lookup"><span data-stu-id="c458e-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="c458e-143">MVC und Web-API, die beide definieren **autorisieren** Attribute, die in verschiedenen Namespaces.</span><span class="sxs-lookup"><span data-stu-id="c458e-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="c458e-144">MVC verwendet **System.Web.Mvc.AuthorizeAttribute**, während der Web-API verwendet **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="c458e-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="c458e-145">Nur Administratoren können jetzt die Seite "Administrator" anzeigen.</span><span class="sxs-lookup"><span data-stu-id="c458e-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="c458e-146">Wenn Sie eine HTTP-Anforderung an den Controller Admin senden, muss die Anforderung außerdem ein Authentifizierungscookie enthalten.</span><span class="sxs-lookup"><span data-stu-id="c458e-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="c458e-147">Wenn dies nicht der Fall ist, wird der Server sendet eine HTTP 401 (nicht autorisiert)-Antwort.</span><span class="sxs-lookup"><span data-stu-id="c458e-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="c458e-148">Sehen Sie dies in Fiddler durch Senden einer GET-Anforderung an `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="c458e-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c458e-149">[Zurück](using-web-api-with-entity-framework-part-3.md)
[Weiter](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="c458e-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>