
<properties
    pageTitle="Az Office használata Azure RemoteApp |} Microsoft Azure" 
    description="Megtudhatja, hogyan Office Azure RemoteApp működik együtt"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-office-with-azure-remoteapp"></a>Az Office használata Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Office-alkalmazások Azure RemoteApp szolgáltatója két lehetőség van: Office 365 ProPlus vagy az Office 2013 Professional Plus próbaverziója.

**Fontosnak tudta, hogy van egy új, még jobb, amint lecserélnie Ez a cikk? Nézze meg az [Office 365-előfizetés Azure RemoteApp használatáról](remoteapp-officesubscription.md). Bemutatja, hogy az Office 365 + Azure RemoteApp használatához szükséges összes információ.**

## <a name="office-365-proplus"></a>Az Office 365 ProPlus
Létrehozhat egy RemoteApp webhelycsoport használata az Office 365 ProPlus sablonként képe. Ezzel a beállítással az Office 365 szolgáltatás kiterjesztése RemoteApp. Rendelkeznie kell egy meglévő előfizetés tervet, és a felhasználók az Office 365 ProPlus szolgáltatás vagy különálló licenccel kell rendelkezniük, vagy az Office 365 segítségével szolgáltatás csomagok.

RemoteApp támogatja az Office 365-ben megosztott számítógép aktiválása. Amikor megosztott számítógép aktiválási engedélyezése, és az [Office-telepítő eszköz](http://www.microsoft.com/download/details.aspx?id=36778) használata a telepítéshez, az Office 365 ProPlus telepíti aktiválás nélkül. Ha egy felhasználó jelek gyűjteménye, amely tartalmazza az Office 365-be, az Office ellenőrzi, hogy ha a felhasználó rendelkezik az Office 365 ProPlus kiépítve. Ha igen, az Office ideiglenes aktiválja az Office 365 ProPlus - ezt az aktiválást továbbra is fennáll, hogy felhasználók jelek kívül a szolgáltatás-ig.

Az Office 365-ben megosztott számítógép aktiválási használatához szeretne [Egyéni sablon](remoteapp-create-custom-image.md) létrehozása és telepítése az Office 365 ProPlus, követően [ezeket az utasításokat](https://technet.microsoft.com/library/dn782858.aspx).

A felhasználók Office 365-licencek, az [Office 365 felügyeleti központon](https://portal.office365.com/)kezelheti. Olvassa el [az Office 365 szolgáltatás csomagok](http://technet.microsoft.com/library/office-365-plan-options.aspx)további információt.  


## <a name="office-2013-professional-plus-trial"></a>Az Office 2013 Professional Plus próbaverziója
Egy 30 napos próbaverzióját RemoteApp, során az Office 2013 Professional Plus (próbaverzió) sablonként képe RemoteApp gyűjtemény létrehozásához is használhatja. A próbaidőszak gyűjtemény Azure Active Directory munkahelyi fiók vagy a Microsoft-fiók használatával is adhatnak a felhasználóknak. Nincsenek további előfizetési szükség.

Ez a indítása a abroncsok és egy jó feeling RemoteApp az Office nagyszerű lehetőséget. Ez a beállítás azonban tervezett értékeléséhez és tesztelés csak. Az Office 2013 Professional Plus (próbaverzió) sablonként képe használatával létrehozott RemoteApp gyűjtemények gyártási mód nem lehet felhasználói, és végén található a próbaidőszak le lesznek tiltva.

## <a name="switching-from-trial-to-production"></a>A gyártási próba-ről
A 30 napig ingyenes próbaverziója indításakor a portálon RemoteApp részében Megjegyzés program tájékoztatja, mennyi ideig kilépett a próba előtt áttérni a Térítéses fiók szükséges. Aktiválja fiókját, és a feljegyzés hivatkozásával gyártási módba vált.

A fiók aktiválásakor ez hatással a fiók a RemoteApp gyűjtemények.

- A Windows Server 2012 R2 vagy az Office 365 ProPlus a webhelysablonok képét futó gyűjtemények fognak Váltás az gyártási programcsomagra. Felhasználói adatok és a beállítások, beleértve a folyamatban lévő munkamenetek, változatlanok maradnak.
- Feltöltött saját egyéni sablon képeket, ha ezeket a képek használatáról gyűjtemények is fog zökkenőmentes lehet.
- Kiértékelés csak az Office 2013 Professional Plus (próbaverzió) sablonként képe szól. Webhelycsoportok fut ez a sablon képe nem kell az-re áttelepített gyártási. "Letiltva" állapotú kerülnek.


Ha Ön nem átmenet gyártási mód a próbaidőszak letelte szerint, a RemoteApp gyűjtemények le lesznek tiltva. Ne aggódjon, – a beállításokat, és a felhasználói adatok menti a program egy másik 90 napon, hogy továbbra is a szolgáltatás aktiválása, és az adatvesztés nélkül gyártási módba vált.
