<properties 
   pageTitle="CHAP beállítása az StorSimple eszköz |} Microsoft Azure"
   description="Állítsa be a kérdés Handshake Authentication Protocol (CHAP) StorSimple eszközön ismerteti."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>Az StorSimple eszköz CHAP konfigurálása

Ebből az oktatóanyagból megtudhatja, hogyan CHAP konfigurálása StorSimple mobileszközére. A lépéseket a jelen cikkben részletes StorSimple 8000 sorozat, valamint a StorSimple 1200 eszközök vonatkozik.

CHAP hitelesítési protokoll jelöli. A távoli ügyfél azonosítója érvényesítéséhez kiszolgáló által használt hitelesítési eljárás. Az igazolás egy megosztott jelszavát vagy a titkos alapul. Lehet, hogy CHAP egyirányú (egyirányú) vagy kölcsönös (kétirányú). Egyirányú CHAP van, amikor a tároló hitelesíti egy kezdeményező. Kölcsönös vagy fordított CHAP, azonban, és elő kell készítenie, hogy a célhely hitelesíteni a kezdeményező a kezdeményező hitelesítse a cél. Kezdeményező hitelesítési valósítható cél hitelesítés nélkül. Cél hitelesítési azonban valósítható, csak akkor, ha a kezdeményező hitelesítési is tartalmazzák. 

Legjobb módszer azt javasoljuk, hogy CHAP használatával iSCSI biztonság fokozása.

>[AZURE.NOTE] Ne feledje, hogy IPSEC jelenleg nem támogatott StorSimple eszközökön.

A CHAP beállítások StorSimple az eszközre az alábbi módon állítható be:

- Egyirányú vagy egyirányú hitelesítés

- Kétirányú vagy kölcsönös vagy fordított hitelesítés

Mindegyik ezekben az esetekben az eszköz és a iSCSI kezdeményező kiszolgálószoftver a portál kell beállítania. Ebben a konfigurációban a lépések részletes leírását a következő oktatóanyagra.

## <a name="unidirectional-or-one-way-authentication"></a>Egyirányú vagy egyirányú hitelesítés

Egyirányú hitelesítés esetén a cél hitelesíti a kezdeményező. Ez a hitelesítés meg kell adni a CHAP kezdeményező beállítások StorSimple eszköz és a iSCSI kezdeményező szoftver az állomáson. A StorSimple eszköz és a Windows host részletes eljárások következő témakörben olvashat.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Az eszközt a egyirányú hitelesítés konfigurálása

1. Az **eszközök** lapon az Azure klasszikus portálon kattintson a **beállítás** fülre.

    ![Kezdeményező CHAP](./media/storsimple-configure-chap/IC740943.png)

2. Görgessen lefelé, ezen a lapon, és a **Kezdeményező CHAP** szakaszban:
                                                    
    1. Adja meg a CHAP kezdeményező felhasználó nevét.

    2. Adja meg a CHAP kezdeményező tartozó jelszót.

         > [AZURE.IMPORTANT] A CHAP felhasználónév 233-nál kevesebb karakterből kell tartalmaznia. A CHAP jelszót 12 és 16 karaktert között kell lennie. Egy hosszabb, felhasználónév vagy jelszó a Windows állomáson hitelesítési hibát eredményez.
    
    3. Erősítse meg a jelszót.

4. Kattintson a **Mentés**gombra. A megerősítést kérő üzenet jelenik meg. Kattintson az **OK gombra** a módosítások mentéséhez.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>A Windows-kiszolgálón egyirányú hitelesítés konfigurálása

1. A Windows kiszolgálón indítsa el a kezdeményező iSCSI.

2. A **kezdeményező tulajdonságok iSCSI** ablakban végezze el az alábbi lépéseket:
                                                    
    1. Kattintson a **Feltárás** fülre.

        ![iSCSI kezdeményező tulajdonságai](./media/storsimple-configure-chap/IC740944.png)

    2. Kattintson a **portálon felfedezése**.

3. A **Target portál felfedezése** párbeszédpanelen:
                                                    
    1. Adja meg az eszköz IP-címét.

    3. Kattintson a **Speciális**gombra.

        ![Cél portál felfedezése](./media/storsimple-configure-chap/IC740945.png)

4. A **Speciális beállítások** párbeszédpanelen:
                                                    
    1. Jelölje be a **Bejelentkezés CHAP engedélyezése** jelölőnégyzetet.

    2. A **név** mezőben adja meg a felhasználó nevét a klasszikus portálon CHAP kezdeményező megadott.

    3. A **cél titkos** mezőben adja meg a klasszikus portálon CHAP kezdeményező megadott jelszót.

    4. Kattintson az **OK gombra**.

        ![Általános speciális beállítások](./media/storsimple-configure-chap/IC740946.png)

