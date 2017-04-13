<properties 
   pageTitle="Lépjen kapcsolatba a Microsoft támogatási |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy támogatási kérelmet, és támogatási indítása StorSimple eszközén."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="contact-microsoft-support"></a>Lépjen kapcsolatba a Microsoft terméktámogatás

Ha a Microsoft Azure StorSimple megoldás az esetleges problémák, létrehozhat egy szolgáltatási kérelmet a technikai támogatási. Online munkamenetben és a támogatási adatbázismodellbe is szükség lehet támogatási indítása StorSimple eszközén. Ez a cikk bemutatja az:

- Hogyan lehet létrehozni egy támogatási kérelmet.
- Hogyan lehet támogatási indítása a Windows PowerShell felületén a StorSimple eszköz.

Tekintse át a [StorSimple 8000 sorozat támogatási SLA és információt](https://msdn.microsoft.com/library/mt433077.aspx) egy támogatási kérelmet létrehozása előtt.

## <a name="create-a-support-request"></a>Támogatási kérelem létrehozása

Végezze el a támogatási összehívást szeretne létrehozni az alábbi lépéseket:

#### <a name="to-create-a-support-request"></a>Támogatási kérelem létrehozása

1. Az [Azure klasszikus portál](https://manage.windowsazure.com/)jobb felső sarokban kattintson a fiók nevére, és kattintson a **Kapcsolatfelvétel a Microsoft támogatási szolgálattal**.

    ![Partner MS-támogatás ManagementPortal keresztül](./media/storsimple-contact-microsoft-support/Ibiza1.png)

2. Az új Azure-portálra (portal.azure.com) irányítja. Kattintson az **új támogatási kérelem** csempére.

    ![Partner MS-támogatás új portálon keresztül](./media/storsimple-contact-microsoft-support/Ibiza2.png)

    A képernyő jobb oldalán megjelenik az **új támogatási kérelem** ablaktábla. 

    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza3a.png)

3. **Alapvető** párbeszédpanelen végezze el a következőket:                                
    1. A **probléma típusa** legördülő listában válassza a **technikai**.
    2. Jelölje ki az **előfizetés** a legördülő listából.
    3. A **szolgáltatás** legördülő listából válassza ki a **StorSimple**. 
    4. A legördülő listából válassza ki a **támogatási csomagot** . Térítéses támogatás tervezi engedélyezése a technikai támogatási van szüksége.

4. Kattintson a **Tovább**gombra. A **probléma** párbeszédpanel jelenik meg.

    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 

5. **A probléma** párbeszédpanelen végezze el a következőket:

    1.  Jelölje be a **szinttel** a legördülő listából.
    2.  A **probléma típus** válassza a legördülő listából.
    3.  **Kategória** válassza a legördülő listából. 
    4.  A **Részletek** mezőben látható röviden jellemzi a problémát.
    5.  A **időkereten** mezőbe azt jelzik, a dátum, idő és időzóna, amely megfelel a legutóbbi előfordulás a problémát.
    6.  A **fájl feltöltése**kattintson a mappaikonra, tallózással keresse meg a támogatási csomag.
    7.  Jelölje be a **diagnosztikai információkat oszthat meg** jelölőnégyzetet.

6. Kattintson a **Tovább**gombra. A **kapcsolattartási adatok** párbeszédpanel jelenik meg.

    ![Új támogatási kérelem ablaktábla](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 

7. Adja meg az elérhetőségi adatait, és válassza ki a kapcsolatfelvételi módot (telefon vagy e-mailek). 

8. Jelölje be a **jövőbeli támogatási kérelmek kapcsolattartó változtatások mentése** jelölőnégyzetet.

9. Kattintson a **létrehozása**gombra.

Miután elküldte a kérést, támogatási szakértőtől kapcsolatba, amint lehetséges, hogy a kérelem folytatásához.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Támogatás munkamenet indítása a Windows PowerShell-StorSimple

A StorSimple eszközzel esetleg fellépő esetleges problémák elhárításához szüksége lesz a Microsoft Support csoporttal végzését. Microsoft Support szükség lehet a támogatási munkamenet használatával jelentkezzen be az eszközt. 

A következő lépésekkel támogatási indítása:

#### <a name="to-start-a-support-session"></a>A támogatási munkamenet indítása

1. Az eszköz elérheti, közvetlenül a soros konzol használatával, illetve távoli számítógépről telnet munkamenet keresztül. Ehhez hajtsa végre az [Használata gitt való csatlakozáshoz a soros eszköz-konzol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)című témakör lépéseit.

2. Megnyílik a munkamenetben nyomja le az **Enter** billentyű lenyomásával beszerzése a parancssor parancsot.

3. A soros konzol menüben beállítással 1, **Jelentkezzen be a teljes hozzáférést biztosít**.

4. A parancssorba írja be az alábbi jelszó: 

    `Password1`

5. A parancssorba írja be a következő parancsot:

    `Enable-HcsSupportAccess`

6. Egy titkosított karakterlánc, be kell mutatni. Ez a karakterlánc másol egy szövegszerkesztőben, például a Jegyzettömbben.

7. Mentse a karakterlánc, és az e-mailt küldeni a Microsoft Support. 

> [AZURE.IMPORTANT] Támogatás az access letilthatja az operációs rendszert futtató `Disable-HcsSupportAccess`. A StorSimple eszköz megkísérli is támogatás hozzáférés letiltása a 8 órát után a munkamenet kezdeményeztek. Célszerű a legjobb módszer a StorSimple eszköz hitelesítő adatok módosítása után a támogatási munkamenet indítása gombra.
