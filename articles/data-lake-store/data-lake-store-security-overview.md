<properties
   pageTitle="Biztonsági adatok tó áruház áttekintése |} Microsoft Azure"
   description="Mi a Azure tó adattár az biztonságosabbá nagy adattár ismertetése"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Az Azure tó adattár biztonsága

Sok nagyvállalatoknak telik segíti őket az intelligens döntések üzleti mélyebb elemzéséhez nagy adatok előnyeit. Előfordulhat, hogy a szervezet összetett és szabályozott környezetben, a különböző felhasználók növekvő számú. Nagyon fontos kattintva győződjön meg arról, hogy kritikus üzleti adatok tárolásának több biztonságosan a megfelelő szintű hozzáféréssel rendelkezzenek az egyes felhasználók a vállalati. Azure tó adattár projektcsapatok e biztonsági követelményeknek. Ebben a cikkben megismerkedhet a a biztonsági funkciókkal adatok tó áruház, például:

* Hitelesítés
* Engedély
* Hálózati elkülönítési
* Adatok védelme
* A naplózás

## <a name="authentication-and-identity-management"></a>Hitelesítés és -Identitáskezelés kezelése

Hitelesítés amennyivel egy felhasználó személyazonosságára hitelesítését a felhasználók hogyan kommunikáljon a tó áruházzal vagy bármely szolgáltatással tó adattár kapcsolódó során a rendszer. Az Identitáskezelés és a hitelesítéshez tó adattár használja [Azure Active Directory](../active-directory/active-directory-whatis.md), a teljes azonosítása és a hozzáférés kezelése felhőalapú megoldás egyszerűbbé teszi a felhasználók és csoportok kezelése.

Lehet, hogy minden Azure előfizetés az Azure Active Directory-példány társítva. Csak a felhasználók és a szolgáltatás identitások az Azure Active Directory szolgáltatásban definiált tó adattár fiókja hozzáférhetnek a Azure portál parancssori eszközök használatával, vagy ügyfélalkalmazásokban keresztül a szervezet hozza létre az Azure adatok tó áruházból SDK segítségével. Azure Active Directory használata egy központi access vezérlő mechanizmusként főbb előnyei a következők:

* Egyszerűsített identitás életciklus-kezelése. Felhasználó vagy egy szolgáltatást (a szolgáltatás fő azonosító) gyorsan létrehozható és gyorsan visszavonva egyszerűen törlése vagy letiltása a fiókot, a címtárban.

* Többtényezős hitelesítés. [Többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication.md) tartalmaz egy további felhasználói bejelentkezések és tranzakciók biztonsági szintet.

* Hitelesítés bármely ügyfélprogramból, például OAuth vagy OpenID egy szabványos megnyitott protokollon keresztül.

* A vállalati címtárszolgáltatásaival és a felhő Identitásszolgáltatók összevonási.

## <a name="authorization-and-access-control"></a>Hitelesítés és a hozzáférés-vezérlés

Azure Active Directory hitelesíti a felhasználót, hogy a felhasználó hozzáférhet az Azure adatok tó tár, miután engedélyezési szabályozza az adatok tó tároló engedélyeinek hozzáférését. Tó adattár elválasztja a fiókkal kapcsolatos és adatok kapcsolatos műveletek engedélyt a következő módon:

* [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) Fiókok kezelése Azure által biztosított (RBAC)
* Az áruházban eléréséhez POSIX vezérlés


### <a name="rbac-for-account-management"></a>A fiókok kezelésének RBAC

Négy alapvető szerepkörök tó adattár alapértelmezés szerint megadva. A szerepkörök lehetővé teszi a különböző műveletek az Azure portálon, a PowerShell-parancsmagok és a REST API-khoz tó adattár-fiókjában. A tulajdonos és munkatársi szerepkörök felügyeleti függvények számos végezhet a fiókot. A felhasználók, akik csak használhatja az Olvasó szerepkör rendelhet.

