<properties
    pageTitle="Azure alkalmazás szolgáltatás tervek alapos áttekintés |} Microsoft Azure"
    description="Megtudhatja, hogyan alkalmazás szolgáltatás Azure alkalmazás szolgáltatás munka tervet, és hogyan azok összekapcsolhatók az adatkezelési folyamatok."
    keywords="alkalmazás, azure alkalmazás szolgáltatás, a méretarány méretezhető, alkalmazás szolgáltatáscsomagja, alkalmazás szolgáltatás költség"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Azure alkalmazás szolgáltatás tervek alapos áttekintés#

Az alkalmazás szolgáltatáscsomagja funkciók és használt több alkalmazások megosztató kapacitás csoportját képviseli. Web Apps alkalmazások, a Mobile-alkalmazások, a függvény alkalmazások vagy a API-alkalmazások [Azure alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714) az összes alkalmazás szolgáltatás csomagjával futnak. Ezekről a csomagokról öt árak rétegek támogatja: *szabad*, *megosztott*, *egyszerű*, *normál*és *prémium verzióban*. Minden réteg rendelkezik saját funkciók és kapacitása. Az azonos előfizetés és földrajzi hely alkalmazások terv is megoszthatja. Által megosztott terv minden alkalmazás használja a funkciók és a határozzák meg a terv réteg funkciói. Terv társított összes alkalmazás, amely a terv határozza meg az erőforrások futtatható.

Például ha csomagja szeretné használni a szokásos szolgáltatás réteg "– kicsi" két példánya van beállítva, terv társított összes alkalmazás mindkét esetben futtatni és hozzáférhet az a szabványos réteg szolgáltatást. Terv példányok, amelyen alkalmazások futtatja az, hogy teljes körű felügyelt és könnyen hozzáférhető.

Ez a cikk ismerteti a főbb tulajdonságait, például a réteg és -szolgáltatási alkalmazás csomagot, és hogyan származnak lejátszása közben az alkalmazások kezelése az beosztásának.

## <a name="apps-and-app-service-plans"></a>Alkalmazások és az alkalmazás milyen szolgáltatáscsomagok

Lehet, hogy az adott időpontban csak egy alkalmazás szolgáltatáscsomagja társított-szolgáltatási alkalmazás alkalmazás.

Alkalmazások és a csomagok egy erőforrás csoportban találhatók. Erőforráscsoport minden erőforrás, hogy azt az életciklus oszlopazonosító szolgál. Az alkalmazás részei együttes kezelése az erőforrás csoportok is használhatja.

Egyetlen erőforráscsoport több App milyen szolgáltatáscsomagok is van, mert rendel a különböző alkalmazások különböző fizikai erőforrásokat. Például az erőforrások között fejlesztők, tesztelése és a gyártási környezetekben is elválaszthatja egymástól. Külön környezetekben, gyártás-és a fejlesztők/próba problémákat lehetővé teszi az erőforrások azonosítása. Ezzel a módszerrel a betöltés ellen alkalmazás új verziót tesztelése nem versenyt ugyanazokat az erőforrásokat, mint a termelési alkalmazásokat, amely valódi ügyfelek szolgálnak.

Amikor egy erőforrás csoport több tervek, az alkalmazások földrajzi régiók nyúló is meghatározhat. Egy könnyen hozzáférhető alkalmazást futtató két régióban legalább két tervek, egy, az egyes régiókra vonatkozó és minden terv társított egy alkalmazást tartalmazza. Ebben az esetben az alkalmazás az összes példányainak egyetlen erőforráscsoport majd tartalmazza. Több csomagok és több alkalmazások erőforráscsoport problémákat egyszerűen kezelése, kezelése és az alkalmazás állapotának megtekintése.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Hozzon létre egy alkalmazás szolgáltatáscsomagja vagy meglévőt használ

