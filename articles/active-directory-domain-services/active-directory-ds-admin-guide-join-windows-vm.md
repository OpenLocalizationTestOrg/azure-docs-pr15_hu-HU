<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Vegye fel a Windows Server virtuális egy felügyelt tartományba |} Microsoft Azure"
    description="A Windows Server virtuális gép csatlakozzon az Azure Active Directory tartományi szolgáltatások"
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

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Vegye fel a Windows Server virtuális gép egy felügyelt tartományba

> [AZURE.SELECTOR]
- [Azure klasszikus portál – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [A PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

Ez a cikk bemutatja, hogyan be szeretne kapcsolódni egy virtuális gép fut a Windows Server 2012 R2 az Azure Active Directory tartományi szolgáltatások felügyelt tartományhoz az Azure klasszikus portálon.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Lépés: 1: A Windows Server virtuális gép létrehozása
Kövesse az utasításokat a [virtuális gép létrehozása az Azure klasszikus portálon Windows rendszerű](../virtual-machines/virtual-machines-windows-classic-tutorial.md) oktatóprogram során tagolt. Fontos, hogy az újonnan létrehozott virtuális gép Azure Active Directory tartományi szolgáltatások engedélyezve ugyanahhoz a virtuális hálózathoz csatlakozik ahhoz. A gyors létrehozása beállítás nem engedélyezi a csatlakozás a virtuális gép virtuális hálózathoz. Ezért akkor használja a "A gyűjtemény" beállítás a virtuális gép létrehozása.

A következő lépésekkel hozhat létre, amelyben az Azure Active Directory tartományi szolgáltatások engedélyezte a virtuális hálózathoz csatlakozik a Windows virtuális géphez.

1. Az Azure klasszikus portálra a parancssávon a ablak alján kattintson az **Új**gombra.

2. Alapján **számítja ki**akkor kattintson a **virtuális gép**, majd **A gyűjtemény**.

3. Első képernyőjén segítségével **Válassza ki a képet** a virtuális gép elérhető képek a listából. Válassza ki a megfelelő képet.

    ![Jelölje ki a kép](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. A második képernyő segítségével válassza ki a számítógép nevét, méretét, és felügyeleti felhasználónév és jelszó. Használja a réteg és az alkalmazás vagy terhelést futtatásához szükséges méretét. A felhasználó nevét, válassza a Itt a számítógépen egy helyi rendszergazdai felhasználói. Írja be ide a tartományi fiók hitelesítő adatait.

    ![Virtuális gép konfigurálása](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. A harmadik képernyő lehetővé teszi a hálózat, tárolására és elérhetőség erőforrások konfigurálása. Győződjön meg arról, jelölje ki a virtuális hálózat, amelyben engedélyezett a Azure Active Directory tartományi szolgáltatások, a **Régió/affinitás csoport/virtuális hálózati** legördülő listából. A virtuális gép értelemszerűen adja meg a egy **Felhőalapú szolgáltatás DNS-nevét** .

    ![Jelölje ki a virtuális gép virtuális hálózati](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Győződjön meg arról, hogy a virtuális gép, amelyben az Azure Active Directory tartományi szolgáltatások engedélyezte virtuális hálózatához csatlakozik. A virtuális gép eredményeként "Lásd" a tartományt, és a feladatokat, például a tartományhoz. Ha úgy dönt, hogy egy másik virtuális hálózaton a virtuális gép létrehozása, Csatlakozás virtuális hálózat, amelyben az Azure Active Directory tartományi szolgáltatások engedélyezte a virtuális hálózat.

6. A negyedik képernyő teszi lehetővé a virtuális Agent telepítse és állítsa be, a rendelkezésre álló bővítmények részét.

    ![Kész](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. A virtuális gép létrehozását követően a a klasszikus portál sorolja fel az új virtuális gép csoportban a **virtuális gépeken futó** csomópontot. A virtuális gép és felhőbeli szolgáltatástól automatikusan indított, mind **operációs rendszert futtató**néven jelenik meg a állapotát.

    ![Virtuális gép már működik](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Lépés: 2: A Windows Server virtuális gépet használ, a helyi rendszergazdafiók csatlakoztatása
Most azt csatlakozni az újonnan létrehozott Windows Server virtuális gép, a tartomány csatlakozni. Használja a virtuális gép való létrehozásakor meghatározott helyi rendszergazdai hitelesítő adatait.

Hajtsa végre az alábbi lépésekkel csatlakozhat a virtuális gépen.

1. Nyissa meg a klasszikus portálon **virtuális gépeken futó** csomópontot. Jelölje ki a virtuális gépen, 1 lépésben a létrehozott, és a parancssávon az ablak alján kattintson a **Csatlakozás** gombra.

    ![Csatlakozás Windows virtuális géphez](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. A klasszikus portál kéri, hogy a fájl megnyitását vagy mentését egy, a virtuális gépen való kapcsolódáshoz használt '.rdp' kiterjesztésű. Kattintson arra a fájlra, amikor a letöltés befejeződik.

3. Jelentkezzen be a parancssorba írja be a **rendszergazdai hitelesítő adataival**, amely a virtuális gép létrehozásakor megadott. Ha például "localhost\mahesh" használtuk ebben a példában.

Ekkor meg kell kell bejelentkeznie az újonnan létrehozott Windows virtuális géphez helyi rendszergazdai hitelesítő adataival. A következő lépésként vegye fel a virtuális gép a tartományba.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Lépés 3: A Windows Server virtuális gépen AAD-tartományi felügyelt csatlakozás
Hajtsa végre az alábbi lépéseket a Windows Server virtuális gép AAD-tartományi felügyelt csatlakoztatni.

1. A Windows Server kapcsolódni a 2 látható módon. Nyissa meg a kezdőképernyőn **Kiszolgálókezelő**.

2. A kiszolgáló Manager ablak bal oldali ablaktáblájában kattintson a **Helyi kiszolgálóra** .

    ![Indítsa el a Kiszolgálókezelő virtuális gépen](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. A **Tulajdonságok** csoportban kattintson a **munkacsoport** . A **Rendszertulajdonságok** tulajdonság lapon kattintson a **Módosítás** a tartományhoz.

    ![Rendszerlap tulajdonságai](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Adja meg a tartomány nevét a Azure Active Directory tartományi szolgáltatások felügyelt az a **tartomány** mezőben lévő értéket tartományát, és kattintson az **OK gombra**.

    ![Adja meg a tartományhoz a tartomány](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. Adja meg a hitelesítő adatait, és a tartományhoz kéri. Győződjön meg arról, hogy akkor **Adja meg a felhasználó a AAD Adatközpont rendszergazdáknak tartozó hitelesítő adatok** csoport. Csak a csoport tagjai gépek vegye fel a felügyelt tartományba jogosultsága.

    ![Adja meg a hitelesítő adatok tartomány illesztés](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. Hitelesítő adatok az alábbi módszerek egyikével adhatók meg:

    - Egyszerű Felhasználónévi formázása: Adja meg a felhasználói fiók UPN-utótag beállítva az Azure Active Directory. Ebben a példában a felhasználó "Péter" UPN-utótag van 'bob@domainservicespreview.onmicrosoft.com'.

    - SAMAccountName formátum: az SAMAccountName formátum adhatja meg a fiók nevét. Ebben a példában a felhasználó "Péter" kellene "CONTOSO100\bob" adja meg.

        > [AZURE.NOTE] **Azt javasoljuk, hogy az egyszerű Felhasználónévi formátum használata a hitelesítő adatait.** Az SAMAccountName is lehet automatikusan létrehozott Ha a felhasználó egyszerű Felhasználónévi előtag túl hosszú (például az "joereallylongnameuser"). Ha több felhasználó van a Azure AD-bérlő azonos UPN előtaggal (például "Péter"), a SAMAccountName formátum lehetnek a szolgáltatás által automatikusan létrehozott. Ezekben az esetekben az egyszerű Felhasználónévi formátum használható biztos, hogy jelentkezzen be a tartományt.

7. Után tartomány illesztés sikeres, megjelenik a következő üzenet üdvözli meg a tartományhoz. Indítsa újra a virtuális gép a tartomány join művelet elvégzéséhez.

    ![Üdvözli a tartomány](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Tartomány illesztés – hibaelhárítás
### <a name="connectivity-issues"></a>Kapcsolódási problémák
Ha a virtuális gép nem tudja a tartomány keresése, olvassa el az alábbi hibaelhárítási lépéseket:

- Győződjön meg arról, hogy a a virtuális gép csatlakoztatva van-e, mint a virtuális hálózatához engedélyezte a tartományi szolgáltatások. Ha nem, akkor a virtuális gép nem tud csatlakozni a tartomány, és ezért nem tudja a tartományhoz.

- Ha a virtuális gép egy másik virtuális hálózathoz csatlakozik, győződjön meg arról, hogy virtuális hálózat, amelyben a tartományi szolgáltatások engedélyezte a virtuális hálózathoz csatlakoztatva van-e.

- Próbálja meg a ping a tartomány felügyelt tartomány (például: "ping contoso100.com") a tartomány nevét használja. Ha nem sikerült, próbálja meg a ping az IP-címek tartomány megjelenik a lapon, ha engedélyezve van az Azure Active Directory tartományi szolgáltatások (például "küldjön ping parancsot 10.0.0.4"). Ha tudja a ping az IP-cím, de nem a tartomány, DNS előfordulhat, hogy megfelelően konfigurálva. Előfordulhat, hogy nem konfigurálta az IP-címek a tartomány DNS-kiszolgálók virtuális hálózatok.

- Próbálja ki a virtuális gépen ("ipconfig/flushdns"), a DNS-feloldó gyorsítótárának kiürítése.

Ha a párbeszédpanelt, amely rákérdez a hitelesítő adatait, és a tartományhoz tartozó, nem rendelkezik a csatlakozási problémákat tapasztal.


### <a name="credentials-related-issues"></a>Hitelesítő adatokkal kapcsolatos problémák
Ha problémái vannak a hitelesítő adatait, és nem tudja a tartományhoz olvassa el az alábbi lépéseket.

- Próbálja meg az egyszerű Felhasználónévi formátum használata a hitelesítő adatait. A fiók az SAMAccountName lehet, automatikusan létrehozott a bérlő azonos UPN előtaggal több felhasználó esetén, vagy ha az egyszerű Felhasználónévi előtag túl hosszú. Emiatt a fiók SAMAccountName formátumának eltérhet azt számíthat, vagy használja a helyszíni tartomány.

- Próbálja meg használni egy felhasználói fiókot, a "AAD Adatközpont rendszergazdák" csoport gépek csatlakozik a felügyelt tartományhoz tartozó hitelesítő adatait.

- Ellenőrizze, hogy a [Jelszó-szinkronizálás engedélyezve](active-directory-ds-getting-started-password-sync.md) megfelelően a az első lépések útmutató című témakörben ismertetett lépéseket.

- Győződjön meg róla, a felhasználó (UPN) használja az Azure Active Directory beállítva (például 'bob@domainservicespreview.onmicrosoft.com') bejelentkezni.

- Győződjön meg arról, hogy meg van várta jelszó-szinkronizálás tartaniuk a az első lépések útmutató elvégzéséhez.


## <a name="related-content"></a>Kapcsolódó tartalom

- [Azure Active Directory tartományi szolgáltatások – első lépések útmutató](./active-directory-ds-getting-started.md)

- [Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány felügyelete](./active-directory-ds-admin-guide-administer-domain.md)
