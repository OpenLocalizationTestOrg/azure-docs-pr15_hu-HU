<properties
   pageTitle="Üzembe StorSimple virtuális tömb 3 - fájl kiszolgálójával a virtuális eszköz beállítása"
   description="Harmadik oktatóprogram StorSimple virtuális tömb környezetben arra utasítja, hogy a fájl kiszolgálójával virtuális eszköz beállítása."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Üzembe StorSimple virtuális tömb - beállítás be fájl kiszolgálójával

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>– Bevezetés 

Ez a cikk a Microsoft Azure StorSimple virtuális tömbképletek (más néven StorSimple helyszíni virtuális eszköz vagy StorSimple virtuális eszköz) futó március 2016 általános elérhetőség (kiadás) verziójára vonatkozik. Ez a cikk ismerteti, hogyan kezdeti telepítés végrehajtásához, a StorSimple fájlkiszolgálóra regisztrálni, az eszköz telepítéséhez és létrehozása és csatlakoztatása a kis-és Középvállalatok megosztások. Ez az üzembe helyezési oktatóprogram teljesen telepítéséhez a virtuális tömb fájlkiszolgálóra vagy egy iSCSI kiszolgálón szükséges a sorozat az utolsó cikk.

A beállítás és konfiguráció folyamat befejezéséhez körülbelül 10 percig is eltarthat.


## <a name="setup-prerequisites"></a>Telepítési előfeltételek

Mielőtt konfigurálása és a virtuális StorSimple az eszköz beállítása, győződjön meg arról, hogy:

-   A virtuális eszköz kiépítéstől van, és csatlakozik [egy StorSimple virtuális tömböt a Hyper-V biztosítása](storsimple-ova-deploy2-provision-hyperv.md) vagy [egy StorSimple VMware virtuális tömbben rendelkezést](storsimple-ova-deploy2-provision-vmware.md)ismertetett módon.

-   Ha a szolgáltatás regisztrációs billentyűt az Ön által létrehozott StorSimple virtuális eszközök kezeléséhez StorSimple Manager szolgáltatásból. További tudnivalókért lásd: [2 lépés: a szolgáltatás regisztrációs kulcs első](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple virtuális tömb.

-   Ha a második vagy későbbi virtuális eszköz, amely egy meglévő StorSimple kezelő szolgáltatás regisztrál, rendelkeznie kell adatokat titkosítókulcs. A kulcs jött létre, amikor az első eszközön sikeresen regisztrálva a szolgáltatáshoz. Ha elvesztette a következő kulcsot, olvassa el a [titkosítókulcs szolgáltatás adatok beolvasása](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) a StorSimple virtuális tömb.

## <a name="step-by-step-setup"></a>Lépésenkénti beállítása

Az alábbi lépésenkénti útmutató segítségével állíthatja be, és állítsa be az StorSimple virtuális eszközt.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Lépés: 1: Fejezze be a helyi webes felhasználói felület beállítást, és regisztráljon az eszközön 


#### <a name="to-complete-the-setup-and-register-the-device"></a>Fejezze be a beállítást, és regisztráljon az eszközön

1.  Nyisson meg egy böngészőablakot, és csatlakozzon a felhasználói felület helyi webes. Írja be: 

    `https://<ip-address of network interface>`

    A csatlakozási URL-cím használata az előző lépésben jegyezni. Ekkor megjelenik egy hiba, jelezve, hogy a webhely biztonsági tanúsítványával probléma van. Kattintson a **Folytatás gombra a weblap**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Jelentkezzen be a webes a virtuális eszköz felhasználói felület **StorSimpleAdmin**. Írja be a módosított eszközt rendszergazdai jelszavát a 3: [rendelkezés egy StorSimple virtuális tömböt a Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) vagy [egy StorSimple VMware virtuális tömbben rendelkezést](storsimple-ova-deploy2-provision-vmware.md), indítsa el a virtuális eszköz.

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  Ekkor megnyílik a **Kezdőlap** lapot. Ez a lap azt ismerteti, hogy a különféle beállításokkal konfigurálása és a StorSimple kezelő szolgáltatás regisztrálhatja a virtuális eszköz szükséges. Megjegyzendő, hogy a **hálózati beállításai** **webes proxybeállításokat**és **időbeállítások** nem kötelező. A csak szükséges beállításokat, hogy **beállításai** és a **felhő beállítások**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  A **hálózati beállítások** lapon a **hálózati kapcsolatok**adatok 0 automatikusan konfigurálja a rendszer meg. Minden egyes hálózati kapcsolat beállítása alapértelmezés szerint letöltésére IP-cím (DHCP). Emiatt az IP-cím, a alhálózat és az átjáró automatikusan osztják (az IPv4 és az IPv6).

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Ha egynél több hálózati kapcsolaton adta meg az eszköz kiépítési során, Itt konfigurálhatók. Megjegyzés: beállíthatja a hálózati kapcsolat IPv4, csak vagy másként IPv4 és az IPv6. Az IPv6 nem támogatottak, csak a konfigurációk.

1.  A DNS-kiszolgálók szükség, mivel az eszköz használatával kommunikál a felhőalapú tárolási szolgáltatók vagy az eszköz feloldásához alkalommal, amikor egy fájl kiszolgálójával konfigurálásakor név szerint. A **hálózati beállítások** lapon, a **DNS-kiszolgálók**csoportban:

    1.  Az elsődleges és másodlagos DNS-kiszolgáló automatikusan konfigurálja a rendszer. Ha úgy dönt, hogy statikus IP-címek konfigurálása, megadhatja a DNS-kiszolgálók. Magas elérhetőségét ajánlott egy elsődleges és másodlagos DNS-kiszolgáló konfigurálása.

    2.  Kattintson a **alkalmazása**gombra. Ezzel alkalmazása és a hálózat beállításainak ellenőrzése.

2.  A **hangeszköz beállításai** lapon:

    1.  Rendeljen egy egyedi **nevet** az eszközére. Ez a név 1-15 karakterből állhat, és betűvel, számokat és kötőjelet is tartalmazhat.

    2.  Kattintson a **fájl kiszolgálói** ikonra ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) hoz létre eszköz **típusa** . Fájlkiszolgálóra lehetővé teszi, hogy hozzon létre a megosztott mappák.

    3.  Mivel az eszköz fájlkiszolgálóra, szüksége lesz az eszköz csatlakozik a tartományhoz. Írja be a **tartománynevét**.

    1.  Kattintson a **alkalmazása**gombra.

