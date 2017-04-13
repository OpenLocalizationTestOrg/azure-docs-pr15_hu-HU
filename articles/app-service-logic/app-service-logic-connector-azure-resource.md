<properties
   pageTitle="Az Azure erőforrás összekötő használata összefüggés-alkalmazások |} Microsoft Azure alkalmazás szolgáltatás"
   description="Hogyan hozhat létre és konfigurálása az Azure erőforrás összekötő vagy API alkalmazást, és vele az Azure alkalmazás szolgáltatás összefüggés-alkalmazásban"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Első lépések az Azure erőforrás összekötő, és adja hozzá az összefüggés-alkalmazás
>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik.

Az Azure erőforrás összekötő segítségével egyszerűen Azure erőforrások belül az logika alkalmazást.

## <a name="create-the-azure-resource-connector"></a>Az Azure erőforrás összekötő létrehozása
Az Azure erőforrás összekötő API-alkalmazást használja, akkor kell először hozzon létre egy példánya. Ez történik vagy beágyazott egy összefüggés-alkalmazás létrehozása során, vagy válassza az Azure erőforrás-kezelő összekötő API-alkalmazást az Azure piactérről.

Adja meg, meg kell azt be a szolgáltatás egyszerű műveleteket Azure-ban elvégzendő jogosultsággal rendelkező állítani. Az összes hívások a szolgáltatás egyszerű beállított a nevében – az történik. Ez lehetővé teszi, hogy a hatókör csak pontosan mi kíván használni, hogy végezze el az összekötőt, és semmi további.

David Ebbo van írt [remek blogbejegyzésbe](http://blog.davidebbo.com/2014/12/azure-service-principal.html) hogyan állíthatja ezt be. Hajtsa végre az összes az ott található útmutatást, és a **Bérlői azonosítója**, az **Ügyfél-azonosító** és a **titkos**. Ezeket a mezőket, valamint az **Előfizetés azonosítója**, a mi szükségesek állítsa be az összekötő.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Az Azure erőforrás összekötő használata a logika alkalmazások Designer alkalmazásban
### <a name="trigger"></a>Eseményindító
Van két eseményindítók, az összekötő támogatott:

név | Leírás
---- | -----------
Esemény | Az előfizetés az erőforráshoz esemény bekövetkezésekor eseményindító.
Metrikus metszéspontja küszöbérték |  Ha egy mérőszám megfelel egy bizonyos küszöbértéket eseményindító.

### <a name="action"></a>Művelet

Hasonlóképpen akkor megadhatja az műveletek nagyszámú belül az Azure előfizetés:

Az **erőforrás-csoportok** a következőkre van lehetősége:

név | Leírás
---- | -----------
Az erőforrás-csoportok listája | A lista összes erőforrás csoportot az előfizetés.
Erőforráscsoport beszerzése | Erőforráscsoport kapni a azonosítójával.
Erőforrás-csoport létrehozása | Létrehozása vagy módosítása egy erőforrás csoportot.
Erőforráscsoport törlése | Erőforrás csoport törlése.

Az **erőforrások** közül választhat:

név | Leírás
---- | -----------
Lista-erőforrások | Az előfizetés különböző típusú szűrőket erőforrásainak listában.
Erőforrás beszerzése | Egy erőforrás az erőforrás azonosító beszerzése
Létrehozása vagy módosítása az erőforrás | Hozzon létre egy erőforrást, vagy frissítse a meglévő erőforrás. Az a tulajdonságokat az adott erőforrás kell adnia.
Erőforrás művelet |  Erőforrás bármilyen egyéb művelet végrehajtása Kell tudnia, hogy a művelet nevét és a tartalom, amely ezt a műveletet (ha vannak ilyenek).
Erőforrás törlése | Erőforrás törlése.

**Erőforrás** -szolgáltatók a következőkre van lehetősége:

név | Leírás
---- | -----------
Erőforrás-szolgáltatók lista | A lista összes-előfizetésben elérhető erőforrás-szolgáltatónál.

**Erőforrás** -csoport telepítésekhez a következőkre van lehetősége:

név | Leírás
---- | -----------
Lista-telepítések | A lista összes egy erőforrás csoportban a telepítés.
Telepítési beszerzése | Egy sablon telepítésének kapni a azonosítójával.
Telepítési létrehozása | Hozzon létre egy új erőforrás csoport telepítési sablonként segítségével.

Az **események** erőforrásokról a következőkre van lehetősége:

név | Leírás
---- | -----------
Események beszerzése | Ismerkedés az események, az előfizetést, vagy egy erőforrás.

**Mértékek** erőforrásokról esetében a következőkre van lehetősége:

név | Leírás
---- | -----------
Mértékek beszerzése | Egy mérőszám beszerzése egy erőforrás azonosítójával.

## <a name="do-more-with-your-connector"></a>További műveletek az összekötő
Most, hogy az összekötő hoz létre, felveheti egy üzleti folyamat egy logikai alkalmazással. Lásd: [logika alkalmazások melyek?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók Ismerkedés az Azure összefüggés-alkalmazásokhoz, [próbálja logika alkalmazás](https://tryappservice.azure.com/?appservice=logic), ahol azonnal létrehozhat egy rövid életű starter logika alkalmazást az alkalmazás szolgáltatás megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

A Swagger REST API hivatkozásának megtekintése [összekötők és az API alkalmazások hivatkozás](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
