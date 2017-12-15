---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Behandlung von Postbacks aus einem Popupsteuerelement ohne UpdatePanel (c#) | Microsoft Docs
author: wenz
description: "Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Tritt ein Postback in \"su\"..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="6edf5-104">Behandlung von Postbacks aus einem Popupsteuerelement ohne UpdatePanel (c#)</span><span class="sxs-lookup"><span data-stu-id="6edf5-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="6edf5-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6edf5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6edf5-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6edf5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="6edf5-107">Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="6edf5-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6edf5-108">Wenn ein Postback in einem solchen Bereich erfolgt und ggf. mehrere Bereiche auf der Seite werden ist es schwierig, zu bestimmen, welcher Bereich geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="6edf5-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="6edf5-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="6edf5-109">Overview</span></span>

<span data-ttu-id="6edf5-110">Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="6edf5-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6edf5-111">Wenn ein Postback in einem solchen Bereich erfolgt und ggf. mehrere Bereiche auf der Seite werden ist es schwierig, zu bestimmen, welcher Bereich geklickt wurde.</span><span class="sxs-lookup"><span data-stu-id="6edf5-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="6edf5-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="6edf5-112">Steps</span></span>

<span data-ttu-id="6edf5-113">Bei Verwendung einer `PopupControl` mit einem Postback, aber ohne eine `UpdatePanel` auf der Seite die Control-Toolkit nicht bieten eine Möglichkeit zu bestimmen, welches Clientelement das Popup ausgelöst wurde, die wiederum das Postback verursacht hat.</span><span class="sxs-lookup"><span data-stu-id="6edf5-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="6edf5-114">Ein kleiner Trick bietet jedoch eine problemumgehung für dieses Szenario an.</span><span class="sxs-lookup"><span data-stu-id="6edf5-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="6edf5-115">Erstens, sieht die grundlegende Einrichtung: zwei Textfelder, die beide die gleiche Popup, einen Kalender auslösen.</span><span class="sxs-lookup"><span data-stu-id="6edf5-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="6edf5-116">Zwei `PopupControlExtenders` Versammeln Sie Textfelder und Popupfenster.</span><span class="sxs-lookup"><span data-stu-id="6edf5-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="6edf5-117">Die Grundidee ist ein ausgeblendetes Formularfeld im Hinzufügen der &lt; `form` &gt; Element, das das Textfeld enthält, der das Popup gestartet:</span><span class="sxs-lookup"><span data-stu-id="6edf5-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="6edf5-118">Wenn die Seite geladen ist, fügt JavaScript-Code einen Ereignishandler beide Textfelder: Wenn der Benutzer in einem Textfeld klickt, seinen Namen in der ausgeblendetes Formularfeld geschrieben:</span><span class="sxs-lookup"><span data-stu-id="6edf5-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="6edf5-119">In den serverseitigen Code muss der Wert des ausgeblendeten Felds gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="6edf5-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="6edf5-120">Da ausgeblendete Felder zum Bearbeiten von trivialen sind, ist ein Positivliste Ansatz zur Überprüfung des Werts der ausgeblendeten erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6edf5-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="6edf5-121">Nachdem die richtige Textfeld identifiziert wurde, wird das Datum im Kalender hinein geschrieben.</span><span class="sxs-lookup"><span data-stu-id="6edf5-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="6edf5-122">[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6edf5-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="6edf5-123">Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6edf5-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="6edf5-124">[![Durch Klicken auf ein Datum in das Textfeld eingefügt](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6edf5-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="6edf5-125">Durch Klicken auf ein Datum in das Textfeld eingefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6edf5-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6edf5-126">[Zurück](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Weiter](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6edf5-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>