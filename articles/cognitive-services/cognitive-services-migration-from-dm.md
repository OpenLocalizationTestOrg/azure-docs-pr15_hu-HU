
<properties
    pageTitle="Áttelepítésének API Azure kognitív szolgáltatások javaslatok a DataMarket ajánlások API |} Microsoft Azure"
    description="Azure gépi tanulási javaslatok – javaslatok kognitív szolgáltatás áttelepítése"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="luisca"/>


# <a name="migrate-to-azure-cognitive-services-recommendations-api-from-the-datamarket-recommendations-api"></a>Áttelepítésének API Azure kognitív szolgáltatások javaslatok a DataMarket ajánlások API
Ebből a cikkből megtudhatja, hogy miként telepítheti át a [Microsoft Azure kognitív szolgáltatások javaslatok API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api)a [Microsoft DataMarket javaslatok API -val](https://datamarket.azure.com/dataset/amla/recommendations) .

A DataMarket javaslatok API fog fogadását új vevők December 31-én, 2016-ban, és 2017 február 28 elavult lesz.

## <a name="how-do-i-start-using-the-azure-cognitive-services-recommendations-api"></a>Hogyan kezdhetem meg, hogy a Azure kognitív szolgáltatások javaslatok API-e?

Az áttelepítendő a csökkent értelmi szolgáltatások javaslatok API-hoz, tegye a következőket:

1.  Ha még nem rendelkezik az Azure előfizetéssel, [Jelentkezzen](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) az egyik. 

1.  Megnyithatja a [Rövid útmutató az első lépésekhez](cognitive-services-recommendations-quick-start.md) a csökkent értelmi szolgáltatások javaslatok API regisztrálhat, és vele programozás útján lépésenkénti útmutatást. 

1.  Nyissa meg a [csökkent értelmi szolgáltatások javaslatok API céloldal](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) ismerje a szolgáltatás-dokumentációt.

## <a name="i-used-the-recommendations-ui-to-build-my-models-is-there-a-similar-tool-for-the-cognitive-services-recommendations-api"></a>A javaslatok felhasználói felületének a modellek létrehozásához használt. Van-e a hasonló eszközben a csökkent értelmi szolgáltatások javaslatok API?

Teljes mértékben! Az új [Javaslatok felhasználói felület](http://recommendations-portal.azurewebsites.net/) URL-je http://recommendations-portal.azurewebsites.net. 

>[AZURE.NOTE] A hitelesítő adatait DataMarket itt nem működnek. Előfizetés az Azure-portálon regisztrálhat, és a fiók billentyűt az új [Javaslatok felhasználói felület](http://recommendations-portal.azurewebsites.net/)használatához szükséges el.
Ha nem egy Fiókkulcs, lásd: a tevékenység 1, a [Rövid útmutató az első lépésekhez](cognitive-services-recommendations-quick-start.md).

##<a name="is-the-new-api-format-the-same-as-the-datamarket-recommendations-api"></a>Formátuma az új API ugyanaz, mint a DataMarket javaslatok API-t?

Az API támogatja az azonos esetek és a folyamat flow ezeket a DataMarket verziója támogatott forgatókönyvek, de a tényleges API felülettel felelnek meg a [Microsoft REST API-irányelvek](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)változásáról. Az API-k egységesebb és munka jobb az eszközök támogató Swagger.

Minden egyes az API-k megértéséhez, az [API explorer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3db)kivétele.
Használja a *próbálja* meg API-hívásban tesztelése gombra. A javaslatok API (katalógus és használati fájlok) elfogyasztott fájlok formátumának nem változott, ezért akkor is folytassa a ugyanazon a fájlok, illetve azokat a fájlokat készítése megírt infrastruktúra.

##<a name="what-are-some-new-features-in-the-cognitive-services-recommendations-api"></a>Mik azok a csökkent értelmi szolgáltatások javaslatok API új funkciókkal?

Az utolsó két hónapokban Microsoft kiadott új funkciók a csökkent értelmi szolgáltatások javaslatok API:
-   Javaslatok felhasználói felület képzés és anélkül, hogy írja be az egysoros kód modellek tesztelése
-   A köteg pontozási megadnia javaslatok ezer egyszerre
-   Lekérdezési javaslatok modellek minőségének mértékek támogatási összeállítása
-   Az üzleti szabályok támogatása
-   Számba veheti a webhely és a használati és a katalógus fájlok letöltése
-   Lekérdezési javaslatok modell elem szolgáltatásainak minősége rangsorolási build támogatása
-   A felvett azt jelenti, hogy a katalógus termékhez keresése

## <a name="when-does-microsoft-stop-supporting-the-datamarket-recommendations-api"></a>Ha nem Microsoft le a DataMarket javaslatok API támogatására?

[API javaslatok a DataMarket a](https://datamarket.azure.com/dataset/amla/recommendations) leállítja a új vevők elfogadása után December 31-én, 2016-ban, és 2017 február 28 fog kell teljesen megszűnt. 

## <a name="what-if-i-dont-have-the-data-that-i-need-to-recreate-my-models-in-the-cognitive-services-recommendations-api"></a>Mi történik, ha nincs hozza létre a modellek, a csökkent értelmi szolgáltatások javaslatok API kell adatokat?

Kíván-e a áttűnés olyan egyszerű, lehetséges, szeretnénk. Segíthetünk helyezze át a régi modellek DataMarket-fiókjából új Azure kognitív szolgáltatások javaslatok API-előfizetéséhez. Kapcsolatfelvétel a [mlapi@microsoft.com](mailto://mlapi@microsoft.com). Meg kell adnia az előfizetés azonosítója és a csökkent értelmi szolgáltatások előfizetés azonosítója datamarket-csatornából, mielőtt a modellek társítása azt az új fiók fog kérjük.
