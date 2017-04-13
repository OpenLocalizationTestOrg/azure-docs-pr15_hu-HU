<properties
    pageTitle="Azure kormányzati szolgáltatások |} Microsoft Azure"
    description="Itt és Azure kormányzati elérhető szolgáltatások áttekintése"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc" />


#  <a name="security"></a>Biztonsági

##  <a name="principles-for-securing-customer-data-in-azure-government"></a>Az Azure kormányzati ügyféladatok védelme alapelvei

Azure kormányzati tartalmaz egy cellatartomány funkciók és szolgáltatások, amely szabályozott/ellenőrzött adatok igényekhez felhő megoldások is használhatja. Kompatibilis ügyfél megoldást semmi több, mint ki-be az Azure kormányzati funkciók, folytonos adatok biztonsági ajánlott párosított hatékony végrehajtását.

Ha Ön üzemelteti a kormányzati Azure megoldást, a Microsoft kezeli ezeknek a követelményeknek, felhőalapú infrastruktúrájának szintre számos.

Az alábbi ábra mutatja az Azure védelmet a z a modell. Például a Microsoft tartalmaz egyszerű felhőalapú infrastruktúrájának DDOS, együtt ügyfél lehetőségek, például biztonsági készülékek ügyfélnek szól alkalmazás DDOS van szüksége.

![helyettesítő szöveg](./media/azure-government-Defenseindepth.png)

Ezen az oldalon ismertet a biztosítása érdekében a szolgáltatások és alkalmazások, hogyan alkalmazza a; nyújt útmutatást és a gyakorlati tanácsok a eligazodást elvek Ez azt jelenti hogyan ügyfelek kell használniuk intelligens Azure kormányzati megfeleljen a kötelezettségek és a szükséges információkat ITAR kezeli megoldás feladatkörök.

 A átívelő alapelvei ügyféladatok védelme a következők:

- Titkosítással adatok védelme
- Titkos kulcsok kezelése
- Elkülönítési adatokhoz való hozzáférés korlátozása

###  <a name="protecting-customer-data-using-encryption"></a>Titkosítással ügyféladatok védelme

Kockázatcsökkentő, és az értekezlet-szabályozó kötelezettségek ösztönzése, a növekvő fókusz és adatok titkosítása fontosságát. Egy hatékony titkosítási végrehajtása használata tartalmasabbá teszi a jelenlegi hálózati és az alkalmazás adatbiztonsági funkcióról – és a felhő környezet teljes kockázat csökkentése.

#### <a name="encryption-at-rest"></a>A többi titkosítás:
A titkosítást, a többi adatot a védelmet a lemez tárolt ügyfél tartalom vonatkozik. Ez akkor fordulhat több módja van:

#### <a name="storage-service-encryption"></a>Tárterület szolgáltatás titkosítás:

Azure tároló szolgáltatás titkosítás engedélyezve tároló fiók szintre, de így blokk BLOB és Azure tárolóhoz írásakor az automatikusan titkosított, oldal BLOB. Azure tárhelyről olvasni az adatokat, ha azt fog fejthető vissza a tárterület-szolgáltatás által visszaadott előtt. Ezzel az adatok biztonságos anélkül, hogy módosítsa vagy adja hozzá a kódot és más alkalmazások.

#### <a name="client-side-encryption"></a>Ügyféloldali titkosítás:
Ügyféloldali titkosítás beépített a Java- és a .NET tároló ügyfél tárai, amely használhat Azure kulcs tárolóra API-hoz, így túl bonyolult feladat végrehajtásához. Azure kulcs tárolóra segítségével hozzájuthat a titkos kulcsok, az Azure kulcs tárolóból elemre az Azure Active Directory használata egy konkrét munkatársnak.

#### <a name="encryption-in-transit"></a>A hálózaton átvitt titkosítás:

A rendelkezésre álló Azure kormányzati elérhetőségének egyszerű titkosítás átviteli szint Security (TLS) 1.2-es protokoll és X.509 tanúsítványok támogatja. Szövetségi Information Processing Standard (FIPS) 140-2 1-es szintű titkosítási algoritmust is használja az infrastruktúra hálózati kapcsolatainak Azure kormányzati adatközpontokkal.  A Windows Server 2012 R2 és a Windows 8-plus VMs és Azure fájlmegosztások használható kis-és Középvállalatok 3.0 titkosítás a virtuális és fájlmegosztás között. Az adatok titkosítása előtt átkerül a tároló ügyfélalkalmazás, és az adatok visszafejteni az azt követő átkerülnek tároló ki ügyféloldali-titkosítás használatára.

