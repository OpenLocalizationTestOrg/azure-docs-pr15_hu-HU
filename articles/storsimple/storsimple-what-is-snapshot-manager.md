<properties 
   pageTitle="Mi az StorSimple pillanatkép Manager? | Microsoft Azure"
   description="A StorSimple pillanatkép felettes, annak architektúrája és szolgáltatásai ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="what-is-storsimple-snapshot-manager"></a>Mi az StorSimple pillanatkép Manager?

## <a name="overview"></a>– Áttekintés

StorSimple pillanatkép felügyelője egy Microsoft Management Console (MMC) beépülő modul, amely egyszerűbbé teszi az adatok védelme és a biztonságimásolat-kezelési StorSimple a Microsoft Azure környezetben. StorSimple pillanatkép kezelővel, kezelheti az adatközpont, és a felhőben Microsoft Azure StorSimple adatok egyetlen integrált tárolási megoldásként, így biztonsági folyamatok egyszerűsítése és költségek csökkentése.

Az Áttekintés a StorSimple pillanatkép kezelő vezet be, annak funkcióit ismerteti, és a Microsoft Azure StorSimple szerepkörében ismerteti. 

A teljes Microsoft Azure StorSimple rendszer, többek között például a StorSimple eszköz StorSimple kezelő szolgáltatás, StorSimple pillanatkép Manager, és StorSimple kártya, SharePoint-áttekintése [StorSimple 8000 sorozat: hibrid felhőalapú tárolási megoldást](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- Microsoft Azure StorSimple virtuális tömbök (más néven StorSimple helyszíni virtuális eszközök) kezelése StorSimple pillanatkép Manager nem használható.
>
>- Ha telepíteni StorSimple frissítés 2 StorSimple eszközén, feltétlenül a legújabb StorSimple pillanatkép kezelője letölti és telepíti **StorSimple frissítés 2 telepítése előtt**. A legújabb StorSimple pillanatkép Manager visszamenőleges kompatibilitás működik, és működik-e a Microsoft Azure StorSimple kiadott verziói. Az előző verzióhoz képest StorSimple pillanatkép Manager használatakor szüksége lesz frissítheti (, nem kell az új verzió telepítése előtt, távolítsa el az előző verzióhoz képest).

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple pillanatkép Manager célja és architektúra

StorSimple pillanatkép-kezelővel egy központi kezelőkonzol egységes, létrehozásához használható helyi biztonsági másolatait pont és az idő és a felhő adatokat. Ha például a konzol is használhatja:

- Állítsa be, biztonsági mentése és törlése a kötet.
- Állítsa be a mennyiségi csoportok annak érdekében, hogy a biztonsági mentésben adatok egységesek alkalmazás.
- Biztonsági házirendek kezelése, úgy, hogy az adatok biztonsági másolatot készíteni az előre meghatározott időközönként.
- Hozzon létre helyi, és a felhő pillanatfelvételek, amely a felhőbe, és vészhelyreállítás használható.

A StorSimple pillanatkép kezelő beolvassa az alkalmazáslistából az állomáson a VSS szolgáltatónál regisztrálta. Ezután alkalmazás egységes biztonsági másolatok létrehozására azt ellenőrzi, az alkalmazás által használt mennyiségek és a javításukhoz javasolt mennyiségi csoportok konfigurálása. StorSimple pillanatkép Manager mennyiségi csoportokhoz használ, amelyek alkalmazás egységes biztonsági másolatok készítése. (Alkalmazás konzisztencia létezik, ha az összes kapcsolódó fájlok és adatbázisokban vannak szinkronizálva, és a valós időben egy adott pontján az alkalmazás állapotát képviseli.) 

StorSimple pillanatkép Manager biztonsági másolatok növekményes pillanatképek, amely csak a változásokat rögzítése a legutóbbi biztonsági mentés óta formájában. Emiatt biztonsági másolatok megkövetelése kevesebb tárterület és lehet létrehozni és gyorsan visszaállítani. StorSimple pillanatkép Manager biztosítása, hogy a pillanatképek alkalmazás egységes adatok rögzítése a Windows mennyiségi szolgáltatás (Árnyékmásolata) használja. (További információ nyissa meg a Windows kötet árnyék szolgáltatás szakasz integrációja.) StorSimple pillanatkép kezelővel, létrehozhat ütemterveket biztonsági vagy azonnali biztonsági másolatokat készíthet, szükség szerint. Ha módosítani szeretné a biztonsági másolat, StorSimple pillanatkép Manager lehetővé teszi, hogy az adatok visszaállítása helyi katalógus vagy felhőalapú pillanatképek közül választhat. Azure StorSimple visszaállítja a csak az adatokat, hogy szükség van szükség van, mint amely késések megakadályozza az adatok elérhetősége visszaállítása műveletek során.)

