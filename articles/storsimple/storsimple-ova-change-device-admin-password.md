<properties 
   pageTitle="A StorSimple virtuális eszköz rendszergazdai jelszó módosítása |} Microsoft Azure"
   description="Az Azure klasszikus portálon vagy a felhasználói felület StorSimple virtuális tömb webes használatával módosíthatja a eszközt rendszergazdai jelszó ismerteti."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>A virtuális tömb StorSimple eszközt rendszergazdai jelszavának módosítása

## <a name="overview"></a>– Áttekintés

A Windows PowerShell-felület használatakor a StorSimple virtuális eszköz eléréséhez szükségesek írjon be egy eszközt rendszergazdai jelszót. Amikor először kiépítve és elindítása a StorSimple eszközt, az alapértelmezett jelszava *Jelszo1*. Az adatok biztonsága érdekében az alapértelmezett jelszó lejár az első alkalommal jelentkezik be, és akkor van szüksége a jelszó módosítása.

A helyi webes felhasználói felület vagy a az Azure klasszikus portált a eszközt rendszergazdai jelszó módosítása után az eszközre telepítve van a üzemi környezetben bármikor is használhatja. Ezeket a műveleteket leírását a jelen cikkben.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>Az Azure klasszikus portal segítségével a jelszó módosítása

A következő lépésekkel módosíthatja a eszköz rendszergazdai jelszavát az Azure klasszikus portálon keresztül.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Az Azure klasszikus portálon keresztül eszközt rendszergazdai jelszavának megváltoztatása

1. A portálon, kattintson az **eszközök** > **konfigurációs** az eszközön.

2. Görgessen lefelé az **Eszköz rendszergazdáját jelszó** szakaszig. Adja meg a 8-ban 15 karaktereket tartalmazó rendszergazdai jelszót. A jelszó nagybetűsre, kisbetűsre, numerikus és speciális karakterek kombinációjából kell lennie.

3. Erősítse meg a jelszót.

4. A lap alján a **Mentés** gombra.

Az eszköz rendszergazdai jelszó most frissítve lesznek. A módosított jelszót az eszköz helyileg eléréséhez is használhatja.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>A virtuális tömb StorSimple webes felhasználói felület használatával a jelszó módosítása

A következő lépésekkel módosíthatja a eszközt rendszergazdai jelszó keresztül a helyi webes felhasználói felület.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Felhasználói felület helyi webről eszközt rendszergazdai jelszavának megváltoztatása

1. Kattintson a helyi webes felhasználói felület **karbantartási** > a**jelszó módosítása** az eszközére.

    ![Jelszo1 módosítása](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Írja be az **aktuális jelszót**.

3. **Új jelszó**megadását. A jelszó legalább 8 karakterből kell lennie. 3, 4, a következőket kell tartalmaznia: nagybetűsre, kisbetűsre, numerikus és különleges karaktereket.

    Figyelje meg, hogy a jelszó nem lehet ugyanaz, mint a legutóbbi 24 jelszavakat.

3. Írja be újra a jelszót újra.

    ![jelszó2 módosítása](./media/storsimple-ova-change-device-admin-password/image41.png)

4. A lap alján kattintson az **Alkalmaz**gombra. Az új jelszót majd kell alkalmazni. Ha a jelszó módosítása nem jár sikerrel, a következő hiba jelenik meg.

    ![jelszó hiba](./media/storsimple-ova-change-device-admin-password/image42.png)

    Miután sikeresen frissül a jelszót, akkor értesítést kap. Azután használhatja a módosított jelszó helyileg az eszköz eléréséhez.

## <a name="next-steps"></a>Következő lépések

További tudnivalók [a StorSimple virtuális tömb felügyelete](storsimple-ova-web-ui-admin.md).
