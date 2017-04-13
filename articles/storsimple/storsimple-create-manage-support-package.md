<properties
   pageTitle="Hozzon létre egy StorSimple támogatási csomagot |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre, visszafejtése és szerkesztése a StorSimple eszközökre készült támogatási csomagot."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Létrehozása és kezelése egy StorSimple támogatási csomag

## <a name="overview"></a>– Áttekintés

StorSimple támogatási csomag egy könnyen használható mechanizmusa, amely az összes kapcsolódó naplók bármilyen StorSimple eszköz problémáinak megkönnyítése Microsoft Support gyűjti össze. Az összegyűjtött naplók titkosított és tömörítve.

Ebben az oktatóanyagban kell létrehozni vagy kezelni a támogatási csomag lépésenkénti útmutatást tartalmaz.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Hozzon létre és támogatási csomag az Azure klasszikus portálon feltöltése

Hozzon létre, és töltse fel támogatási csomagot a Microsoft Support webhelyre a **karbantartása** lap a szolgáltatás az Azure klasszikus portálon keresztül.

> [AZURE.NOTE] A feltöltés támogatási hitelesítő szükséges. A támogatás adatbázismodellbe biztosítania kell ez Önnek e-mailben.

Egy titkosított és tömörített támogatási csomag (.cab fájl) létrejön, és töltenek fel a támogatási webhelyén. A támogatás adatbázismodellbe majd beolvasható ebben a csomagban a támogatási webhelyén, az alábbiakkal a problémát.

A következő lépésekkel a támogatás csomag létrehozása klasszikus portálon.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Támogatás csomag létrehozása az Azure klasszikus portálon

1. Jelölje ki a **eszközöket** > **karbantartási**.

2. A **csomag támogatás** szakaszában kattintson a **létrehozása és feltöltése támogatási csomag**.

3. **Támogatás-csomag létrehozása és feltöltése** párbeszédpanelen tegye a következőket:

    ![Támogatás csomag létrehozása](./media/storsimple-create-manage-support-package/IC740923.png)

    - A **Támogatás hitelesítő** szöveg mezőbe írja be a hozzáférési kódot. A Microsoft támogatási adatbázismodellbe küldjön e hitelesítő Önnek e-mailben.

    - Jelölje ki a jelölőnégyzet bejelölésével adja meg a felhasználó hozzájárul ahhoz automatikus feltöltéséhez a támogatási csomagot a Microsoft Support webhelyet.

    - Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>A támogatás csomag létrehozása manuálisan

Egyes esetekben kell létrehozása manuálisan StorSimple a támogatási csomagot Windows Powershellen keresztül. Példa:

- Ha szeretne eltávolítani a naplófájlok előtt megosztása a Microsoft Support bizalmas adatokat.

- Ha problémái vannak a való kapcsolódási problémák miatt csomag feltöltése.

A kézzel létrehozott támogatási csomag megoszthatja Microsoft Support, e-mailek fölé. A következő lépésekkel támogatási csomagot készíthet a Windows PowerShellben StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Támogatás csomag létrehozása a Windows PowerShell-StorSimple

1. Indítsa el a Windows PowerShell-munkamenetet a távoli számítógépen való csatlakozás StorSimple eszközére használt rendszergazdaként, írja be a következő parancsot:

    `Start PowerShell`

