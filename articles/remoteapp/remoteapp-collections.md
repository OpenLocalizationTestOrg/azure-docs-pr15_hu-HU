<properties 
    pageTitle="Milyen típusú webhelycsoport van szüksége az Azure RemoteApp? | Microsoft Azure" 
    description="Tudnivalók a rendelkezésre álló Azure RemoteApp gyűjtemények típusú." 
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



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Milyen típusú webhelycsoport van szüksége az Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp alkalmazások és az erőforrások megosztása bármilyen eszközről felhasználókkal lehetővé teszi. Azt ehhez tartsa lenyomva az alkalmazásokat, és az erőforrások gyűjtemények létrehozásával, és a felhasználók e gyűjtemények ossza. Lehetőség 2 különböző webhelycsoport, a különböző hálózati és a hitelesítési beállítások – vagyis szükséges lépéseket?

Vegyük végigvezetik a különböző szempontok és a választási lehetőségek, végezze el az Azure RemoteApp webhelycsoport ki a legtöbbet kell. 


## <a name="quick-differences-between-the-collection-types"></a>A webhelycsoport-típusokat rövid eltérések

|           | Felhőalapú | Hibrid |
|-----------|-------|--------|
|Egy meglévő VNET használata| igen| igen|
|Active Directory-csatlakoztatott fiókok lehetőséget (DirSync) igényel| nem| igen|
|Tartományokhoz való csatlakozás szükséges| nem| igen|
|A tartományvezérlőnek VNET számára hozzáférhető igényel| nem| igen|

## <a name="cloud-collections"></a>Felhőalapú gyűjtemények
- Gyors létrehozása – a webhelycsoport már gyorsan kiépítve, ami azt jelenti, az alkalmazások beszerzése a felhasználók gyorsabban.
- Jelenítse meg a saját alkalmazások, vagy megoszthatja a miénk. Egyéni kép (az Azure virtuális a beépített) vagy a képeket az előfizetéshez tartozó egyik is használhatja.
- Nem kell konfigurálása a webhelycsoport és a helyszíni tartomány közötti kapcsolat.
- De tetszés szerint a saját Azure VNET hozzáférést biztosít az adatok megosztása a helyszíni környezetbe vagy használhatja a Windows-hitelesítés az erőforrások, mint az SQL Server (az adatbázis-hitelesítés használatával) segítségével.


Az OK gombra Hogyan hozhatok létre egyet?

- Csak a felhő? Hozza létre a **Gyors létrehozása** lehetőség a portálon.
- Felhőalapú + VNET? Hozzon létre, **a VNET létrehozása** parancs, de nem dönt, hogy a tartományhoz.

## <a name="hybrid-collections"></a>Hibrid gyűjtemények
- A helyszíni hálózaton + Azure VNET teljes hozzáférést biztosít.
- Tartomány csatlakozás access-alkalmazások és az adatok tartalmazza. Távoli alkalmazás is hitelesítését a helyszíni Active Directory - hozzáférhet az Ön tartományába tartozó erőforrások.
- Speciális figyelő és felügyeleti meglévő System Center megoldások és a Windows-csoportházirendekkel (keresztül egyéni képe, a Windows Server 2012 R2 épül) engedélyezése
- [Készült ExpressRoute](https://azure.microsoft.com/services/expressroute/) az Azure VNET csatlakozni a helyi VNET támogatja.

Hozzon létre, **a VNET létrehozása** parancs, és válassza a tartományhoz.

## <a name="authentication-options"></a>Hitelesítési beállítások
Azure RemoteApp támogatja a Microsoft-fiókok és Azure Active Directory-fiókok is, de nem az összes webhelycsoportjában minden módszer támogatja. 

| Fiók típusa                      |                                                             | Felhőalapú | Felhőalapú + VNET | Hibrid |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Microsoft-fiók                 |                                                             | igen   | igen          | nem     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Csak az Azure AD                                               | igen   | igen          | nem     |
|                                   | A jelszó-szinkronizálás AD Connect                               | igen   | igen          | igen    |
|                                   | Jelszó-szinkronizálás nélkül AD Connect                            | igen   | igen          | nem     |
|                                   | Active Directory Összevonási szolgáltatásokkal a AD Connect                                       | igen   | igen          | igen    |
|                                   | 3rd Azure támogatott Identitásszolgáltatók (például a Ping) | igen   | igen          | igen    |
| Többtényezős hitelesítés       |                                                             | igen   | igen          | igen    |



### <a name="cloud-and-cloud--vnet"></a>Felhőalapú és Felhőbeli + VNET 
Az felhő gyűjtemények Microsoft-fiók, Azure Active Directory-fiókok vagy a kettő vegyesen is használhatja. A felhasználók számára legjobb működő fiókokba.

Vannak nem adott követelményeket, a Microsoft-fiók használatával. 

Ha szeretné használni az Azure Active Directory-fiókok, győződjön meg arról, hogy a Azure AD-bérlő megfelel az előfizetéséhez társított szeretne. Az Azure RemoteApp előfizetés létrehozásakor a Azure AD-bérlő használta az előfizetés automatikus társított volt. Bármelyik szóló engedély megadása Azure AD-felhasználó ugyanahhoz a bérlőhöz szükséges. Ha szükséges, azt is megteheti, [módosítsa a Azure AD-bérlő](remoteapp-changetenant.md) az előfizetéséhez társított.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hibrid (vagy a felhőben, + az Azure Active Directory + AD)

Azure Active Directory használatával + helyszíni Active Directory rendszer hibrid gyűjteménye. Akkor használja a AD Connect integrálása a két könyvtárak. De van néhány választási lehetőségek, amikor AD Connect beállításaitól. 

Forgatókönyv 2 AD Connect - jelszó-szinkronizálás vagy Active Directory összevonási használatával. Nézze meg az [információkat AD Connect](../active-directory/active-directory-aadconnect.md) tudni, amelyek e működik a legjobban meg.

Azure Active Directory + AD egy felhőalapú gyűjteménnyel is használhatja. Ellenőrizze, hogy hajtsa végre ugyanezt beállítási lépések.

Nézze meg [Azure Active Directory Azure RemoteApp Active Directory-követelményei a +](remoteapp-ad.md) Azure Active Directory beállításához szükséges lépések és az Active Directory.

## <a name="go-create-your-collection"></a>Nyissa meg a webhelycsoport létrehozása!
Az OK gombra hiszem azt már kiegészítéseként azt, ezért csak egyvalamihez balra, hajtsa végre, – az első Azure RemoteApp webhelycsoport létrehozása.

[A felhő webhelycsoport létrehozása](remoteapp-create-cloud-deployment.md) vagy [Hozzon létre egy hibrid gyűjteményt](remoteapp-create-hybrid-deployment.md) – csak első létrehozása.