![RBAC szerepkörök] (./media/data-lake-store-security-overview/rbac-roles.png "RBAC szerepkörök")

Tartsa szem előtt, hogy szerepkörök számlakezelési hozzá vannak rendelve, bár néhány szerepkör adatokhoz való hozzáférés befolyásolja. Akkor használja a hozzáférés-vezérlési listák való hozzáférés korlátozása műveleteket hajthat végre a felhasználó a fájlrendszerben. A következő táblázat mutatja a management rights és az alapértelmezett szerepkörök adatok hozzáférési engedélyeit.

| Szerepkörök                    | A rights Management               | Adatok jogokkal | MAGYARÁZAT |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Nincs szerepkört         | Nincs lehetőség                            | Vezérlés tevékenységére    | A felhasználó nem használható az Azure portálja vagy az Azure PowerShell-parancsmagok tallózással keresse meg a tó adattár. A felhasználó csak parancssori eszközök használhatja.
| Tulajdonos  | Az összes  | Az összes  | A tulajdonos feladata rendszeradminisztrátori. A szerepkör kezelhetik a szolgáltatás és az teljes hozzáférés az adatokhoz.
| A képernyőolvasók   | Csak olvasható  | Vezérlés tevékenységére    | Az Olvasó szerepkör megtekintése minden vonatkozó számlakezelési, amelyek például felhasználói milyen szerepkör van rendelve. Az Olvasó szerepkör nem bármilyen módosítást.   |
| Közös munka              | Hozzáadás és eltávolítás szerepkörök kivéve | Vezérlés tevékenységére    | A közös munka szerepkör-fiókkal rendelkezik, például a telepítések és létrehozásáról és értesítések kezelése néhány menően kezelheti. A munkatársi szerepkörök nem lehet hozzáadni, vagy távolítsa el a szerepköröket.
| Felhasználói hozzáférés-rendszergazda | Hozzáadás és eltávolítás szerepkörök            | Vezérlés tevékenységére    | A felhasználói hozzáférés rendszergazdai szerepkör fiókok felhasználó hozzáférését kezelheti. |

Útmutatásért olvassa el a [felhasználó vagy biztonsági csoportokat tó adattár fiókokhoz hozzárendelése](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts)című témakört.

### <a name="using-acls-for-operations-on-file-systems"></a>Hozzáférés-vezérlési listák használata műveletek a fájlrendszerben

Tó adattár hierarchikus fájlrendszer például Hadoop elosztott fájl rendszer (hdfs) lehetőségre, és [POSIX hozzáférés-vezérlési listák](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)támogat. Olvasás (r) ellenőrzi, írási (w) és végrehajtása (x) engedéllyel a tulajdonos szerepkört, a tulajdonosok csoport és a más felhasználók és csoportok erőforrások. Az adatok tó áruházból nyilvános előnézetben (a jelenlegi kiadás) a hozzáférés-vezérlési listák engedélyezve vannak a legfelső szintű mappa, almappák és egyes fájlokat. A hozzáférés-vezérlési listák gyökérmappájából alkalmazható is alkalmazása az összes alárendelt mappákat és fájlokat.

