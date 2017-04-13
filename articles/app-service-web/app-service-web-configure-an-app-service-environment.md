<properties
    pageTitle="Az alkalmazás-szolgáltatási környezetben beállítása |} Microsoft Azure"
    description="Konfigurációs, kezelése és alkalmazás környezetek figyelemmel kísérése"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>Alkalmazás szolgáltatás környezet beállítása #

## <a name="overview"></a>– Áttekintés ##

Magas szintű egy Azure alkalmazás szolgáltatási környezetben több fő összetevői áll:

- Az alkalmazás-szolgáltatási környezetben futtatott számítási erőforrások üzemeltetett szolgáltatás
- Tárhely
- Egy adatbázis
- Egy Classic(V1) vagy az erőforrás Manager(V2) Azure virtuális hálózati (VNet) 
- A benne lévő futó alkalmazás szolgáltatási környezetben is szolgáltatás alhálózat

### <a name="compute-resources"></a>Erőforrások kiszámítása

A számítási erőforrásokat a négy erőforráskészletek-et.  Minden alkalmazás szolgáltatási környezetben (SKB) első végű és a három lehetséges dolgozó készletek tartalmaz. Nem kell használni az összes három dolgozó készletek – Ha azt szeretné, használhatja nyugodtan egy vagy két.

A hosts (első végű és munkatársak) erőforráskészletek a nem érhetők el közvetlenül a bérlők. Távoli asztali Protocol (RDP) használatával csatlakozhat hozzájuk, módosítása, hogy kiépítési nem, vagy használni kívánt rendszergazda őket.

Beállíthatja, hogy készlet erőforrás-mennyiség és a méret. Egy ASE négy méret beállításokat, amelyek vannak címkézett keresztül P4 P1 lehetősége van. Ezeket a méretét, és azok árak kapcsolatos részletekért olvassa el a [alkalmazás szolgáltatás árak](../app-service/app-service-value-prop-what-is.md)című témakört.
A mennyiség és méretének módosítása a méretezés művelet neve.  A művelet csak egy skála lehet folyamatban egyszerre.

**Első vége**: az első leállítja a HTTP-/ HTTPS-alkalmazások, amely a ASE tartják végpontokat is. Az első végét munkaterhelésekből nem futtatnia.

- Egy ASE kezdődik két P2s, amely elegendő a fejlesztők/próba munkaterhelésének és a követő gyártási munkaterhelésekből. Nyomatékosan javasoljuk P3s a közepes nehéz gyártási munkaterhelésekből szeretne.
- Közepes, hogy nehéz gyártási munkaterhelésekből javasoljuk, hogy rendelkezik-e annak érdekében, hogy van elegendő első végű operációs rendszert futtató ütemezett karbantartás bekövetkezésekor legalább négy P3s. Ütemezett karbantartást tevékenységek le egy előtér egyszerre állapotba kerül. Ez a teljes csökkenti karbantartási tevékenységek alatt elérhető előtér kapacitása.
- Új előtér-példány nem azonnali vehet fel. 2 – 3 órát kiépítése tarthat.
- A további skála pontosabb beállításra, érdemes figyelni a Processzor százalékos, a memóriahasználat százalék és az aktív kérelmek mértékek az előtér-készlet. Ha a Processzor vagy a memória százalékértékek 70 % feletti P3s fut, adja hozzá további első leállítja. Ha az aktív kérelmek értéket az előtér / 20 000 kérelmek 15 000 átlagának kiszámítása, további első végű is fel kell. Általános célja, hogy az alábbi Processzor- és százalékértékek 70 %-os megőrzése, és átlagolása való alatti 15 000 kérelmek, egy elülső aktív kérelmek fejeződik be, amikor P3s futtat.  

**Munkatársak**: A dolgozók, ahol valójában futtatja az alkalmazásokat. Amikor meg az App milyen szolgáltatáscsomagok felfelé méretezéséhez be a kapcsolódó dolgozó készletben dolgozók használó.

