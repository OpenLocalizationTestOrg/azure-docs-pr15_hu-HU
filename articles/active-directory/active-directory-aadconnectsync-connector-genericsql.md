<properties
   pageTitle="Általános SQL-összekötő |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan kell konfigurálni a Microsoft általános SQL-összekötő."
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

# <a name="generic-sql-connector-technical-reference"></a>Általános SQL-összekötő technikai útmutató

Ez a cikk ismerteti az általános SQL-összekötő. A cikk az alábbi termékek vonatkozik:

- A Microsoft Identitáskezelő 2016 (MIM2016)
- A Forefront identitáskezelő 2010 R2 (FIM2010R2)
    -   Használjon 4.1.3671.0 változata, illetve az újabb [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 és FIM2010R2 az összekötő érhető el a [Microsoft letöltőközpontból](http://go.microsoft.com/fwlink/?LinkId=717495)letöltésként.

Ez a művelet az összekötő megtekintéséhez ismertető [Általános SQL összekötő lépésről lépésre](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Az általános SQL-összekötő áttekintése

Az általános SQL-összekötő lehetővé teszi a szinkronizálási szolgáltatás integrálása egy adatbázis-rendszer, amely felajánlja az ODBC-kapcsolat.  

A magas szintű szemszögéből az alábbi funkciók a kiadásban az összekötő által támogatott:

A szolgáltatás | Támogatás
--- | ---
Csatlakoztatott adatforrás | Az összekötő összes 64 bites ODBC-illesztőprogramok támogatott. A következő teszteléssel: <li>A Microsoft SQL Server- és az SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 és 11 g</li><li>MySQL 5.x</li>
Felhasználási területei   | <li>Objektum életciklus-kezelése</li><li>Jelszó kezelése</li>
Műveletek | <li>A teljes importálás és Delta importálás, exportálás</li><li>Az exportálás: Hozzáadása, törlése, frissítés, és cseréje</li><li>Jelszó beállítása, a jelszó módosítása</li>
Séma | <li>Objektumok és attribútumok dinamikus felfedezése</li>

### <a name="prerequisites"></a>Előfeltételek
Mielőtt az összekötő használni, győződjön meg arról, hogy a szinkronizálás kiszolgálón a következő:

- 4.5.2 a Microsoft .NET-keretrendszer vagy újabb verzió
- 64 bites ODBC ügyfél-illesztőprogramok

### <a name="permissions-in-connected-data-source"></a>Engedélyek az csatlakoztatott adatforrás
Hozzon létre, vagy a támogatott műveletek elvégzését az általános SQL-összekötő, kell rendelkeznie:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Portok és protokollok
A az ODBC-illesztőprogram működéséhez szükséges portokon olvassa el az adatbázis szállító dokumentációját.

## <a name="create-a-new-connector"></a>Hozzon létre egy új összekötőt
Általános SQL összekötő létrehozásához **Szinkronizálási szolgáltatás** kijelölése **Management Agent** és **létrehozása** Válassza az **Általános SQL (Microsoft)** összekötő.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Kapcsolat
Az összekötő ODBC DSN-fájlként kapcsolatot használja. A **Felügyeleti eszközök**start menüjében található **ODBC-adatforrások** DSN-fájl létrehozása. A felügyeleti eszközben hozzon létre egy **Fájl DSN** , az összekötő is biztosítható.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

A csatlakozási képernyője az első állapotában, hozzon létre egy új általános SQL összekötőt. Először meg kell adnia az alábbi adatokat:

- DSN fájl elérési útja
- Hitelesítés
    - Felhasználónév
    - Jelszó

Az adatbázis támogatnia kell az alábbi hitelesítési módszerek egyikét:

- **Windows-hitelesítés**: A hitelesítő adatbázis használja a Windows-hitelesítőadatok a felhasználó ellenőrzésére. A felhasználónév és jelszó megadott használják a adatbázissal hitelesítést végezni. Ehhez a fiókhoz szükséges engedélyeket az adatbázishoz.
- **SQL-hitelesítés**: A hitelesítő adatbázis használ, a felhasználónév és jelszó definiálása a csatlakozási képernyője az adatbázishoz való csatlakozáshoz. Ha a felhasználó nevét és jelszavak a DSN fájlt tárolja, a csatlakozási képernyőn megadott hitelesítő adatok elsőbbséget.
- **Azure SQL-adatbázis hitelesítés**: további információért olvassa el a [Csatlakozás az SQL adatbázis által használatával Azure Active Directory-hitelesítés](..\sql-database\sql-database-aad-authentication.md).

**DN horgony**: Ha ezt a beállítást választja, a DN üzenetként is a horgony attribútum. Az egyszerű körű is használható, hanem is rendelkezik az alábbi korlátozást:

-   Összekötő csak egy objektum típusa támogatja. Hivatkozás attribútumokat ezért csak hivatkozhat ugyanaz az objektumtípus.

**Exportálása típusát: az objektum csere**: exportálás során csak néhány attribútum megváltozott, amikor a teljes objektum összes attribútumokkal rendelkező exportálásának és lecseréli a meglévő objektumot.

### <a name="schema-1-detect-object-types"></a>Séma (Hibakeresés objektumtípusok) 1
Ezen a lapon fogja állítsa be, hogyan fog az összekötő a különböző objektumtípusok megkeresése az adatbázisban.

Minden objektumtípus partíciót mutatják be, és az konfigurált további **partíciók konfigurálása**és hierarchiákat.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Objektumtípus észlelési módszer**: az összekötő támogatja az objektum típusa észlelési módszerek.

- **Rögzített érték**: Ön megadja-e a objektumtípusok listáját vesszővel tagolt listáját. Példa: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Táblázat, a nézet és a tárolt eljárás**: Adja meg a tábla, nézet/tárolt eljárás nevét és az oszlop nevét, amelybe a objektumtípusok listáját. Ha a tárolt eljárás, majd is biztosít paraméterek formátumban **[név]: [irány]: [érték]**. Adja meg az egyes paraméterek külön sorban (Ctrl + Enter billentyűkombinációt az új sor első kell használni).  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **SQL-lekérdezés**: Ezzel a beállítással, amely visszaadja a egyetlen oszlopot tartalmazó objektumtípusok, így például SQL-lekérdezés megadása `SELECT [Column Name] FROM TABLENAME`. A visszaadott karakterlánc típusú (varchar) kell lennie.

### <a name="schema-2-detect-attribute-types"></a>Séma 2 (Hibakeresés attribútum tartalomtípusoknál)
Ezen a lapon fogja állítsa be, hogyan attribútum nevét és típusát szeretnék az általuk észlelt. Minden objektumtípus az előző oldal észlelt a beállítási lehetőségek jelennek meg.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Attribútum típusa észlelési módszer**: az összekötő támogatja az attribútum típusa észlelési módszerek minden észlelt objektumtípus séma 1 képernyőn.

- **Táblázat, a nézet és a tárolt eljárás**: Adja meg a tábla, nézet/tárolt eljárás, amellyel az attribútum nevek keresése a nevét. Ha a tárolt eljárás, majd is biztosít paraméterek formátumban **[név]: [irány]: [érték]**. Adja meg az egyes paraméterek külön sorban (Ctrl + Enter billentyűkombinációt az új sor első kell használni). A többértékű attribútum attribútum nevét észlelésére adja meg a táblák vagy nézetek vesszővel tagolt listáját. Többértékű esetek nem támogatottak, amikor a szülő-gyermek táblázat van nevű oszlopot.
- **SQL-lekérdezés**: Ezzel a beállítással, például attribútum neveivel egyetlen oszlopot ad eredményül egy SQL-lekérdezés megadása `SELECT [Column Name] FROM TABLENAME`. A visszaadott karakterlánc típusú (varchar) kell lennie.

### <a name="schema-3-define-anchor-and-dn"></a>Séma 3 (meghatározás horgony és DN)
Ezen az oldalon DN attribútumot minden észlelt objektumtípus, és horgony konfigurálása teszi lehetővé. Megadhatja, hogy a horgony egyedi több attribútum.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Logikai és a többértékű attribútum nem jelennek.
- Azonos attribútum nem használható DN és horgony, kivéve, ha az kezelése lap **DN horgony** van jelölve.
- Ha **DN horgony** ki van jelölve a kapcsolat lapon, az ezen az oldalon csak a DN attribútum szükséges. Az attribútum is tenné a horgony attribútum használható.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Séma 4 (meghatározás attribútum típusa, hivatkozás és irányban)
Ez a lap lehetővé teszi, hogy állítsa be az attribútum típust, például a egész szám, bináris, vagy logikai érték, az egyes attribútumok irányba. Összes attribútum, oldal **séma 2** szerepelnek, beleértve a többértékű attribútum.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Adattípus**: segítségével az attribútum típusa feleltesse meg ezeket a szinkronizálási motor által ismert fájltípusok. Az alapértelmezett érték használhatja az azonos típusú, mint az SQL-séma található, de a DateTime és a hivatkozás nem egyszerűen észlelhető. Azok meg kell adni a **DateTime** vagy **hivatkozás**.
- **Irányát**: beállíthatja, hogy az attribútum irány importálás, exportálás vagy ImportExport. ImportExport alapértelmezett érték.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Megjegyzések:

- Ha az attribútum típusa nem észlelhető az összekötő, akkor a String adattípus.
- **Beágyazott táblázatok** egyoszlopos adatbázistáblák tekinthető meg. Az Oracle tárolja a sorok, beágyazott táblázat nem meghatározott sorrendben követik egymást. Jó helyen jár amikor a beágyazott táblázatot egy PL/SQL változó be, a sorok kapnak 1 kezdve egymást követő alsó index lehetőséget. Amely tömb hasonló hozzáférést biztosít egyes sorok.
- Az összekötő az **VARRYS** által nem támogatott.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Séma 5 (meghatározás partíciót hivatkozás attribútumok)
Ezen a lapon konfigurálnia kell az összes hivatkozás attribútumok melyik partíciót (Objektumtípus) attribútum hivatkozik.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Ha **DN horgony**, majd kell használnia ugyanaz az objektumtípus mint a hivatkozott. Nem hivatkozhat másik objektum típusát.

### <a name="global-parameters"></a>A globális paraméterek
A globális paraméterek lap Delta importálása, a dátum/idő formátum és a jelszó módszer konfigurálása szolgál.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

Az általános SQL-összekötő a következő módszerek Delta importálása támogatja:

- **Eseményindító**: lásd [létrehozása indítók segítségével Delta nézeteket](https://technet.microsoft.com/library/cc708665.aspx).
- **Vízjel**: egy általános megközelítés bármilyen adatbázis használható. A vízjel lekérdezés nem előre megadott az adatbázis szállító alapján. Vízjel oszlop szerepelnie kell a minden használt tábla/nézet. Ebben az oszlopban kell nyomon követnie beszúrása és frissítések, mint a táblák és a függő (többértékű vagy egy Gyermekszint) tábla. A szinkronizálási szolgáltatás és az adatbázis-kiszolgáló közötti órák szinkronizálni kell. Ha nem, akkor előfordulhat, hogy elhagyják a delta importálása bizonyos tételeket.  
Korlátozása:
    - Vízjel stratégia nem támogatja a törölt objektumok nem.
- **Pillanatkép**: (csak a Microsoft SQL Server működik) [előállító Delta nézetek pillanatképek használata](https://technet.microsoft.com/library/cc720640.aspx)
- **Változások követése**: (csak a Microsoft SQL Server működik) [tudnivalók a változások követése](https://msdn.microsoft.com/library/bb933875.aspx)  
Korlátozások:
    - DN attribútum- és horgony elsődleges kulcs a tábla a kijelölt objektum tagjának kell lennie.
    - SQL-lekérdezés importálása és exportálása a változások követése során nem támogatott.

**További paramétereket**: Adja meg, jelezve, hogy hol található az adatbázis-kiszolgáló adatbázis-kiszolgálón beállított időzónát. A dátum és idő attribútumok különböző formátumokat támogatja ezt az értéket szolgál.

Az összekötő dátum- és a dátum / idő mindig UTC formátum tárol. Engedélyezni szeretné, hogy helyesen konvertálja a dátumot és időpontot, az adatbázis-kiszolgáló és a használt formátum időzónáját meg kell adni. A formátum .net formátumban kell megadni.

Exportálás során minden dátum-idő attribútum meg kell adni az összekötő UTC időformátumban.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Jelszó beállítása**: az összekötő jelszó-szinkronizálás lehetőségeket kínál, és támogatja beállítása és a jelszó módosítása.

Az összekötő támogatja a jelszó-szinkronizálás két módszert kínál:

- **Tárolt eljárás**: ehhez a módszerhez a támogatási beállítása és módosítása a két tárolt eljárások jelszót. Írja be az összes paramétert a Hozzáadás gombra, és módosítsa a jelszó **Megadása-SP jelszó** és a **Jelszó SP módosítása** paraméterek rendre megegyezik egy példa alatt műveletet.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Jelszó kiterjesztés**: ehhez a módszerhez jelszó kiterjesztés DLL (meg kell adnia, hogy a [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) felület végrehajtó kiterjesztés DLL neve). Jelszó bővítmény összeállítás kell mutatnia bővítmény mappában, hogy az összekötő betöltheti futásidőben a DLL-t.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Akkor is, ahhoz, hogy a **Bővítmény konfigurálása** lapon a jelszavak kezelését.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Partíciót és hierarchiák beállítása:
A partíciók és hierarchiákat lapján jelölje be az összes objektum típusa. Minden egyes objektumtípus a saját partíciót.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

A **kapcsolat** vagy **Globális paraméterek** lapon az értékek is bármikor felülbírálva.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Horgonyok konfigurálása
Ezen az oldalon csak olvasható, mivel a horgony már meg van adva. A kijelölt horgony attribútum mindig a annak érdekében, hogy egyedi marad objektumtípusok végig az objektum típusa van ellátva.

![horgonyok](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Állítsa be a Futtatás lépés paraméter
Ezeket a lépéseket a Futtatás profilokról az összekötő úgy vannak beállítva. Ezek a konfigurációk hajtsa végre az adatok importálása és exportálása a tényleges munkát.

### <a name="full-and-delta-import"></a>Teljes és a Delta importálása
Általános SQL-összekötő támogatási teljes és Delta importálása használata az alábbi módszerek egyikét:

- Táblázat
- Nézet
- Tárolt eljárás
- SQL-lekérdezés

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Táblázat vagy megtekintése**  
Többértékű attribútum az objektum importálása, küldje el a fölérendelt tábla **nevét a Multi-Valued** táblázat/nézetekben vesszővel tagolt tábla/nézet nevét és megfelelő illesztési feltételt a **illesztési feltétel** van.

Példa: Szeretné importálni az alkalmazott objektum és az összes többértékű attribútum. Nincsenek két tábla (elsődleges tábla) alkalmazott és részleg (többértékű).
Tegye a következőket:

- Típus **alkalmazottak** **Tábla/nézet/SP**.
- Írja be a részleg **nevét a Multi-Valued**táblázat/nézetét.
- Írja be az illesztés feltétel alkalmazott & részleg között például **Illesztési feltétel**, `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Tárolt eljárás**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Ha sok adatot, javasolt a tárolt eljárásokat az oldalszámozás végrehajtásához.
- Az oldalszámozás támogatja a tárolt eljárás meg kell adnia a indexet indítása és befejezése indexet. Lásd: [hatékony személyhívó segítségével nagy mennyiségű adatot](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexés @EndIndex cseréli, amelyek a végrehajtás során **Lépés konfigurálása** lapon beállított a megfelelő lap méret értéket. Ha például amikor az összekötő beolvassa az első oldalra, és az oldalméret értéke 500, ilyen esetben @StartIndex lenne 1 és @EndIndex 500. Ezek az értékek növelése, amikor összekötő olvassa be a következő lapok és módosítása a @StartIndex & @EndIndex értéket.
- A paraméteres tárolt eljárás végrehajtásához adja meg a paraméterek `[Name]:[Direction]:[Value]` formátum. Adja meg az egyes paraméterek külön sorban (használata Ctrl + Enter veheti az új sor).
- Általános SQL-összekötő csatolt kiszolgálókat az importálási művelet is támogatja a Microsoft SQL Server. Ha az adatokat a tábla a csatolt kiszolgáló lehet beolvasni, majd táblázat kell megadni formátumban:`[ServerName].[Database].[Schema].[TableName]`
- Általános SQL-összekötő csak a hasonló szerkezet (mind alias nevű és adattípusú típusa) közötti futtatása lépéseket információk és -sémafájlok észlelési tartalmazó objektumok támogatja. Ha a kijelölt objektumot séma és futtatása lépésben megadott információk eltér, SQL-összekötő nem támogatja az ilyen típusú esetek.

**SQL-lekérdezés**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Több eredményhalmazt lekérdezések nem támogatott.
- SQL-lekérdezés támogatja a sortávolságot, és adja meg a kezdő Index és a záró Index változóként támogatja az oldalszámozás.

### <a name="delta-import"></a>Delta importálása
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta konfiguráció importálása teljes importálás összehasonlítva néhány további konfiguráció szükséges.

- Ha úgy dönt, hogy az eseményindító vagy pillanatkép megközelítésre, a delta változások nyomon követése, majd meg kell adnia táblába vagy pillanatkép adatbázis **táblába vagy pillanatkép adatbázis neve** mezőbe.
- Adja meg például illesztési feltétel előzmények és fölérendelt tábla, közötti szükséges`Employee.ID=History.EmployeeID`
- Nyomon követheti a tranzakció az előzmények értékeket a fölérendelt tábla, meg kell adnia az (Hozzáadás és frissítés/törlése) művelet információkat tartalmazó oszlop nevét.
- Ha úgy dönt, hogy a vízjel a delta változások nyomon követése, majd adja meg a **Vizet megjelölés oszlop neve**a művelet információkat tartalmazó oszlop neve.
- A **Type attribútum módosítása** oszlop szükség a típus módosítása. Ez az oszlop, amelyre az elsődleges vagy többértékű táblában egy típusának módosítása a delta nézetben való módosítása rendeli. Ez az oszlop tartalmazhat attribútum szintű módosítása vagy egy hozzáadása, módosítása, típus Modify_Attribute módosítása vagy törlése az Objektumszintű Változástípus típusának megváltoztatásához. Ha az oldalszámozást nem az alapértelmezett érték hozzáadása, módosítása vagy törlése, akkor ezeket az értékeket Ez a beállítás használatával adhatja meg.

### <a name="export"></a>Exportálás
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Általános SQL-összekötő módszerekkel négy támogatott például: exportálás támogatja:

- Táblázat
- Nézet
- Tárolt eljárás
- SQL-lekérdezés

**Táblázat vagy megtekintése**  
Ha a tábla/nézet lehetőséget választja, az összekötő a saját lekérdezések az Exportálás hoz létre.

**Tárolt eljárás**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

A tárolt eljárás lehetőséget választja, ha az Exportálás három különböző tárolt eljárások, Beszúrás és frissítés/törlése műveletek elvégzéséhez szükséges.

- **SP név hozzáadása**: Ez SP fut, ha olyan objektumokat tartalmaz, a megfelelő tábla beszúrási az összekötő.
- **Frissítés SP neve**: Ez SP hajtja végre, ha bármelyik objektumot összekötő frissítése a megfelelő táblázatban megtalálható.
- **SP név törlése**: A SP hajtja végre, ha bármelyik objektumot megtalálható a megfelelő tábla törlésre összekötőre.
- A paraméter értékként a tárolt eljárás használt séma kijelölt attribútum. Ha például `EmployeeName: INPUT: @EmployeeName` (EmployeeName be van jelölve a összekötő sémában és az összekötő lecseréli a megfelelő értéket exportálás közben)
- A paraméteres tárolt eljárás futtatásához adja meg a paraméterek `[Name]:[Direction]:[Value]` formátum. Adja meg az egyes paraméterek külön sorban (használata Ctrl + Enter veheti az új sor).

**SQL-lekérdezés**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Ha az SQL-lekérdezés lehetőséget választja, az Exportálás három különböző lekérdezésekben, Beszúrás és frissítés/törlése műveletek elvégzéséhez szükséges.

- **Lekérdezés beszúrása**: ezt a lekérdezést futtat, ha olyan objektumokat tartalmaz, a megfelelő tábla beszúrási az összekötő.
- **Lekérdezés frissítése**: A lekérdezés futtatása során, ha olyan objektumokat tartalmaz, összekötő frissítése a megfelelő tábla.
- **Lekérdezés törlése**: A lekérdezés futtatása során, ha olyan objektumokat tartalmaz, összekötő törlésre a megfelelő tábla.
- A séma használhatja a paraméter értéke a lekérdezéshez, mint például a kijelölt attribútum`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Hibaelhárítás

-   Kapcsolatos hibák elhárítása az összekötő naplózás engedélyezése a további tudnivalókért lásd az [annak engedélyezése esemény-nyomkövetés nyomon követése az összekötőkhöz](http://go.microsoft.com/fwlink/?LinkId=335731).