5. Az **iSCSI kezdeményező tulajdonságok** ablak a **tárolók** lapon az Eszközállapot **Connected**szerint jelenjenek meg. StorSimple 1200 eszköz használatakor majd minden egyes mennyiségi csatlakoztatási iSCSI-tároló mint alább látható módon. Ezért 3-4-es lépéseket kell minden kötet kell ismételni.

    ![Külön célként csatlakoztatott kötet](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Ha módosítja a iSCSI nevét, az új nevet az új iSCSI munkamenetek használható. Az új beállítások nem használják a meglévő munkamenetek mindaddig, amíg Ön kijelentkezik, és jelentkezzen be újra.

További információt a CHAP beállítása a Windows-kiszolgálón megnyitásához [További szempontok](#additional-considerations).


## <a name="bidirectional-or-mutual-authentication"></a>Kétirányú vagy kölcsönös hitelesítés

A tároló hitelesíti a kezdeményező kétirányú hitelesítés, és kattintson a kezdeményező hitelesíti a céllistában. Ehhez a felhasználót, hogy a CHAP kezdeményező beállításait, valamint a fordított CHAP beállítások konfigurálása a eszköz és iSCSI kezdeményező szoftver az állomáson. Az alábbi eljárások bemutatják a lépéseket követve konfigurálása az kölcsönös az eszközre, és a Windows állomáson.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Az eszközt a kölcsönös hitelesítés konfigurálása

1. Az **eszközök** lapon az Azure klasszikus portálon kattintson a **beállítás** fülre.

    ![CHAP cél](./media/storsimple-configure-chap/IC740948.png)

2. Görgessen lefelé, ezen a lapon, és a **Cél CHAP** szakaszban:
                                                    
    1. Az eszköz a **felhasználónév fordított CHAP** szükséges.

    2. Adja meg az eszköz **fordított CHAP jelszót** .

    3. Erősítse meg a jelszót.

3. **Kezdeményező CHAP** csoportban:
                                                
    1. Az eszköz a **felhasználónév** szükséges.

    1. Az eszköz a szükséges **jelszót** .

    3. Erősítse meg a jelszót.

4. Kattintson a **Mentés**gombra. A megerősítést kérő üzenet jelenik meg. Kattintson az **OK gombra** a módosítások mentéséhez.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Kétirányú hitelesítés beállítása a Windows-kiszolgálón

1. A Windows kiszolgálón indítsa el a kezdeményező iSCSI.

2. **Kezdeményező tulajdonságok iSCSI** ablakában kattintson a **beállítás** lapon.

3. Kattintson a **CHAP**.

4. **Kezdeményező kölcsönös CHAP titkos iSCSI** párbeszédpanelen tegye a következőket:
                                                    
    1. Írja be az Azure klasszikus portálon konfigurált **Fordított CHAP jelszót** .

    2. Kattintson az **OK gombra**.

        ![iSCSI kezdeményező kölcsönös CHAP titkos kulcs](./media/storsimple-configure-chap/IC740949.png)

5. Kattintson a **cél** fülre.

6. Kattintson a **Csatlakozás** gombra. 

7. **Csatlakozás a cél** párbeszédpanelen kattintson a **Speciális**gombra.

8. A **Speciális tulajdonságok** párbeszédpanelen:
                                                    
    1. Jelölje be a **Bejelentkezés CHAP engedélyezése** jelölőnégyzetet.

    2. A **név** mezőben adja meg a felhasználó nevét a klasszikus portálon CHAP kezdeményező megadott.

    3. A **cél titkos** mezőben adja meg a klasszikus portálon CHAP kezdeményező megadott jelszót.

    4. Jelölje be a **végrehajtandó kölcsönös hitelesítés** jelölőnégyzetet.

        ![Speciális beállítások kölcsönös hitelesítés](./media/storsimple-configure-chap/IC740950.png)

    5. Kattintson az **OK gombra** a CHAP konfiguráció befejezéséhez
     
További információt a CHAP beállítása a Windows-kiszolgálón megnyitásához [További szempontok](#additional-considerations).

## <a name="additional-considerations"></a>További megfontolandó szempontok

A **Gyors csatlakozás** funkció nem támogatja a kapcsolatok alapértelmezés szerint CHAP engedélyezve van. Ha engedélyezve van-e az CHAP, győződjön meg arról, hogy használja-e a cél csatlakozni a **tárolók** lapon a **Csatlakozás** gombra.

![Cél csatlakoztatása](./media/storsimple-configure-chap/IC740947.png)

Bemutatott **cél csatlakozás** párbeszédpanelen jelölje be a **kedvenc példányok listáját a kapcsolat hozzáadása** jelölőnégyzetet. Ezzel biztosíthatja, hogy minden olyan alkalommal, amikor a számítógép újraindítása kísérlet visszaállítani a kapcsolatot a iSCSI kedvenc célok.

## <a name="errors-during-configuration"></a>Hibák konfigurálása során

Ha a CHAP konfigurációs helytelen, majd valószínűleg **hitelesítési hiba** hibaüzenet közli.

## <a name="verification-of-chap-configuration"></a>CHAP konfigurációjának ellenőrzése

Az alábbi lépéseket követve használt CHAP győződhet meg.

#### <a name="to-verify-your-chap-configuration"></a>A CHAP beállításainak ellenőrzése

1. Kattintson a **kedvenc célokat**.

2. Jelölje be a cél, amelynek hitelesítési engedélyezett.

3. Kattintson a **Részletek**gombra.

    ![kedvenc célok a iSCSI kezdeményező tulajdonságai](./media/storsimple-configure-chap/IC740951.png)

4. A **Kedvenc cél részletei** párbeszédpanelen figyelje meg a **hitelesítés** mezőben lévő bejegyzésnek. Ha a konfigurációs sikeres volt, érdemes például **CHAP**.

    ![Kedvenc cél részletei](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a [StorSimple biztonsági](storsimple-security.md).

- További tudnivalók [felügyelheti StorSimple eszköze az StorSimple kezelő szolgáltatás használatával](storsimple-manager-service-administration.md).
