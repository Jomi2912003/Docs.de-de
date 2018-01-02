---
title: "Razor-Seiten mit EF-Core - lesen verknüpften Daten - 6 8"
author: rick-anderson
description: "In diesem Lernprogramm lesen und Anzeigen von verknüpften Daten – d. h. die Daten, die das Entity Framework in Navigationseigenschaften lädt."
keywords: "ASP.NET Core, Entity Framework Core, verknüpfte Daten verknüpft."
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ba9b17ecdcb605d39117d03230b1db37e8e4d0dd
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2017
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="5396c-104">Lesen bezogene Daten per Push – EF-Core mit Razor-Seiten (6 von 8)</span><span class="sxs-lookup"><span data-stu-id="5396c-104">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="5396c-105">Durch [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5396c-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="5396c-106">In diesem Lernprogramm werden Daten gelesen und angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5396c-106">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="5396c-107">Verwandte Daten sind Daten, die in den Navigationseigenschaften EF Core lädt.</span><span class="sxs-lookup"><span data-stu-id="5396c-107">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="5396c-108">Wenn Probleme können nicht zu lösen auftreten, laden Sie die [abgeschlossene Anwendung für diese Stufe](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="5396c-108">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="5396c-109">Die folgenden Abbildungen zeigen die abgeschlossenen Seiten für dieses Lernprogramm:</span><span class="sxs-lookup"><span data-stu-id="5396c-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Kurse Indexseite](read-related-data/_static/courses-index.png)

