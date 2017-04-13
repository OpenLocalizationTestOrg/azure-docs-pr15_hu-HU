<properties 
   pageTitle="Virtuális tömb StorSimple iSCSI server telepítése |} Microsoft Azure"
   description="Kezdeti telepítés végrehajtásához, a StorSimple iSCSI kiszolgáló regisztrálása és eszköz telepítéséhez ismerteti."
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
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Üzembe StorSimple virtuális tömb – egy iSCSI kiszolgálójával a virtuális eszköz beállítása

![iSCSI beállítási folyamatot.](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban telepítési a Microsoft Azure StorSimple virtuális tömbképletek (más néven a StorSimple helyszíni virtuális eszköz vagy a StorSimple virtuális eszköz) futó március 2016 általános elérhetőség (kiadás) kiadásra vonatkozik. Ebből az oktatóanyagból megtudhatja, hogy miként kezdeti telepítés végrehajtásához, a StorSimple iSCSI kiszolgáló regisztrálása, az eszköz telepítéséhez és majd létrehozása, csatlakoztatása, inicializálni és formázhatja a StorSimple virtuális eszköz iSCSI kiszolgálón őket. Ebben a cikkben a StorSimple telepítési információk csak StorSimple virtuális tömbök vonatkozik. 

Ismertetett eljárások itt hogy körülbelül 30 percig befejezéséhez 1 órára. Ez a cikk közzétett információkra csak StorSimple virtuális tömbök vonatkozik.

## <a name="setup-prerequisites"></a>Telepítési előfeltételek

Mielőtt konfigurálása és a virtuális StorSimple az eszköz beállítása, győződjön meg arról, hogy:

- Virtuális eszköz kiépítéstől, és a kapcsolódó [Üzembe StorSimple virtuális tömb - rendelkezés egy virtuális tömböt a Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) vagy [Üzembe StorSimple virtuális tömb - rendelkezés egy virtuális tömbben VMware](storsimple-ova-deploy2-provision-vmware.md)ismertetett módon.

- Ha a szolgáltatás regisztrációs billentyűt az Ön által létrehozott StorSimple virtuális eszközök kezeléséhez StorSimple Manager szolgáltatásból. További tudnivalókért lásd: **2 lépés: a szolgáltatás regisztrációs kulcs első** [üzembe StorSimple virtuális](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key)tömbben - készítse elő a portálon.

- Ha a második vagy későbbi virtuális eszköz, amely egy meglévő StorSimple kezelő szolgáltatás regisztrál, rendelkeznie kell adatokat titkosítókulcs. A kulcs jött létre, amikor az első eszközön sikeresen regisztrálva a szolgáltatáshoz. Ha elvesztette a következő kulcsot, nézze meg az **adatok titkosítókulcs első** [használata a webes felület felügyelheti a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Lépésenkénti beállítása 

Az alábbi lépésenkénti útmutató segítségével állíthatja be, és állítsa be az StorSimple virtuális eszközt:

-  [Lépés: 1: Fejezze be a helyi webes felhasználói felület beállítást, és regisztráljon az eszközön](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Lépés: 2: A szükséges eszköz telepítéséhez](#step-2-complete-the-required-device-setup)
-  [3 lépés: A hangerő hozzáadása](#step-3-add-a-volume)
-  [Lépés: 4: Csatlakoztatása inicializálni és kötet formázása](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Lépés: 1: Fejezze be a helyi webes felhasználói felület beállítást, és regisztráljon az eszközön 

#### <a name="to-complete-the-setup-and-register-the-device"></a>Fejezze be a beállítást, és regisztráljon az eszközön

1. Nyisson meg egy böngészőablakot, és írja be a webes felhasználói felület csatlakozni:

    `https://<ip-address of network interface>`

    A csatlakozási URL-cím használata az előző lépésben jegyezni. Ekkor megjelenik egy hiba jelenik meg, amely tudatja, hogy a webhely biztonsági tanúsítványával probléma van. Kattintson a **Folytatás gombra a weblap**.

    ![biztonsági tanúsítványt hiba](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Jelentkezzen be a webes a virtuális eszköz felhasználói felület **StorSimpleAdmin**. Írja be a módosított eszközt rendszergazdai jelszavát a 3: Indítsa el a virtuális eszköz üzembe StorSimple virtuális vagy [Tömbben](storsimple-ova-deploy2-provision-hyperv.md) lévő - kiépítése a Hyper-V virtuális eszköz [Üzembe StorSimple virtuális tömb - rendelkezést VMware virtuális eszközt](storsimple-ova-deploy2-provision-vmware.md).

    ![Bejelentkezési lapja](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. Ekkor megnyílik a **Kezdőlap** lapot. Ez a lap azt ismerteti, hogy a különféle beállításokkal konfigurálása és a StorSimple kezelő szolgáltatás regisztrálhatja a virtuális eszköz szükséges. Megjegyzendő, hogy a **hálózati beállításai** **webes proxybeállításokat**és **időbeállítások** nem kötelező. A csak szükséges beállításokat, hogy **beállításai** és a **felhő beállítások**.

    ![A Kezdőlap lapon](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. A **hálózati beállítások** lapon a **hálózati kapcsolatok**0 adatok automatikusan konfigurálja a rendszer meg. Minden hálózati kapcsolat alapértelmezés szerint van állítva az IP-cím letöltésére (DHCP). Ezért IP-cím, alhálózat és átjáró automatikusan osztják (az IPv4 és az IPv6).

    Tervezze meg az eszköz telepítése egy iSCSI kiszolgálójával (a továbbfejlesztett fájlblokkolás tároló létesítése), azt javasoljuk, hogy az **első IP-cím automatikusan** a beállításnak a letiltását, és statikus IP-címek konfigurálása.

    ![Hálózati beállítások lap](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Ha egynél több hálózati kapcsolaton adta meg az eszköz kiépítési során, Itt konfigurálhatók. Megjegyzés: beállíthatja a hálózati kapcsolat IPv4, csak vagy másként IPv4 és az IPv6. Az IPv6 nem támogatottak, csak a konfigurációk.

5. Mivel az eszköz használatával kommunikál a felhőalapú tárolási szolgáltatók, vagy úgy oldhatja fel az eszközön, nevét, ha egy fájl kiszolgálójával be van állítva alkalommal, amikor a DNS-kiszolgálók szükség. A **hálózati beállítások** lapon a **DNS-kiszolgálók**csoportban:

    1. Az elsődleges és másodlagos DNS-kiszolgáló automatikusan konfigurálja a rendszer. Ha úgy dönt, hogy statikus IP-címek konfigurálása, megadhatja a DNS-kiszolgálók. Magas elérhetőségét ajánlott egy elsődleges és másodlagos DNS-kiszolgáló konfigurálása.

    2. Kattintson a **alkalmazása**gombra. Ezzel alkalmazása és a hálózat beállításainak ellenőrzése.

6. A **hangeszköz beállításai** lapon:

    1. Rendeljen egy egyedi **nevet** az eszközére. Ez a név 1-15 karakterből állhat, és betűvel, számokat és kötőjelet is tartalmazhat.

    2. Kattintson a **iSCSI kiszolgáló** ikonra ![iSCSI server ikonja](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) hoz létre eszköz **típusa** . Egy iSCSI kiszolgáló lehetővé teszi, blokkokból álló tároló létesítése való.

    3. Adja meg, ha ez az eszköz kell a tartományhoz. Ha eszköze egy iSCSI kiszolgálót, majd a tartományhoz a lépés nem kötelező. Ha úgy dönt, hogy a tartományba való csatlakozás nélkül a iSCSI kiszolgáló, kattintson az **Alkalmaz**gombra, a várja meg a beállításokat alkalmazza, és ugorja át a következő lépéssel.

        Ha azt szeretné, ha be szeretne kapcsolódni az eszköz a tartományhoz. Írja be a **tartománynevét**, és kattintson az **Alkalmaz**gombra.

        > [AZURE.NOTE] Ha iSCSI kiszolgálója csatlakozás tartomány csoportban ellenőrizze, hogy a virtuális tömb a saját szervezeti egységre Microsoft Azure Active Directory és nem csoportházirend-objektumok (GPO) vonatkoznak rá.

    5. Egy párbeszédpanel jelenik meg. Adja meg a tartomány hitelesítő adatait a megadott formátumban. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Rendszer ellenőrzi a tartomány hitelesítő adatokat. Hibaüzenet jelenik meg, ha helytelen a hitelesítő adatokat.

        ![hitelesítő adatok](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Kattintson a **alkalmazása**gombra. Ez alkalmazni, és ellenőrizze a hangeszköz beállításai.
 
7. (Nem kötelező), állítsa be a webes proxykiszolgálón. Webes proxy beállítások nem kötelező, bár ügyeljen arra, hogy egy webes proxykiszolgáló használata esetén csak beállíthatja azt az alábbi.

    ![webes proxy beállítása](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    A **Web proxy** lapon:

    1. Adja meg a **webes proxy URL-címe** a következő formátumban: *http://host-IP cím* vagy *FDQN:Port szám*. Figyelje meg, hogy a HTTPS URL-címek nem támogatottak.

    2. Adja meg a **hitelesítési** **egyszerű** vagy a **Nincs lehetőséget**.

    3. Ha hitelesítést használ, akkor is kell **felhasználónév** és **jelszó**megadására.

    4. Kattintson a **alkalmazása**gombra. Ez az érvényesítés, és a beállított webes proxybeállítások alkalmazása.
 
8. (Nem kötelező) adja meg az eszközre, például az időzóna és az elsődleges és másodlagos alapú kiszolgálók idő beállításait. ALAPÚ kiszolgálók szükség, mert az eszközön, hogy az a felhő szolgáltatókkal hitelesíteni tudja kell szinkronizálni idő.

    ![Időbeállítások.](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    Az **idő-beállítások** lapon:

    1. A legördülő listából válassza ki az **Időzóna** , amelyben az eszközre telepítve van a földrajzi helye alapján. Az alapértelmezett időzóna mobileszközére PST. Az eszköz összes ütemezett művelet Ez az időzóna fogja használni.

    2. Adjon meg egy **elsődleges NTP-kiszolgáló** mobileszközére, vagy fogadja el a time.windows.com az alapértelmezett értéket. Győződjön meg arról, hogy a hálózat lehetővé teszi, hogy NTP forgalmat a adatközponthoz átadhatja az internethez.

    3. Ha szükséges, adjon meg egy **másodlagos NTP-kiszolgálót** az eszközhöz.

    4. Kattintson a **alkalmazása**gombra. Ez az érvényesítés, és alkalmazza a beállított idő beállításokat.

9. A felhőben az eszköz beállításainak konfigurálása Ebben a lépésben Végezze el a helyi eszközzel konfigurálását, és kattintson az eszköz regisztrálása a StorSimple kezelő szolgáltatás.

    1. Írja be a **szolgáltatás regisztrációs kulcs** szerezte be az **2 lépés: a szolgáltatás regisztrációs kulcs első** [üzembe StorSimple virtuális](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key)tömbben - készítse elő a portálon.

    2. Ha nem az első eszközön, amely regisztrál az ezt a szolgáltatást, szüksége lesz a **szolgáltatás adatok titkosítási kulcs**megadásához. A kulcs szükség a szolgáltatás regisztrációs kulccsal további eszközök regisztrálhatja a StorSimple Manager szolgáltatással. További tudnivalókért olvassa el az [adatok titkosítókulcs beszerzése](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) a helyi webes felhasználói felület.

    3. Kattintson a **Regisztrálás**gombra. Ez lesz indítsa újra az eszközt. Előfordulhat, ha megvárja, mielőtt az eszköz sikeresen regisztrált 2 – 3 perc. Az eszköz újraindítása után venni a bejelentkezési lapra.

       ![Regisztráljon az eszközön](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Térjen vissza az Azure klasszikus portálon. Az **eszközök** lapon ellenőrizze, hogy az eszköz sikeresen csatlakozott a szolgáltatás állapota felfelé megjeleníti. Az Eszközállapot kell lennie **az aktív**.

    ![Eszközök lap](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Lépés: 2: A szükséges eszköz telepítéséhez

Az eszköz konfigurációjának StorSimple készüléke befejezéséhez kell:

- Jelölje ki a tárhely fiókot társíthatja az eszközön.

- Válassza ki az adatokat, hogy a rendszer elküldi a felhő titkosítási beállításokat.

Az Azure klasszikus portálon a szükséges eszköz telepítéséhez kövesse az alábbi lépéseket.

#### <a name="to-complete-the-minimum-device-setup"></a>A minimális eszköz telepítéséhez

1. Az **eszközök** lapon jelölje ki az imént létrehozott eszközt. Ez az eszköz **aktív**módon jelenik meg. Ezután kattintson a nyílra az eszköz nevét, és kattintson a **Quick Start**.

    ![Eszközök lap](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Kattintson a **teljes eszközbeállítások** eszköz konfigurálása varázsló elindításához.

    ![Eszköz varázsló konfigurálása](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. Az **Alapvető beállítások** lapon az eszköz beállítása varázsló tegye a következőket:

   1. Adjon meg az eszköz használható tárterületet fiókot. Ez az előfizetés kijelölhet egy meglévő tárterület-fiókot a legördülő listából, vagy megadhatja a **További hozzáadása** a válasszon ki egy fiókot egy másik előfizetésből.

   2. A többi, amelyet a felhőbe az adatokat a titkosítási beállításainak megadása (A StorSimple AES-256 titkosítást használ.) Ha titkosítani szeretné az adatokat, jelölje be a **engedélyezése felhőalapú tárolási titkosítási** jelölőnégyzetet. Adja meg, amely 32 karaktereket tartalmaz, felhőalapú tárolási-titkosítás. Írja be újra a kulcsot a megerősítéshez.

   3. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Alapvető beállításai](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    A beállításokat ekkor frissülnek. Miután beállítások frissítése sikerült, a teljes eszköz beállítása gomb nem érhető el. Ad vissza, az eszköz **Rövid** kezdőlapjára.                                                        

>[AZURE.NOTE]Minden más eszközbeállítások elérésével **a lap** bármikor módosíthatja.

## <a name="step-3-add-a-volume"></a>3 lépés: A hangerő hozzáadása

A következő lépésekkel kötet létrehozása az Azure klasszikus portálon.

#### <a name="to-create-a-volume"></a>Kötet létrehozása

1. Eszköz **Első lépések** lapon kattintson a **Hozzáadás a mennyiségi**gombra. Hozzáadása mennyiségi varázsló elindul.

2. A Hozzáadás a mennyiségi varázsló az **Alapvető beállítások**csoportban tegye a következőket:

    1. Adjon egy egyedi nevet a mennyiségi. A név olyan karakterlánc, amely a 3-127 karakterből kell lennie.

    2. Írja be a kötet leírását. A leírás segítséget nyújt a mennyiségi tulajdonosok azonosítása.

    3. Válassza ki a mennyiségi használatát típusát. Felhasználás típusa lehet **Tiered mennyiségi** vagy **helyileg kiemelt mennyiségi.** (**Tiered** hangereje az alapértelmezett.) Helyi garanciákkal, alacsony késések és nagyobb teljesítmény igénylő munkaterhelésekből jelölje ki a **kiemelt helyileg** **mennyiségi**. Egyéb adatok jelölje ki a **mennyiségi** **Tiered** .

        A helyi meghajtóra rögzített mennyiségi thickly már kiépítve, és biztosíthatja, hogy a kötet elsődleges adatainak megmarad az eszközön, és nem spill a felhőbe. Egy helyi rögzített mennyiségi hoz létre, ha az eszköz ellenőrzi, a rendelkezésre álló hely a helyi rétegek kötet létrehozása a kívánt méretre. Helyi meghajtóra rögzített kötet létrehozása szükség lehet a felhőbe az eszközről a meglévő adatok kiömlést, és előfordulhat, hogy a kötet létrehozásához szükséges idő hosszú. A teljes ideje méretét a kiépített kötet, rendelkezésre álló hálózati sávszélesség és az adatokat az eszközön függ.

        A többszintű kötet azonban vékonyan már kiépítve és a nagyon gyorsan hozhatók létre. A többszintű mennyiségi létrehozásakor a szóköz körülbelül 10 %-át a helyi rétegben már kiépítve, és a szóköz 90 %-át a felhőben már kiépítve. Például, a kiépítéstől 1 TB mennyiségig, ha a helyi lemezterület 100 GB volna tárolnak és 900 GB szeretné használni, a felhőben Ha az adatok rétegek. Ez az azt jelenti, hogy ha az eszközön, a helyi lemezterület fogyni a (azért, mert nem lesz elérhető a 10 %-ot), nem kiépítése többszintű megosztás.

    4. Adja meg, hogy a kötet kiépített kapacitása. Figyelje meg, hogy a megadott kapacitás kisebb, mint a rendelkezésre álló kapacitás kell lennie. A többszintű mennyiségi hoz létre, ha a mérete 500 GB és 5 TB között kell lennie. Egy helyi rögzített kötet adja meg a mennyiségi méretet 50 GB és 500 GB között. Használja a rendelkezésre álló kapacitás kötet kiépítési útmutatóját. Ha a rendelkezésre álló helyi kapacitás 0 GB, majd meg nem használható helyileg rögzített vagy többszintű kötet hozhatók létre.

        ![Alapvető beállításai](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Kattintson a nyíl ikonra ![nyíl ikonra](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) Ugrás a következő lapra.

3. A **További beállítások** lapon új rekord access vezérlőelem (ACR):

    1. Adja meg egy **nevet** a ACR az.

    2. A **iSCSI kezdeményező nevét**, adja meg a a iSCSI minősített neve (IQN-neve): a Windows-szolgáltató. Ha még nincs telepítve a IQN, nyissa meg a [függelék A: beolvasása a Windows Server állomás IQN](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. Azt javasoljuk, hogy engedélyezze az alapértelmezett biztonsági másolatot az **alapértelmezett biztonsági másolatot készít a mennyiségi engedélyezése** jelölőnégyzet bejelölésével. Az alapértelmezett biztonsági mentési hoz létre egy házirendet, amely végrehajtja a 22:30 naponta (eszköz idő) és a hangerő felhő pillanatképét hoz létre.

        ![a további beállítások](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Ez a parancs elindítja a mennyiségi létrehozási feladatot. Folyamatban az alábbihoz hasonló üzenet jelenik meg.

        ![folyamatban üzenet](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        A mennyiségi létrejön a megadott beállításokkal. Alapértelmezés szerint figyelése és biztonsági másolat engedélyezi a kötet.

    5. Győződjön meg arról, hogy a kötet sikeresen létrejött, hogy a **kötet** lap megnyitásához. Meg kell jelennie a hangerő szerepel a listában.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Lépés: 4: Csatlakoztatása inicializálni és kötet formázása

A következő lépésekkel csatlakoztatásához, inicializálni és formázhatja a StorSimple őket a Windows Server állomáson.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Csatlakoztassa, inicializálni és kötet formázása

1. Indítsa el a Microsoft iSCSI kezdeményező.

2. Kattintson a **iSCSI kezdeményező tulajdonságok** ablak a lap **feltáráshoz használt** **Felfedezése portálon**.

    ![Fedezze fel a portálon](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. **Cél portál felfedezése** párbeszédpanelen adja meg a hálózati iSCSI engedélyező illesztő IP-címét, és kattintson **az OK**gombra.

    ![IP-cím](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. A **iSCSI kezdeményező tulajdonságok** ablak a **tárolók** lapon keresse meg a **Discovered célok**. (Az egyes mennyiségi észlelt cél lesznek.) Az Eszközállapot szerint **Inaktív**jelenjenek meg.

    ![észlelt cél](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Válasszon egy cél eszközt, és kattintson a **Csatlakozás**gombra. Miután az eszköz csatlakozik, az állapot módosítsa **csatlakoztatva**. (A Microsoft iSCSI kezdeményező használatával kapcsolatos további tudnivalókért lásd: [telepítése és konfigurálása a Microsoft iSCSI kezdeményező] [1].

    ![Jelölje be a cél eszköz](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. A Windows állomáson nyomja le a Windows billentyű + X billentyűkombinációt, és válassza a **Futtatás**gombra.

7. A **Futtatás** párbeszédpanelen írja be a **Diskmgmt.msc**. Kattintson az **OK gombra**, és a **Lemez kezelése** párbeszédpanel jelenik meg. A jobb oldali ablaktáblában jelennek meg a kötet az állomáson.

8. A **Merevlemez-kezelés** ablakban a csatlakoztatott kötet jelenik meg az alábbi ábrán látható módon. Kattintson a jobb gombbal az észlelt mennyiségi (kattintson a lemez neve), és kattintson az **Online**.

    ![lemezen kezelése](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Kattintson a jobb gombbal, és válassza ki a **Lemezen inicializálni**.

    ![1 lemez inicializálni](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. A párbeszédpanelen jelölje be a lemez(ek) inicializálni, és kattintson **az OK**gombra.

    ![2 lemez inicializálni](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. Az új egyszerű kötet varázsló elindul. Válassza ki a lemezen, és kattintson a **Tovább**gombra.

    ![új mennyiségi varázsló 1](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Egy betűjele hozzárendelése a hangerőt, és kattintson a **Tovább gombra**.

    ![új mennyiségi varázsló 2](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Adja meg a paraméterek a kötet formázása. **A Windows Serveren csak NTFS használata támogatott.** Állítsa az Ausztráliai 64K. A mennyiségi a címke szükséges. Ajánlott a legjobb megegyeznek az StorSimple virtuális eszközön megadott mennyiségi nevét a név. Kattintson a **Tovább**gombra.

    ![új mennyiségi varázsló 3](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Jelölje be a mennyiségi értékeit, és kattintson a **Befejezés gombra**.

    ![új mennyiségi varázsló 4](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    A kötet **Online** megjelennek a **Lemez kezelése** lapon.

    ![online kötet](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként felügyelheti [A StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md)helyi webes felület használata.

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>A: függelék Beolvasása a Windows Server állomás IQN

A következő lépésekkel veheti a iSCSI minősített a nevét (IQN) a Windows Server 2012-kiszolgáló állomás Windows.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>A Windows állomás IQN eléréséhez

1. Indítsa el a Microsoft iSCSI kezdeményező a Windows állomáson.

2. A **iSCSI kezdeményező tulajdonságok** ablak a **beállítás** lapon jelölje ki és másolja a vágólapra a karakterlánc a **Kezdeményező neve** mezőben.

    ![iSCSI kezdeményező tulajdonságai](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Mentse a karakterlánc.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