- Munkatársak azonnali hozzáadására. Ezt a 2 és 3 óra kiépítése készíthet, függetlenül attól, hogy hány felvesznek.
- Méretezés bármely készlet számítási erőforrás méretének lép, hogy a 2-3 órát per frissítése tartomány. Egy ASE 20 frissítés tartományok szerepelnek. Ha Ön átméretezi a számítási mérete 10 példányok együtt dolgozó erőforráskészlethez tartozik, között 20 – 30 óráig is eltelhet.
- Ha egy dolgozó készletben használt számítási erőforrások méretének módosításához okoz a hideg elindítja az adott dolgozó készletre futó alkalmazások.

Összes alkalmazás nem futó dolgozó erőforráskészlethez tartozik számítási erőforrás méretének módosításához leggyorsabb módja, ha:

- Méretezze át a példányok száma a 0 lefelé. Körülbelül 30 percig tart a példányok felszabadítása.
- Jelölje be az új számítási méret példányainak száma. Innen kiindulva tart, 2-3 órát befejezéséhez között.

Ha az alkalmazások számítási erőforrás nagyobb méretű kér, akkor nem kihasználhatja az előző útmutatást. Helyett, amelyen az alkalmazásban a dolgozó készlet méretének módosítása, a dolgozók a kívánt méretet a másik dolgozó készletbe feltöltéséhez, és helyezhet át az alkalmazásokat, hogy a készlet.

- Hozzon létre egy másik dolgozó készlet szükséges számítási méretének további előfordulásait. Ez eltarthat 2 és 3 óra befejezéséhez.
- Az App milyen szolgáltatáscsomagok, az alkalmazások van szükség az újonnan konfigurált dolgozó készlet nagyobb méretű tároló rendelése. Ez a egy gyors műveletet, amely kell vennie egy perc alatt befejezéséhez.  
- Ha még nem használt azokban az esetekben amelyekre már nincs szüksége, méretezze lefelé az első készlet dolgozó. Ehhez a művelethez a befejezéséhez kell tudni a 30 percig tart.

**Autoscaling**: az eszközök, amelyek segítséget nyújtanak a számítási erőforrás-felhasználás kezelése egyik autoscaling. Használható autoscaling az előtér- vagy dolgozó készletek. Dolgot kell tennie például növelése bármilyen készlet a példányok délelőtt, és esti csökkentése érdekében. Vagy talán is hozzáadhatja példányok, amikor dolgozók dolgozó erőforráskészlethez tartozik a rendelkezésre álló száma egy bizonyos küszöbértéket alá esik.

Ha szeretne Automatikus méretezéssel szabályok számítási erőforrás készlet mértékek köré, majd tartsa szem előtt az kiépítési szükséges időt. Többet szeretne tudni az alkalmazás környezetek autoscaling, témakö [állítsa be az alkalmazás-szolgáltatási környezetben Automatikus méretezéssel][ASEAutoscale].

### <a name="storage"></a>Tárhely

Minden egyes ASE 500 GB-nyi tárhelyet van beállítva. Ezt a helyet a ASE az összes alkalmazás keresztül használják. A rendelkezésre álló tárterület méretének a ASE része, és jelenleg nem lehet másikra váltani a rendelkezésre álló tárterület méretének használni. Ha a virtuális hálózati útválasztás vagy biztonsági végez módosításokat, továbbra is az Azure-tárolóhoz – access engedélyeznie kell, vagy a ASE nem fog működni.

### <a name="database"></a>Adatbázis

Az adatbázis tárolja az információkat, amely meghatározza a környezet, valamint az alkalmazásokat, benne futó részleteit. Az túl az Azure-re előfizetéshez tartozik. Még nem, amit egy közvetlen azt jelenti, hogy módosítására van. Ha a virtuális hálózati útválasztás vagy biztonsági végez módosításokat, továbbra is az SQL Azure – való hozzáférés engedélyezése van szüksége, vagy a ASE nem működik a.

### <a name="network"></a>Hálózati

