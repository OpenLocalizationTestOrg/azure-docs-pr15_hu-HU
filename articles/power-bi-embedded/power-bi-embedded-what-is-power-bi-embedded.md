<properties
   pageTitle="Mi az a Microsoft Power BI beágyazott?"
   description="A Power BI beágyazott lehetővé teszi a integrálni szeretné a Power BI szolgáltatásban a webhelyen vagy a mobilalkalmazások így nem kell a felhasználók adatok ábrázolásához egyéni megoldások"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="what-is-microsoft-power-bi-embedded"></a>Mi az a Microsoft Power BI beágyazott?

A **Power BI beágyazott**közvetlenül egy webhelyen vagy mobilalkalmazások integrálhatja a Power BI szolgáltatásban.

![](media\powerbi-embedded-whats-is\what-is.png)

A Power BI beágyazott az **Azure szolgáltatás** lehetővé teszi a ISV és az alkalmazás fejlesztők felszínre hozatala a Power BI adatok élményt kérelmeiket belül. Fejlesztői már készített korábban alkalmazások, és az alkalmazások van a saját felhasználók és a különböző szolgáltatások. Ezeket az alkalmazásokat is fordulhat elő, hogy bizonyos beépített elemek, például diagramok és kimutatások, amely szerint a Microsoft Power BI beágyazott most is hajtott. Felhasználók a Power BI-fiók az alkalmazás használatához nincs szükség. Jelentkezzen be az alkalmazás hasonlóan előtt, és megtekintheti és-lapok használata a Power BI jelentéskészítési funkciói további licencelési anélkül továbbra is.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Microsoft Power BI licencelésének beágyazott

A **Microsoft Power BI beágyazott** használatát modell a Power BI licencelési feladata, nem a végfelhasználói.  Ehelyett **-alapú** , amely a vizuális rendszer használata más az alkalmazás fejlesztője által vásárolt, és az előfizetést, ezek az erőforrások tulajdonosa-előfizetést terhelő.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI beágyazott fogalmi modell

![](media\powerbi-embedded-whats-is\model.png)

Bármely más szolgáltatás Azure-ban, például a Power BI beágyazott erőforrások kiépített környezetet nyújt az [Azure ARM API -khoz](https://msdn.microsoft.com/library/mt712306.aspx)keresztül. Ebben az esetben az erőforrás, amely akkor kiépítése olyan, **A Power BI munkaterület webhelycsoport**.

## <a name="workspace-collection"></a>Munkaterület-gyűjtemény

**Munkaterület webhelycsoport** a legfelső szintű Azure tároló erőforrások, amely tartalmazza a 0 és a további **munkaterületek**.  **Munkaterület** **a webhelycsoport** összes a szokásos Azure tulajdonságok, valamint a következő magában:

-   **Hívóbetűk** – billentyűk használhatók a Power BI API-hoz (a egy újabb szakaszban ismertetett) biztonságosan hívásakor.
-   **Felhasználók** – a Power BI munkaterület gyűjteményben az Azure portálon keresztül vagy ARM API kezelése a rendszergazdai jogosultságokkal rendelkező felhasználók Azure Active Directory (AAD).
-   **Terület** – kiépítési **Munkaterület webhelycsoport**részeként jelölje ki a terület a kell építenie. További tudnivalókért lásd: [Azure régiók](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Munkaterület

**Munkaterület** a Power BI-tartalmakra, amelyek tartalmazhatnak adatkészleteket, jelentések és irányítópultok tároló. **Munkaterület** argumentum üres, amikor az először létrehozott. A minta során fogja a szerző elkészíti a teljes tartalom, a Power BI Desktop használata, és tölti az egyik a munkaterületek használatáról a [Power BI REST API -khoz](http://docs.powerbi.apiary.io/reference).

## <a name="using-workspace-collections-and-workspaces"></a>A munkaterület-webhelycsoportok és munkaterületek
**Munkaterület-webhelycsoportok** és **munkaterületek** olyan tartalom használt és a legjobb bármelyik felöltési módot a Tervező készítésekor az alkalmazás illeszkedik rendezett tárolók. Számos különböző módon, hogy a tartalom határértékeket is rendezze el lesz. Választhatja a teljes tartalom elhelyezni egy munkaterületen belül, majd később alkalmazás tokenek további megkönnyíti az egyik ablaktábláról a másikra az ügyfelekkel a tartalmat. Dönthet úgy is, külön munkaterületek elhelyezni az összes az ügyfelekkel, hogy bizonyos elkülönítése. Vagy lehet szeretné rendezni a régió telepítik felhasználói felhasználókat. Ez a rugalmas tervezés lehetővé teszi a legjobb mód a tartalom rendszerezéséhez.

## <a name="cached-datasets"></a>Gyorsítótárazott adatkészleteket

Gyorsítótárazott adatkészleteket a előzetes verziójában is használható.  Gyorsítótárazott adatokat azonban, miután betöltött már a **Microsoft Power BI beágyazott**nem tudja frissíteni.

## <a name="authentication-and-authorization-with-app-tokens"></a>Hitelesítés és az alkalmazás alapelemeket engedélyezés

**Microsoft Power BI beágyazott** eltér az alkalmazás minden szükséges felhasználói hitelesítés és engedélyezési végrehajtásához. Nincs explicit követelmény, hogy a végfelhasználói kell ügyfelek az Azure Active Directory (Azure Active Directory) nem.  Ehelyett az alkalmazás kifejező, **A Microsoft Power BI beágyazott** engedély jeleníti meg a jelentést Power BI **Alkalmazás hitelesítési tokenek (alkalmazás tokenekre)**használatával.  Ezek a **Alkalmazás tokenek** jönnek létre, ha az alkalmazás csevegni jeleníti meg a jelentés szükség szerint.  Lásd: [alkalmazás tokenek](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Microsoft Power BI beágyazott**hitelesítő használt **Alkalmazás hitelesítési tokenek (alkalmazás tokenekre)** .  Az **Alkalmazás tokenek**három típusa van:

1.  A kiépítési tokenek - használt kiépítésekor **munkaterület** új **Munkaterület gyűjtemény**
2.  Fejlesztési tokenek - használja, ha a hívások átirányítása közvetlenül a **Power BI REST API -khoz** tétele
3.  Tokenek - használt híváskezdeményezési jeleníti meg egy jelentést a beágyazott iframe beágyazási

A tevékenységek a **Microsoft Power BI beágyazott**különböző szakaszainak tokenek segítségével.  A tokenek szolgálnak, hogy átadhatja az alkalmazás engedélyek Power BI. További tudnivalókért olvassa el a [Alkalmazás jogkivonat Flow](power-bi-embedded-app-token-flow.md)című témakört.

## <a name="see-also"></a>Lásd még:
- [Microsoft Power BI beágyazott tipikus esetei](power-bi-embedded-scenarios.md)
- [Ismerkedés a Microsoft Power BI beágyazott](power-bi-embedded-get-started.md)
