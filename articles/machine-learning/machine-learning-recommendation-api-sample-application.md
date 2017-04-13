<properties 
    pageTitle="Gyakori műveletek a gépi tanulási javaslatok API |} Microsoft Azure" 
    description="Azure Machine Learning ajánlási minta alkalmazás" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 


# <a name="recommendations-api-sample-application-walkthrough"></a>Javaslatok API minta alkalmazás útmutató

>[AZURE.NOTE]Ez a verzió nem kell a javaslatok kognitív API-szolgáltatás használatbavétele. A javaslatok kognitív szolgáltatást fogja cseréje ezt a szolgáltatást, és az új szolgáltatások vannak készüljön. Új funkciók támogatása, jobban API Explorer, a tisztább API felületre, egységesebb előfizetési és számlázási élmény stb kötegelés például rendelkezik.
> További tudnivalók [az új kognitív szolgáltatás áttelepítése](http://aka.ms/recomigrate)

##<a name="purpose"></a>Cél

A dokumentum megjelenítése az Azure gépi tanulási javaslatok API segítségével a [minta alkalmazás](https://code.msdn.microsoft.com/Recommendations-144df403)használatát.

Ez az alkalmazás nem célja, hogy az összes funkcióját tartalmazza, és nem használja az összes az API-khoz. Azt mutatja be, néhány gyakori műveleteket hajtja végre, amikor először szeretné lejátszani a gépi tanulási javaslat szolgáltatással. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="introduction-to-machine-learning-recommendation-service"></a>Gépi tanulási ajánlási szolgáltatás – bevezetés

Gépi tanulási javaslat Service javaslatok engedélyezi a rendszer generál egy ajánlási modell alapján a következő adatokat:

* Javasoljuk, más néven egy katalógushoz kívánt elemre a tárházba
* A felhasználó vagy a munkamenet (Ez szerezhető keresztül adatbeszerzési, nem a minta app részeként időbeli) elem használatát, amely adatok

Ajánlási modell épül, miután vele, amely a felhasználó lehet elemek érdeklik, elemek (vagy egy elem) szerint a felhasználó előrejelzésére kijelöli.

Az előző forgatókönyvben engedélyezéséhez tegye a következőket a gépi tanulási javaslat szolgáltatásban:

* Hozzon létre egy a modellben: Ez az olyan logikai tároló, amely az adatokat (katalógus és használati) és az előrejelzési modellek. Minden egyes modell tároló azonosítjuk keresztül egy egyedi Azonosítót, amely után van hozzárendelve. Az azonosító neve a modell azonosító, és az API-hoz a legtöbb használja azt. 
* Feltöltés a katalógus: modell tároló létrehozásakor rendelhet hozzá a katalógus.

**Megjegyzés**: adatmodell létrehozása és feltöltése a katalógus általában végzi a egyszer a modell életciklusa.

* Töltse fel a használatát: használati adatainak hozzáadása a modellhez tároló.
* Javaslat modell összeállítása: után elég adat, az ajánlási modell készíthet. Ehhez a művelethez használja a felső gépi tanulási algoritmusok ajánlási modell létrehozása. Minden egyes build társítva egy egyedi azonosítót. Az azonosító nyomon követése, mert bizonyos API-k működéséhez szükség szükség.
* Az épület folyamat figyelése: ajánlási modell építés aszinkron műveletet, és több órát, attól függően, hogy az adatokat (a katalógus és a használat) és az összeállítás paramétereket összegét is eltarthat néhány percig, amíg a. Ezért kell figyelni összeállítása. Javaslat modell hoz létre, csak akkor, ha a társított összeállítás befejeződik.
* (Nem kötelező) Válassza az aktív ajánlási modell készítése: ezt a lépést csak akkor szükség, ha egynél több ajánlási modell a modell tároló beépített van. Javaslatok anélkül, hogy az aktív ajánlási modell jelző megszerezni kérelmet automatikusan irányította az alapértelmezett aktív fejlesztése a rendszer. 

**Megjegyzés**: az aktív ajánlási modell gyártási készen áll, és munkakörnyezeti terhelés jön létre. Ez különbözik egy nem aktív ajánlási modell, amely a próba-szerű környezetben (más néven átmeneti) maradjon.

* Tekintse át a javasolt: után ajánlási modell, a javaslatok egyetlen elem vagy a kijelölt elemek listájának válthat. 

Első ajánlási általában meghívják, ha egy adott időszakra. Adott idő alatt átirányítás használati adatainak hozzáadja ezeket az adatokat a megadott modell tároló gépi tanulási javaslat rendszer. Ha elég használati adatainak, készíthet egy új ajánlási modellt, amely magában foglalja a további használati adatok. 

##<a name="prerequisites"></a>Előfeltételek

* Visual Studio 2013-ban
* Internetkapcsolat 
* A javaslatok (https://datamarket.azure.com/dataset/amla/recommendations) API-előfizetés.

##<a name="azure-machine-learning-sample-app-solution"></a>Azure gépi tanulási minta alkalmazásmegoldássá

A megoldás a forráskód, példa használatra, katalógus fájl és a fordítás szükséges csomagok letöltése irányelvek tartalmazza.

##<a name="the-apis-used"></a>A használt API-k

Az alkalmazás gépi tanulási javaslat funkció keresztül érhető el API-t használja. A következő API-khoz is igazolni az alkalmazásban:

* Hozzon létre a modellt: adatok és ajánlási modellek tartása logikai tároló létrehozása. A modell azonosítjuk nevét, és nem hozható létre több modell azonos nevű.
* Katalógus fájl feltöltése: töltse fel a katalógusadatok segítségével.
* Használatát fájl feltöltése: használati adatainak feltöltheti.
* Szerkesztés elindítása: ajánlási adatmodell létrehozása.
* Szerkesztés, végrehajtás figyelni: használata Lync-ajánlási modell építés állapotát.
* Válasszon ki egy beépített modellt a megjelöléséről: mely ajánlási modellt alapértelmezés szerint bizonyos modell tároló jelölése. Ez a lépés nem szükséges, csak akkor, ha egynél több ajánlási modell van, és az aktív ajánlási modellben nem aktív építés aktiválni kívánt.
* Első javaslat: szeretné lekérni a megadott egyetlen elem vagy egy sor olyan elemek szerint ajánlott elemek. 

Az API-k teljes körű leírását használatáról a Microsoft Azure piactérről talál. 

**Megjegyzés**: A modell beállíthatja, hogy több buildjeiben időbeli (nem egyidejű). Minden egyes build azonos vagy egy frissített katalógus és további használati adatainak jön létre.

## <a name="common-pitfalls"></a>Gyakori csapdák

* Meg kell adnia felhasználónevét és a Microsoft Azure piactérről elsődleges fiókkulcs a minta alkalmazás futtatásához.
* A minta alkalmazás egymás után futtatása meghiúsul. Az alkalmazás áramlás tartalmaz létrehozása, feltöltése, létrehozása a monitor és javaslatok ahhoz, hogy egy előre definiált modell; Emiatt azt meghiúsul, egymást követő végrehajtása a Ha nem változtatja meg a modell nevét, között.
* Javaslatok adatok nélkül feltétlenül járnak. A minta alkalmazás nagyon kicsi katalógus és használati fájlt használ. Egyes elemeket a katalógusról emiatt nem ajánlott elemek fog rendelkezni.

## <a name="disclaimer"></a>Jogi nyilatkozat
A minta alkalmazás nem célja munkakörnyezetben futtatásához. A katalógus által megadott adatok nagyon kicsi, és nem fog szolgálni egy jól érthető ajánlási modell. Az adatai a bemutató. 
 
