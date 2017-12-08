---
title: "Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows"
author: rick-anderson
description: "Tworzenie składnika web API platformy ASP.NET Core MVC i Visual Studio dla systemu Windows"
keywords: "Platformy ASP.NET Core, WebAPI, interfejs API sieci Web, REST, HTTP, usługi, usługa HTTP"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 617b11cd7652e393c06446c62138802e4a4e90df
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="f1079-104">Tworzenie składnika web API platformy ASP.NET Core i Visual Studio dla systemu Windows</span><span class="sxs-lookup"><span data-stu-id="f1079-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="f1079-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f1079-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f1079-106">W tym samouczku będziesz kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania".</span><span class="sxs-lookup"><span data-stu-id="f1079-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f1079-107">Nie Konstruuj interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f1079-107">You won’t build a UI.</span></span>

<span data-ttu-id="f1079-108">Istnieją 3 wersje tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="f1079-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="f1079-109">Systemu Windows: Interfejs API z programu Visual Studio dla systemu Windows (w tym samouczku) sieci Web</span><span class="sxs-lookup"><span data-stu-id="f1079-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="f1079-110">System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="f1079-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="f1079-111">System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="f1079-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="f1079-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f1079-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="f1079-113">Zobacz [ten plik PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) dla wersji platformy ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="f1079-113">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="f1079-114">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="f1079-114">Create the project</span></span>

<span data-ttu-id="f1079-115">W programie Visual Studio, wybierz **pliku** menu > **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f1079-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="f1079-116">Wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="f1079-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="f1079-117">Nazwij projekt `TodoApi` i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1079-117">Name the project `TodoApi` and select **OK**.</span></span>

![Okno dialogowe nowego projektu](first-web-api/_static/new-project.png)

<span data-ttu-id="f1079-119">W **nowe podstawowe aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz opcję **interfejsu API sieci Web** szablonu.</span><span class="sxs-lookup"><span data-stu-id="f1079-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="f1079-120">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1079-120">Select **OK**.</span></span> <span data-ttu-id="f1079-121">Czy **nie** wybierz **Włącz obsługę Docker**.</span><span class="sxs-lookup"><span data-stu-id="f1079-121">Do **not** select **Enable Docker Support**.</span></span>

![Okno dialogowe nowego aplikacji sieci Web platformy ASP.NET z szablonem projektu interfejsu API sieci Web z platformy ASP.NET Core szablonów](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="f1079-123">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="f1079-123">Launch the app</span></span>

<span data-ttu-id="f1079-124">W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="f1079-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="f1079-125">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port/api/values`, gdzie *portu* jest numeru portu losowo wybrany.</span><span class="sxs-lookup"><span data-stu-id="f1079-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="f1079-126">Chrome, Microsoft Edge i przeglądarki Firefox wyświetlane są następujące:</span><span class="sxs-lookup"><span data-stu-id="f1079-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="f1079-127">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="f1079-127">Add a model class</span></span>

<span data-ttu-id="f1079-128">Model jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f1079-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="f1079-129">W takim przypadku tylko model jest zadanie do wykonania.</span><span class="sxs-lookup"><span data-stu-id="f1079-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f1079-130">Dodaj folder o nazwie "Modele".</span><span class="sxs-lookup"><span data-stu-id="f1079-130">Add a folder named "Models".</span></span> <span data-ttu-id="f1079-131">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt.</span><span class="sxs-lookup"><span data-stu-id="f1079-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="f1079-132">Wybierz **dodać** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="f1079-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f1079-133">Nazwa folderu *modele*.</span><span class="sxs-lookup"><span data-stu-id="f1079-133">Name the folder *Models*.</span></span>

<span data-ttu-id="f1079-134">Uwaga: Klasy modelu Przejdź dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.</span><span class="sxs-lookup"><span data-stu-id="f1079-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f1079-135">Dodaj `TodoItem` klasy.</span><span class="sxs-lookup"><span data-stu-id="f1079-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="f1079-136">Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="f1079-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f1079-137">Nazwa klasy `TodoItem` i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f1079-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="f1079-138">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="f1079-138">Replace the generated code with the following:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f1079-139">Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="f1079-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="f1079-140">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="f1079-140">Create the database context</span></span>

<span data-ttu-id="f1079-141">*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych.</span><span class="sxs-lookup"><span data-stu-id="f1079-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f1079-142">Ta klasa jest tworzona przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="f1079-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f1079-143">Dodaj `TodoContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="f1079-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="f1079-144">Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="f1079-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f1079-145">Nazwa klasy `TodoContext` i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f1079-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="f1079-146">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="f1079-146">Replace the generated code with the following:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="f1079-147">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="f1079-147">Add a controller</span></span>

<span data-ttu-id="f1079-148">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu.</span><span class="sxs-lookup"><span data-stu-id="f1079-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="f1079-149">Wybierz **dodać** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="f1079-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="f1079-150">W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasy kontrolera interfejsu API sieci Web** szablonu.</span><span class="sxs-lookup"><span data-stu-id="f1079-150">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="f1079-151">Nazwa klasy `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f1079-151">Name the class `TodoController`.</span></span>

![Okno dialogowe nowego elementu z kontrolerem dodatek do wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

<span data-ttu-id="f1079-153">Zastąp wygenerowany kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="f1079-153">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="f1079-154">Uruchom aplikację</span><span class="sxs-lookup"><span data-stu-id="f1079-154">Launch the app</span></span>

<span data-ttu-id="f1079-155">W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="f1079-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="f1079-156">Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port/api/values`, gdzie *portu* jest liczbą losowo wybranego portu.</span><span class="sxs-lookup"><span data-stu-id="f1079-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="f1079-157">Jeśli używasz programu Chrome, krawędzi lub Firefox, będą wyświetlane dane.</span><span class="sxs-lookup"><span data-stu-id="f1079-157">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="f1079-158">Jeśli używasz programu Internet Explorer programu Internet Explorer będzie Monituj o po otwarciu lub zapisać *values.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="f1079-158">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="f1079-159">Przejdź do `Todo` kontrolera właśnie utworzyliśmy `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="f1079-159">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

