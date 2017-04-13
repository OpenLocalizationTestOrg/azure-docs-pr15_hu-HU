<properties
   pageTitle="Dokumentumok index létrehozása az Azure keresési többnyelvű |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
   description=" Azure keresési feljebb helyezése nyelvi gázelemzők Lucene és természetes nyelvi feldolgozása technológia a Microsofttól származó 56 nyelven, támogatja."
   services="search"
   documentationCenter=""
   authors="yahnoosh"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="07/14/2016"
   ms.author="jlembicz"/>

# <a name="create-an-index-for-documents-in-multiple-languages-in-azure-search"></a>Dokumentumok index létrehozása az Azure keresési több nyelven
> [AZURE.SELECTOR]
- [Portál](search-language-support.md)
- [TÖBBI](https://msdn.microsoft.com/library/azure/dn879793.aspx)
- [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.analyzername.aspx)

Az nyelvi gázelemzők hatványon unleashing ugyanolyan egyszerű, mint a beállítás egy tulajdonságot a tárgymutató-definícióban kereshető mező alapján. Most már műveleteket hajthat végre ezt a lépést a portálon.

Azure keresés definiálása az index séma lehetővé tévő az alábbiakban az Azure-portálon lemezt képernyőképek. Ez a lap, a felhasználók létrehozása az összes mező és a analyzer tulajdonság minden egyes őket.

> [AZURE.IMPORTANT] Csak beállíthatja, hogy a nyelv analyzer során mező definícióját, mint az az alapokról a új index létrehozása felfelé, vagy ha új mezőt egy meglévő indexet szeretne. Ellenőrizze, hogy az összes attribútum Analyzer eszközt, beleértve a mező létrehozásakor teljesen adja meg. Nem tudja tulajdonságainak szerkesztése vagy analyzer típusának módosítása után a módosítások mentéséhez.

## <a name="define-a-new-field-definition"></a>Új mező definition meghatározása

1. Jelentkezzen be az [Azure-portálra](https://portal.azure.com) , és nyissa meg a szolgáltatás lap a keresési szolgáltatás.
2. A parancssáv indítsa el a új index létrehozása a szolgáltatás-irányítópult tetején kattintson a **Hozzáadás index** , vagy nyissa meg a meglévő indexet egy analyzer állíthatja be az új mezőket egy meglévő indexet ad.
3. A mezők lap jelenik meg, definiálásáról az index, a séma lehetőségeit többek között a Analyzer lap használható a nyelv analyzer kiválasztása.
4. A mezők nyisson meg mező definícióját kezeléséről a nevét, az adatok típusának kiválasztása és beállításával attribútumok megjelölni a mező teljes szövegének kereshető, a keresési eredményeknél használható dimenzió navigációs szerkezetben beolvasható rendezhető és stb. 
5. Mielőtt a következő mezőre, nyissa meg a **Analyzer** fülre. 

   
![][1]
*Egy analyzer kijelöléséhez kattintson a mezők lap Analyzer lapján*

## <a name="choose-an-analyzer"></a>Válasszon egy analyzer

6. Görgetéssel keresse meg a definiálása mezőben. 
7. Ha még nem jelölt meg a mező kereshető, jelölje be az most, hogy **kereshető**megjelölése.
8. Kattintson a Analyzer területre elérhető gázelemzők listájának megjelenítéséhez.
9. Válassza ki a használni kívánt analyzer.

![][2]
*Jelölje ki azt a támogatott gázelemzőknek az egyes mezők*

Alapértelmezés szerint minden kereshető mezők használják a [szabványos Lucene analyzer](http://lucene.apache.org/core/4_10_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) , amely nyelvi felismerése nélkül. Támogatott gázelemzők teljes listájának megtekintéséhez, olvassa el a [Nyelvek támogatásáról az Azure keresési](https://msdn.microsoft.com/library/azure/dn879793.aspx)című témakört.

A nyelv analyzer mező kijelölése után azt használandó minden az indexelés és a keresési kérelem ezt a mezőt. Lekérdezés különböző gázelemzők használatával több mezőkre elküldésekor a lekérdezés fog feldolgoztatni egymástól függetlenül az egyes mezők esetében a megfelelő gázelemzőknek.

Számos webes és mobilalkalmazások szolgál a felhasználók körül a földgömb nyelvek használata. Egy ilyen esetben az index meghatározása: hozzon létre egy mezőt az egyes támogatott nyelvekhez lehetőség.

![][3]
*Index definíciója az egyes támogatott nyelvekhez leírás mezővel*

Ha a lekérdezés kiállító agent nyelvének ismert, a is a keresési kérelem hatóköre egy adott mező a **searchFields** lekérdezési paraméter használatával. Az alábbi lekérdezés indítandó csak a Lengyel leírásának ellen:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=description_pl&api-version=2015-02-28`

A tárgymutatóhoz a portálon illessze be a fent látható egy hasonló lekérdezés a **keresési** Intézővel kérdezhet. A szolgáltatás lap a parancssávon a keresési ablak érhető el. Lásd: a [lekérdezés a Azure keresési index a portálon](search-explorer.md) további információt.

Előfordul, hogy a lekérdezés kiállító agent nyelvének állapota ismeretlen, ebben az esetben a lekérdezés kiadható összes mezőkre egyidejű. Ha szükséges, beállításait eredménye a megadott nyelven a definiálható [profilok pontozási](https://msdn.microsoft.com/library/azure/dn798928.aspx)használatával. Az alábbi példában a leírás angolul találat pontszáma lesz magasabb viszonyított lengyel és francia egyezéseket:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2015-02-28`

Ha a .NET fejlesztő, jegyezze fel a nyelvi gázelemzők az [Azure keresési .NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Search)segítségével állíthatja be. A legújabb kiadásra, valamint a Microsoft nyelvi gázelemzőknek támogatását tartalmazza.

<!-- Image References -->
[1]: ./media/search-language-support/AnalyzerTab.png
[2]: ./media/search-language-support/SelectAnalyzer.png
[3]: ./media/search-language-support/IndexDefinition.png
