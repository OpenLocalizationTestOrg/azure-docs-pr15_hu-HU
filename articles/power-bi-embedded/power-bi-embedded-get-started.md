<properties
   pageTitle="Ismerkedés a Microsoft Power BI beágyazott"
   description="A Power BI beágyazott, és adja meg az üzleti intelligencia alkalmazásba interaktív a Power BI szolgáltatásban"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Ismerkedés a Microsoft Power BI beágyazott

**A Power BI beágyazott** egy Azure szolgáltatás, amely lehetővé teszi, hogy a fejlesztők interaktív a Power BI-jelentések felvétele a saját alkalmazások az alkalmazás. **A Power BI beágyazott** működik-e a meglévő alkalmazások áttervez őt vagy módosítása a felhasználók jelentkezzen be.

**Microsoft Power BI beágyazott** anyagok kiépített környezetet nyújt az [Azure ARM API -khoz](https://msdn.microsoft.com/library/mt712306.aspx)keresztül. Ebben az esetben az erőforrás, kiépítése olyan, **A Power BI munkaterület webhelycsoport**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>A munkaterület webhelycsoport létrehozása
**Munkaterület webhelycsoport** a legfelső szintű Azure erőforrás- és fog ágyazhatók be az alkalmazás tartalmát tároló. **Munkaterület webhelycsoport** kétféle módon hozhatók létre:

   -    Manuálisan a az Azure portál használatával
   -    Programozás útján az Azure erőforrás Manager(ARM) API-k használata

Vegyük lépéseinek elvégzése az Azure-portálon **Munkaterület webhelycsoport** össze.

   1.   Nyissa meg, és jelentkezzen be az **Azure-portálon**: [http://portal.azure.com](http://portal.azure.com).

   2.   Válassza a **+ Új** felső lapján.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   **Adatok + Analytics** csoportban kattintson a **Power BI beágyazott**.
   4.   A **Létrehozás lap**adja meg a szükséges információkat. **Árak**olvassa el a [Power BI beágyazott árak](http://go.microsoft.com/fwlink/?LinkID=760527)című témakört.

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Kattintson a **létrehozása**gombra.

A **Munkaterület webhelycsoport** néhány percet vesz igénybe rendelkezést. Befejezése után a fogja venni, hogy a **Munkaterület gyűjtemény a lap**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

A **Létrehozás lap** , kell fordulnia a API-k munkaterületek készíthetnek és helyezhetnek üzembe a tartalom rájuk információkat tartalmazza.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Megtekintése a Power BI API Access kulcsok

A **Hívóbetűk**a legfontosabb információkat vehet hívja fel a Power BI REST API-hoz szükséges közül. Ezek az API-összehívásokban hitelesítést végezni használt **alkalmazás tokenek** létrehozásához használt. A **Hívóbetűk**megtekintéséhez kattintson a **Beállítások lap** **Hívóbetűk** . További információ **alkalmazás tokenek**című témakörben talál [Authenticating, és a Power BI beágyazott engedélyezése](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Megjelenik az "értesítés, hogy rendelkezik-e két kulcsot.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Másolja a billentyűk, és biztonságos módon tárolja őket az alkalmazás. Nagyon fontos, hogy az billentyűk kezelje a jelszavát, ugyanúgy, mert fogja biztosítják a **Munkaterület webhelycsoport**összes a tartalomhoz való hozzáférés.

Két billentyűk szerepel a listán, miközben kulcs csak egy adott időpontban van szükség. A második kulcsnak biztosít, így a billentyűk rendszeres helyreállíthatók a szolgáltatáshoz való hozzáférés megszakítása nélkül.

Most, hogy van egy Power BI példányának a alkalmazás és a **Hívóbetűk**, importálhatja a jelentés a saját alkalmazásba. Megismerheti, hogy hogyan importálhatja egy jelentést, mielőtt a következő szakaszban létrehozása a Power BI adatkészleteket, és jelentéseket beágyazása egy alkalmazás mutatja be.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Power BI adatkészleteket és beágyazása egy alkalmazás jelentések létrehozása

Most, hogy a Power BI egy példánya az alkalmazás nem hozott létre, és **Az Access**-billentyűparancsok, szüksége lesz a Power BI adatkészleteket, és a jelentések, akkor a beágyazni kívánt. **A Power BI Desktop**használatával adatkészleteket, és jelentéseket hozhat létre. [A Power BI Desktop az ingyenes](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)töltheti le. Vagy az első lépésekhez, töltse le a [kiskereskedelmi Analysis minta PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547). **A Power BI Desktop**használatával kapcsolatos további információért olvassa el a [Power BI Desktop – első lépések](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop)című témakört.

**A Power BI Desktop**akkor csatlakozzon az adatforrás importálja az adatokat egy példányát **A Power BI Desktop** vagy közvetlenül kapcsolódás az adatforráshoz a **DirectQuery**.

Az alábbiakban az **Importálás** és a **DirectQuery**közötti különbségeket.

|Importálás | DirectQuery
|---|---
|Táblázatok, oszlopok, *és az adatok* importálása vagy **A Power BI Desktop**másolja. Munka közben a megjelenítések, a **Power BI Desktop** kérdezi le az adatokat egy példányát. Ha látni szeretné a mögöttes adatok előfordult módosításokat, frissítése, vagy importálja, a teljes, az aktuális adatkészlet újra.|Csak a *táblázatok és oszlopok* importált vagy **A Power BI Desktop**másolja. Munka közben a megjelenítések, a **Power BI Desktop** a mögöttes adatforrásban, ami azt jelenti, hogy mindig az aktuális adatok megtekintésekor kérdezi le.

Több adatforrás való csatlakozással kapcsolatban olvassa el a [Csatlakozás adatforráshoz](power-bi-embedded-connect-datasource.md).

Miután menti a munkát **A Power BI Desktop**, PBIX fájl jön létre. A fájl tartalmaz a jelentés. Ezenkívül ha az adatok importálása a PBIX a teljes adatkészlet vagy **DirectQuery**használata esetén a PBIX tartalmazza csak egy adatkészlet séma. A munkaterületre, a [Power BI importálása API](https://msdn.microsoft.com/library/mt711504.aspx)a PBIX programozás útján rendszerbe.

> [AZURE.NOTE] **A Power BI beágyazott** további API-k módosítása a kiszolgáló és az adatbázist, amely az adatkészlet mutat, és be egy szolgáltatási fiók hitelesítő adatait által az adatkészlet az adatbázishoz való csatlakozáshoz használt tartalmaz. [Bejegyzés SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) és [javítás átjáró adatforrás](https://msdn.microsoft.com/library/mt711498.aspx)témakörben talál.

## <a name="next-steps"></a>Következő lépések
Az előző lépésekben létrehozott egy munkaterület gyűjtemény és az első jelentés és az adatkészlet. Most pedig a **Power BI beágyazott**kódírás című témakörből. Megismerheti az alapokat segítségével létrehozott minta webalkalmazást: [első lépések a minta](power-bi-embedded-get-started-sample.md). A példa bemutatja, hogyan szeretné:

  - Rendelkezés tartalom
      - Munkaterület létrehozása
      - PBIX-fájl importálása
      - Frissítse a kapcsolati karakterláncot, és adja meg a adatkészleteket hitelesítő adatait.

  - Jelentés biztonságosan beágyazása

## <a name="see-also"></a>Lásd még:
- [Minta – első lépések](power-bi-embedded-get-started-sample.md)
- [Hitelesítése és a Power BI beágyazott engedélyezése](power-bi-embedded-app-token-flow.md)
- [A Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