Az alkalmazás létrehozásakor figyelembe erőforrás a csoport létrehozása. Azonban ha az alkalmazás, amely a létrehozni kívánt nagyobb alkalmazáshoz összetevő, ez az alkalmazás kell létrehozni az nagyobb alkalmazás számára lefoglalandó erőforrás csoporton belül.

-E az új alkalmazás egy teljesen új alkalmazást, vagy nagyobb egy részét, válassza a meglévő alkalmazás szolgáltatáscsomagja segítségével tárolni, vagy hozzon létre egy újat. A döntési további kapacitás és a tervezett terhelés kérdése.

Ha az új alkalmazás fog használni a sok erőforrást, és rendelkezik különböző tényezők más alkalmazásokból méretezés egy meglévő csomagban, azt javasoljuk, azonosítani a saját csomagban.

Amikor létrehoz egy tervet, lefoglalhat egy új sor erőforrások az alkalmazás, és jobban kézben tarthatók az erőforrás-hozzárendelés nyereség, mivel minden terv megkapja a saját példányok csoportja.

Alkalmazások áthelyezheti a csomagok között, mert módosíthatja, hogy az erőforrás van rendelve, a nagyobb alkalmazás között.

Végül ha létre szeretne hozni egy alkalmazást egy másik régióbeli, és a régió nem csomaggal rendelkezik-e egy meglévő, terv létrehozása az adott régióban engedélyezni szeretné üzemeltetni van az alkalmazás

## <a name="create-an-app-service-plan"></a>Alkalmazás szolgáltatás terv létrehozása