Azt javasoljuk, hogy definiálása hozzáférés-vezérlési listák több felhasználóhoz [biztonsági csoportok](../active-directory/active-directory-accessmanagement-manage-groups.md)használatával. Felhasználók felvétele egy olyan biztonsági csoportot, és hozzárendelheti a hozzáférés-vezérlési listák fájl vagy mappa az adott biztonsági csoport. Ez akkor hasznos, ha szeretné egyéni hozzáférést, mert Ön korlátozott hozzáadása egyéni hozzáférési kilenc bejegyzéseinek legfeljebb. Jobban biztonságos adatok adatok tó tárban tárolt Azure Active Directory biztonsági csoportok használatával kapcsolatos további tudnivalókért olvassa el a [felhasználó vagy biztonsági csoport, a hozzáférés-vezérlési listák az Azure tó adattár fájlrendszerben hozzárendelése](data-lake-store-secure-data.md#filepermissions)című témakört.

![Normál és egyéni access lista] (./media/data-lake-store-security-overview/adl.acl.2.png "Normál és egyéni access lista")

## <a name="network-isolation"></a>Hálózati elkülönítési

Hozzáférés szabályozása a hálózaton szinten a adattár segít a adatok tó pontra. Tűzfalak létrehozni, és az IP-címtartományokat meghatározása a megbízható ügyfélalkalmazások. Az IP-címtartományokat csak a IP-címet, a megadott tartományba ügyfelek tó adattár csatlakozhat.

![Tűzfal beállításainak és IP-hozzáférés] (./media/data-lake-store-security-overview/firewall-ip-access.png "Tűzfal beállításainak és IP-cím")

## <a name="data-protection"></a>Adatok védelme

Szervezetek szeretné biztosítani, hogy a fontos üzleti adatok biztonságos az életciklus során. A hálózaton átvitt adatok esetén tó adattár szabványos átviteli Layer Security (TLS) protokoll segítségével védik az adatokat, amely áthelyezi egy ügyfél és tó adattár között.

Adatok védelmére vonatkozó adatokat a többi érhetők el a későbbi kiadásokban.

## <a name="auditing-and-diagnostic-logs"></a>Naplózás és a diagnosztikai naplók

Attól függően, hogy keres kezelésével kapcsolatos tevékenységek vagy a tevékenységekkel kapcsolatos adatok naplók naplózási vagy a diagnosztikai naplók is használhatja.

*  Tevékenységek kezelésével kapcsolatos Azure erőforrás-kezelő API-hoz és a naplókat keresztül is jelenik meg az Azure-portálon.
*  Adatok tevékenységekkel WebHDFS REST API-khoz használja, és a diagnosztikai naplók keresztül is jelenik meg az Azure-portálon.

### <a name="auditing-logs"></a>A naplózás naplók

Ahhoz, hogy megfeleljenek előírásokat, egy szervezet módosításához szükség lehet a megfelelő könyvvizsgálati ellenőrzéshez Ha alaposabban meg az adott események. Tó adattár beépített figyelése és naplózás, és naplózza minden fiók kezelése elemre.

A fiók kezelése könyvvizsgálati ellenőrzéshez megtekintése, és jelentkezzen be a kívánt oszlop kijelöléséhez. Naplókat Azure tárolóhoz lehet exportálni.

![Naplókat] (./media/data-lake-store-security-overview/audit-logs.png "Naplókat")

### <a name="diagnostic-logs"></a>A diagnosztikai naplók

Könyvvizsgálati ellenőrzéshez adatokhoz való hozzáférés beállítása az Azure-portálon (a diagnosztikai beállítások), és a naplókat tároló Azure Blob tároló fiókot létrehozni.

![A diagnosztikai naplók] (./media/data-lake-store-security-overview/diagnostic-logs.png "A diagnosztikai naplók")

Miután beállította a diagnosztikai beállítások, megtekintheti a naplókat a **Diagnosztikai naplók** lapon.

A diagnosztikai naplók Azure adatok tó áruházzal használata a további tudnivalókért lásd: [tó adattár az Access a diagnosztikai naplók](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Összefoglalás

Vállalati felhasználóknak igény adatok analytics felhő platformot biztonságos és könnyen használható. Azure tó adattár projektcsapatok ezeknek a követelményeknek, az Identitáskezelés és Azure Active Directory-integráció, vezérlés-alapú hitelesítés, hálózati elkülönítési, és a hálózaton átvitt adatok titkosítása útján hitelesítéshez vigye (lesz a jövőben) címét és naplózás.

Ha azt szeretné, hogy tó adattár új szolgáltatások, küldjön visszajelzést [adatok tó áruházból UserVoice fórumán](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Lásd még:

- [Azure tó adattár áttekintése](data-lake-store-overview.md)
- [Első lépések a tó adattárhoz](data-lake-store-get-started-portal.md)
- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