A használt a ASE VNet lehet egy korábban elvégzett az ASE létrehozásakor vagy időszakokra végrehajtott. Az alhálózathoz ASE létrehozása során létrehozásakor kényszeríti a a ASE el szeretné helyezni a virtuális hálózat azonos erőforrás csoportba. Ha az erőforráscsoport, használja a ASE lehetnek, mint amit a VNet, majd létrehozásához szükséges a ASE egy erőforrás-kezelő sablon segítségével.

Vannak bizonyos korlátozások, amellyel egy ASE virtuális hálózaton:
 
- A virtuális hálózat kell lennie a területi virtuális hálózat.
- Szükség van egy alhálózat 8 vagy több címekkel hol a ASE telepítve van.
- Miután alhálózat egy ASE tárolni használja, a cím tartomány alhálózat nem módosíthatók. Emiatt azt javasoljuk, hogy az alhálózathoz tartalmaz legalább 64 címek minden jövőbeni ASE növekedés igazodik.
- Nem lehet mást a alhálózat, de a ASE.

A szolgáltatott szolgáltatás, amely tartalmazza a ASE, a [virtuális hálózati] eltérően[ virtualnetwork] és alhálózat felhasználói felügyelete alatt áll.  A virtuális hálózat a virtuális hálózati felhasználói felület vagy az PowerShell használatával felügyelheti a.  A klasszikus vagy az erőforrás-kezelő VNet egy ASE telepíthető.  A portál és az API-szolgáltatások kissé eltérő klasszikus és erőforrás-kezelő VNets között, de a ASE felület megegyezik a.

A VNet egy ASE tárolni használt használhat vagy saját RFC1918 IP-címek, illetve nyilvános IP-címet használhatja.  Ha egy IP-tartomány, amely nem hatálya alá RFC1918 használni kívánt (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16), majd létre kell hoznia a VNet és alhálózat ASE létrehozása előtt a ASE való használatra.

Mivel ez a képesség helyezi a Azure alkalmazás szolgáltatást, a virtuális hálózatba, az azt jelenti, hogy az alkalmazások, a ASE futtatott most már hozzáférhet információforrások, amelyek érhetők el készült ExpressRoute vagy a webhely virtuális magánhálózat (VPN) közvetlenül. Az alkalmazásokat, amelyek az alkalmazás szolgáltatás környezetén belül hozzáférhető a virtuális hálózat, amelyen az alkalmazás-szolgáltatási környezetben eléréséhez további hálózati szolgáltatásokkal nincs szükség. Ez azt jelenti, hogy nem kell használatával VNET integrációs vagy hibrid kapcsolatok erőforrásokat, illetve azok a virtuális hálózathoz. Továbbra is használhatja mindkét azokat a funkciókat, hogy az access-erőforrások hálózatok, amely nem csatlakozik a virtuális hálózat.

Például, hogy az előfizetése van, de nem csatlakozik a virtuális hálózat, amely a ASE virtuális hálózat integrálása VNET integrációs is használhatja. Továbbra is használhatja hibrid kapcsolatok a más hálózatokon ugyanúgy, mint a szokásos módon is erőforrások eléréséhez.  