#### <a name="best-practices-for-encryption"></a>Gyakorlati tanácsok a titkosítási

- IaaS VMs: Azure merevlemez-titkosítás használatára. Tárterület szolgáltatás titkosítás titkosítása a virtuális fájlokat, készítsen biztonsági másolatot az Azure-tárolóban lévő adott lemezt használt bekapcsolása, de csak titkosítja újonnan írt adatok. Ez azt jelenti, hogy ha hozzon létre egy virtuális, és a tárterület-fiók a virtuális fájlt tároló engedélyeznie kell a tároló szolgáltatás titkosítást, a változtatások csak titkosítva lesz, nem az eredeti virtuális fájlt.
- Ügyféloldali titkosítás: Ez a legbiztonságosabb mód az adataihoz titkosítására, mert titkosítja a hálózaton átvitt előtt, és az adatokat a többi titkosítja. Azonban kell hozzá kód hozzáadása a alkalmazások tárolására, előfordulhat, hogy nem szeretne tenni. Ebben az esetben is használhatja HTTPs az adatoknak a hálózaton átvitt és a tárhely szolgáltatás titkosítás titkosítása többi adatait. Ügyféloldali titkosítás is magában foglalja az ügyfél több terhelést – különösen akkor, ha titkosítása és átvitele az adatokat a méretezhetőség tervek a fiók kell.

###  <a name="protecting-customer-data-by-managing-secrets"></a>Ügyféladatok védelme kezelésével titkos kulcsok

Biztonságos kulcskezelő elengedhetetlen adatvédelem a felhőben. Ügyfelek kulcskezelő egyszerűsítése, és a felhő alkalmazások és szolgáltatások adatainak titkosítására használt kulcsok vezérlő karbantartása kell törekedjen.

#### <a name="best-practices-for-managing-secrets"></a>Gyakorlati tanácsok a titkos kulcsok kezelése

- Titkos kulcsok csomagolásukkor konfigurációs fájlokat parancsfájlok, vagy forráskód kitéve kockázatok csökkentése érdekében használja a kulcs tárolóból elemre. Azure kulcs tárolóra titkosítja kulcsok (például a titkosítási kulcsok, az Azure lemezen titkosítás) és a titkos kulcsok (például jelszavak), és tárolja őket FIPS 140-2 szintet 2 érvényesített hardver biztonsági modulokat (HSMs). A hozzáadott garancia importálhat és kulcsok létrehozásához az alábbi HSMs.
- Alkalmazás kódját, és a sablonok kell csak URI mutató hivatkozásokat tartalmaz a titkos kulcsok (ami azt jelenti, hogy a tényleges titkos kulcsok nem szerepelnek kódot, konfigurációs vagy forráskód tárházakban). Megakadályozza, hogy a kulcsfontosságú adathalászati célú támadásokkal, a belső és külső repók, például betakarítás-botok a GitHub.
- Csatlakozást erős RBAC vezérlők belül kulcs tárolóból elemre. Ha egy megbízható operátor elhagyja a vállalat vagy átvitelek a vállalaton belül egy új csoportba, azok a titkos kulcsok eléréséhez nem lehet letiltja.

További információ <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure kulcs tárolóra nyilvános dokumentációt.</a>

###  <a name="isolation-to-restrict-data-access"></a>Elkülönítési adatokhoz való hozzáférés korlátozása

Elkülönítési összes határai, szegmens és tárolók használata csak a hitelesített felhasználóknak, a szolgáltatások és alkalmazások adathozzáférés korlátozhatja az. Például a bérlők közötti szétválasztására multitenant felhő platformok, például a Microsoft Azure-alapvető biztonsági mechanizmusa is. Logikai elkülönítési segítségével megakadályozhatja, hogy a egy bérlői bármelyik másik bérlőn tevékenységének zavarnák.

#### <a name="environment-isolation"></a>Környezet elkülönítési
Az Azure kormányzati környezete egy fizikai különböző nézeten a hálózat többi részén a Microsoft-példányt. Ez az alábbiakra terjed ki fizikai, logikai és vezérlőelemeket sorozata keresztül érhető el:

- Biztonságossá tétele biometrikus eszközök és a fényképezőgépek fizikai korlátokat.
- Adott hitelesítő adatait, és az éles üzemi környezetben való logikai hozzáférést igénylő Microsofton többtényezős hitelesítést használata.
- Az összes szolgáltatás infrastruktúra Azure kormányzati az Egyesült Államokból helyezkedik el.

