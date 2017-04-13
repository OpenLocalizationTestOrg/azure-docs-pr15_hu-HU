<properties
    pageTitle="Szerepköralapú hozzáférés ellenőrző hibaelhárítása |} Microsoft Azure"
    description="Segítség kérése az problémák vagy szerepkör alapú hozzáférés-vezérlés erőforrások kérdéseket."
    services="azure-portal"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="kgremban"/>

# <a name="role-based-access-control-troubleshooting"></a>Szerepköralapú hozzáférés-vezérlés – hibaelhárítás

## <a name="introduction"></a>– Bevezetés

[Szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md) egy olyan hatékony szolgáltatás, amely lehetővé teszi az Azure erőforrásokra külön hozzáférés átadása. Ez azt jelenti, hogy érzi benne, hogy egy adott személy pontosan mi szükséges használati joga megadásának és többé. Jó helyen jár időnként az erőforrás modell Azure erőforrások bonyolult lehet, és megértéséhez, hogy pontosan mi meg vannak engedélyek megadása a nehéz lehet.

A dokumentum közli, hogy mire számíthat, amikor az Azure-portálon használja a szerepkörök. Ezek a szerepkörök az összes erőforrástípus terjed ki:

- Tulajdonos  
- Közös munka  
- A képernyőolvasók  

Tulajdonosok és a munkatársak is teljes hozzáférése az adatkezelési folyamatok, de a munkatárs nem tud hozzáférést adhat más felhasználók vagy csoportok. Dolog, amit első kissé érdekesebb olvasó szerepkör, hogy az adott azt fogja hosszabb időt szánni. Nézze meg a [szerepköralapú hozzáférés-vezérlés első lépések a cikk](role-based-access-control-configure.md) további információt a hozzáférést.

## <a name="app-service-workloads"></a>Alkalmazás szolgáltatás munkaterhelésekből

### <a name="write-access-capabilities"></a>Írási funkciók

Adjon hozzáférést a felhasználó csak olvasható egyetlen webalkalmazást, ha egyes funkciók le vannak tiltva, akkor előfordulhat, hogy nem a várt érték. A következő szolgáltatásairól web App alkalmazással (munkatársak és a tulajdonosok) **írási** engedély szükséges, és nem lesz elérhető bármely csak olvasható használatával.

- Parancsok (pl. indítása, leállítása stb.)
- Beállítások módosítása például általános konfigurációs, a méretarány-beállításai, a biztonsági mentés beállításait, és a ellenőrzési beállítások
- Közzétételi hitelesítő adatok és más titkos kulcsok, például a alkalmazás beállításainak és a kapcsolati karakterláncot elérése
- A folyamatos átvitelű naplók
- A diagnosztikai naplók konfigurálása
- Console (parancssor)
- Aktív, és a legutóbbi-alapú telepítés (helyi mely számjegy folyamatos telepítés)
- Becsült töltött
- Webes vizsgálatok
- Virtuális hálózati (csak egy olvasót, ha egy virtuális hálózati korábban beállított írási jogosultsággal rendelkező felhasználó által látható).

Ha nem tud hozzáférni közülük, kell kérje meg a rendszergazdát a web app közreműködői eléréséhez.

### <a name="dealing-with-related-resources"></a>Kapcsolódó források kezelése

Web Apps alkalmazások vannak bonyolultan néhány másik információforrások, amelyek együttműködésnek jelenléte. Az alábbiakban néhány-webhely szokásos erőforráscsoport:

![Web app erőforráscsoport](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Eredményként, ha engedélyez valaki access csak a web App alkalmazásban, nagy, a webhely lap az Azure-portálon kattintson a funkciók le lesznek tiltva.

Ezeket az elemeket, amely megfelel a webhely **alkalmazás szolgáltatáscsomagja** **írási** engedély szükséges:  

- A web app megtekintése a árak réteg (szabad és normál)  
- Méretezés konfigurációs (példányok, a virtuális gép méretét, a Automatikus méretezéssel beállítások száma)  
- Kvóták (tárhely, a sávszélesség Processzor)  

Ezeket az elemeket, amely tartalmazza a webhely teljes **erőforráscsoport** **írási** engedély szükséges:  

- Az SSL-tanúsítványok és kötések (Ennek oka, hogy az SSL-tanúsítványok megosztható az azonos erőforráscsoport webhelyek és geo-hely között)  
- A figyelmeztetési szabályok  
- Automatikus méretezéssel beállításai  
- Háttérismeretek alkalmazásösszetevők  
- Webes vizsgálatok  

## <a name="virtual-machine-workloads"></a>Virtuális gép munkaterhelésekből

Sokkal például az web Apps alkalmazásokkal, néhány a virtuális gép lap a szolgáltatásokhoz írási hozzáféréssel a virtuális gép, illetve más erőforrások erőforráscsoport.

Virtuális gépeken futó tartomány neve, virtuális hálózatok, tároló fiókok és riasztási szabályok kapcsolódnak.

Ezeket az elemeket a **virtuális gép** **írási** engedély szükséges:

- Végpontok  
- IP-címek  
- Lemezen  
- Bővítmények  

**Írja be** a hozzáférést a **virtuális gép**, mind a **erőforráscsoport** (együtt a tartománynév), a csak ezek:  

- Elérhetőség beállítása  
- Kiegyensúlyozott betöltése  
- A figyelmeztetési szabályok  

Ha nem tud hozzáférni közülük, kell kérje meg a rendszergazdát a erőforráscsoport közreműködői eléréséhez.

## <a name="see-more"></a>További lehetőségek jelennek meg
- [Szerepkör-alapú hozzáférés szabályozása](role-based-access-control-configure.md): Ismerkedés a RBAC az Azure-portálon.
- [Beépített szerepkörök](role-based-access-built-in-roles.md): részleteket szeretne megtudni a szabványos RBAC a szerepköröket.
- [Azure RBAC egyéni szerepkörök](role-based-access-control-custom-roles.md): megtudhatja, hogy miként hozhat létre egyéni szerepkörök access igényeinek.
- [Access létrehozása előzmények jelentés módosítása](role-based-access-control-access-change-history-report.md): nyomon követheti a RBAC módosuló szerepkör-hozzárendelés.
