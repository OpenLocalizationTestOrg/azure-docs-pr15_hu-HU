<properties 
   pageTitle="StorSimple tárterület-fiókjának kezelését |} Microsoft Azure"
   description="Megtudhatja, hogyan hozzáadása, szerkesztése és törlése a StorSimple Manager lap használatával, vagy biztonsági kulcsok tárterület-fiók elforgatása."
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
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Tárterület-fiókjának kezelését a StorSimple kezelő szolgáltatás használatával

## <a name="overview"></a>– Áttekintés

**A lap** jeleníti meg a StorSimple kezelő szolgáltatás létrehozott összes globális szolgáltatás paramétert. Ezek a paraméterek is alkalmazható a szolgáltatáshoz való kapcsolódás összes eszközt, és tartalmazza:

- Tárterület-fiókok 
- Sávszélesség-sablonok 
- Access-vezérlő rekordok 

Ebből az oktatóanyagból megtudhatja, hogyan használhatja **a lap** hozzáadása, szerkesztése, vagy tároló fiókok törlése vagy elforgatása a biztonsági kulcsokat tároló fiókot.

 ![Lap konfigurálása](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Tárterület-fiókok tartalmazzák a hitelesítő adatokat, az eszköz által használt tárterület fiókja a felhőben szolgáltatónál eléréséhez. Microsoft Azure tároló partnerekre ezek a hitelesítő adatait, például a fiók nevét és az access elsődleges kulcs. 

**A lap** a számlázási előfizetés létrehozott összes tárterület-fiókok a következő információkat tartalmazó táblázatos formában jelenik meg:

- **Név** – a fiók létrehozásakor jött létre automatikusan hozzárendelt egyedi nevet.
- **SSL engedélyezése** – akár az SSL engedélyezve van, és a biztonságos csatornán az eszköz a felhőbe kommunikációs.
- **Által használt** tárterület-fiókkal kötet száma.

**A lap** elvégezhető tároló fiókok kapcsolatos leggyakoribb feladatok a következők:

- Tárterület-fiók felvétele 
- Tárterület fiók szerkesztése 
- Tárterület fiók törlése 
- Kulcs Elforgatás tárterület-fiókok 

## <a name="types-of-storage-accounts"></a>Fióktípus tárhely

Háromféle StorSimple az eszközén használt tárterület-fiókok van.

- **Automatikusan létrehozott tároló fiókok** – javaslatokat tesz a nevét, mint tárterület-fiókot a következő típusú automatikusan létrejön a szolgáltatás első létrehozásakor. További tudnivalók a tárterület-fiók létrehozását, [lépés 1: hozzon létre egy új](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) a [Deploy helyszíni StorSimple eszközére](storsimple-deployment-walkthrough.md). 
- **Tárterület-fiókok szolgáltatás előfizetésben** – ezek az Azure tároló fiókok társított ugyanabban az előfizetésben, mint amit a szolgáltatás. További információ arról, hogy ezen tároló létre fiók című témakörben [Azure tároló fiókokat kapcsolatban](../storage/storage-create-storage-account.md). 
- **Tárterület-fiókok kívül a szolgáltatás előfizetése** – ezek az Azure tároló fiókok, amelyek nem a szolgáltatásokhoz kapcsolódó és valószínűleg létezik, mielőtt a szolgáltatás hozták létre.

## <a name="add-a-storage-account"></a>Tárterület-fiók felvétele

Egy tároló fiókot egy egyedi nevet és a tárterület-fiókjával (a megadott felhő szolgáltató) hozzákapcsolhatja hozzáférési hitelesítő adatok megadásával. Akkor is, hogy a secure sockets layer (SSL) mód engedélyezése hozhat létre az eszközön, és a felhő között a hálózati kommunikációs biztonságos csatornát.

Egy adott felhő szolgáltató több fiókot is létrehozhat. Ne feledje azonban, hogy a tárterület-fiók létrehozását követően nem lehet módosítani a felhőalapú szolgáltatás.

A tárterület-fiók van mentve, miközben a szolgáltatás megkísérli a felhőben szolgáltatóval kommunikálhat. A hitelesítő adatokat, és az access anyag megadott adott időben sikerült hitelesíteni. A tároló fiók csak akkor, ha a hitelesítés sikeres létrehozását. Ha nem sikerül a hitelesítés, megfelelő hibaüzenet fog megjelenni.

Erőforrás-kezelő tárterület-fiókok létrehozása az Azure-portálon is használhatók a StorSimple. Az erőforrás-kezelő tárterület-fiókok nem jelennek meg a legördülő listában kijelölt Ha mennyiségi tároló, csak a tárhely fiókok létrehozása az Azure klasszikus portálon létrehozni fog megjelenni. Erőforrás-kezelő tároló fiókok kell hozzá egy fiókot tároló leírása alább az eljárás használatával.

> [AZURE.NOTE] Tárterület-fiók hozzáadásakor eljárása alapján a StorSimple szoftver verzióját használja. Ügyeljen arra, hogy kövesse a helyes StorSimple verziójával.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Tárterület fiók szerkesztése

Módosíthatja egy mennyiségi tároló által használt tárterület-fiókot. Ha módosítja a tárterület-fiókot, amelyet jelenleg használatban van, a csak módosítása rendelkezésére névmezője a hívóbetű a tárterület-fiókom. Adja meg az új tároló access kulcsot, és mentse a frissített beállításokat.

#### <a name="to-edit-a-storage-account"></a>A tároló fiók szerkesztése

1. A szolgáltatás érkezési lapon jelölje ki azt a szolgáltatást, kattintson duplán a szolgáltatás, és kattintson a **beállítás**.

2. **Tárterület-fiók hozzáadása, szerkesztése**gombra.

3. **Tárterület-fiókok hozzáadása/módosítása** párbeszédpanelen:

  1. **Tárterület-fiókok**legördülő listájában válassza ki egy meglévő-fiókot, amely a módosítani kívánt. Ez a tárterület-fiókok, amely automatikusan jött létre, a szolgáltatás első létrehozásakor is lehetnek.
  2. Ha szükséges, módosíthatja a **SSL-mód engedélyezése** kijelölés.
  3. Ha el szeretné forgatni a tárterület-fiók hívóbetűk választhat. Lásd: a [tárterület fiókok kulcs Forgatás](#key-rotation-of-storage-accounts) miként végezheti el a fő Forgatás további információt.
  4. Kattintson az ellenőrzés ikon ![ikon ellenőrzése](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) beállításainak mentéséhez. A beállítások **a lap** frissül. Továbbfejlesztett beállításainak mentéséhez a **Mentés** gombra.

     ![Tárterület fiók szerkesztése](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Tárterület fiók törlése

> [AZURE.IMPORTANT] A tároló fiók törölheti csak akkor, ha mennyiségi tároló nem használja. Ha mennyiségi tároló egy tárterület-fiókkal használják, törölnie kell a mennyiségi tároló, és törölje a társított tárterület-fiókot.

#### <a name="to-delete-a-storage-account"></a>Tárterület fiók törlése

1. StorSimple kezelő szolgáltatás érkezési lapon jelölje ki azt a szolgáltatást, kattintson duplán a szolgáltatást, és kattintson a **beállítás**.

2. Táblázatos tároló fiókok listájában mutasson a törölni kívánt fiókot.

3. Tárterület-fiókhoz tartozó a rendkívüli jobb oldali oszlopban megjelennek a Törlés (**x**) ikonra. Kattintson a hitelesítő adatok törlése az **x** ikonra.

4. A program megerősítést kér, kattintson az **Igen gombra** a törlési folytatásához. A változások megjelennek a táblázatos listaelem frissülnek.

## <a name="key-rotation-of-storage-accounts"></a>Fő Forgatás tárterület-fiókok

Biztonsági okokból kulcs Forgatás legtöbbször követelmények adatközpontokban között. 

> [AZURE.NOTE] Az alábbi főbb Forgatás adatokat és a Forgatás eljárás csak a Microsoft Azure tároló fiókra alkalmazhatja. Ha másik cloud-szolgáltató használata esetén kezelheti tároló fiók billentyűk adott szolgáltató irányítópult keresztül.
 
Minden Microsoft Azure-előfizetés beállíthatja, hogy egy vagy több társított tárterület-fiókot. Az alábbi fiókok elérése minden tároló fiók előfizetés és az access billentyűparancsai szabályozza. 

Amikor létrehoz egy tárterület-fiókkal, a Microsoft Azure két 512 bites tároló hívóbetűk, a tárterület-fiók megnyitásakor hitelesítéshez használt hoz létre. Két tároló hívóbetűk lehetővé teszi, hogy a tárhelyszolgáltatáshoz megszakítás nélkül vagy a szolgáltatás a hozzáférést a billentyűk újbóli. A kulcs, amely jelenleg használatban van, az *elsődleges* kulcs és a biztonsági másolat kulcs hivatkozik, amely a *másodlagos* kulcsa. Két billentyűk egyikét kell megadni, ha a Microsoft Azure StorSimple eszköz segítségével érheti el, felhőalapú tárolási szolgáltatótól.

## <a name="what-is-key-rotation"></a>Mi az főbb Elforgatás?

Általában alkalmazások használata a billentyűk közül csak az adatok eléréséhez. Egy megadott idő után beállíthatja, hogy az alkalmazások használata a második kulcsnak átkapcsolhatná. Az alkalmazások, a másodlagos kulcsa van váltott, miután kivonni az első billentyűt, és kattintson az új kulcs generálása. A két billentyűk segítségével, így lehetővé teszi, hogy az alkalmazások hozzáférés bármely legrövidebb leállás nélkül.

A tároló fiók billentyűk mindig a titkosított formában szolgáltatásban tárolja. Azonban ezek vissza tudja állítani a StorSimple kezelő szolgáltatás használatával. A szolgáltatás elérheti az elsődleges és másodlagos kulcsa a tárterület-fiókok ugyanabban az előfizetésben, beleértve a tároló szolgáltatás, valamint az alapértelmezett tároló létrehozott létre fiók jön létre, amikor először a StorSimple-kezelő szolgáltatást. A StorSimple kezelő szolgáltatás eljuthassanak billentyűk az Azure klasszikus portálról, és a majd titkosított módon tárolja őket.

## <a name="rotation-workflow"></a>Forgatás munkafolyamat

A Microsoft Azure-rendszergazda újragenerálása vagy módosíthatja az elsődleges és másodlagos kulcsa közvetlenül elérése a tárhely fiók (a Microsoft Azure tároló service). Automatikusan a StorSimple kezelő szolgáltatás nem látható ez a változás.

A StorSimple kezelő szolgáltatás a módosításról tájékoztatni, a StorSimple-kezelő szolgáltatás eléréséhez a tárterület-fiók elérésére, és ezt követően szinkronizálja az elsődleges és másodlagos kulcsa (attól függően, hogy melyiket módosult). A szolgáltatás majd megkapja a legfrissebb billentyűt, a billentyűk titkosítja, és elküldi az eszköznek a titkosított billentyűt.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Tárterület-fiókok (csak Azure) szolgáltatásként ugyanabban az előfizetésben billentyűparancsok szinkronizálása

1. A **szolgáltatások** lapján kattintson a **beállítás** fülre.

2. **Tárterület-fiók hozzáadása, szerkesztése**gombra.

3. A párbeszédpanelen tegye a következőket:

  1. Jelölje ki a tárterület-fiókot a szinkronizálni kívánt billentyűvel együtt. A tároló fiók billentyűk képernyőn megjelenő titkosított.
  2. A StorSimple kezelő szolgáltatás módosítania kell a korábban megváltozott a Microsoft Azure tárhelyszolgáltatáshoz billentyűt. Ha az elsődleges hívóbetű módosult (regenerált), kattintson a **szinkronizálása az elsődleges kulcs**. A másodlagos kulcsa módosult, kattintson a **másodlagos kulcs szinkronizálása**gombra.

    ![billentyűk szinkronizálása](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Szinkronizál a tárterület-fiókok kívül a szolgáltatás előfizetése billentyűparancsai

1. A **szolgáltatások** lapján kattintson a **beállítás** fülre.

2. **Tárterület-fiók hozzáadása, szerkesztése**gombra.

3. A párbeszédpanelen tegye a következőket:

  1. Jelölje ki a tárterület-fiókot frissíteni szeretné a hozzáférési kulccsal.
  2. Meg kell frissíteni a tárhely hívóbetű az StorSimple kezelő szolgáltatás. Ebben az esetben a tárhely hívóbetű láthatja. Írja be az új kulcs a **Tárterület, hozzáférés Fiókkulcs**y mezőbe. 
  3. A módosítások mentéséhez. Most frissíteni kell a tároló fiókkulcs access.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a [StorSimple biztonsági](storsimple-security.md).
- További tudnivalók [felügyelheti StorSimple eszköze az StorSimple kezelő szolgáltatás használatával](storsimple-manager-service-administration.md).