2. A Windows PowerShell-munkamenetet a csatlakozás a SSAdmin konzol az eszköz:

    - A parancssorba írja be:

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. A párbeszédpanelen adja meg a eszközt rendszergazdai jelszavát. Az alapértelmezett jelszava:

        `Password1`

        ![A PowerShell hitelesítő adatok párbeszédpanelen](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Kattintson **az OK gombra**.
    1. A parancssorba írja be:

        `Enter-PSSession $MS`

3. A megnyíló munkamenetet írja be a megfelelő parancsot.

    - Jelszóval védett hálózati megosztás írja be:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        Kéri a jelszót, a hálózaton megosztott mappára, és egy titkosító jelszó elérési útját (azért, mert a támogatási csomag titkosítva van). A támogatási csomag majd jön létre a mappához.

    - Amelyek nem jelszóval védett megosztások szükségtelen a `-Credential` paraméter. Adja meg az alábbiakat:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        A támogatás csomag mindkét vezérlők a megadott hálózati megosztott mappában jön létre. Érdemes lehet küldeni a Microsoft Support hibaelhárítási egy titkosított, tömörített fájltípust. További tudnivalókért lásd: [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>A Export-HcsSupportPackage parancsmag paraméterek
A következő paraméterek Export-HcsSupportPackage parancsmag használhatja.

| Paraméter            | Kötelező és választható | Leírás                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Szükséges          | Adja meg a megosztott hálózati mappát, amelyben a támogatási csomag kerül helyét használatával.                                                                 |
| `-EncryptionPassphrase` | Szükséges          | Adja meg a segítséget a támogatási csomag titkosítása jelszó használatával.                                                                                                        |
| `-Credential`           | Nem kötelező.          | A hálózaton megosztott mappához a hozzáférési hitelesítő adatok megadására használható.                                                                                        |
| `-Force`                | Nem kötelező.          | Használatával a titkosítási a jelszó megerősítése hagyja.                                                                                                                |
| `-PackageTag`           | Nem kötelező.          | Segítségével megadhatja egy bejegyzésnél *elérési útját* , amelyben a támogatási csomag kerül. Az alapértelmezett érték [eszköz neve]-[aktuális dátum és time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Nem kötelező.          | Adja meg a két vezérlők **fürt** (alapértelmezett) támogatási csomag létrehozása. Ha csak az aktuális vezérlő csomag létrehozása szeretne, adja meg a **vezérlő**. |


## <a name="edit-a-support-package"></a>A támogatás csomag szerkesztése

Támogatási csomag hozott létre, miután szükség lehet szerkeszteni a csomagot, ha el szeretné távolítani a bizalmas információkat. Ez is elhelyezhet a mennyiségi nevek eszköz IP-címek és a naplófájlból biztonsági másolat nevét.

> [AZURE.IMPORTANT] Csak akkor szerkeszthető támogatási csomag StorSimple Windows Powershellen keresztül hozott létre. Az Azure klasszikus portálon, kijelölt StorSimple kezelő szolgáltatás létrehozott csomag nem szerkeszthetők.

Támogatási csomag feltöltése a a Microsoft Support webhelyen előtt szerkesztéséhez először visszafejtése a támogatási csomag, a fájlok szerkesztése és újbóli titkosítsa a azt. Hajtsa végre az alábbi lépéseket.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Módosíthatja a Windows PowerShellben támogatási csomag StorSimple

1. A támogatás csomag készítése a leírtak szerint, [a Windows PowerShell-StorSimple támogatási csomag létrehozása](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. [Töltse le a parancsfájl](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) helyileg az az ügyfélnek.

3. Importálás a Windows PowerShell-modult. Adja meg, hogy a parancsprogram letöltötte, amelyben a helyi mappa elérési útját. A modul importálása, írja be:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Az összes fájlok *.aes* fájlok, amelyeket a tömörített és titkosított. Kibontásához visszafejteni fájlokat, és írja be:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Figyelje meg, hogy a tényleges fájlkiterjesztések jelennek meg az összes fájlt.

    ![Támogatás csomag szerkesztése](./media/storsimple-create-manage-support-package/IC750706.png)

5. Ha a titkosítási jelszó-rákérdez, adja meg a támogatási csomag létrehozásakor használt jelszó.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Nyissa meg azt a mappát, amely tartalmazza a naplófájlok. A naplófájlok most kitömörítés és visszafejteni, mivel ezek fog rendelkezni az eredeti fájlkiterjesztések. Módosítsa a bármilyen ügyfél adatait, például a mennyiségi és eszköz IP-címek, távolítsa el ezeket a fájlokat, és menti a fájlokat.

7. Zárja be a gzip tömörítése őket, és a AES-256 titkosítani a fájlokat. Ez a sebességétől és a biztonság a támogatási csomag átvitele hálózaton keresztül. Tömörítése titkosítani a fájlokat, és írja be a következőt:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Támogatás csomag szerkesztése](./media/storsimple-create-manage-support-package/IC750707.png)

8. Amikor a rendszer kéri, adja meg egy titkosító jelszó a módosított támogatási csomag.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Jegyezze fel az új jelszó, így megoszthat Microsoft Support kérésekor.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Példa: A támogatási csomag egy jelszóval védett megosztott fájlok szerkesztése

A következő példa bemutatja, hogyan visszafejtése, szerkesztése és újbóli titkosítása támogatási csomag.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Következő lépések

- További [támogatási csomag és eszköz naplók eszköz telepítéssel kapcsolatos hibák elhárítása](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting)használatával.

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.