#### <a name="per-customer-isolation"></a>Ügyfél elkülönítési
Hálózati hozzáférés-vezérlés Azure eszközök és keresztül virtuális elkülönítési, a hozzáférés-vezérlési listák, feladatkörök betöltése balancers és IP-szűrők

Ügyfelek további szétválaszthatja az erőforrások előfizetések, az erőforrás csoportok, virtuális hálózatok és alhálózat keresztül.

## <a name="screening"></a>Szűrés

A legutóbb bejelentett FedRAMP a magas, a 4 védelmet részleg (DoD) hatása szint akkreditációs. Van, emelt teljes Azure kormányzati környezet a biztonság és megfelelőség sáv meg.

Azt is most szűrése nemzeti ügynökség, jelölje be a minden az operátorokat a jogszabályok és a hitelkártya (NACLC) szakaszban 5.6.2.2, a DoD felhő számítások biztonsági követelményeknek útmutató (SRG) meghatározott:

>[AZURE.NOTE] A minimális háttér vizsgálat szükséges CSP személyzeti férhessenek hozzá szint 4-es és 5 információk alapján a "nem kritikus érzékeny" (például DoD's ADP-2) egy nemzeti ügynökség ellenőrzése a jogszabályok és a hitelkártya (NACLC) (a "nem kritikus érzékeny" vállalkozóknak) vagy egy Mérsékelt kockázat háttér vizsgálat (MBI) a "mérsékelt kockázatot" pozíció kijelölési.

Az alábbi táblázat összefoglalja az Azure kormányzati operátorok az aktuális szűrés:

Azure Gov screenings és a háttér ellenőrzése | Leírás|
---|---|
Amerikai Egyesült Államok polgárság |Amerikai Egyesült Államok polgárság ellenőrzése.
Microsoft cloud háttér jelölőnégyzetet (két év)|Társadalombiztosítási szám keresés, büntetőjogi előzmények jelölőnégyzetet, az Office az idegen eszközök szabálygyűjteményt (OFAC), a Bureau of Industry és biztonsági lista (BIS) Office védelmet kereskedelmi vezérlők Debarred személyek listában.
Nemzeti ügynökség jelölőnégyzet a jogszabályok és a hitelkártya (NACLC) (öt év) | Összeadja az ujjlenyomat háttérben jelölőnégyzet FBI adatbázisok szemben. További információért lépjen az<a href="https://www.opm.gov/investigations/background-investigations/federal-investigations-notices/1997/fin97-02/"> Office személyzeti felügyeleti webhely</a>. | 
<a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/CJIS">Büntetőbírósági Information Services (CJIS)</a> | CJIS állapot, helyi és FBI kormányzati mely folyamatok ujjlenyomat. kiszűrése a program, illetve működési munkatársai, akik büntetőbírósági fontos információt (CJI) adatokhoz való hozzáférés meg lehet adni a büntetőjogi alábbi előzményeinek ellenőrzi.  Egyes állapotok végzi a saját háttér ellenőrzése és az esetleges CJI hozzáféréssel rendelkező összes alkalmazottak későbbi jóváhagyási.|

Azure műveletek személyzeti az alábbi access elvek érvényesek:

- Feladatok világosan meg vannak határozva, igénylő, jóváhagyása és üzembe helyezése a módosítások külön feladatokkal.
- Teljes egészében funkcióiról rendelkező definiált felületeken keresztül.
- Az Access just-a-time (igény szerinti), és csak alapon alkalmanként vagy egy adott karbantartási eseményt, és mindig egy korlátozott ideig adják.
- Teljes egészében szabály alapján, a definiált szerepkörökhöz rendelt csak a hibaelhárítási szükséges engedélyekkel.

Szűrés szabványokat bele az érvényesítési az Amerikai Egyesült Államok polgárság az összes Microsoft terméktámogatási és műveleti oktatói, előtt hozzáférést Azure kormányzati által üzemeltetett rendszerek. Támogatási munkatársaknak telepíteniük kell a adatátviteli Azure kormányzati biztonságos lehetőségeit használni. Biztonságos adatátvitel külön hitelesítő hozzáférést igényel. Például rendszer metaadatai eléréséhez műveletek személyzeti használata adott webes belső felügyeleti eszközök csak olvasható API-k és igény szerinti illetéktelen.

## <a name="next-steps"></a>Következő lépések

A kiegészítő információk és frissítések előfizetés adjon a <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure kormányzati Blog.</a>