2.  Egy párbeszédpanel jelenik meg. Adja meg a tartomány hitelesítő adatait a megadott formátumban. Kattintson a jelölőnégyzet ikonra. Rendszer ellenőrzi a tartomány hitelesítő adatokat. Hibaüzenet jelenik meg, ha helytelen a hitelesítő adatokat.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Kattintson a **alkalmazása**gombra. Ez alkalmazni, és ellenőrizze a hangeszköz beállításai.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Győződjön meg arról, hogy a virtuális tömb saját szervezeti egységre az Active Directory, és nem csoportházirend-objektumok (GPO) alkalmazásával vagy örökli. A csoportházirend előfordulhat, hogy alkalmazásokat, például a víruskereső szoftver telepítése a StorSimple virtuális tömb. Külön szoftver telepítése nem támogatott, és vezethet adatsérülés. 

1.  (Nem kötelező), állítsa be a webes proxykiszolgálón. Webes proxy beállítások nem kötelező, bár ügyeljen arra, hogy egy webes proxykiszolgáló használata esetén csak beállíthatja azt az alábbi.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    A **Web proxy** lapon:

    1.  Adja meg a **webes proxy URL-címe** a következő formátumban: *http://&lt;host-IP cím vagy FQDN&gt;: Port száma*. Figyelje meg, hogy a HTTPS URL-címek nem támogatottak.

    2.  Adja meg a **hitelesítési** **egyszerű** vagy a **Nincs lehetőséget**.

    3.  Ha a hitelesítés használatával is szüksége lesz a **felhasználónév** és **jelszó**megadására.

    4.  Kattintson a **alkalmazása**gombra. Ez az érvényesítés, és a beállított webes proxybeállítások alkalmazása.

