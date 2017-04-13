<properties
   pageTitle="Azure AD Connect szinkronizálása: műveleti feladatok és szempontok |} Microsoft Azure"
   description="Ez a témakör műveleti feladatok Azure AD Connect szinkronizálási és Felkészülés az összetevő működési módját ismerteti."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect szinkronizálása: szempontokat és műveleti feladatok
Ez a témakör célja a Azure AD Connect szinkronizálásához műveleti feladatok ismertetik.

## <a name="staging-mode"></a>Fejlesztői üzemmód
Fejlesztői üzemmód több alkalmazási eseteit, beleértve a használhatók:

-   Magas elérhető.
-   Tesztelje és üzembe új konfigurációs módosítások.
-   Új kiszolgáló bevezetésére, és a régi le.

A kiszolgáló átmeneti tárolásra szolgáló módban, a módosításokat végezni a konfiguráción, és megtekintése a módosításokat, mielőtt aktívvá tenni a kiszolgáló. Lehetővé teszi a teljes importálás, és ellenőrizze, hogy minden módosítás várható módosítása előtt tanácsos lehet ezeket a termelési környezetén be teljes szinkronizálás futtatása.

A telepítés során jelölje ki a kiszolgáló **átmeneti tárolására módot**kell. Nem működik, akkor bármelyik exportnak, de ez a művelet a kiszolgáló importálása és szinkronizálása aktív lesz. A kiszolgáló átmeneti tárolásra szolgáló módban nem fut a jelszó-szinkronizálás vagy jelszó visszaírást, még akkor is, ha a kijelölt ezek a funkciók a telepítés során. Fejlesztői üzemmód letiltása esetén a kiszolgáló elindítja az exportálás, lehetővé teszi, hogy a jelszó-szinkronizálás, és lehetővé teszi, hogy a jelszó visszaírást.

Az Exportálás kényszeríthet továbbra is a szinkronizálási szolgáltatás segítségével.

A kiszolgáló átmeneti tárolásra szolgáló módban továbbra is az Active Directory és Azure Active Directory kap a módosításokat. Egy másik kiszolgáló feladatai fölé egy példányát a legutóbbi változások és lehet nagyon gyors figyelembe veszi mindig rendelkezik. Ha az elsődleges kiszolgálóhoz konfigurációs módosítások elvégzése feladata a azonos módosításához a kiszolgáló átmeneti tárolására módban.

Azok az azt ismerő régebbi szinkronizálási technológiákkal az átmeneti tárolásra szolgáló mód eltér, mert a kiszolgáló rendelkezik saját SQL-adatbázis. E architektúra lehetővé teszi, hogy az átmeneti tárolásra szolgáló mód kiszolgáló a egy másik adatközponthoz található.

### <a name="verify-the-configuration-of-a-server"></a>A kiszolgáló konfigurációjának ellenőrzése
Ez a módszer alkalmazásához kövesse az alábbi lépéseket:

1. [Előkészítése](#prepare)
2. [Importálása és szinkronizálása](#import-and-synchronize)
3. [Ellenőrzése](#verify)
4. [Váltás az active server](#switch-active-server)

#### <a name="prepare"></a>Előkészítése

1. Telepítse az Azure AD Connect, válassza a **mód átmeneti tárolására**, és a kijelölés megszüntetése, **Indítsa el a szinkronizálást** a telepítés varázsló utolsó lapján. Ebben az üzemmódban lehetővé teszi a szinkronizálási motor kézi futtatása.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Jelentkezzen ki a/bejelentkezési és a start menüből válassza a **Szinkronizálási szolgáltatás**.

#### <a name="import-and-synchronize"></a>Importálása és szinkronizálása

1. Jelölje ki az **összekötőket**, és jelölje ki az első összekötőt az **Active Directory tartományi szolgáltatások**típusú. Kattintson a **Futtatás**parancsra, jelölje be a **teljes**, és **az OK gombra**. Az ilyen típusú összekötők kövesse az alábbi lépéseket.
2. A típus **Azure Active Directory (Microsoft)**, jelölje ki az összekötőt. Kattintson a **Futtatás**parancsra, jelölje be a **teljes**, és **az OK gombra**.
3. Ellenőrizze, hogy az összekötők lap van kijelölve. Az egyes összekötő típusú **Active Directory tartományi szolgáltatások**kattintson a **Futtatás**parancsra, jelölje be a **Szinkronizálás Delta**és **az OK gombra**.
4. A típus **Azure Active Directory (Microsoft)**, jelölje ki az összekötőt. Kattintson a **Futtatás**parancsra, jelölje be a **Szinkronizálás Delta**és **az OK gombra**.

Most már meg van előkészített exportálás változik Azure Active Directory és a helyszíni Active Directory (Ha a hibrid Exchange-telepítés használata esetén). A következő lépésekkel nézze meg, mi az készül váltani az adatok exportálása a könyvtárak ténylegesen megkezdése előtt teszi lehetővé.

#### <a name="verify"></a>Ellenőrzése

1. Egy cmd kérdés és megnyitásához`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Futtatása:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Az összekötő neve szinkronizálás szolgáltatásban találhatók. Van egy hasonló "contoso.com – AAD" nevet az Azure Active Directory.
3. Futtatása:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Ha a % temp %, amely a Microsoft Excel is vizsgálni export.csv nevű fájlban. Ez a fájl lehet exportálni kívánt minden módosítást tartalmaz.
5. Az adatok és a konfigurációs el a szükséges módosításokat, és ezeket a lépéseket ismét (importálása és szinkronizálása és ellenőrzés) futtatása mindaddig, amíg a módosításokat, amely lehet exportálni kívánt várják.

**A export.csv fájl ismertetése**

A fájlt a legtöbb értetődő. Néhány rövidítéseket lehet áttekinteni a tartalmat:

- OMODT – objektumtípus módosítását. Azt jelzi, hogy a művelet egy objektum szintjén egy hozzáadása, frissítése vagy törlése.
- AMODT – attribútum módosítás típusa. Azt jelzi, hogy a művelet attribútum szinten egy hozzáadása, frissítése vagy törlése.

Ha az attribútumérték többértékű, nem minden módosításról jelenik meg. Csak az értékek hozzáadása és eltávolítása egy látható.

#### <a name="switch-active-server"></a>Váltás az active server

1. A jelenleg aktív kiszolgálón kapcsolja ki a kiszolgáló (DirSync/FIM/Azure AD-szinkronizálás), azt nem exportálja az Azure Active Directory, vagy állítsa be azt az átmeneti tárolására mód (Azure AD Connect).
2. A telepítés varázslót a kiszolgáló **átmeneti tárolására üzemmód** és **átmeneti tárolására üzemmód**letiltása.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Vészhelyreállítás
A végrehajtás tervezés része a Mi a teendő, ha van egy katasztrófa hol elveszti a szinkronizálási kiszolgáló megtervezése. Vannak más modellek és melyiket használja attól függ, számos tényező, többek között:

-   Mi az a hibatűrést nem tudja módosíthassa objektumok Azure AD az állásidőt során?
-   A jelszó-szinkronizálás használata esetén végezze el a felhasználók fogadja el, hogy kell-e a régi jelszót használja az Azure Active Directory abban az esetben, hogy módosítja a helyszíni?
-   Rendelkezik a valós idejű műveletek, például a jelszó visszaírást függőség?

Attól függően, hogy a fenti kérdések és a szervezet házirend válasz a következő stratégiák egyik valósítható meg:

-   Szükség esetén újraépítéséhez.
-   Van **mód átmeneti**neve tartalék készenléti kiszolgáló.
-   Virtuális gépeken futó használja.

Ha nem használja a beépített SQL Express adatbázist, majd is nézzen meg a [SQL magas](#sql-high-availability) elérhetősége.

### <a name="rebuild-when-needed"></a>Szükség esetén újraépítéséhez
Egy életképes stratégia szükség esetén a kiszolgáló újraépítő megtervezése. Általában telepítése a szinkronizálási motor és a kezdeti importálása és szinkronizálása tud végezni néhány órán belül. Tartalék kiszolgáló nem érhető el, érdemes lehet ideiglenes használatával a tartományvezérlőnek a sync engine host.

A szinkronizálási motor kiszolgáló nem tárolja a objektumokról bármely állam, az adatbázis is újraépíti, az Active Directory és Azure Active Directory adatokból. Ha be szeretne kapcsolódni az objektumok a helyszíni és felhőbeli **sourceAnchor** attribútum használják. Ha a kiszolgáló a meglévő objektumok a helyszíni és felhőbeli, újra kell építenie, majd a szinkronizálás motor megfelelő objektumaihoz közös újra meg újratelepítése. Ha a dokumentum mentése dolgok a kiszolgálón, például a szűrés és a szinkronizálás szabályok beállításainak módosításait. Előzetes teendők szinkronizálása újra kell alkalmazza az egyéni őket.

### <a name="have-a-spare-standby-server---staging-mode"></a>Tartalék készenléti kiszolgáló - átmeneti tárolására módban van
Ha egy összetettebb környezetben, majd az egy vagy több készenléti kiszolgálók problémákat ajánlott. A telepítés során lehetősége van engedélyezni a kiszolgáló **átmeneti tárolására módot**kell.

További információra kíváncsi olvassa el a [mód átmeneti tárolására](#staging-mode)című témakört.

### <a name="use-virtual-machines"></a>Virtuális gépeken futó használata
A közös és támogatott módja a szinkronizálási motor virtuális gépen futtatásához. Abban az esetben, ha a fogadó problémái vannak, a kép a szinkronizálási motor kiszolgálóval telepíthető át egy másik kiszolgálóra.

### <a name="sql-high-availability"></a>SQL-magas elérhetősége
Ha nem használ az SQL Server Express Azure AD Connect épített, majd az SQL Server magas elérhetősége figyelembe kell venni. A támogatott csak magas elérhetősége megoldást SQL fürtözés. Nem támogatott megoldások közé tartoznak, tükrözés, majd mindig a.

## <a name="next-steps"></a>Következő lépések

**Témakörök – áttekintés**  

- [Azure AD Connect szinkronizálása: dátumtáblázatok ismertetése és szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)  
- [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)  