>[AZURE.TIP] Ha az alkalmazás-szolgáltatási környezetben van áttekintheti a dokumentáció adott alkalmazás-szolgáltatási környezetben itt: [az alkalmazás szolgáltatás megtervezése az alkalmazás-szolgáltatási környezetben létrehozása](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Egy üres alkalmazás szolgáltatáscsomagja hozhat létre, az alkalmazás szolgáltatás terv Tallózás élmény vagy alkalmazás létrehozási részeként.

Az [Azure-portálon](https://portal.azure.com)kattintson az **Új** > **webes + mobile**, és jelölje be a **Web App** vagy információlisták alkalmazás szolgáltatás alkalmazás.
![-Alkalmazás létrehozása az Azure-portálon.][createWebApp]

Ezután jelölje ki, és az alkalmazás szolgáltatás megtervezése az új alkalmazás létrehozása.

 ![Hozzon létre egy alkalmazás szolgáltatáscsomagja.][createASP]

Létrehoz egy új alkalmazás szolgáltatás tervet, kattintson a **[+] létrehozása új**, írja be az **alkalmazás szolgáltatáscsomagja** nevét, és válassza ki a megfelelő **helyre**. Kattintson a **réteg árak**, és válassza ki a megfelelő árak réteg a szolgáltatás. Jelölje ki a **nézet összes** árak további lehetőségek, például az **ingyenes** , **megosztott**megtekintése. Miután kiválasztotta a árak réteg, kattintson a **Válasszon** gombra.

## <a name="move-an-app-to-a-different-app-service-plan"></a>Az alkalmazás áthelyezése egy másik alkalmazás szolgáltatás csomagra

Az alkalmazás viheti át egy másik alkalmazás szolgáltatáscsomagja az [Azure-portálon](https://portal.azure.com). Áthelyezheti alkalmazások csomagok között, mindaddig, amíg a csomagok azonos erőforráscsoport és földrajzi területhez tartozik.

-Alkalmazását át egy másik csomagra, keresse fel az áthelyezni kívánt alkalmazást. Kattintson a **Beállítások** menü keresse meg a **Változás alkalmazás szolgáltatás megtervezése**.

**Alkalmazás szolgáltatás Tervmódosítás** nyílik meg, hogy az **alkalmazás szolgáltatáscsomagja** Adatkijelölő. Ezen a ponton, válasszon egy meglévő tervet, vagy hozzon létre egy újat. Csak érvényes csomagok (az azonos erőforráscsoport és földrajzi hely) jelennek meg.

![Alkalmazás szolgáltatás terv Adatkijelölő.][change]

Minden terv saját árak szint rendelkezik. Például ha áthelyezése egy webhely egy ingyenes réteg egy szabványos réteg az alkalmazás letöltése segítségével a szolgáltatások és a szokásos réteg erőforrásait.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Az alkalmazás alkalmazás szolgáltatás másik csomagra másolása
Az alkalmazás áthelyezése másik területére, egy alternatív szükség alkalmazás klónozhatja. Egy példányát az új vagy meglévő alkalmazás szolgáltatáscsomagja vagy alkalmazás szolgáltatási környezetben bármelyik tartományban lévő klónozhatja lehetővé teszi.

 ![Az alkalmazás klónozhatja.][appclone]

Kattintson az **eszközök** menü talál **Adatfeliratsor alkalmazást** .

Bizonyos korlátozások, hogy az [alkalmazás-szolgáltatási alkalmazás Azure klónozhatja Azure portálon](../app-service-web/app-service-web-app-cloning-portal.md)olvashat klónozhatja tartalmaz.

## <a name="scale-an-app-service-plan"></a>Az alkalmazás szolgáltatáscsomagja méretezése

Háromféleképpen terv méretezése:

- **A terv módosítása árak réteg**. Például a egyszerű réteg csomagra konvertálható egy normál vagy prémium réteg, és minden alkalmazás társított, hogy a csomag letöltése, amely felajánlja az új szolgáltatási réteg funkcióit használhatja.
- **A terv példány méretének módosítása**. Példaként a kis példányok használó egyszerű réteg csomagra nagy példányok használandó módosítható. Minden alkalmazás társított terv most használja a további memória és Processzor erőforrások, amely felajánlja a példány nagyobb méretű.
- **Módosítsa a terv példányok száma**. Szabványos terv ki három példányaiban méretezett például megadhat 10 példányaiban. Premium csomagra (fizetnie elérhetőség) 20 példányaiban is átméretezi. Minden alkalmazás társított terv most már használja a további memória és Processzor erőforrások, amely felajánlja a nagyobb példányok száma.

Módosíthatja a réteg és példány árak méretének **Skála be** elemre kattintva vagy az alkalmazást, vagy az alkalmazás szolgáltatáscsomagja beállítások csoportban. Az alkalmazás szolgáltatáscsomagja alkalmazása a módosításokat, és hatással vannak az összes alkalmazás üzemeltetett.

 ![Ha át kívánja méretezni, az alkalmazás mentése értékek beállítása.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Alkalmazás szolgáltatás megtervezése karbantartása
**Alkalmazás szolgáltatás csomagok** , amelyekre nincs rájuk kapcsolódó alkalmazások továbbra is díjak merülnek fel, mivel az alkalmazás szolgáltatás terv skála tulajdonságok beállított számítási kapacitás lefoglalása továbbra is.
Az eredményül kapott üres alkalmazás szolgáltatáscsomagja nem várt költségek elkerülésére, amikor az utolsó alkalmazást, az alkalmazás szolgáltatás csomagjával is törlődik, is törlődik.


## <a name="summary"></a>Összefoglalás

App milyen szolgáltatáscsomagok funkciók és az alkalmazások között megosztató kapacitás jelenítik meg. App milyen szolgáltatáscsomagok rugalmasan lefoglalhat források használatával egy adott alkalmazások és az Azure erőforrás-kihasználtság további optimalizálása ad. Ezzel a módszerrel pénzt mentéséhez kattintson a tesztelés környezet tetszés megoszthat egy tervet használt több alkalmazások. Méretezéssel azt régiók és a csomagok között gyártási környezetben is maximalizálása kapacitása.

## <a name="whats-changed"></a>Mi változott

* Útmutató a módosítása a webhelyekre alkalmazás szolgáltatás, lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
