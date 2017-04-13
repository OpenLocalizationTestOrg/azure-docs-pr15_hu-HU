<properties
   pageTitle="Általános LDAP összekötő |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan kell konfigurálni a Microsoft általános LDAP összekötő."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-ldap-connector-technical-reference"></a>Általános LDAP összekötő technikai útmutató
Ez a cikk ismerteti az általános LDAP összekötő. A cikk az alábbi termékek vonatkozik:

- A Microsoft Identitáskezelő 2016 (MIM2016)
- A Forefront identitáskezelő 2010 R2 (FIM2010R2)
    -   Használjon 4.1.3671.0 változata, illetve az újabb [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 és FIM2010R2 az összekötő érhető el a [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=717495)letöltésként.

Hivatkozó IETF RFC, amikor a dokumentum használja a formátum (RFC [RFC szám] [RFC dokumentumban szakasz] /), például (RFC 4512/4.3).
További információt találhat a http://tools.ietf.org/html/rfc4500 (kell 4500 cserélje ki a megfelelő RFC számot).

## <a name="overview-of-the-generic-ldap-connector"></a>Az általános LDAP összekötő áttekintése
Az általános LDAP Connector lehetővé teszi a szinkronizálási szolgáltatás integrálása v3 LDAP-kiszolgálóhoz.

A IETF RFC bizonyos tevékenységeket és sémaelemek, például a delta importálása, végrehajtásához szükséges nincs megadva. Ezeket a műveleteket csak LDAP könyvtárak explicit módon megadva támogatottak.

A magas szintű szemszögéből az alábbi funkciók a kiadásban az összekötő által támogatott:

A szolgáltatás | Támogatás
--- | --- |
Csatlakoztatott adatforrás | Az összekötő támogatott összes LDAP v3 kiszolgálókkal (RFC 4510 kompatibilis). A következő teszteléssel: <li>Microsoft Active Directory Lightweight Directory szolgáltatások (AD LDS)</li><li>Microsoft Active Directory globális katalógus AD</li><li>389 címtár-kiszolgáló</li><li>Apache címtár-kiszolgáló</li><li>IBM Tivoli DS</li><li>Isode címtár</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Nyissa meg DJ</li><li>Nyissa meg a Tartományi</li><li>Nyissa meg az LDAP (openldap.org)</li><li>Az Oracle (korábban vasárnap) címtár Server Enterprise Edition</li><li>RadiantOne virtuális könyvtár Server (VDS)</li><li>Címtár-kiszolgáló egy vasárnap</li>**Főbb könyvtárak nem támogatott:** <li>A Microsoft Active Directory tartományi szolgáltatások (AD DS) [használja helyette az Active Directory beépített Connector]</li><li>Az Oracle internetes címtár (Objektumazonosító)</li>
Felhasználási területei   | <li>Objektum életciklus-kezelése</li><li>Csoportok kezelése</li><li>Jelszó kezelése</li>
Műveletek |Az alábbi műveleteket minden LDAP könyvtárak támogatottak: <li>Teljes importálása</li><li>Exportálás</li>A következő műveleteket csak a megadott könyvtárak támogatja:<li>Delta importálása</li><li>Jelszó beállítása, a jelszó módosítása</li>
Séma | <li>Séma lép fel a LDAP séma (RFC3673 és RFC4512/4.2)</li><li>Támogatja a szerkezeti osztályok, aux osztályok és extensibleObject objektum class (RFC4512/4.3.)</li>

### <a name="delta-import-and-password-management-support"></a>Delta importálása és a jelszavát a Dokumentumkezelés támogatásának
A támogatott könyvtárak Delta importálása és a jelszó kezelése:

- Microsoft Active Directory Lightweight Directory szolgáltatások (AD LDS)
    - Támogatja az összes művelet delta importálásra
    - Támogatja a jelszó megadása
- Microsoft Active Directory globális katalógus AD
    - Támogatja az összes művelet delta importálásra
    - Támogatja a jelszó megadása
- 389 címtár-kiszolgáló
    - Támogatja az összes művelet delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása
- Apache címtár-kiszolgáló
    - Nem támogatja delta importálása, mivel ezt a címtárat nem rendelkezik egy állandó módosítási napló
    - Támogatja a jelszó megadása
- IBM Tivoli DS
    - Támogatja az összes művelet delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása
- Isode címtár
    - Támogatja az összes művelet delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása
- Novell eDirectory és NetIQ eDirectory
    - Támogatja a delta importálása műveletek hozzáadása, frissítése és átnevezése
    - Nem támogatja a törlési műveletek delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása
- Nyissa meg DJ
    - Támogatja az összes művelet delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása
- Nyissa meg a Tartományi
    - Támogatja az összes művelet delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása
- Nyissa meg az LDAP (openldap.org)
    - Támogatja az összes művelet delta importálásra
    - Támogatja a jelszó megadása
    - Nem támogatja a jelszó módosítása
- Az Oracle (korábban vasárnap) címtár Server Enterprise Edition
    - Támogatja az összes művelet delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása
- RadiantOne virtuális könyvtár Server (VDS)
    - 7.1.1 verzióját kell használnia vagy újabb
    - Támogatja az összes művelet delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása
-  Címtár-kiszolgáló egy vasárnap
    - Támogatja az összes művelet delta importálásra
    - Támogatja beállítása a jelszó és a jelszó módosítása

### <a name="prerequisites"></a>Előfeltételek
Mielőtt az összekötő használni, győződjön meg arról, hogy a szinkronizálás kiszolgálón a következő:

- 4.5.2 a Microsoft .NET-keretrendszer vagy újabb verzió

### <a name="detecting-the-ldap-server"></a>Annak ellenőrzése, az LDAP-kiszolgáló
Az összekötő támaszkodó különféle technikákat, amelyekkel észleli és azonosítsa az LDAP-kiszolgálóval. Az összekötő a legfelső szintű RootDSE-ben, a szállítói név vagy verzióját használja, és azt megvizsgálja, hogy egyedi objektumok és attribútumok bizonyos LDAP-kiszolgálók olyan ismert keresése a sémában. Ezeket az adatokat, ha található, előre feltölteni az összekötő a konfigurációs beállítások használják.

### <a name="connected-data-source-permissions"></a>Csatlakoztatott adatforrás-engedélyek
Importálás végrehajtása, és az objektumok a csatlakoztatott címtárban műveleteket exportálásához, az összekötő fiók szükséges engedélyekkel kell rendelkeznie. Az összekötő van szüksége az írási engedélyek engedélyezni szeretné exportálni, és olvasási jogosultságát importálhat. Engedélyek konfigurálása a cél könyvtár magát a kezelés változat belül végezhető el.

### <a name="ports-and-protocols"></a>Portok és protokollok
Az összekötő a portszámot, amely alapértelmezés szerint 389 LDAP és az LDAPS 636 a konfigurációban megadott használja.

A LDAPS SSL 3.0-s vagy TLS kell használnia. Az SSL 2.0-s verziója nem támogatott, és nem lehet aktiválni.

### <a name="required-controls-and-features"></a>Szükséges vezérlőket és szolgáltatások
LDAP vezérlők/szolgáltatásai akkor működik megfelelően az összekötő LDAP-kiszolgálón elérhetők:  
`1.3.6.1.4.1.4203.1.5.3`IGAZ vagy hamis szűrők

Az IGAZ vagy hamis szűrő gyakran nem készként való LDAP könyvtárak által támogatott és nem lehet, hogy a **Globális lapon** a **Kötelező funkciók nem található**. LDAP-lekérdezésekben, például amikor több Objektumtípusok importálása **vagy** szűrőkkel szolgál. Ha egynél több objektum típusa importálásához a LDAP-kiszolgáló támogatja ezt a szolgáltatást.

Könyvtárában használatakor a horgony egy egyedi azonosítót esetén az alábbi is kell rendelkezésre (lásd a jelen cikk további információt a [Konfigurálása horgonyok](#configure-anchors) szakasz):  
`1.3.6.1.4.1.4203.1.5.1`Az összes üzemi attribútumok

Ha a címtárban mi is férnek el a címtárhoz egy hívás több objektum van, majd ajánlott lapozási használni. Lapozás a munkát, van szüksége az alábbi lehetőségek közül:

**1 beállítást:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**2 lehetőség:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Ha az összekötő beállítás engedélyezve van a mindkét kérdésre, pagedResultsControl használják.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl csak a törölt objektumok láthatja az importálási módszer USNChanged delta segítségével.

Az összekötő próbálja feltárása a jelenlegi beállításokat a kiszolgálón. Ha a rendszer nem észleli a beállításokat, a program figyelmeztetésben szerepel a globális lapon összekötő tulajdonságok. Nem minden LDAP-kiszolgálók bemutató minden vezérlők/funkciók támogatása, és akkor is, ha ez a figyelmeztetés szerepel, az összekötő problémamentes működik-e.

### <a name="delta-import"></a>Delta importálása
Ha támogatási könyvtárában található, a delta importálása csak érhető el. Az alábbi módszerek jelenleg használatosak:

- LDAP Accesslog. Lásd: a [naplózás http://www.openldap.org/doc/admin24/overlays.html#Access](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP változásnaplója. Lásd: [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Időbélyeg. Az összekötő Novell/NetIQ eDirectory, utolsó dátum/idő használja, hogy első létrehozása és frissítése az objektumokat. Novell/NetIQ eDirectory nem ad egy ezzel egyenértékű azt jelenti, hogy a törölt objektumok beolvasásához. Ez a beállítás is használható, ha nincs más delta importálása módszerrel nem aktív az LDAP-kiszolgálón. Ez a beállítás nem tudja importálása törölt objektumok.
- USNChanged. Lásd: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nem támogatott
A következő LDAP funkciók nem támogatottak:

- LDAP átirányítások kiszolgálók (RFC 4511/4.1.10) között

## <a name="create-a-new-connector"></a>Hozzon létre egy új összekötőt
Általános LDAP összekötő létrehozásához **Szinkronizálási szolgáltatás** válassza a **Kezelés ügynök** és **létrehozása**. Válassza az **Általános LDAP (Microsoft)** összekötő.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Kapcsolat
A kapcsolat lapon a Host, portokkal és Kötésük adatokat kell megadnia. Attól függően, hogy kötési vagyis kijelölt, további adatokat lehet adni, az alábbi szakaszok.

![Kapcsolat](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- Amikor a séma észlelését a kapcsolat időtúllépési beállítást csak használatos az első kapcsolat a kiszolgálóval.
- Ha a kötelező névtelen, majd sem felhasználónév / jelszó és a tanúsítvány nem használhatók.
- A más kötések, adja meg az adatok vagy a felhasználónevét és jelszavát, vagy válasszon egy tanúsítványt.
- Alkalmazás használatakor Kerberos hitelesítést végezni, majd is meg kell adnia a tartomány és a felhasználó tartományát.

A **attribútum alias** mezőbe a sémában RFC4522 szintaxis szerint meghatározott attribútumok szolgál. Következő attribútumok séma észlelése során nem észleli, és az összekötő van szüksége az adott attribútumok azonosítása súgó. A következő példa, hogy a attribútum alias mezőben megfelelően azonosítani a userCertificate attribútum, mint egy bináris attribútum van szükség:

`userCertificate;binary`

Az alábbi képen az, hogy miként a fenti konfiguráció ábrája a táblagéptől:

![Kapcsolat](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Jelölje be a kiszolgáló által létrehozott attribútumok is tartalmazza **olyan sémában műveleti attribútumok** jelölőnégyzetet. Ezek közé tartozik, például amikor az objektum-létrehozott utolsó frissítés ideje attribútumok.

Jelölje be a **Felvétel bővíthető attribútumok sémában** , ha bővíthető objektumok (RFC4512/4.3) használja, és ezzel a beállítással lehetővé teszi, hogy minden attribútumot minden objektum használandó. Ezt a lehetőséget választva lehetővé teszi a séma igen nagy, kivéve, ha a csatlakoztatott könyvtár használja ezt a szolgáltatást az ajánlási legyen a beállítást.

### <a name="global-parameters"></a>A globális paraméterek
A globális paraméterek lapon állítsa be a megkülönböztető delta – módosítási napló és további LDAP funkciókhoz. A lap már előre kitölt lesz az LDAP-kiszolgáló által megadott adatokkal.

![Kapcsolat](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

A felső részén látható magát, a kiszolgáló például a nevet, a kiszolgáló által megadott adatokkal. Az összekötő is ellenőrzi, hogy a kötelező vezérlők szerepel a legfelső szintű RootDSE-ben. Ha ezeket a vezérlőket nem szerepel a listán, egy figyelmeztetés jelenik meg. Néhány LDAP könyvtárak nem szerepelhet a legfelső szintű RootDSE-ben az összes szolgáltatás, és lehetséges, hogy a az összekötő problémamentes működik, akkor is, ha egy figyelmeztető üzenetet nem tartalmaz adatokat.

A **vezérlők támogatott** jelölőnégyzetek működését az egyes műveletek:

- Fastruktúrájú kijelölt Törlés paranccsal, hierarchia törlődik egy LDAP-hívást. A fa törlése nincs bejelölve az összekötő egy rekurzív törlése jelent, ha szükséges.
- Kijelölt lapozható eredményekkel az összekötő nem egy lapozható importálás futtatása a lépéseket a méretet.
- A VLVControl SortControl az ahelyett, hogy az adatok beolvasása az LDAP-címtár pagedResultsControl.
- Ha az összes három lehetőség közül választhat (pagedResultsControl, VLVControl és SortControl) is ki nem jelölt, akkor az összekötő importál egy műveletet, előfordulhat, hogy nem, ha egy nagyméretű könyvtár összes objektumot.
- ShowDeletedControl csak böngészik Delta importálási módszer USNChanged.

A módosítási napló DN az elnevezési környezetben használja a delta – módosítási napló, például **cn = változásnaplója**. Ennek az értéknek meg kell adni a delta importálás láthatja.

Az alábbiakban látható alapértelmezett – módosítási napló DNs listája:

A címtár | Delta – módosítási napló
--- | ---
A Microsoft Active Directory LDS és az Active Directory globális Katalógus | Automatikusan észleli. USNChanged.
Apache címtár-kiszolgáló | Nem érhető el.
A címtár 389 | Módosítási napló. Alapértelmezett értéket kell használnia: **cn = változásnaplója**
IBM Tivoli DS | Módosítási napló. Alapértelmezett értéket kell használnia: **cn = változásnaplója**
Isode címtár | Módosítási napló. Alapértelmezett értéket kell használnia: **cn = változásnaplója**
Novell/NetIQ eDirectory | Nem érhető el. Időbélyeg. Az összekötő felhasználási utoljára frissítve a dátum/idő, hogy a hozzáadott és frissített rekordok.
Nyissa meg a DJ/DS | Módosítási napló.  Alapértelmezett értéket kell használnia: **cn = változásnaplója**
Nyissa meg az LDAP | Az Access jelentkezzen be. Alapértelmezett értéket kell használnia: **cn = accesslog**
Az Oracle DSEE | Módosítási napló. Alapértelmezett értéket kell használnia: **cn = változásnaplója**
RadiantOne VDS | Virtuális könyvtár. A könyvtár VDS csatlakoztatott függ.
Címtár-kiszolgáló egy vasárnap | Módosítási napló. Alapértelmezett értéket kell használnia: **cn = változásnaplója**

A jelszó attribútum a nevét, az összekötő segítségével adja meg a jelszót a jelszó módosítása attribútum és jelszó megadása műveletek.
Ez az érték **userPassword** meg alapértelmezés szerint be van, de módosítható, ha egy adott LDAP rendszer szükséges.

További partíciók listájában található további névtér nem automatikusan észleli hozzáadni. Ez a beállítás például használható, ha több kiszolgálók alkotó egy logikai fürtre, és az összes kell importálni egy időben. Ugyanúgy, mint az Active Directory beállíthatja, hogy több tartományt az egyik erdő, de minden tartomány megosztása egy séma, azonos is lehet szimulált: a további névtér ír ebbe a mezőbe. Minden névtér importálhat különböző kiszolgálók, és további van-e beállítva a partíciók konfigurálása és hierarchiákat lapon. Ctrl + Enter segítségével egy új sort.

### <a name="configure-provisioning-hierarchy"></a>Állítsa be a kiépítési hierarchia
Ezen az oldalon a DN összetevőjét, például a szervezeti feleltesse meg az objektum típusa, a kiépítéstől kell, például szervezeti_egység szolgál.

![Hierarchia kiépítése](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

A kiépítési hierarchia konfigurálásával beállíthatja az összekötő automatikusan szerkezetbe szükség esetén. Ha például egy névtér adatközpont esetén = contoso, adatközpont com és egy új objektum cn = Mintaügyvezető, ou = = Seattle, c = US, adatközpont = contoso, adatközpont = com már kiépítve, majd az összekötő típusa ország az Amerikai Egyesült Államok és a Seattle-szervezeti_egység objektum hozhat létre, ha azokat, még nem szerepel a címtár.

### <a name="configure-partitions-and-hierarchies"></a>Partíciót és hierarchiák beállítása:
A partíciók és hierarchiákat lapján jelölje be minden névtér az objektumok importálása és exportálása szeretné.

![Partíciót](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Minden névtér hogy az is lehetséges a csatlakozási képernyőn megadott értékeket szeretne felülíró csatlakozási beállítások konfigurálása. Ha ezeket az értékeket az alapértelmezett üres értéket marad, az adatokat az adatkapcsolat képernyőről használják.

Is érdemes lehet kiválasztani, mely tárolók és szervezeti az összekötő kell importálása és exportálása.

### <a name="configure-anchors"></a>Horgonyok konfigurálása
Muszáj mindig egy előre konfigurált érték erre a lapra, és nem módosíthatók. Ha a kiszolgáló szállító van megadva, majd a horgony előfordulhat, hogy töltik meg megváltoztatható tulajdonság, például egy objektum GUID. Ha nem észlelhető vagy ismert, hogy nem megváltoztatható tulajdonság, majd az összekötő használja a horgony dn (megkülönböztető név).

![horgonyok](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Az alábbiakban látható LDAP-kiszolgálók és a használt horgony listája:

A címtár | Horgony attribútum
--- | ---
A Microsoft Active Directory LDS és az Active Directory globális Katalógus | objectGUID
389 címtár-kiszolgáló | DN
Apache címtár | DN
IBM Tivoli DS | DN
Isode címtár | DN
Novell/NetIQ eDirectory | GLOBÁLISAN EGYEDI AZONOSÍTÓJA
Nyissa meg a DJ/DS | DN
Nyissa meg az LDAP | DN
Az Oracle ODSEE | DN
RadiantOne VDS | DN
Címtár-kiszolgáló egy vasárnap | DN

## <a name="other-notes"></a>Más jegyzetek
Ez a témakör szempontokat, ez az összekötő jellemző vagy más okokból kell szem előtt tartani az adatokat.

### <a name="delta-import"></a>Delta importálása
A megnyitott LDAP delta vízjel UTC dátum/idő. Emiatt szinkronizálni kell a órák FIM-szinkronizálási szolgáltatás és a megnyitott LDAP között. Ha nem, a delta – módosítási napló bizonyos elemeit előfordulhat, hogy hagyható.

A Novell eDirectory a delta importálása nem érzékeli bármelyik objektum törlése. Emiatt egy teljes importálásban rendszeres keresse meg az összes törölt objektumok futtatásához szükséges.

Egy dátum/idő alapuló delta – módosítási napló könyvtárak, az ajánlott rendszeres időközönként egy teljes importálás futtatásához. Ez a folyamat lehetővé teszi, hogy a szinkronizálási motor kereséséhez és eltérő az LDAP-kiszolgáló és a jelenleg a összekötő terület tartalma között.

## <a name="troubleshooting"></a>Hibaelhárítás

-   Kapcsolatos hibák elhárítása az összekötő naplózás engedélyezése a további tudnivalókért lásd az [annak engedélyezése esemény-nyomkövetés nyomon követése az összekötőkhöz](http://go.microsoft.com/fwlink/?LinkId=335731).
