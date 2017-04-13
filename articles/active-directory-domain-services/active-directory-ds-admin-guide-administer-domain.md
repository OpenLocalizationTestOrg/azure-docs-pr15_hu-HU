<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Administer felügyelt tartomány |} Microsoft Azure"
    description="Felügyelt tartományok Azure Active Directory tartományi szolgáltatások felügyelete"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány felügyelete
Ez a cikk bemutatja, hogyan az Azure Active Directory (AD) felügyelt tartomány felügyeletére.


## <a name="before-you-begin"></a>Első lépések
A jelen cikkben felsorolt feladatok elvégzéséhez szükséges:

1. **Azure előfizetés**érvényes.

2. **Azure Active directory** - vagy szinkronizálja a helyszíni címtárhoz vagy a felhőben könyvtár.

3. **Azure Active Directory tartományi szolgáltatások** az Azure Active Directory engedélyezve kell lennie. Ha még nem tette, kövesse az [első lépések útmutató](./active-directory-ds-getting-started.md)című témakörben ismertetett feladatokat.

4. **Virtuális gép tartományhoz tartozó** , ahonnan az Azure Active Directory tartományi szolgáltatások felügyelete kezeli a tartomány. Ha nincs telepítve a virtuális géphez, kövesse az összes a [Bekapcsolódás egy felügyelt tartományra Windows virtuális gép](./active-directory-ds-admin-guide-join-windows-vm.md)című cikkben ismertetett műveletek.

5. A címtárban való felügyelt tartománya szüksége egy **felhasználói fiókot, a "AAD Adatközpont rendszergazdák" a csoportba tartozó** hitelesítő adatait.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Felügyeleti feladatokat végezheti el a felügyelt tartományban
A "AAD Adatközpont rendszergazdák" csoport tagjainak kapnak jogosultságokkal felügyelt tartományban, amely engedélyezi őket a feladatok elvégzésére:

- A felügyelt tartományhoz gépek csatlakoztatni.

- A "AADDC számítógépek" és "AADDC felhasználók" tárolók beépített csoportházirend-objektum beállítása a felügyelt tartományban.

- A felügyelt tartomány a DNS kezelése.

- Egyéni szervezeti egységeket felügyelt tartományban továbbépítheti, és hozzon létre.

- Nyereség számítógépek rendszergazdai hozzáférése a tartományhoz felügyelt.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>A felügyelt tartomány nem rendelkezik rendszergazdai jogosultságokkal
A Microsoft, ideértve például a javítási figyelés, és a biztonsági mentés tevékenységeket kezeli a tartomány. Ezért a tartomány zárolva van, és nem rendelkezik a tartomány bizonyos felügyeleti feladatok elvégzésére. Néhány példa a tevékenységek nem tudja elvégezni alatt találhatók.

- A felügyelt tartomány tartományi rendszergazdája vagy vállalati rendszergazdai jogokkal nem kapnak.

- A felügyelt tartomány séma nem bővítése.

- Távoli asztali változatában felügyelt tartományhoz tartozó domain vezérlők való kapcsolódást.

- A felügyelt tartomány tartomány vezérlőket nem lehet hozzáadni.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Tevékenység-1 - tartományhoz tartozó Windows Server virtuális gép távoli felügyelete a felügyelt tartomány kiépítése
Azure Active Directory tartományi szolgáltatások felügyelt tartományok is kezelhetők a már jól ismert az Active Directory felügyeleti eszközök, például az Active Directory felügyeleti központ (ADAC) vagy az Active Directory PowerShell használatával. Bérlői rendszergazdák nincs jogosultsága, tartomány vezérlők felügyelt tartományban távoli asztali keresztül csatlakozhat. Emiatt a "AAD Adatközpont rendszergazdák" csoport tagjainak felügyelheti a felügyelt tartományok távolról a tartományhoz a felügyelt Windows Server-ügyfél számítógépén AD a felügyeleti eszközök használatával. Active Directory felügyeleti eszközök telepíthető Windows Server és a felügyelt tartományhoz ügyfél gépek a távoli kiszolgáló felügyeleti eszközei (RSAT) választható funkció részeként.

Első lépésként állíthat be, hogy a tartományhoz a felügyelt Windows Server virtuális géphez. Útmutatásért olvassa el a [Csatlakozás a Windows Server virtuális gépek egy felügyelt Azure Active Directory tartományi szolgáltatások tartományt](active-directory-ds-admin-guide-join-windows-vm.md)című cikk.

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Távoli felügyelete a felügyelt tartomány egy ügyfélszámítógép (például a Windows 10)
Ez a cikk használata a Windows Server virtuális gép útmutatásának megfelelően a AAD DS felügyelete kezeli a tartomány. Azonban is megadhatja a Windows (például a Windows 10) virtuális ügyfélgép használni ehhez.