1.  (Nem kötelező) adja meg az eszközre, például az időzóna és az elsődleges és másodlagos alapú kiszolgálók idő beállításait. ALAPÚ kiszolgálók szükség, mert az eszközön, hogy az a felhő szolgáltatókkal hitelesíteni tudja kell szinkronizálni idő.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    Az **idő beállítása** lapon:

    1.  A legördülő listából válassza ki az **Időzóna** , amelyben az eszközre telepítve van a földrajzi helye alapján. Az alapértelmezett időzóna mobileszközére PST. Az eszköz összes ütemezett művelet Ez az időzóna fogja használni.

    2.  Adjon meg egy **elsődleges NTP-kiszolgáló** mobileszközére, vagy fogadja el a time.windows.com az alapértelmezett értéket. Győződjön meg arról, hogy a hálózat lehetővé teszi, hogy NTP forgalmat a adatközponthoz átadhatja az internethez.

    3.  Ha szükséges, adjon meg egy **másodlagos NTP-kiszolgálót** az eszközhöz.

    4.  Kattintson a **alkalmazása**gombra. Ez az érvényesítés, és alkalmazza a beállított idő beállításokat.

1.  A felhőben az eszköz beállításainak konfigurálása Ebben a lépésben Végezze el a helyi eszközzel konfigurálását, és kattintson az eszköz regisztrálása a StorSimple kezelő szolgáltatás.

    1.  Írja be a **szolgáltatás regisztrációs kulcs** szerezte be az [2 lépés: a szolgáltatás regisztrációs kulcs első](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple virtuális tömb.

    2.  Ugorja át ezt a lépést, ha ez az első eszköz regisztráció ezt a szolgáltatást, és folytassa a következő lépéssel. Ha nem az első eszközön, amely regisztrál az ezt a szolgáltatást, szüksége lesz a **szolgáltatás adatok titkosítási kulcs**megadásához. A kulcs szükség a szolgáltatás regisztrációs kulccsal további eszközök regisztrálhatja a StorSimple Manager szolgáltatással. További tudnivalókért olvassa el a [szolgáltatás adatok titkosítási kulcs](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) beszerzése a helyi webes felhasználói felület.

    3.  Kattintson a **Regisztrálás**gombra. Ez lesz indítsa újra az eszközt. Előfordulhat, ha megvárja, mielőtt az eszköz sikeresen regisztrált 2 – 3 perc. Az eszköz újraindítása után venni a bejelentkezési lapra.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Térjen vissza az Azure klasszikus portálon. Az **eszközök** lapon ellenőrizze, hogy az eszköz sikeresen csatlakozott a szolgáltatás állapota felfelé megjeleníti. Az Eszközállapot kell lennie **az aktív**.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Lépés: 2: A szükséges eszköz telepítéséhez

Az eszköz konfigurációjának StorSimple készüléke befejezéséhez kell:

-   Jelölje ki a tárhely fiókot társíthatja az eszközön.

-   Válassza ki az adatokat, hogy a rendszer elküldi a felhő titkosítási beállításokat.

Az [Azure klasszikus portált](https://manage.windowsazure.com/) a szükséges eszköz telepítéséhez kövesse az alábbi lépéseket.

#### <a name="to-complete-the-minimum-device-setup"></a>A minimális eszköz telepítéséhez

1.  Az **eszközök** lapon jelölje ki az imént létrehozott eszközt. Ez az eszköz **aktív**módon jelenik meg. Kattintson a nyílra az eszköz neve ellen, és kattintson a **Quick Start**.

2.  Kattintson a **teljes eszközbeállítások** eszköz konfigurálása varázsló elindításához.

3.  Konfigurálása eszköz varázsló az **Alapvető beállítások** lapon tegye a következőket:

    1.  Adja meg az eszközhöz használandó a tárterület-fiókot. Válasszon ki egy meglévő tároló számlát az előfizetés a legördülő listából, vagy adja meg a **További hozzáadása** egy fiókot választhat egy másik előfizetést.

    2.  Az összes adat-a-többi (AES titkosítást), amelyet a felhőbe titkosítási beállításainak megadása Titkosítsa az adatokat, jelölje be a beviteli listára, **felhőalapú tárolási titkosítási kulcs**engedélyezéséhez. Adja meg, amely 32 karaktereket tartalmaz, felhőalapú tárolási-titkosítás. Írja be újra a kulcsot a megerősítéshez. 256 bites AES kulcs titkosításhoz a felhasználó által definiált kulccsal lesz.

    3.  Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

A beállításokat ekkor frissülnek. Miután beállítások frissítése sikerült, a teljes eszköz beállítása gombra szürke lesz. Ad vissza, az eszköz **Rövid** kezdőlapjára.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> Minden más eszközbeállítások elérésével **a lap** bármikor módosíthatja.

## <a name="step-3-add-a-share"></a>Lépés 3: Megosztás hozzáadása

Az [Azure klasszikus portál](https://manage.windowsazure.com/) megosztás létrehozásához kövesse az alábbi lépéseket.

#### <a name="to-create-a-share"></a>Megosztás létrehozása

1.  Eszköz **Első lépések** lapon kattintson a **Hozzáadás a megosztás**gombra. Ez a parancs elindítja felvétele megosztási varázslóban.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Az **Alapvető beállítások** lapon tegye a következőket:

    1.  Adja meg, hogy egy egyedi nevet a megosztást. A név olyan karakterlánc, amely a 3-127 karakterből kell lennie.

    2.  (Nem kötelező) A megosztás leírást. A leírás azonosításához megosztás tulajdonosai.

    3.  Egy használatát adja meg a megosztást. Felhasználás típusa lehet **Tiered** vagy a **kiemelt helyi meghajtóra**, többszintű alatt az alapértelmezett. Helyi garanciákkal, alacsony késések és nagyobb teljesítmény igénylő munkaterhelésekből válassza a **helyi meghajtóra a kiemelt** megosztás. Egyéb adatok jelölje ki a **Tiered** megosztás.

    A helyi meghajtóra a kiemelt megosztás thickly már kiépítve, és biztosíthatja, hogy az elsődleges adatokat a megosztott maradjon helyi az eszközre, és nem a felhőbe spill. A többszintű megosztás azonban már vékonyan kiépítve. A többszintű megosztás létrehozásakor 10 %-a hely a helyi rétegben már kiépítve, és a szóköz 90 %-át már kiépítve a felhőben. Például, a kiépítéstől 1 TB mennyiségig, ha a helyi lemezterület 100 GB volna tárolnak és 900 GB szeretné használni, a felhőben Ha az adatok rétegek. Ez az azt jelenti, hogy ha a helyi lemezterület fogyni az eszközön, akkor nem kiépítése többszintű megosztás.

1.  Adja meg a megosztás kiépített kapacitása. Figyelje meg, hogy a megadott kapacitás kisebb, mint a rendelkezésre álló kapacitás kell lennie. A többszintű megosztás használata esetén a megosztás mérete 500 GB és 20 TB között kell lennie. Helyben a kiemelt megosztás adja meg a megosztás méretet 50 GB és 2 TB között. Útmutató a megosztás kiépítése a rendelkezésre álló kapacitás használnak. Ha a rendelkezésre álló helyi kapacitás 0 GB, majd meg nem használható helyi vagy többszintű megosztások hozhatók létre.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  A nyílikonra ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) Ugrás a következő lapra.

1.  A **További beállítások** lapon az engedélyeket társíthat a felhasználó vagy csoport a megosztás csatlakozó. A felhasználó vagy a felhasználó csoport nevének megadása *<john@contoso.com>* formátum. Azt javasoljuk, hogy egy felhasználó csoport (helyett egy felhasználó) használatával lehetővé teszi a rendszergazdai jogosultsággal a megosztást. Itt az engedélyek hozzárendelése után az engedélyek módosítása azután használhatja a Windows Intézőben.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). A megosztás a megadott beállításokkal jön létre. Alapértelmezés szerint figyelemmel kísérésére és biztonsági másolat engedélyezi a megosztás.

## <a name="step-4-connect-to-the-share"></a>Lépés: 4: A megosztás csatlakoztatása

Most kell a hozzádni az előző lépésben létrehozott csatlakozni. Hajtsa végre ezeket a lépéseket a Windows Server állomáson.

#### <a name="to-connect-to-the-share"></a>A megosztás eléréséhez

1.  Nyomja le a ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. A Futtatás párbeszédpanelen adja meg a * \\ * a mappa elérési útjaként cseréje a fájl kiszolgálói rendelt eszköz nevű *fájl kiszolgálónevet* . Kattintson az **OK gombra**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Ennek hatására megnyílik a Explorer be. Most már láthatja az Ön által létrehozott megosztások mappaként. Jelölje ki, majd kattintson duplán a tartalom megosztás (mappa).

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  Most fájlok hozzáadása a megosztást, és a biztonsági másolatot készíthet.

![videó ikon](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **Videó érhető el**

Nézze meg a videóból megtudhatja, hogyan konfigurálása és egy StorSimple virtuális tömböt fájlkiszolgálóra rögzíteni.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként felügyelheti [A StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md)helyi webes felület használata.
