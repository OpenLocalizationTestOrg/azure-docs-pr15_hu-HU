<properties 
   pageTitle="StorSimple tárterület-fiókjának kezelését |} Microsoft Azure"
   description="Ebből a cikkből megtudhatja, hogyan használhatja a StorSimple Manager lap hozzáadása, szerkesztése, törlése vagy a StorSimple virtuális tömb társított tárterület-fiók biztonsági kulcsok elforgatása."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>Tárterület-fiókok kezelése StorSimple virtuális tömb a StorSimple kezelő szolgáltatás használatával

## <a name="overview"></a>– Áttekintés

**A lap** a globális szolgáltatás paramétereit a StorSimple kezelő szolgáltatás létrehozott jeleníti meg. Ezek a paraméterek is alkalmazható a szolgáltatáshoz való kapcsolódás összes eszközt, és tartalmazza:

- Tárterület-fiókok 
- Access-vezérlő rekordok 

Ebből az oktatóanyagból megtudhatja, hogyan használhatja **a lap** hozzáadása, szerkesztése és törlése a tárterület-fiókok a StorSimple virtuális tömb. Az információk ebben az oktatóanyagban csak a rendszert futtató március 2016 Georgia verziójú szoftvereihez StorSimple virtuális tömb vonatkozik.

 ![Lap konfigurálása](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Tárterület-fiókok tartalmazzák a hitelesítő adatokat, az eszköz által használt tárterület fiókja a felhőben szolgáltatónál eléréséhez. Microsoft Azure tároló partnerekre ezek a hitelesítő adatait, például a fiók nevét és az access elsődleges kulcs. 

**A lap** a számlázási előfizetés létrehozott összes tárterület-fiókok a következő információkat tartalmazó táblázatos formában jelenik meg:

- **Név** – a fiók létrehozásakor jött létre automatikusan hozzárendelt egyedi nevet.
- **SSL engedélyezése** – akár az SSL engedélyezve van, és a biztonságos csatornán az eszköz a felhőbe kommunikációs.

**A lap** elvégezhető tároló fiókok kapcsolatos leggyakoribb feladatok a következők:

- Tárterület-fiók felvétele 
- Tárterület fiók szerkesztése 
- Tárterület fiók törlése 


## <a name="types-of-storage-accounts"></a>Fióktípus tárhely

Háromféle StorSimple az eszközén használt tárterület-fiókok van.

- **Automatikusan létrehozott tároló fiókok** – javaslatokat tesz a nevét, mint tárterület-fiókot a következő típusú automatikusan létrejön a szolgáltatás első létrehozásakor. Ha többet szeretne tudni, hogyan a tárhely hozzon létre egy fiókot, olvassa el a [létrehozása új szolgáltatás](storsimple-ova-manage-service.md#create-a-service)című témakört. 
- **Tárterület-fiókok szolgáltatás előfizetésben** – ezek az Azure tároló fiókok társított ugyanabban az előfizetésben, mint amit a szolgáltatás. További információ arról, hogy ezen tároló létre fiók című témakörben [Azure tároló fiókokat kapcsolatban](../storage/storage-create-storage-account.md). 
- **Tárterület-fiókok kívül a szolgáltatás előfizetése** – ezek az Azure tároló fiókok, amelyek nem a szolgáltatásokhoz kapcsolódó és valószínűleg létezik, mielőtt a szolgáltatás hozták létre.

Minden egyes StorSimple virtuális tömb (ha benne egy előtag hcs) tároló társított tároló fiók hoz létre. Ez a tároló mobileszközére felhő adatokat tartalmaz. Ne töröljön tároló való hozzáféréssel a Azure tároló szolgáltatáson keresztül, a művelet az adatok elvesztését eredményezi.

## <a name="add-a-storage-account"></a>Tárterület-fiók felvétele

A tároló fiók egy egyedi nevet és a tárterület-fiókhoz kapcsolt hozzáférési hitelesítő adatok megadásával felveheti a StorSimple kezelő szolgáltatás beállítása. Akkor is, hogy a secure sockets layer (SSL) mód engedélyezése hozhat létre az eszközön, és a felhő között a hálózati kommunikációs biztonságos csatornát.

Egy adott felhő szolgáltató több fiókot is létrehozhat. A tárterület-fiók van mentve, miközben a szolgáltatás megkísérli a felhőben szolgáltatóval kommunikálhat. A hitelesítő adatokat, és az access anyag megadott adott időben sikerült hitelesíteni. A tároló fiók csak akkor, ha a hitelesítés sikeres létrehozását. Ha nem sikerül a hitelesítés, megfelelő hibaüzenet fog megjelenni.

Erőforrás-kezelő tárterület-fiókok létrehozása az Azure-portálon is használhatók a StorSimple. Az erőforrás-kezelő tárterület-fiókok nem jelennek meg a kijelölés, akkor jelenik meg a fiók létrehozása az Azure klasszikus portálon tárolására legördülő listában. Erőforrás-kezelő tároló fiókok kell adhatók hozzá a eljárással az alábbiaknak tároló fiók hozzáadása.

Az eljárás Azure klasszikus tárterület-fiók hozzáadásakor részletes alatt.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Tárterület fiók szerkesztése

Az eszköz által használt tárterület fiók is szerkesztése. Ha módosítja a tárterület-fiókot, amelyet jelenleg használatban van, módosítására használható mezők pedig a hívóbetű a SSL mód a tárterület-fiókom. Adja meg az új tároló access kulcsot vagy módosítsa az **Enable SSL üzemmód** kiválasztása, és a frissített beállítások mentéséhez.

#### <a name="to-edit-a-storage-account"></a>A tároló fiók szerkesztése

1. A szolgáltatás érkezési lapon jelölje ki azt a szolgáltatást, kattintson duplán a szolgáltatás, és kattintson a **beállítás**.

2. **Tárterület-fiók hozzáadása, szerkesztése**gombra.

3. **Tárterület-fiókok hozzáadása/módosítása** párbeszédpanelen:

  1. **Tárterület-fiókok**legördülő listájában válassza ki egy meglévő-fiókot, amely a módosítani kívánt. 
  2. Ha szükséges, módosíthatja az **SSL-mód engedélyezése** kijelölés.
  3. Megadhatja, hogy a tárterület-fiók hívóbetűk újbóli. További tudnivalókért lásd: [a tárhely fiók billentyűk újragenerálása](storage-create-storage-account.md#manage-your-storage-access-keys). Adja meg az új tároló fiókkulcs. Azure tárterület-fiók esetén az access elsődleges kulcs. 
  4. Kattintson az ellenőrzés ikon ![ikon ellenőrzése](./media/storsimple-ova-manage-storage-accounts/checkicon.png) beállításainak mentéséhez. A beállítások **a lap** frissül. 
  5. A lap alján kattintson a **Mentés** továbbfejlesztett beállításainak mentéséhez. 

     ![Tárterület fiók szerkesztése](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Tárterület fiók törlése

> [AZURE.IMPORTANT] Csak akkor, ha még nem használt törölhet is egy tárterület-fiókkal. Ha egy tárterület-fiókot használja, akkor kap értesítést.

#### <a name="to-delete-a-storage-account"></a>Tárterület fiók törlése

1. StorSimple kezelő szolgáltatás érkezési lapon jelölje ki azt a szolgáltatást, kattintson duplán a szolgáltatást, és kattintson a **beállítás**.

2. Táblázatos tároló fiókok listájában mutasson a törölni kívánt fiókot.

3. Tárterület-fiókhoz tartozó a rendkívüli jobb oldali oszlopban megjelennek a Törlés (**x**) ikonra. Kattintson a hitelesítő adatok törlése az **x** ikonra.

4. A program megerősítést kér, kattintson az **Igen gombra** a törlési folytatásához. A változások megjelennek a táblázatos listaelem frissülnek.

5. A lap alján kattintson a **Mentés** továbbfejlesztett beállításainak mentéséhez.


## <a name="next-steps"></a>Következő lépések

- További tudnivalók [A StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md)felügyelete.