Azt is megteheti a [távoli kiszolgáló felügyeleti eszközei (RSAT) telepítése](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) Windows ügyfél virtuális gépre a TechNet webhely utasításait követve.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Tevékenység 2 – telepítés Active Directory felügyeleti eszközök a virtuális gépen
A következő lépésekkel telepítse az Active Directory felügyeleti eszközök a tartományhoz csatlakoztatott virtuális gépen. TechNeten talál további [információkat telepítésével és használatával a távoli kiszolgáló felügyeleti eszközei](https://technet.microsoft.com/library/hh831501.aspx).

1. Nyissa meg az klasszikus portálon Azure **virtuális gépeken futó** csomópontot. Jelölje ki a virtuális gép létrehozott tevékenység 1 és a parancssávon az ablak alján kattintson a **Csatlakozás** gombra.

    ![Csatlakozás Windows virtuális géphez](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. A klasszikus portál kéri, hogy a fájl megnyitását vagy mentését egy, a virtuális gépen való kapcsolódáshoz használt '.rdp' kiterjesztésű. Kattintson arra a fájlra, amikor a letöltés befejeződik.

3. A bejelentkezési parancssorba használja, a "AAD Adatközpont rendszergazdák" a csoportba tartozó felhasználó hitelesítő adatait. Ha például azt az 'bob@domainservicespreview.onmicrosoft.com' abban az esetben.

4. Nyissa meg a kezdőképernyőn **Kiszolgálókezelő**. Kattintson a **Szerepkörök hozzáadása és szolgáltatások** elemre a Kiszolgálókezelő ablakban központi ablaktáblájában.

    ![Indítsa el a Kiszolgálókezelő virtuális gépen](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. **Mielőtt elkezdené** lapján a **Szerepkörök hozzáadása és szolgáltatások varázslót**kattintson a **Tovább**gombra.

    ![Mielőtt elkezdené lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. A **Telepítési típus** lapon hagyja a **szerepköralapú vagy a szolgáltatás-alapú telepítés** választógombot bejelölve, és kattintson a **Tovább**gombra.

    ![Telepítési típus lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. **Kiszolgáló kiválasztása** lapon válassza ki az aktuális virtuális gép a kiszolgáló készletből, és kattintson a **Tovább**gombra.

    ![Kiszolgáló kiválasztása lapon](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. A **Kiszolgálói szerepkörök** lapon kattintson a **Tovább**gombra. Ez a lap azt átugrása, mivel azt bármilyen szerepkörök nem telepíti a kiszolgálón.

9. A **szolgáltatások** lapon kattintással bontsa ki a **Távoli kiszolgáló felügyeleti eszközei** csomópontot, és kattintson a bontsa ki a **Szerepkör-felügyeleti eszközök** csomópontot gombra. Jelölje ki az **Active Directory tartományi szolgáltatások és eszközök Directory** szolgáltatás szerepkör-felügyeleti eszközök listája.

    ![Szolgáltatások lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. A **Megerősítés** lapon kattintson a **telepítés** és telepítése az Active Directory és a Directory eszközök szolgáltatás a virtuális gépen. Szolgáltatás telepítése befejeződik, kattintson a **Bezárás** gombra a **Szerepkörök hozzáadása és szolgáltatások** kilépéshez.

    ![Megerősítés lapon](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>3 – tevékenység csatlakozzon, és ismerje meg a felügyelt tartomány
Most, hogy az Active Directory felügyeleti eszközök telepítve van a tartomány virtuális gép csatlakozott, azt ezeket az eszközöket, hogy feltárása és a felügyelt tartomány felügyelete.

> [AZURE.NOTE] A felügyelt tartomány felügyelheti a "AAD Adatközpont rendszergazdák" csoport tagjának kell szüksége.

1. A kezdőképernyőn kattintson a **Felügyeleti eszközök**elemre. Meg kell jelennie virtuális a gépre van telepítve az Active Directory felügyeleti eszközei.

    ![Telepítve van a kiszolgáló felügyeleti eszközei](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Kattintson **az Active Directory felügyeleti központ**elemre.

    ![Active Directory felügyeleti központ](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Ismerje meg a tartomány, kattintson a tartomány nevét, a bal oldali ablaktáblában (például contoso100.com). Figyelje meg a "AADDC számítógépek" és "AADDC felhasználók" nevű rendre két tárolók.

    ![ADAC - nézet tartomány](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. A tároló nevű **AADDC felhasználók** összes felhasználók és csoportok felügyelt tartományhoz tartozó gombra. Megjelenik a felhasználói fiókok és csoportokat közvetlenül az Azure Active Directory bérlői megjelenítése be a tárolót. Ebben a példában az értesítés, egy felhasználói fiókot, a felhasználó neve "Péter" és "AAD Adatközpont rendszergazdák" nevű csoportot a tárolóban érhetők el.

    ![ADAC - tartomány felhasználók](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Lásd: a számítógépek, a felügyelt tartományhoz **AADDC számítógép** neve a tároló gombra. Meg kell jelennie egy bejegyzést a tartományhoz kapcsolódó aktuális virtuális gép. Az összes számítógép tartományhoz az Azure Active Directory tartományi szolgáltatások felügyelt fiókok ebben a "AADDC számítógépek" tárolóban tárolja.

    ![ADAC - tartományt az illesztés számítógépek](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Kapcsolódó tartalom

- [Azure Active Directory tartományi szolgáltatások – első lépések útmutató](./active-directory-ds-getting-started.md)

- [A Windows Server virtuális gép vegye fel az Azure Active Directory tartományi szolgáltatások felügyelt tartományba](active-directory-ds-admin-guide-join-windows-vm.md)

- [Távoli kiszolgáló felügyeleti eszközei terjesztése](https://technet.microsoft.com/library/hh831501.aspx)
