---
title: "Przekazywanie plików do Razor strony platformy ASP.NET Core"
author: guardrex
description: "Dowiedz się, jak przekazać pliki do strony Razor."
keywords: "Platformy ASP.NET Core, Razor, stron Razor, IFormFile przekazywania pliku, przekazywaniem plików"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 3c5841f8c623f09530b60cc9997281dcb8e3c4f6
ms.sourcegitcommit: 94b7e0f95b92c98b182a93d2b3dc0287e5f97976
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2017
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>Przekazywanie plików do Razor strony platformy ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

W tej sekcji przedstawiono przekazywania plików ze stroną Razor.

[Filmu stron Razor Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) w ten samouczek używa modelu prostego powiązania Aby przekazać pliki, które działa dobrze w przypadku przekazywania małych plików. Aby uzyskać informacje na przesyłanie strumieniowe dużych plików, zobacz [przekazywania dużych plików z przesyłania strumieniowego](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

W poniższych krokach należy dodać do przykładowej aplikacji funkcji przekazywania pliku filmu harmonogramu. Harmonogram film jest reprezentowana przez `Schedule` klasy. Klasa zawiera dwie wersje harmonogramu. Jednej wersji są przekazywane klientom, `PublicSchedule`. Druga wersja jest używana dla pracowników firmy `PrivateSchedule`. Każda wersja jest przekazany jako oddzielny plik. Samouczek pokazuje, jak wykonać dwa przekazywania plików ze strony z jednego wpisu na serwerze.

## <a name="add-a-helper-method-to-upload-files"></a>Dodaj metodę pomocnika, aby przekazać pliki

Aby uniknąć zduplikowania kodu do przetwarzania plików przekazane harmonogramu, najpierw Dodaj metodę pomocnika statycznych. Utwórz *narzędzia* folderu w aplikacji i Dodaj *FileHelpers.cs* pliku o następującej zawartości. Metoda pomocnika `ProcessFormFile`, przyjmuje [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) i zwraca ciąg zawierający rozmiar pliku i jego zawartości. Typ zawartości i długości są sprawdzane. Jeśli plik nie przeszły sprawdzanie poprawności, błąd jest dodawany do `ModelState`.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a>Dodaj klasę harmonogramu

Kliknij prawym przyciskiem myszy *modele* folderu. Wybierz **dodać** > **klasy**. Nazwa klasy **harmonogram** i dodaj następujące właściwości:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

Używa klasy `Display` i `DisplayFormat` atrybuty, które przyjazną tytułów i formatowania podczas renderowania danych harmonogramu.

## <a name="update-the-moviecontext"></a>Aktualizacja MovieContext

Określ `DbSet` w `MovieContext` (*Models/MovieContext.cs*) dla harmonogramów:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>Dodaj tabelę harmonogramu do bazy danych

Otwórz konsolę Menedżera pakietów (PMC): **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

W kryterium wykonaj następujące polecenia. Te polecenia powodują dodanie `Schedule` tabeli w bazie danych:

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-fileupload-class"></a>Dodaj klasę przekazywaniem plików

Następnie dodaj `FileUpload` klasy, która jest powiązana ze stroną uzyskać dane harmonogramu. Kliknij prawym przyciskiem myszy *modele* folderu. Wybierz **dodać** > **klasy**. Nazwa klasy **przekazywaniem plików** i dodaj następujące właściwości:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

Klasa ma właściwość tytułu harmonogramu i właściwości dla każdego z dwóch wersji harmonogramu. Wszystkie trzy właściwości są wymagane, i tytuł musi wynosić 3 – 60 znaków.

## <a name="add-a-file-upload-razor-page"></a>Dodaj stronę Razor przekazywania pliku

W *stron* folderu, Utwórz *harmonogramy* folderu. W *harmonogramy* folderu, Utwórz stronę o nazwie *Index.cshtml* o przekazywaniu harmonogram o następującej treści:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

Każda grupa formularz zawiera  **\<Etykieta >** który wyświetla nazwę każda właściwość klasy. `Display` Atrybutów w `FileUpload` modelu Podaj wartości wyświetlania etykiet. Na przykład `UploadPublicSchedule` nazwy wyświetlanej właściwości ustawiono `[Display(Name="Public Schedule")]` i w związku z tym Wyświetla "Harmonogram publiczny" w etykiecie podczas renderowania formularza.

Każda grupa formularz zawiera weryfikacji  **\<span >**. Jeśli użytkownik wejściowych nie spełniają atrybuty właściwości ustawione `FileUpload` klasy lub jeśli któryś z `ProcessFormFile` sprawdzanie poprawności pliku metody kończyć się niepowodzeniem, model kończy się niepowodzeniem do sprawdzania poprawności. Podczas sprawdzania poprawności modelu nie powiedzie się, komunikat dotyczący sprawdzania poprawności pomocne jest renderowany do użytkownika. Na przykład `Title` właściwość jest oznaczona przy `[Required]` i `[StringLength(60, MinimumLength = 3)]`. Jeśli użytkownik nie może podać tytuł, otrzymają komunikat informujący, że wymagana jest wartość. Jeśli użytkownik wprowadzi wartość mniej niż 3 znaków ani więcej niż 60 znaków, otrzymają komunikat informujący, że wartość ma nieprawidłową długość. Jeśli plik jest pod warunkiem, że nie ma zawartości, zostanie wyświetlony komunikat, że plik jest pusty.

## <a name="add-the-code-behind-file"></a>Dodaj plik CodeBehind

Dodaj plik CodeBehind (*Index.cshtml.cs*) do *harmonogramy* folderu:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

Model strony (`IndexModel` w *Index.cshtml.cs*) wiąże `FileUpload` klasy:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

Model korzysta również z listą harmonogramy (`IList<Schedule>`) do wyświetlenia przechowywanych w bazie danych na stronie harmonogramów:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

Załadowanie strony z `OnGetAsync`, `Schedules` jest wypełnione z bazy danych i używane do generowania tabeli HTML załadować harmonogramów:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

Gdy formularz jest przesyłana do serwera, `ModelState` jest zaznaczony. Jeśli jest to nieprawidłowa `Schedule` odbudowaniu i renderuje stronę z jednego lub więcej komunikatów dotyczących sprawdzania poprawności, podając, dlaczego nie można sprawdzić poprawności strony. Jeśli jest prawidłowa, `FileUpload` właściwości są używane w *OnPostAsync* aby zakończyć przekazywanie plików dla obu wersji harmonogramu i utworzyć nową `Schedule` obiektu do przechowywania danych. Harmonogram jest następnie zapisywana w bazie danych:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>Link przekazywania pliku Razor strony

Otwórz *_Layout.cshtml* i dodać łącze do paska nawigacyjnego, aby przejść do strony przekazywania plików:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Dodaj stronę, aby potwierdzić usunięcie harmonogramu

Gdy użytkownik kliknie przycisk, aby usunąć harmonogram, mają mieć możliwość anulowania operacji. Dodaj stronę potwierdzenia usunięcia (*Delete.cshtml*) do *harmonogramy* folderu:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

Plik CodeBehind (*Delete.cshtml.cs*) ładuje jeden harmonogram identyfikowane przez `id` w danych trasy żądania. Dodaj *Delete.cshtml.cs* pliku *harmonogramy* folderu:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

`OnPostAsync` Obsługuje metoda usuwania harmonogramu przez jego `id`:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

Po pomyślnym usunięciu harmonogram, `RedirectToPage` wysyła użytkownika z powrotem do harmonogramów *Index.cshtml* strony.

## <a name="the-working-schedules-razor-page"></a>Pracy harmonogramy Razor strony

Podczas ładowania strony, etykiety i dane wejściowe dla tytułu harmonogram harmonogram publicznego i prywatnego harmonogramu są renderowane z przycisk przesyłania:

![Planuje Razor strony, jak pokazano na ładowania początkowego bez błędów weryfikacji i puste pola](uploading-files/_static/browser1.png)

Wybieranie **przekazać** przycisk bez wypełniania pól narusza `[Required]` atrybutów w modelu. `ModelState` Jest nieprawidłowy. Komunikatów o błędach są wyświetlane dla użytkownika:

![Komunikatów o błędach są wyświetlane obok każdej kontrolki wprowadzania](uploading-files/_static/browser2.png)

Wpisz dwie litery w **tytuł** pola. Sprawdzanie poprawności zmienia się na wskazują, czy tytuł musi należeć do zakresu od 3 do 60 znaków:

![Tytuł komunikatu weryfikacji zmienione](uploading-files/_static/browser3.png)

Po przekazaniu co najmniej jeden harmonogram **załadować harmonogramy** sekcji renderuje załadować harmonogramów:

![Daty UTC, rozmiar pliku publicznej wersji i rozmiar pliku w prywatnej wersji przekazać tabeli załadować harmonogramy, przedstawiający tytuł każdy z harmonogramów](uploading-files/_static/browser4.png)

Użytkownik może kliknąć **usunąć** łącza z tego miejsca do widoku potwierdzenie usunięcia, które mają możliwość potwierdzenia lub anulowania operacji usuwania.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Aby uzyskać informacje o rozwiązywaniu problemów z `IFormFile` przekazywania, zobacz [przekazywania plików w ASP.NET Core: Rozwiązywanie problemów z](xref:mvc/models/file-uploads#troubleshooting).

Dziękujemy za korzystanie z wprowadzenia do stron Razor. Dziękujemy za wszelkie komentarze, które pozostaną. [Wprowadzenie do programu MVC i podstawowe EF](xref:data/ef-mvc/intro) jest doskonałym uzupełnianie w tym samouczku.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [W przypadku platformy ASP.NET Core przekazywania plików](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[Poprzednie: Sprawdzanie poprawności](xref:tutorials/razor-pages/validation)