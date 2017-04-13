<properties 
    pageTitle="Adatok adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása" 
    description="Ismerje meg, beállítása és kezelése az adatok megőrzése a prémium réteg Azure vgx.dll gyorsítótár-példányok" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-data-persistence-for-a-premium-azure-redis-cache"></a>Adatok adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása

Azure vgx.dll gyorsítótár tartalmaz a gyorsítótár méretét és funkciói, köztük az új prémium réteg választható rugalmasságot biztosít, amelyek különböző gyorsítótár szeretne rendelni.

Az Azure vgx.dll gyorsítótár prémium réteg szolgáltatásai, például fürtözés, az adatmegőrzési és a virtuális hálózat támogatása. Ez a cikk ismerteti, hogyan adatmegőrzési konfigurálása a prémium Azure vgx.dll gyorsítótár-példány.

További információkért más prémium gyorsítótár című témakörben [az Azure-gyorsítótár prémium vgx.dll réteg](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Mi az adatok adatmegőrzési?
Adatmegőrzési vgx.dll vgx.dll tárolt adatok megmaradnak teszi lehetővé. Készíthet egy adott időpontban érvényes is, és készítsen biztonsági másolatot az adatokat, amelyek betöltheti hardver hiba esetén. Ez a egy nagyon nagy előnye, ahol minden adatát a memóriában van tárolva, és hol találhatók a gyorsítótár csomópontok lefelé hiba esetén adatvesztést lehet Basic vagy normál réteg szemben. 

Azure vgx.dll gyorsítótár kínál vgx.dll megőrzése a [Rekordadatbázis modell](http://redis.io/topics/persistence), az adatok tárolásának helye az Azure tárterület-fiók használatával. Adatmegőrzési konfigurálásakor Azure vgx.dll gyorsítótár továbbra is fennáll a vgx.dll gyorsítótár vgx.dll bináris formátumban egy konfigurálható biztonsági gyakoriság alapján lemezre pillanatképét. Esetben a katasztrofális eseményeket, amely letiltja az elsődleges és a replika gyorsítótárat a gyorsítótár újraépíti a legutóbbi pillanatkép használatával.

Adatmegőrzési beállítható az **Új vgx.dll gyorsítótárat** a lap a gyorsítótár létrehozása során, és kattintson a **Beállítások** lap, a meglévő prémium gyorsítótárát.

## <a name="create-a-premium-cache"></a>Hozzon létre egy prémium gyorsítótárban

Gyorsítótár létre, és állítsa be az adatmegőrzési, bejelentkezés az [Azure portálra](https://portal.azure.com) , és kattintson az **Új**->**adatok + tárhely**>**Vgx.dll gyorsítótár**.

![Hozzon létre egy vgx.dll gyorsítótár][redis-cache-new-cache-menu]

Adatmegőrzési konfigurálása, először jelölje ki azt a **prémium** gyorsítótárát az a **Válassza ki a árak réteg** lap.

![Válassza ki a árak szint][redis-cache-premium-pricing-tier]

Miután az egy réteg árak prémium ki van jelölve, kattintson a **adatmegőrzési vgx.dll**.

![Adatmegőrzési vgx.dll][redis-cache-persistence]

A következő szakaszban szereplő lépéseket ismertetik, hogyan lehet vgx.dll adatmegőrzési konfigurálja az új prémium gyorsítótárat. Miután vgx.dll megőrzése jelölőnégyzet be van állítva, a **létrehozása** az új prémium gyorsítótár létrehozását vgx.dll adatmegőrzési gombra.

## <a name="configure-redis-persistence"></a>Adatmegőrzési vgx.dll konfigurálása

Vgx.dll adatmegőrzési be van állítva, a az **adatok adatmegőrzési vgx.dll** lap. Az új gyorsítótárát Ez a lap érhető el a gyorsítótárban létrehozásának folyamata során az előző szakaszban leírtak szerint. A meglévő gyorsítótárát az **adatok adatmegőrzési vgx.dll** lap a **Beállítások** lap a gyorsítótár a érhető el.

![Vgx.dll beállításai][redis-cache-settings]

Ahhoz, hogy az adatmegőrzési vgx.dll, válassza a **engedélyezett** ahhoz, hogy Rekordadatbázis (vgx.dll adatbázis) biztonsági mentése lehetőséget. Egy korábban engedélyezett prémium gyorsítótáron vgx.dll adatmegőrzési letiltása, válassza a **Letiltva**lehetőséget.

Adja meg a biztonsági másolat időközt, jelölje be a **Biztonsági mentés gyakoriságának** a legördülő listából. Választási lehetőségek **15 percet**, **30 percig**, **60 perc**, **6 órával**, **12 órás**és **24 óra**tartalmazzák. Az intervallum elindul számlálás az előző biztonsági művelet sikeresen befejeződött, és a program, amikor egy új biztonsági másolatot kezdeményezett után.

Kattintson a **Tárterület-fiók** , jelölje be a tárterület-fiók használata, és jelölje ki az **elsődleges kulcs** vagy **másodlagos kulcsa** a **Tárhely kulcs** legördülő használja. Ugyanabban a régióban, mint a gyorsítótár kell választania egy tárterület-fiókot, és egy **Prémium tárterület** -fiókkal ajánlott, mert magasabb átviteli prémium tároló. 

>[AZURE.IMPORTANT] A tároló billentyűt az adatmegőrzési fiók jön, ha kell, a **Tárhely kulcs** legördülő rechoose a a kívánt billentyűt.

![Adatmegőrzési vgx.dll][redis-cache-persistence-selected]

Kattintson **az OK** gombra az adatmegőrzési konfiguráció mentéséhez.

A következő biztonsági másolat (vagy az új gyorsítótárát az első biztonsági másolat) után a biztonsági mentés gyakoriságának időtartam kezdeményezni.



## <a name="persistence-faq"></a>Adatmegőrzési – gyakori kérdések

Az alábbi lista tartalmazza az Azure vgx.dll gyorsítótár adatmegőrzési kapcsolatos gyakori kérdésekre adott válaszok.

-   [Egy korábban létrehozott gyorsítótáron adatmegőrzési engedélyezése](#can-i-enable-persistence-on-a-previously-created-cache)
-   [Megváltoztathatom-e a biztonsági mentés gyakoriságának a gyorsítótár létrehozása után?](#can-i-change-the-backup-frequency-after-i-create-the-cache)
-   [Miért van a biztonsági mentés gyakoriságának 60 perces van-e több mint 60 perc biztonsági másolatok között?](#why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
-   [Mi történik a régi biztonsági másolatokat, amikor egy új biztonsági másolat készül?](#what-happens-to-the-old-backups-when-a-new-backup-is-made)
-   [Mi történik, ha a lehet egy másik értékre van méretezése és biztonsági visszahelyezi a méretezési művelet előtt végrehajtott?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Egy korábban létrehozott gyorsítótáron adatmegőrzési engedélyezése

Igen, beállítható az adatmegőrzési vgx.dll gyorsítótárának létrehozása, mind a meglévő prémium gyorsítótárát.

### <a name="can-i-change-the-backup-frequency-after-i-create-the-cache"></a>Megváltoztathatom-e a biztonsági mentés gyakoriságának a gyorsítótár létrehozása után?

Igen, módosíthatja az **adatok adatmegőrzési vgx.dll** lap a biztonsági másolat gyakoriságát. Című cikkben olvashat [konfigurálása vgx.dll adatmegőrzési](#configure-redis-persistence).

### <a name="why-if-i-have-a-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Miért van a biztonsági mentés gyakoriságának 60 perces van-e több mint 60 perc biztonsági másolatok között?

Biztonsági mentési gyakoriságának intervallum nem indul el, amíg a korábbi biztonsági mentés sikerült. Ha a biztonsági mentés gyakoriságának 60 perc és tart a biztonsági mentés 15 percet sikeres befejezéséhez, a következő biztonsági mentés nem indul el az után a kezdési időpontot, az előző biztonsági másolat 75 perccel.

### <a name="what-happens-to-the-old-backups-when-a-new-backup-is-made"></a>Mi történik a régi biztonsági másolatokat, amikor egy új biztonsági másolat készül?

A legújabbak kivételével az összes biztonsági másolatokat, automatikusan törli. Előfordulhat, hogy nem kerül sor azonnal a törlés, de a régebbi mentések Csoportosítások végtelen időre szóló nem megmaradnak.

### <a name="what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation"></a>Mi történik, ha a lehet egy másik értékre van méretezése és biztonsági visszahelyezi a méretezési művelet előtt végrehajtott?

-   Ha van átméretezi, a nagyobb méretű, nincs nincs hatással.
-   Ha meg van méretezni, hogy kisebb méretű, és egy egyéni beállítás, amely [adatbázisok](cache-configure.md#databases) értéke nagyobb, mint az [adatbázisok korlátozni](cache-configure.md#databases) esetében az új méret adott adatbázisok adatainak nem állíthatók vissza. További tudnivalókért lásd: [az átméretezés közben érintett beállítás egyéni adatbázisok?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
-   Ha meg van méretezni, hogy kisebb méretű, és nincs elegendő hely a kisebb méretű utolsó biztonsági másolatból adatok tárolására, akkor kulcsok a visszaállítás során, általában a házirenddel [allkeys-lru](http://redis.io/topics/lru-cache) eviction eltávolítani kívánt.

## <a name="next-steps"></a>Következő lépések
Megtudhatja, hogy miként prémium-gyorsítótár további funkcióinak használata.

-   [Az Azure-gyorsítótár prémium vgx.dll réteg – bevezetés](cache-premium-tier-intro.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