Ha egy készült ExpressRoute VPN konfigurált virtuális hálózatához, néhány egy ASE tartalmazó útválasztási igényeinek tudatában kell. Vannak bizonyos felhasználói útvonal (UDR) konfigurációk, amelyek egy ASE kompatibilis. További egy ASE virtuális hálózatban készült ExpressRoute, az operációs rendszert futtató című témakör tartalmaz [egy virtuális hálózatban készült ExpressRoute az alkalmazás-szolgáltatási környezetben futó][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Bejövő adatforgalom biztonságossá tétele

Kétféleképp elsődleges szabályozhatja a ASE bejövő forgalmat.  Hálózati biztonsági Groups(NSGs) használatával szabályozhatja, hogy milyen IP címek hozzáférhetnek a ASE, [hogy miként szabályozhatja az alkalmazás-szolgáltatási környezetben bejövő forgalom](app-service-app-service-environment-control-inbound-traffic.md) az itt ismertetett és az egy belső betöltés Balancer(ILB) is beállíthatja a ASE.  Ezek a funkciók is használható együtt Ha korlátozni szeretné a ILB ASE NSGs elérésének szeretné.

Amikor létrehoz egy ASE, az a VNet egy virtuális hoz létre.  A következő két virtuális közül, a belső és külső.  Ha egy ASE hoz létre egy külső virtuális a ASE az alkalmazás átirányítható IP-címet az internet keresztül érhető el lesz. Belső a ASE kijelölésekor lesz egy ILB konfigurálva, és nem közvetlenül az interneten érhető el.  Egy ILB ASE továbbra is van szükség egy külső virtuális, de csak a Azure kezelési és karbantartás hozzáféréshez használt.  

ILB ASE létrehozása során adja meg az altartomány a ILB ASE által használt, és kezelheti a saját DNS-a megadott altartomány lesz.  Mivel a HTTPS hozzáféréshez használt tanúsítvány kezeléséhez szükséges altartomány nevének megadása  ASE létrehozása után kéri a tanúsítvány megadását.  További információ a létrehozásával és használatával egy ILB ASE olvashatók [Egy belső terheléselosztó-alkalmazás szolgáltatás környezettel rendelkező]minderről[ILBASE]. 


## <a name="portal"></a>Portál

Kezelheti, és az alkalmazás-szolgáltatási környezetben figyelheti a felhasználói felület használatával az Azure-portálon. Ha egy ASE, majd, valószínűleg az alkalmazás szolgáltatás szimbólum látható az oldalsáv. Ez a szimbólum jelző alkalmazás környezetek az Azure-portálon:

![Alkalmazás-szolgáltatási környezetben szimbólum][1]

Kattintva nyissa meg a felhasználói Felülettel, amely felsorolja a alkalmazás szolgáltatási környezetben, használhatja a ikonra, vagy kattintson a sávnyílra (">" szimbólum) kattintson az oldalsávon, jelölje be az alkalmazás-szolgáltatási környezetben alján. A felsorolt ASEs egyik választva nyissa meg a felhasználói Felülettel, amely figyelésére és kezelheti.

![Figyelés, és az alkalmazás-szolgáltatási környezetben kezeléséhez felhasználói felület][2]

Ez a lap első egyes tulajdonságait a ASE, a használati erőforráskészlet diagram metrikus együtt jeleníti meg. Néhány **Essentials** Tiltás megjelenített is, amely nyílik meg a lap azt társított hivatkozásokat. Ha például választhatja ki a **Virtuális hálózat** nevére kattintva nyissa meg a felhasználói Felülettel, amely a ASE fut a virtuális hálózat. **App milyen szolgáltatáscsomagok** és az **alkalmazás** minden pengéit, amely ezeket az elemeket, amelyek a ASE a lista megnyitása.  

### <a name="monitoring"></a>Figyelése

A diagramok lásd: az egyes erőforráskészlet teljesítménymutatók számos teszi lehetővé. Az előtér-készlet figyelheti a Processzor és a memória átlag. Dolgozó-készletek figyelheti a formázás és a mennyiséggel elérhető.

Csomagok fel, hogy több alkalmazás szolgáltatás használata a dolgozók dolgozó erőforráskészlethez tartozik. A terhelést a azonos módon nem van meghatározva, mint az előtér-kiszolgálókkal úgy a Processzor- és használatát nem nyújtanak nagy átszállítást megakadályozzák a hasznos információkat. Fontos további nyomon követéséhez, hány dolgozók Ön és a rendelkezésre álló – különösen akkor, ha Ön kezeli a rendszer, mások számára is.  

Is használhatja az összes a mértékek, nyomon követheti a diagramokban állíthat be értesítéseket. Állítsa be az alábbi figyelmeztetéseket ugyanúgy működik, mint máshol App szolgáltatásban. Beállíthatja, hogy jelzést részéről vagy a **riasztások** felhasználói felület vagy a kimutatáshierarchiában való bármilyen mértékek felhasználói felület, és válassza az **Értesítés hozzáadása**.

![Mértékek felhasználói felület][3]

A mérési módja miatt csak tárgyalt az alkalmazás-szolgáltatási környezetben mértékek. Az alkalmazás szolgáltatás terv szintjén a rendelkezésre álló mértékek is vannak. Ez a hol Processzor- és memóriahasználat figyelése van ilyesmire lehetőség sok.

Egy ASE, a rendszer minden az App milyen szolgáltatáscsomagok dedikált App milyen szolgáltatáscsomagok. Ez azt jelenti, hogy csak alkalmazások futó a kiosztott állomásokon alkalmazás szolgáltatáscsomagja, hogy az adott alkalmazás szolgáltatáscsomagja az appok. A alkalmazás díjcsomagjától részletek megtekintéséhez jelenítse meg az alkalmazás szolgáltatáscsomagja, a felhasználói felület ASE listákban vagy **tallózással keresse meg az App milyen szolgáltatáscsomagok** (amely felsorolja azokat).   

### <a name="settings"></a>Beállítások

A ASE lap belül több fontos funkciókat tartalmazó **Beállítások** szakasz van:

**Beállítások** > **tulajdonságai**: A **Beállítások** lap automatikusan megnyíljon a ASE lap, megjelenítéséhez. A képernyő tetején van **tulajdonságait**. Számos elemek itt felesleges **Essentials**megjelenítéséhez, de mi az rendkívül hasznos **Virtuális IP-címét**, valamint a **Kimenő IP-címek**.

![Beállítások lap és a Tulajdonságok][4]

**Beállítások** > **IP-címek**: a ASE IP Secure Sockets Layer (SSL) alkalmazás hoz létre, amikor szüksége van-e egy IP SSL-címet. Annak érdekében, hogy egy, a ASE IP SSL címek tulajdonosa kiosztható van szüksége. Egy ASE létrehozásakor erre a célra egy IP SSL-címmel rendelkezik, de több hozzáadhat. Nincs díjat az SSL további IP-címeket, az [Alkalmazás szolgáltatás árak] látható módon[ AppServicePricing] (SSL-kapcsolatot szakaszában). A további ár IP SSL ár lesz.

**Beállítások** > **Készletre** / **Dolgozó készletek**: egyes e erőforrás készlet lemezt kínál, az azt jelenti, hogy információt csak az adott erőforráskészlet kívül teljesen méretezni, hogy az erőforráskészlet vezérlők kezeléséről a.  

Az alap lap az egyes erőforráskészlet biztosít, hogy az erőforráskészlet mértékek tartalmazó diagramon. Hasonlóan a ASE lap a diagramokkal léphet a diagramba, és tetszés szerint mailbeli értesítők beállítása. Értesítés beállítása a ASE lap az adott erőforráskészletből származó hajtja végre ugyanaz, mint a erőforráskészletből végezze el. Dolgozó készlet **beállításai** lap van hozzáférése az összes alkalmazás vagy szolgáltatás alkalmazás tervek a dolgozó készlet futó.

![Dolgozó készlet beállításai felhasználói felület][5]

### <a name="portal-scale-capabilities"></a>Portál skála funkciók  

Vannak három skála műveletek:

- Az IP-címek a ASE elérhető IP SSL használatát a szám módosítása.
- Az erőforráskészlet használt számítási erőforrás méretének módosítása.
- Az erőforráskészlet, manuálisan vagy autoscaling használt számítási erőforrások számának módosítása.

Az portálon háromféleképpen szabályozhatja a erőforráskészletek rendelkező hány kiszolgálóra:

- Egy a fő ASE a lap tetején a méretezés művelet. Végezhet több skála beállítások módosítása az előtér és dolgozó készletek. Az összes megfelelően egyetlen műveletként.
- A kézi skála művelet az adott erőforrás készlet **skála** lap, amely **Beállítások**csoportban.
- Autoscaling, az adott erőforrás készlet **skála** lap a beállított.

A méretezés a ASE lap műveletet használja, húzza a csúszkát a kívánt, és mentse a mennyiség. Ez a felhasználói felület is támogat a méretének módosítása.  

![Méretezés felhasználói felület][6]

A kézi és Automatikus méretezéssel funkciók adott erőforráskészlet használni, válassza a **Beállítások** > **Készletre** / **Dolgozó készletek** szükség szerint. Ezután nyissa meg a készlet, amely a módosítani kívánt be. Nyissa meg a **Beállítások** > **skála ki** vagy **beállításai** > **méretezni**. A **Méretezés ki** lap példány mennyiség szabályozhatja teszi lehetővé. **Méretezés felfelé** lehetővé teszi az erőforrás méretét.  

![Felhasználói felület méretarány-beállításai][7]

## <a name="fault-tolerance-considerations"></a>Hibatűrést kapcsolatos szempontok

Beállíthatja, hogy az alkalmazás-környezetből legfeljebb 55 teljes számítási források. Az adott 55 számítási erőforrások csak 50 host munkaterhelésekből használható. Ennek oka az, kétszeres. 2 előtér-számítási erőforrások legalább van.  Akár támogatja a dolgozó-készlet felosztás 53 hagyja, hogy. Annak érdekében, hogy hibatűrést, egy további számítási erőforrás, az alábbi szabályoknak megfelelő felosztott van szükség:

- Minden dolgozó készletben legalább 1, hogy nem érhető el az egyes terhelési további számítási erőforrást van szüksége.
- Ha a meglévő számítási erőforrásainak dolgozó erőforráskészlethez tartozik egy bizonyos érték feletti kerül, egy másik számítási erőforrás hibatűrést szükség. Ez nem a helyzet az előtér-készletben.

Bármely egyetlen dolgozó készlet belül hibatűrést követelményei, amely egy adott dolgozó készletbe hozzárendelt erőforrások X értéket:

- Ha X 2 és 20 között, a leghatékonyabbak számítási források, amelyek segítségével használhatja munkaterhelésekből összeg X-1.
- Ha X 21 és 40 között, a leghatékonyabbak számítási források, amelyek segítségével használhatja munkaterhelésekből összeg X-2.
- Ha X 41 és 53 között, a leghatékonyabbak számítási források, amelyek segítségével használhatja munkaterhelésekből összeg X-3.

A minimális helyigénye 2 előtér- és 2 dolgozók tartalmaz.  A fenti kimutatásokkal együtt majd, Íme néhány példa egyértelművé teheti, hogy:  

- Egy egyetlen készletben ha 30 dolgozók, majd közülük 28 használható host munkaterhelésekből.
- Egy egyetlen készletben Ha 2 dolgozók, majd 1 használható host munkaterhelésekből.
- Egy egyetlen készletben ha 20 dolgozók, majd 19 használható host munkaterhelésekből.  
- Egy egyetlen készletben Ha 21 dolgozók, majd még csak 19 host munkaterhelésekből használható.  

A hibatűrést méretarány fontos, de akkor érdemes szem előtt tartani, méretezheti feletti bizonyos küszöbértéket kell. Ha szeretne további kapacitás 20 példányok változik, majd nyissa meg 22 vagy újabb mivel 21 nem adja hozzá bármilyen további minőségben. Ugyanez igaz fenntartása feletti 40, a következő számát hozzáadja a kapacitás 42 esetén.  

## <a name="deleting-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben törlése ##

Ha törölni szeretné az alkalmazás-szolgáltatási környezetben, majd egyszerűen a művelettel **törlése** az alkalmazás-szolgáltatási környezetben a lap tetején. Ekkor a kéri az alkalmazás-környezetből erősítse meg, hogy valóban művelet nevét. Figyelje meg, hogy az alkalmazás-szolgáltatási környezetben törlésekor, törölje az összes a tartalmat is benne.  

![Az alkalmazás-szolgáltatási környezetben felhasználói felület törlése][9]  

## <a name="getting-started"></a>Első lépések

Az első lépések az alkalmazás-szolgáltatási környezetben, megtudhatja, [hogy miként hozhat létre az alkalmazás-szolgáltatási környezetben](app-service-web-how-to-create-an-app-service-environment.md).

További információt az Azure alkalmazás szolgáltatás platform a [Azure alkalmazás szolgáltatás](../app-service/app-service-value-prop-what-is.md)témakörben olvashat.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