![StorSimple pillanatkép Manager architektúra](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple pillanatkép Manager architektúra** 

## <a name="support-for-multiple-volume-types"></a>Többféle mennyiségi támogatása

A StorSimple pillanatkép kezelő használatával állítsa be, és készítsen biztonsági másolatot az alábbi típusú kötet: 

- **Egyszerű kötet** – egy egyszerű mennyiségi egy egyetlen partíciót egy egyszerű. 

- **Egyszerű kötet** – egyszerű kötet dinamikus lemezről lemezterületet tartalmazó dinamikus kötet. Egyszerű kötet, ahol a lemezen egyetlen terület vagy több, egymáshoz kapcsolódó ugyanazon a lemezen. (Hozhat létre egyszerű kötet dinamikus lemezen.) Egyszerű kötet sem alternatív hibafa.

- **Dinamikus kötet** – dinamikus kötet dinamikus lemezen létrehozott kötet. Dinamikus lemezen adatbázis segítségével szereplő kötet kapcsolatos információk nyilvántartására dinamikus lemezen a számítógépen. 

- **Dinamikus kötet tükrözése az** – tükrözése a dinamikus kötet a RAID 1 architektúra készültek. Azonos adatokat RAID 1, a két vagy több lemezen előállító tükrözött meg írott. Az olvasási kérelem kezelhetik a kért adatokat tartalmazó bármely lemez.

- **Fürt megosztott kötet** – fürt megosztott kötet (CSVs), a Feladatátvevőfürt több csomópontok egyidejű olvashatják vagy ugyanarra a lemezre írása. Csomópont egy másik csomópontra való áttérés akkor fordulhat elő, gyorsan, anélkül, hogy a meghajtó tulajdonosának vagy csatlakoztatása, leválasztása és a kötet eltávolításakor módosítás. 

>[AZURE.IMPORTANT] Ne keverje CSVs és CSVs a azonos pillanatkép. Keverése CSVs és CSVs pillanatkép a nem támogatott. 
 
A teljes mennyiségi csoportok visszaállítása vagy egyéni kötet klónozhatja és egyes fájlok helyreállítása StorSimple pillanatkép Manager is használhatja.

- [Kötet és a hangerő-csoportok](#volumes-and-volume-groups) 
- [Biztonsági másolat típusok és biztonsági házirendek](#backup-types-and-backup-policies) 

StorSimple pillanatkép Manager funkcióival és azok használatáról további információt talál [StorSimple pillanatkép kezelő felhasználói felülettel](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Kötet és a hangerő-csoportok

StorSimple pillanatkép kezelővel, létrehozhat kötet és beállította mennyiségi csoportokba. 

StorSimple pillanatkép Manager mennyiségi csoportok használja, amelyek alkalmazás egységes biztonsági másolatot készíteni. Alkalmazás konzisztencia létezik, ha az összes kapcsolódó fájlok és adatbázisok vannak szinkronizálva, és a valós időben egy adott pontján céljával állapotát képviseli. Mennyiségi csoportok (amelyek más néven *konzisztencia csoportok*) biztonsági elemeznie, vagy a visszaállítási feladat.

Mennyiségi csoportok sem mennyiségi tárolók megegyezik. Mennyiségi tároló tartalmaz egy vagy több kötet, amely egy felhőalapú tárolási fiók és más attribútumait, például a titkosítási és sávszélesség-felhasználás megosztására. Egyetlen mennyiségi tároló legfeljebb 256 vékonyan kiépített StorSimple kötet is tartalmazhat. További információt a mennyiségi tárolók lépjen [a mennyiségi tárolók kezelése](storsimple-manage-volume-containers.md). Mennyiségi csoportok mentés megkönnyítésére konfigurálható kötet gyűjteményei. Ha bejelöli a két kötet, amely különböző mennyiségi tárolók tartoznak, helyezze őket egy egyetlen mennyiségi csoportban, és kattintson a csoporthoz tartozó mennyiségi biztonsági házirend létrehozása, minden egyes mennyiségi másolat készül a megfelelő mennyiségi tárolóban, a megfelelő tárolási fiókkal.

>[AZURE.NOTE] A mennyiség csoportban az összes kötet egyetlen felhő szolgáltató kell származnia.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integráció a Windows kötet árnyék szolgáltatás

StorSimple pillanatkép Manager alkalmazás egységes adatok rögzítése a Windows mennyiségi szolgáltatás (Árnyékmásolata) használja. VSS megkönnyíti az alkalmazás konzisztencia kommunikáció növekményes pillanatképek létrehozásának koordinálása VSS-et használó alkalmazások által. VSS biztosítja, hogy az alkalmazások ideiglenes inaktív vagy videokártyának, amikor pillanatképek vesznek. 

VSS StorSimple pillanatkép Manager végrehajtásának működik-e az SQL Server és az általános kötetek. A folyamat, a következőképpen: 

1. Kérési, amely általában egy adatkezelés és adatvédelmi megoldás (például StorSimple pillanatkép kezelő) vagy egy biztonságimásolat-alkalmazás, VSS elindítja, és kéri, hogy az információk összegyűjtése az a célalkalmazás író szoftvert.

2. VSS kapcsolatot létesít egy leírást az adatok beolvasásához a író összetevőt. Ha minden készülni az adatok leírásának lekérdezésére. 

3. VSS előkészítése az alkalmazás biztonsági másolatának minden jeleket. Minden tranzakció naplók, és így tovább frissítése követve nyissa meg a tranzakciókat, az adatok biztonsági másolatának előkészíti, és ezután értesíti a VSS

4. VSS arra utasítja az író ideiglenes leállításához a az alkalmazás adatait tárolja, és győződjön meg arról, hogy nincsenek adatok íródott a hangerőt, miközben az árnyék példány jön létre. Ebben a lépésben ellenőrzi az adatok konzisztencia, és legfeljebb 60 másodpercet vesz igénybe.

5. VSS utasítja, hogy a szolgáltatót, hogy az árnyék másolatot készíteni. Szolgáltatók, szoftver - vagy hardver-alapú, hogy a kötet, amelyek a futó, így azok árnyék másolatot készíteni az igény kezelése. A szolgáltató az árnyék példányt hoz létre, és értesítést küld VSS, amikor elkészült.

6. VSS lép kapcsolatba az író az alkalmazást, I/O önéletrajz is értesítse, továbbá győződjön meg arról, hogy I/O szüneteltetve sikeresen árnyék másolat létrehozása során. 

7. Ha a másolás sikeres volt, a VSS a másolat helyét a lekérdező adja eredményül. 

8. Adatok készült, miközben az árnyék másolat létrehozása, ha a biztonsági mentés nem következetes lesz. VSS az árnyék másolás törli, és értesítést küld a lekérdező. A lekérdező ismételje meg automatikusan a biztonsági mentés vagy a rendszergazdát, hogy később újra meg értesítést.

Az alábbi ábrán látható.

![VSS folyamatábra](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**A Windows kötet árnyék szolgáltatás folyamatábra** 

## <a name="backup-types-and-backup-policies"></a>Biztonsági másolat típusok és biztonsági házirendek

StorSimple pillanatkép kezelővel, akkor kevésbé részletes adatok és helyi és felhőbeli tárolja azt. Kevésbé részletes adatok azonnal StorSimple pillanatkép Manager is használhatja, vagy biztonsági házirend hozhat létre egy ütemtervet biztonsági másolatok automatikusan véve. Biztonsági házirendek engedélyezze, hogy adja meg, hány pillanatképek megmarad. 

### <a name="backup-types"></a>Biztonsági másolatok típusai

StorSimple pillanatkép Manager hozhat létre a biztonsági másolatok a következő típusú:

- **Helyi pillanatképek** – helyi pillanatképek az StorSimple eszközön tárolt adatok mennyiségi pont és az idő másolatait is. A szokásos biztonsági mentése az ilyen típusú létrehozott is, és gyorsan vissza. Helyi pillanatkép helyi biztonsági másolatot ugyanúgy használhatja.

- **Felhőalapú pillanatképek** – felhő pillanatfelvételek a felhőben tárolt adatok mennyiségi pont és az idő példányainak. Felhőalapú pillanatkép megegyezik egy másik, a helyszínen tároló rendszeren replikált pillanatkép. Felhőalapú pillanatképek különösen hasznosak katasztrófa helyreállítási helyzetekben.

### <a name="on-demand-and-scheduled-backups"></a>Igény szerinti és az ütemezett biztonsági másolatok

A StorSimple pillanatkép kezelővel egyszeri biztonsági másolatot a létrehozandó azonnal kezdeményezhet, vagy ismétlődő biztonsági művelet ütemezése biztonsági házirend is használhatja.

A biztonsági másolat házirend automatikus szabályhalmaz rendszeres biztonsági mentés ütemezése használható. A biztonsági másolat házirend lehetővé teszi, hogy a gyakoriság és a paraméterek egy adott mennyiségi csoport pillanatképek készítése határozhatja meg. Házirendek segítségével adja meg a kezdés és a lejárati dátum, idő, gyakoriságok és adatmegőrzési követelmények teljesítése érdekében, mind a helyi és felhőbeli pillanatképek. A házirend közvetlenül az után, adjon meg egy érvényes. 

Állítsa be, vagy biztonsági házirendek szükség esetén átkonfigurálása StorSimple pillanatkép Manager is használhatja. 

Beállíthatja az egyes biztonsági házirendet, amely hoz létre a következő adatokat:

- **Név** – a kijelölt biztonsági házirend egyedi nevét.

- **Típus** – a típusú biztonsági házirendek; helyi pillanatkép vagy felhőalapú pillanatkép.

- **Mennyiségi csoport** – a mennyiségi csoport, amely a kijelölt biztonsági házirend hozzá van rendelve.

- **Adatmegőrzés** – megőrzéséhez biztonsági másolatok számát. Ha az **összes** jelölőnégyzet összes biztonsági másolatot megőrződnek, eléréséig per mennyiségi biztonsági másolatok maximális száma, mely pontján a házirend sikertelen lesz, és hibát okoz. Másik lehetőségként a biztonsági másolatokat, amely megőrzi az (1 és a 64) közötti szám is megadhat.

- **Dátum** – a biztonsági másolat házirend létrehozásának dátumát.

Biztonsági házirendek beállításával kapcsolatos további információért lépjen [StorSimple hozhat létre és kezelhet biztonsági házirendek pillanatkép Managert használhatja](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Biztonsági mentési feladat figyelő és felügyeleti

A StorSimple pillanatkép kezelő figyelésére és közelgő ütemezett és kész biztonsági feladatok kezelésére is használhatja. Ezenkívül StorSimple pillanatkép-kezelővel akár 64 bejegyzett biztonsági másolatok katalógust. A katalógus található, és visszaállíthatja a kötet, vagy az egyes fájlokat is használhatja. 

Biztonsági mentési feladatok figyeléshez megnyitásához [StorSimple megtekintheti és kezelheti a biztonsági másolat feladatok pillanatkép Managert használhatja](storsimple-snapshot-manager-manage-backup-jobs.md).


## <a name="next-steps"></a>Következő lépések

- További tudnivalók [felügyelheti a StorSimple megoldás StorSimple pillanatkép segítségével](storsimple-snapshot-manager-admin.md).

- Töltse le a [StorSimple pillanatkép Manager](https://www.microsoft.com/download/details.aspx?id=44220).