![Indexseite für Dozenten](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="5396c-112">Eager, explizite und lazy Loading-verknüpfter Daten</span><span class="sxs-lookup"><span data-stu-id="5396c-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="5396c-113">Es gibt mehrere Möglichkeiten, EF Core in die Navigationseigenschaften einer Entität verknüpfte Daten geladen werden können:</span><span class="sxs-lookup"><span data-stu-id="5396c-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="5396c-114">[Unverzüglichem Laden](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="5396c-114">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="5396c-115">Unverzüglichem Laden ist eine Abfrage für einen Entitätstyp auch verknüpfte Entitäten lädt.</span><span class="sxs-lookup"><span data-stu-id="5396c-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="5396c-116">Wenn die Entität gelesen wird, werden ihre verknüpften Daten abgerufen.</span><span class="sxs-lookup"><span data-stu-id="5396c-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="5396c-117">Dies führt normalerweise zu einer einzelnen Join-Abfrage, die alle Daten abruft, die erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5396c-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="5396c-118">EF Core werden mehrere Abfragen für einige Typen von unverzüglichem Laden ausgeben.</span><span class="sxs-lookup"><span data-stu-id="5396c-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="5396c-119">Mehrere Abfragen ausgeben kann effizienter sein, als bei einigen Abfragen in EF6 der Fall war, obwohl eine einzelne Abfrage vorhanden war.</span><span class="sxs-lookup"><span data-stu-id="5396c-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="5396c-120">Unverzüglichem Laden wird angegeben, mit der `Include` und `ThenInclude` Methoden.</span><span class="sxs-lookup"><span data-stu-id="5396c-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Beispiel mit unverzüglichem Laden](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="5396c-122">Unverzüglichem Laden sendet mehrere Abfragen aus, wenn eine Auflistung Nvavigation enthalten ist:</span><span class="sxs-lookup"><span data-stu-id="5396c-122">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="5396c-123">Eine Abfrage für die Hauptabfrage</span><span class="sxs-lookup"><span data-stu-id="5396c-123">One query for the main query</span></span> 
 * <span data-ttu-id="5396c-124">Eine Abfrage für jede Sammlung "Rand" in der Struktur laden.</span><span class="sxs-lookup"><span data-stu-id="5396c-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="5396c-125">Trennen Sie Abfragen mit `Load`: die Daten in eine eigene Abfrage abgerufen werden können, und EF Core "Korrekturen einrichten" die Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="5396c-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="5396c-126">"Korrekturen einrichten" bedeutet, dass es sich bei EF Core füllt automatisch die Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="5396c-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="5396c-127">Trennen Sie Abfragen mit `Load` detaillierter Explicit laden als unverzüglichem Laden.</span><span class="sxs-lookup"><span data-stu-id="5396c-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Beispiel für eine eigene Abfrage](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="5396c-129">Hinweis: EF Core behebt automatisch von Navigationseigenschaften für alle anderen Entitäten, die zuvor in die Kontextinstanz geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="5396c-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="5396c-130">Auch wenn die Daten für eine Navigationseigenschaft ist *nicht* explizit eingeschlossen, die Eigenschaft kann noch immer aufgefüllt werden, wenn einige oder alle verknüpften Entitäten wurden zuvor geladen.</span><span class="sxs-lookup"><span data-stu-id="5396c-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="5396c-131">[Explizites Laden](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="5396c-131">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="5396c-132">Wenn die Entität zuerst gelesen wird, ist nicht verbundene Daten abgerufen.</span><span class="sxs-lookup"><span data-stu-id="5396c-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="5396c-133">Abrufen der zugehörigen Daten bei Bedarf muss Code geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="5396c-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="5396c-134">Explizites Laden mit eine eigene Abfrage führt zu mehreren Abfragen an die Datenbank gesendet.</span><span class="sxs-lookup"><span data-stu-id="5396c-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="5396c-135">Mit expliziten Laden gibt der Code die Navigationseigenschaften geladen werden.</span><span class="sxs-lookup"><span data-stu-id="5396c-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="5396c-136">Verwenden der `Load` Methode, um das explizite laden auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5396c-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="5396c-137">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="5396c-137">For example:</span></span>

 ![Explizites Laden-Beispiel](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="5396c-139">[Verzögertes Laden](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="5396c-139">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="5396c-140">[EF Core unterstützt derzeit keine verzögertes Laden](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="5396c-140">[EF Core does not currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="5396c-141">Wenn die Entität zuerst gelesen wird, ist nicht verbundene Daten abgerufen.</span><span class="sxs-lookup"><span data-stu-id="5396c-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="5396c-142">Eine Navigationseigenschaft zugegriffen wird, das zum ersten Mal werden automatisch für diese Navigationseigenschaft erforderlichen Daten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="5396c-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="5396c-143">Mit der Datenbank wird eine Abfrage gesendet jedes Mal eine Navigationseigenschaft zum ersten Mal zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="5396c-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="5396c-144">Die `Select` Operator lädt nur die verknüpften Daten erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="5396c-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="5396c-145">Erstellen Sie eine Kurse-Seite, die Name der Abteilung anzeigt</span><span class="sxs-lookup"><span data-stu-id="5396c-145">Create a Courses page that displays department name</span></span>

<span data-ttu-id="5396c-146">Die Kurs-Entität enthält, eine Navigationseigenschaft, die enthält die `Department` Entität.</span><span class="sxs-lookup"><span data-stu-id="5396c-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="5396c-147">Die `Department` Entität enthält die Abteilung, die der Kurs zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="5396c-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="5396c-148">So zeigen Sie den Namen der zugewiesenen Abteilung in einer Liste von Kurse an</span><span class="sxs-lookup"><span data-stu-id="5396c-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="5396c-149">Abrufen der `Name` Eigenschaft aus der `Department` Entität.</span><span class="sxs-lookup"><span data-stu-id="5396c-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="5396c-150">Die `Department` Entität stammen aus den `Course.Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="5396c-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Ourse. Abteilung](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="5396c-152">Das Gerüst für die Kurs-Modell erstellen</span><span class="sxs-lookup"><span data-stu-id="5396c-152">Scaffold the Course model</span></span>

* <span data-ttu-id="5396c-153">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5396c-153">Exit Visual Studio.</span></span>
* <span data-ttu-id="5396c-154">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="5396c-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5396c-155">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="5396c-155">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="5396c-156">Die vorherigen Befehl Gerüste der `Course` Modell.</span><span class="sxs-lookup"><span data-stu-id="5396c-156">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="5396c-157">Öffnen Sie das Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5396c-157">Open the project in Visual Studio.</span></span>

<span data-ttu-id="5396c-158">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="5396c-158">Build the project.</span></span> <span data-ttu-id="5396c-159">Der Build generiert Fehler wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5396c-159">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="5396c-160">Globales ändern `_context.Course` auf `_context.Courses` (d. h. hinzufügen ein "s" `Course`).</span><span class="sxs-lookup"><span data-stu-id="5396c-160">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="5396c-161">7 Vorkommen gefunden und aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="5396c-161">7 occurrences are found and updated.</span></span>

<span data-ttu-id="5396c-162">Open *Pages/Courses/Index.cshtml.cs* und untersuchen Sie die `OnGetAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="5396c-162">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="5396c-163">Das Modul Gerüstbau angegeben unverzüglichem Laden für die `Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="5396c-163">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="5396c-164">Die `Include` Methode gibt unverzüglichem Laden.</span><span class="sxs-lookup"><span data-stu-id="5396c-164">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="5396c-165">Führen Sie die app, und wählen Sie die **Kurse** Link.</span><span class="sxs-lookup"><span data-stu-id="5396c-165">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="5396c-166">Department-Spalte zeigt die `DepartmentID`, was nicht nützlich ist.</span><span class="sxs-lookup"><span data-stu-id="5396c-166">The department column displays the `DepartmentID`, which is not useful.</span></span>

<span data-ttu-id="5396c-167">Aktualisieren Sie die `OnGetAsync`-Methode mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="5396c-167">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="5396c-168">Der vorangehende Code fügt `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="5396c-168">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="5396c-169">`AsNoTracking`verbessert die Leistung, da die zurückgegebenen Entitäten nicht nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="5396c-169">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="5396c-170">Die Entitäten werden nicht nachverfolgt werden, da sie nicht im aktuellen Kontext aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="5396c-170">The entities are not tracked because they are not updated in the current context.</span></span>

<span data-ttu-id="5396c-171">Update *Views/Courses/Index.cshtml* mit dem folgenden hervorgehobenen Markup:</span><span class="sxs-lookup"><span data-stu-id="5396c-171">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="5396c-172">An den scaffolded Code wurden die folgenden Änderungen vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="5396c-172">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="5396c-173">Die Überschrift von Index in Courses geändert.</span><span class="sxs-lookup"><span data-stu-id="5396c-173">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="5396c-174">Hinzugefügt eine **Anzahl** Spalte, die zeigt die `CourseID` Eigenschaftswert.</span><span class="sxs-lookup"><span data-stu-id="5396c-174">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="5396c-175">Primärschlüssel werden nicht standardmäßig Gerüstbau, da normalerweise sie Endbenutzern bedeutungslos sind.</span><span class="sxs-lookup"><span data-stu-id="5396c-175">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="5396c-176">Allerdings ist in diesem Fall der Primärschlüssel sinnvoll.</span><span class="sxs-lookup"><span data-stu-id="5396c-176">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="5396c-177">Geändert die **Abteilung** Spalte der Abteilungsname angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5396c-177">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="5396c-178">Der Code zeigt die `Name` Eigenschaft von der `Department` Entität, die geladen wird die `Department` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="5396c-178">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="5396c-179">Führen Sie die app, und wählen Sie die **Kurse** Registerkarte ", um die Liste mit den Abteilungsnamen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5396c-179">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurse Indexseite](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="5396c-181">Laden von verknüpften Daten mit Select</span><span class="sxs-lookup"><span data-stu-id="5396c-181">Loading related data with Select</span></span>

<span data-ttu-id="5396c-182">Die `OnGetAsync` Methode lädt verknüpfte Daten mit der `Include` Methode:</span><span class="sxs-lookup"><span data-stu-id="5396c-182">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="5396c-183">Die `Select` Operator lädt nur die verknüpften Daten erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="5396c-183">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="5396c-184">Für die einzelnen Elemente wie die `Department.Name` verwendet eine SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="5396c-184">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="5396c-185">Für Auflistungen verwendet eine andere Datenbank zugreifen, aber Gleiches gilt für die.`Include`</span><span class="sxs-lookup"><span data-stu-id="5396c-185">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="5396c-186">Operator für Auflistungen.</span><span class="sxs-lookup"><span data-stu-id="5396c-186">operator on collections.</span></span>

<span data-ttu-id="5396c-187">Der folgende Code lädt verknüpfte Daten mit der `Select` Methode:</span><span class="sxs-lookup"><span data-stu-id="5396c-187">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="5396c-188">Die `CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="5396c-188">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="5396c-189">Finden Sie unter [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) und [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) ein vollständiges Beispiel.</span><span class="sxs-lookup"><span data-stu-id="5396c-189">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="5396c-190">Erstellen Sie eine Lehrkräfte-Seite, in der Kurse und Registrierung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5396c-190">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="5396c-191">In diesem Abschnitt wird die Seite "Lehrkräfte" erstellt.</span><span class="sxs-lookup"><span data-stu-id="5396c-191">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="5396c-192">![Indexseite für Dozenten](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="5396c-192">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="5396c-193">Auf dieser Seite liest und zeigt verknüpfte Daten auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="5396c-193">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="5396c-194">Die Liste der Lehrkräfte zeigt verknüpfte Daten aus der `OfficeAssignment` Entität (Office in der vorherigen Abbildung).</span><span class="sxs-lookup"><span data-stu-id="5396c-194">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="5396c-195">Die `Instructor` und `OfficeAssignment` Entitäten sind in einer 1: 0 (null)-oder-1-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="5396c-195">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="5396c-196">Unverzüglichem Laden dient für die `OfficeAssignment` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="5396c-196">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="5396c-197">Unverzüglichem Laden ist in der Regel effizienter, wenn die verknüpften Daten angezeigt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="5396c-197">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="5396c-198">In diesem Fall werden die Office-Zuweisungen für die Kursleiter angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5396c-198">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="5396c-199">Wenn der Benutzer wählt einen Kursleiter (Harui in der vorherigen Abbildung), im Zusammenhang `Course` Entitäten werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5396c-199">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="5396c-200">Die `Instructor` und `Course` Entitäten sind in einer m: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="5396c-200">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="5396c-201">Eager loading für die `Course` Entitäten und die zugehörigen `Department` Entitäten verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="5396c-201">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="5396c-202">In diesem Fall können eine eigene Abfrage effizienter sein, da nur Kurse für den ausgewählten Kursleiter benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="5396c-202">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="5396c-203">Dieses Beispiel zeigt, wie mit unverzüglichem Laden für Navigationseigenschaften in Entitäten, die in den Navigationseigenschaften sind.</span><span class="sxs-lookup"><span data-stu-id="5396c-203">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="5396c-204">Wenn der Benutzer einen Kurs (Chemie in der vorherigen Abbildung), wählt verknüpften Daten aus der `Enrollments` Entität wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5396c-204">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="5396c-205">In der vorherigen Abbildung sind Student-Name und Dienstqualität angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5396c-205">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="5396c-206">Die `Course` und `Enrollment` Entitäten sind in einer 1: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="5396c-206">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="5396c-207">Erstellen Sie ein Ansichtsmodell für die Indexansicht Dozenten</span><span class="sxs-lookup"><span data-stu-id="5396c-207">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="5396c-208">Die Seite "Lehrkräfte" zeigt Daten aus drei verschiedenen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="5396c-208">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="5396c-209">Einem Ansichtsmodell wird erstellt, die die drei Entitäten, die die drei Tabellen darstellt.</span><span class="sxs-lookup"><span data-stu-id="5396c-209">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="5396c-210">In der *SchoolViewModels* Ordner erstellen *InstructorIndexData.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="5396c-210">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="5396c-211">Die Instructor-Modell zu erstellen</span><span class="sxs-lookup"><span data-stu-id="5396c-211">Scaffold the Instructor model</span></span>

* <span data-ttu-id="5396c-212">Beenden Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5396c-212">Exit Visual Studio.</span></span>
* <span data-ttu-id="5396c-213">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="5396c-213">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5396c-214">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="5396c-214">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="5396c-215">Die vorherigen Befehl Gerüste der `Instructor` Modell.</span><span class="sxs-lookup"><span data-stu-id="5396c-215">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="5396c-216">Öffnen Sie das Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5396c-216">Open the project in Visual Studio.</span></span>

<span data-ttu-id="5396c-217">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="5396c-217">Build the project.</span></span> <span data-ttu-id="5396c-218">Der Build generiert Fehler.</span><span class="sxs-lookup"><span data-stu-id="5396c-218">The build generates errors.</span></span>

<span data-ttu-id="5396c-219">Globales ändern `_context.Instructor` auf `_context.Instructors` (d. h. hinzufügen ein "s" `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="5396c-219">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="5396c-220">7 Vorkommen gefunden und aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="5396c-220">7 occurrences are found and updated.</span></span>

<span data-ttu-id="5396c-221">Führen Sie die app, und navigieren Sie zu der Seite "Lehrkräfte".</span><span class="sxs-lookup"><span data-stu-id="5396c-221">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="5396c-222">Ersetzen Sie *Pages/Instructors/Index.cshtml.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="5396c-222">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="5396c-223">Die `OnGetAsync` -Methode akzeptiert optional Routendaten für die ID der ausgewählten Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="5396c-223">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="5396c-224">Untersuchen Sie die Abfrage auf die *Pages/Instructors/Index.cshtml* Seite:</span><span class="sxs-lookup"><span data-stu-id="5396c-224">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="5396c-225">Die Abfrage verfügt über zwei enthält:</span><span class="sxs-lookup"><span data-stu-id="5396c-225">The query has two includes:</span></span>

* <span data-ttu-id="5396c-226">`OfficeAssignment`: Die angezeigte in der [Lehrkräfte Ansicht](#IP).</span><span class="sxs-lookup"><span data-stu-id="5396c-226">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="5396c-227">`CourseAssignments`: Die in der Courses vermittelten bringt.</span><span class="sxs-lookup"><span data-stu-id="5396c-227">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="5396c-228">Aktualisieren Sie die Indexseite Dozenten</span><span class="sxs-lookup"><span data-stu-id="5396c-228">Update the instructors Index page</span></span>

<span data-ttu-id="5396c-229">Update *Pages/Instructors/Index.cshtml* durch Folgendes Markup:</span><span class="sxs-lookup"><span data-stu-id="5396c-229">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="5396c-230">Das vorhergehende Markup nimmt folgende Änderungen:</span><span class="sxs-lookup"><span data-stu-id="5396c-230">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="5396c-231">Updates der `page` -Direktive aus `@page` auf `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="5396c-231">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="5396c-232">`"{id:int?}"`ist eine routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="5396c-232">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="5396c-233">Die routenvorlage ändert Ganzzahl Abfragezeichenfolgen in der URL, um Routendaten.</span><span class="sxs-lookup"><span data-stu-id="5396c-233">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="5396c-234">Z. B. durch Klicken auf die **wählen** Link, um einen Kursleiter, wenn die Page-Anweisung eine URL wie die folgende erzeugt:</span><span class="sxs-lookup"><span data-stu-id="5396c-234">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="5396c-235">Wenn der Seitendirektive ist `@page "{id:int?}"`, wird die vorherige URL:</span><span class="sxs-lookup"><span data-stu-id="5396c-235">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="5396c-236">Seitentitel wird **Lehrkräfte**.</span><span class="sxs-lookup"><span data-stu-id="5396c-236">Page title is **Instructors**.</span></span>
* <span data-ttu-id="5396c-237">Hinzugefügt ein **Office** Spalte `item.OfficeAssignment.Location` nur, wenn `item.OfficeAssignment` ist ungleich null.</span><span class="sxs-lookup"><span data-stu-id="5396c-237">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="5396c-238">Da dies eine 1: 0 (null)-oder-1-Beziehung ist, gibt es möglicherweise nicht verknüpfte OfficeAssignment Entität.</span><span class="sxs-lookup"><span data-stu-id="5396c-238">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="5396c-239">Hinzugefügt eine **Kurse** Spalte Kurse vermittelten jedes Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="5396c-239">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="5396c-240">Finden Sie unter [explizite Zeile Übergang mit `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Weitere Informationen über diese Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="5396c-240">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="5396c-241">Code hinzugefügt, der dynamisch hinzugefügt `class="success"` auf die `tr` Element des ausgewählten Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="5396c-241">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="5396c-242">Hiermit wird eine Hintergrundfarbe für die ausgewählte Zeile mit einem Bootstrap-Klasse.</span><span class="sxs-lookup"><span data-stu-id="5396c-242">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="5396c-243">Einen neuen Link mit der Bezeichnung hinzugefügt **wählen**.</span><span class="sxs-lookup"><span data-stu-id="5396c-243">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="5396c-244">Dieser Link sendet die ausgewählten Instructor-ID, die die `Index` Methode und legt die Hintergrundfarbe fest.</span><span class="sxs-lookup"><span data-stu-id="5396c-244">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="5396c-245">Führen Sie die app, und wählen Sie die **Lehrkräfte** Registerkarte. Die Seite zeigt die `Location` (Office) aus den verknüpften `OfficeAssignment` Entität.</span><span class="sxs-lookup"><span data-stu-id="5396c-245">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="5396c-246">Wenn OfficeAssignment "ist null, wird eine leere Tabellenzelle angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5396c-246">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Lehrkräfte Indexseite, der keine Auswahl](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="5396c-248">Klicken Sie auf die **wählen** Link.</span><span class="sxs-lookup"><span data-stu-id="5396c-248">Click on the **Select** link.</span></span> <span data-ttu-id="5396c-249">Der Stil zeilenänderungen.</span><span class="sxs-lookup"><span data-stu-id="5396c-249">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="5396c-250">Fügen Sie ausgewählte Dozent unterrichtet Kurse hinzu</span><span class="sxs-lookup"><span data-stu-id="5396c-250">Add courses taught by selected instructor</span></span>

<span data-ttu-id="5396c-251">Update der `OnGetAsync` Methode im *Pages/Instructors/Index.cshtml.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="5396c-251">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="5396c-252">Überprüfen Sie die aktualisierte Abfrage:</span><span class="sxs-lookup"><span data-stu-id="5396c-252">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="5396c-253">Die vorhergehende Abfrage fügt die `Department` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="5396c-253">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="5396c-254">Der folgende Code, das ausgeführt wird, wenn Sie ein Kursleiter ausgewählt ist (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="5396c-254">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="5396c-255">Die ausgewählte Kursleiter wird aus der Liste der Kursleiter das Ansichtsmodell abgerufen.</span><span class="sxs-lookup"><span data-stu-id="5396c-255">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="5396c-256">Des Ansichtsmodells `Courses` Eigenschaft geladen wird, mit der `Course` Entitäten aus dieser Dozenten `CourseAssignments` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="5396c-256">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="5396c-257">Die `Where` Methode gibt eine Auflistung zurück.</span><span class="sxs-lookup"><span data-stu-id="5396c-257">The `Where` method returns a collection.</span></span> <span data-ttu-id="5396c-258">In den vorherigen `Where` -Methode, nur einen einzigen `Instructor` Entität zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="5396c-258">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="5396c-259">Die `Single` -Methode konvertiert die Auflistung in einem einzelnen `Instructor` Entität.</span><span class="sxs-lookup"><span data-stu-id="5396c-259">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="5396c-260">Die `Instructor` Entität ermöglicht den Zugriff auf die `CourseAssignments` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="5396c-260">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="5396c-261">`CourseAssignments`ermöglicht den Zugriff auf den zugehörigen `Course` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="5396c-261">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Instructor-Kurse-m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="5396c-263">Die `Single` Methode wird von einer Sammlung verwendet, wenn die Auflistung nur ein Element aufweist.</span><span class="sxs-lookup"><span data-stu-id="5396c-263">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="5396c-264">Die `Single` Methode löst eine Ausnahme aus, wenn die Auflistung leer ist oder wenn mehr als ein Element vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="5396c-264">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="5396c-265">Ist eine Alternative `SingleOrDefault`, womit einen Default-Wert (in diesem Fall null), wenn die Auflistung leer ist.</span><span class="sxs-lookup"><span data-stu-id="5396c-265">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="5396c-266">Mithilfe von `SingleOrDefault` für eine leere Auflistung:</span><span class="sxs-lookup"><span data-stu-id="5396c-266">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="5396c-267">Löst eine Ausnahme (aus beim Suchen nach einem `Courses` Eigenschaft auf einen null-Verweis).</span><span class="sxs-lookup"><span data-stu-id="5396c-267">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="5396c-268">Die Ausnahmemeldung würde weniger deutlich die Ursache des Problems angeben.</span><span class="sxs-lookup"><span data-stu-id="5396c-268">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="5396c-269">Der folgende Code füllt des Ansichtsmodells `Enrollments` Eigenschaft, wenn Sie ein Kurs ausgewählt ist:</span><span class="sxs-lookup"><span data-stu-id="5396c-269">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="5396c-270">Fügen Sie das folgende Markup bis zum Ende der *Pages/Courses/Index.cshtml* Razor-Seite:</span><span class="sxs-lookup"><span data-stu-id="5396c-270">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="5396c-271">Das vorhergehende Markup zeigt eine Liste der Kurse, die im Zusammenhang mit einen Kursleiter beim ein Kursleiter ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="5396c-271">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="5396c-272">Testen der app an.</span><span class="sxs-lookup"><span data-stu-id="5396c-272">Test the app.</span></span> <span data-ttu-id="5396c-273">Klicken Sie auf eine **wählen** Link auf der Seite "Lehrkräfte".</span><span class="sxs-lookup"><span data-stu-id="5396c-273">Click on a **Select** link on the instructors page.</span></span>

![Lehrkräfte Index Seite Dozenten ausgewählt](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="5396c-275">Student-Daten anzeigen</span><span class="sxs-lookup"><span data-stu-id="5396c-275">Show student data</span></span>

<span data-ttu-id="5396c-276">In diesem Abschnitt wird die app aktualisiert, um die Student-Daten für einen ausgewählten Kurs anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5396c-276">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="5396c-277">Aktualisieren Sie die Abfrage in der `OnGetAsync` Methode im *Pages/Instructors/Index.cshtml.cs* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="5396c-277">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="5396c-278">Update *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5396c-278">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="5396c-279">Fügen Sie am Ende der Datei das folgende Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="5396c-279">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="5396c-280">Das vorhergehende Markup zeigt eine Liste der Studenten, die registriert werden in den ausgewählten Kurs.</span><span class="sxs-lookup"><span data-stu-id="5396c-280">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="5396c-281">Aktualisieren Sie die Seite, und wählen Sie einen Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="5396c-281">Refresh the page and select an instructor.</span></span> <span data-ttu-id="5396c-282">Wählen Sie einen Kurs, finden in der Liste der registrierten Studenten und deren Qualitäten.</span><span class="sxs-lookup"><span data-stu-id="5396c-282">Select a course to see the list of enrolled students and their grades.</span></span>

![Lehrkräfte Index Seite Kursleiter und Kurs ausgewählt](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="5396c-284">Verwenden von einzelnen</span><span class="sxs-lookup"><span data-stu-id="5396c-284">Using Single</span></span>

<span data-ttu-id="5396c-285">Die `Single` Methode übergeben kann die `Where` Bedingung statt der `Where` Methode getrennt:</span><span class="sxs-lookup"><span data-stu-id="5396c-285">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="5396c-286">Die vorangehenden `Single` Ansatz bietet keine Vorteile gegenüber `Where`.</span><span class="sxs-lookup"><span data-stu-id="5396c-286">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="5396c-287">Einige Entwickler bevorzugen das `Single` Ansatz Stil.</span><span class="sxs-lookup"><span data-stu-id="5396c-287">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="5396c-288">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="5396c-288">Explicit loading</span></span>

<span data-ttu-id="5396c-289">Der aktuelle Code gibt unverzüglichem Laden für `Enrollments` und `Students`:</span><span class="sxs-lookup"><span data-stu-id="5396c-289">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="5396c-290">Angenommen, Benutzer selten Registrierungen in einen Kurs anzeigen möchten.</span><span class="sxs-lookup"><span data-stu-id="5396c-290">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="5396c-291">In diesem Fall wäre eine Optimierung nur die Registrierung Daten zu laden, wenn diese angefordert wird.</span><span class="sxs-lookup"><span data-stu-id="5396c-291">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="5396c-292">In diesem Abschnitt die `OnGetAsync` wird aktualisiert, um das explizite Laden von verwenden `Enrollments` und `Students`.</span><span class="sxs-lookup"><span data-stu-id="5396c-292">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="5396c-293">Update der `OnGetAsync` durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="5396c-293">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="5396c-294">Der vorangehende Code löscht die *ThenInclude* Methodenaufrufe für Registrierung und Student-Daten.</span><span class="sxs-lookup"><span data-stu-id="5396c-294">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="5396c-295">Wenn ein Kurs ausgewählt ist, ruft der hervorgehobene Code ab:</span><span class="sxs-lookup"><span data-stu-id="5396c-295">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="5396c-296">Die `Enrollment` Entitäten für den ausgewählten Kurs.</span><span class="sxs-lookup"><span data-stu-id="5396c-296">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="5396c-297">Die `Student` Entitäten für jede `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="5396c-297">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="5396c-298">Beachten Sie das vorherige Codebeispiel kommentiert `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="5396c-298">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="5396c-299">Navigationseigenschaften können nur für überwachte Entitäten explizit geladen werden.</span><span class="sxs-lookup"><span data-stu-id="5396c-299">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="5396c-300">Testen der app an.</span><span class="sxs-lookup"><span data-stu-id="5396c-300">Test the app.</span></span> <span data-ttu-id="5396c-301">Aus Benutzersicht mit dem Verhalten der app auf die vorherige Version.</span><span class="sxs-lookup"><span data-stu-id="5396c-301">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="5396c-302">Im nächste Lernprogramm zeigt, wie verknüpfte Daten zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="5396c-302">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="5396c-303">[Zurück](xref:data/ef-rp/complex-data-model)
>[Weiter](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="5396c-303">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>