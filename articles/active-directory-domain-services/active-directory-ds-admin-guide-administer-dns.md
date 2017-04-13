<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Administer felügyelt tartományok DNS |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások felügyelt domains a DNS kezelése"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány a DNS kezelése
Azure Active Directory tartományi szolgáltatások, ahol a DNS-feloldási felügyelt tartományhoz tartozó DNS-(Domain névfeloldás) kiszolgáló tartalmazza. Időnként előfordulhat konfigurálja a DNS-felügyelt tartományban. Előfordulhat, hogy gépekhez, amely nem csatlakozik a tartomány DNS-rekordok létrehozása, terheléselosztókkal virtuális IP-címek konfigurálása és a külső DNS-továbbítókat beállítása. Emiatt a "AAD Adatközpont rendszergazdák" csoportba tartozó felhasználók a felügyelt tartomány DNS felügyeleti jogosultságait kell megadni.


## <a name="before-you-begin"></a>Első lépések
A jelen cikkben felsorolt feladatok elvégzéséhez szükséges:

1. **Azure előfizetés**érvényes.

2. **Azure Active directory** - vagy szinkronizálja a helyszíni címtárhoz vagy a felhőben könyvtár.

3. **Azure Active Directory tartományi szolgáltatások** az Azure Active Directory engedélyezve kell lennie. Ha még nem tette, kövesse az [első lépések útmutató](./active-directory-ds-getting-started.md)című témakörben ismertetett feladatokat.

4. **Virtuális gép tartományhoz tartozó** , ahonnan az Azure Active Directory tartományi szolgáltatások felügyelete kezeli a tartomány. Ha nincs telepítve a virtuális géphez, kövesse az összes a [Bekapcsolódás egy felügyelt tartományra Windows virtuális gép](./active-directory-ds-admin-guide-join-windows-vm.md)című cikkben ismertetett műveletek.

5. A címtárban való felügyelt tartománya DNS szüksége egy **felhasználói fiókot, a "AAD Adatközpont rendszergazdák" a csoportba tartozó** hitelesítő adatait.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Tevékenység 1 - kiépítése a tartományhoz tartozó virtuális gép felügyelt tartományhoz tartozó DNS távoli kezelésére
Azure Active Directory tartományi szolgáltatások felügyelt tartományok távolról a már jól ismert az Active Directory felügyeleti eszközök, például az Active Directory felügyeleti központ (ADAC) vagy az Active Directory PowerShell használatával is kezelhetők. Hasonlóképpen felügyelt tartományhoz tartozó DNS használatával lehet felügyelni távolról a DNS-kiszolgáló felügyeleti eszközei.

Az Azure Active directory rendszergazdái nincs jogosultsága, tartomány vezérlők felügyelt tartományban távoli asztali keresztül csatlakozhat. A "AAD Adatközpont rendszergazdák" csoport tagjainak DNS felügyelheti felügyelt tartományok távolról a tartományhoz a felügyelt Windows Server-ügyfél számítógépén a DNS-kiszolgáló eszközök segítségével. DNS-kiszolgáló eszközök telepíthető Windows Server és a felügyelt tartományhoz ügyfél gépek a távoli kiszolgáló felügyeleti eszközei (RSAT) választható funkció részeként.

Az első tevékenység hozhatók létre, amely a felügyelt tartományhoz csatlakozik a Windows Server virtuális gép. Útmutatásért olvassa el a [Csatlakozás a Windows Server virtuális gépek egy felügyelt Azure Active Directory tartományi szolgáltatások tartományt](active-directory-ds-admin-guide-join-windows-vm.md)című cikk.


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Tevékenység 2 – telepítés DNS-kiszolgáló eszközök a virtuális gépen
Hajtsa végre az alábbi lépéseket a DNS-felügyeleti eszközök telepítése a tartományhoz csatlakoztatott virtuális gépen. [Telepítése, és a távoli kiszolgáló felügyeleti eszközei használatával](https://technet.microsoft.com/library/hh831501.aspx)kapcsolatos további tudnivalókért lásd: a TechNet webhelyen.

1. Nyissa meg az klasszikus portálon Azure **virtuális gépeken futó** csomópontot. Jelölje ki a virtuális gép létrehozott tevékenység 1 és a parancssávon az ablak alján kattintson a **Csatlakozás** gombra.

    ![Csatlakozás Windows virtuális géphez](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. A klasszikus portál kéri, hogy a fájl megnyitását vagy mentését egy, a virtuális gépen való kapcsolódáshoz használt '.rdp' kiterjesztésű. Kattintson a fájlra, amikor a letöltés befejeződik.

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

9. A **szolgáltatások** lapon kattintással bontsa ki a **Távoli kiszolgáló felügyeleti eszközei** csomópontot, és kattintson a bontsa ki a **Szerepkör-felügyeleti eszközök** csomópontot gombra. Jelölje ki a **DNS-kiszolgálói eszközök** szolgáltatás szerepkör-felügyeleti eszközök listája.

    ![Szolgáltatások lap](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. A **Megerősítés** lapon kattintson a **telepítse** a DNS-kiszolgáló eszközök szolgáltatás telepítése a virtuális gépen. Szolgáltatás telepítése befejeződik, kattintson a **Bezárás** gombra a **Szerepkörök hozzáadása és szolgáltatások** kilépéshez.

    ![Megerősítés lapon](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Tevékenység-3 - indítsa el a DNS management console DNS felügyelete
Most, hogy a DNS-kiszolgálói eszközök szolgáltatás telepítve van a tartomány virtuális gép csatlakozott, azt a DNS-eszközök, hogy a felügyelt tartomány a DNS kezelése.

> [AZURE.NOTE] A felügyelt tartomány a DNS kezelése a "AAD Adatközpont rendszergazdák" csoport tagja szüksége.

1. A kezdőképernyőn kattintson a **Felügyeleti eszközök**elemre. Meg kell jelennie a **DNS-** konzolba virtuális a gépre van telepítve.

    ![Felügyeleti eszközök – a DNS-konzolba](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Kattintson a **DNS** a DNS Management console indítása parancsra.

3. **Kapcsolódás DNS-kiszolgálóhoz** párbeszédpanelen **a következő számítógépen**című lehetőségre, és írja be a DNS-tartománynév felügyelt tartomány (például contoso100.com).

    ![DNS-konzolba - tartomány csatlakoztatása](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. A felügyelt tartomány a DNS-konzolba csatlakozik.

    ![DNS-konzolba - tartomány felügyelete](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. A DNS-konzolba segítségével számítógépek belül a virtuális hálózat, amelyben engedélyezte AAD tartományi szolgáltatások DNS-bejegyzések hozzáadása gombra.

> [AZURE.WARNING] Ügyeljen arra, hogy ha a DNS felügyelete a felügyelt tartomány DNS-felügyeleti eszközök segítségével. Győződjön meg arról, hogy Ön **nem törlése, illetve a beépített használt tartomány szolgáltatásai a tartomány DNS-rekordok módosítását**. Beépített DNS-rekordokat a DNS-rekordjait, névkiszolgáló-rekordok és más rekordok Adatközpont hely használt tartalmazza. Ha ezek a rekordok módosítását, megszakadnak tartományi szolgáltatások, a virtuális hálózaton.


Lásd: a DNS kezelésével kapcsolatban további tudnivalókat [a TechNet webhely a DNS-eszközök cikk](https://technet.microsoft.com/library/cc753579.aspx) .


## <a name="related-content"></a>Kapcsolódó tartalom

- [Azure Active Directory tartományi szolgáltatások – első lépések útmutató](./active-directory-ds-getting-started.md)

- [A Windows Server virtuális gép vegye fel az Azure Active Directory tartományi szolgáltatások felügyelt tartományba](active-directory-ds-admin-guide-join-windows-vm.md)

- [Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)

- [A DNS-felügyeleti eszközök](https://technet.microsoft.com/library/cc753579.aspx)
