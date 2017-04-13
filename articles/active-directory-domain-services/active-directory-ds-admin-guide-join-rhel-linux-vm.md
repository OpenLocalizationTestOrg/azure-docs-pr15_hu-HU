<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Csatlakozás egy RHEL virtuális egy felügyelt tartományra |} Microsoft Azure"
    description="Bekapcsolódás egy piros kalap vállalati Linux virtuális gép az Azure Active Directory tartományi szolgáltatások"
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

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Vegye fel a piros kalap vállalati Linux 7 virtuális gép egy felügyelt tartományba
Ez a cikk bemutatja, hogyan be szeretne kapcsolódni egy piros kalap vállalati Linux (RHEL) 7 virtuális gép az Azure Active Directory tartományi szolgáltatások felügyelt tartományhoz.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>A piros kalap vállalati Linux virtuális gép kiépítése
Hajtsa végre az alábbi lépéseket az Azure portálon RHEL 7 virtuális gép hozhatók létre.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

    ![Azure portál irányítópult](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. A bal oldali ablaktáblában kattintson az **Új** gombra, és írja be a Keresés sáv **Piros kalap** az alábbi képernyőképen látható módon. Piros kalap vállalati Linux bejegyzéseinek jelennek meg a keresési eredmények között. Kattintson a **piros kalap vállalati Linux 7.2**.

    ![Jelölje ki a RHEL között](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. A keresési eredmények között kattintson a **minden** ablak szerepelnie kell a piros kalap vállalati Linux 7.2 képet. Kattintson a **piros kalap vállalati Linux 7.2** további információt a virtuális gép kép megtekintéséhez.

    ![Jelölje ki a RHEL között](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. A **piros kalap vállalati Linux 7.2** ablakban láthatók a virtuális gép kép további információt. **Jelölje ki a telepítési modell** legördülő menü **Klasszikus**jelölje ki. Kattintson a **Létrehozás** gombra.

    ![Kép részleteinek megtekintése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. A **Virtuális létrehozása** ablakban adja meg a **Host Name mezőbe** az új virtuális gép. Is megadhatja egy helyi rendszergazdai felhasználónevét a **felhasználónév** mezőbe, és a **jelszavát**. Dönthet úgy is, egy SSH termékkulccsal helyi rendszergazdai csatlakozó felhasználók hitelesítését. A virtuális gép az egy **Réteg árak** is kiválaszthatja.

    ![Hozzon létre virtuális - egyszerű részletei](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Kattintson a **választható beállítási**. A **választható beállítások** ablakban kattintson a **hálózat**.

    ![Hozzon létre virtuális - virtuális hálózat konfigurálása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Ekkor megjelenik egy ablak **hálózati**című. A **hálózati** ablakban kattintson a **Virtuális hálózati** jelölje ki a virtuális hálózat, amelyhez a Linux virtuális telepíthető. Ekkor megnyílik a **Virtuális hálózati** ablakban. A **Virtuális hálózati** ablaktáblában válassza a **meglévő virtuális hálózat használata** lehetőségre. Ezután jelölje ki a virtuális hálózat, amelyben Azure Active Directory tartományi szolgáltatások érhető el. Ebben a példában a "MyPreviewVNet" virtuális hálózat választása azt.

    ![Hozzon létre virtuális - virtuális hálózat kiválasztása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. A **választható config** ablaktáblán kattintson az **OK** gombra.

    ![Virtuális - kijelölt virtuális hálózat létrehozása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Most már készen áll a virtuális gép létrehozásához. **Hozzon létre virtuális** ablaktáblájában kattintson a **Létrehozás** gombra.

    ![Hozzon létre virtuális - kész](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Az új virtuális gép RHEL 7.2 kép alapján telepítési kezdetének.

  ![Virtuális - telepítés elindítva létrehozása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Néhány perc múlva a virtuális gépen kell telepíthető sikeresen és kész való használatra.

  ![Hozzon létre virtuális - rendszerbe](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Csatlakozás távoli az újonnan kiépített Linux virtuális géphez
A RHEL 7.2 virtuális gépre van kiépítve Azure-ban. A következő tevékenység távolról csatlakozni a virtuális gépre.

**Kapcsolódás virtuális RHEL 7.2 gépen** Kövesse a cikk [Linux operációs rendszert futtató virtuális géphez bejelentkezés](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)a.

A további műveletek elvégzéséhez feltételezik gitt SSH ügyfélprogramot való csatlakozáshoz a RHEL virtuális gépen használja. További tudnivalókért lásd: a [gitt töltse le a lapot](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Nyissa meg a gitt programot.

2. A **Host Name mezőbe** írja be az újonnan létrehozott RHEL virtuális gép. Ebben a példában a virtuális gép az állomásnév, "kontraktor – rhel.cloudapp .net" tartalmaz. Ha nem biztos a virtuális állomásneve, olvassa el a virtuális irányítópult az Azure portálon.

    ![Csatlakozás gitt](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Jelentkezzen be a virtuális gép a virtuális gép létrehozásának megadott helyi rendszergazdai hitelesítő adataival. Ebben a példában a helyi rendszergazdafiók "mahesh" azt használja.

    ![GITT bejelentkezés](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>A Linux virtuális gép szükséges csomagok telepítése
A virtuális gépen való csatlakozás után a következő feladatra, hogy a virtuális gépen tartományokhoz való csatlakozás szükséges csomagok telepítése. Hajtsa végre az alábbi lépéseket:

1. **Realmd telepítése:** A realmd csomag tartomány illesztés használják. Írja be a következő parancsot a gitt Terminálszolgáltatások:

    sudo yum telepítés realmd

    ![Realmd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Néhány perc múlva a realmd csomag első telepítése után a virtuális gépen.

    ![realmd telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Sssd telepítése:** A realmd csomag attól függ, hogy sssd tartomány join művelet végrehajtásához. Írja be a következő parancsot a gitt Terminálszolgáltatások:

    sudo yum telepítés sssd

    ![Sssd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Néhány perc múlva a sssd csomag első telepítése után a virtuális gépen.

    ![realmd telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Kerberos telepítése:** Írja be a következő parancsot a gitt Terminálszolgáltatások:

    sudo yum telepítés krb5-munkaállomás krb5-könyvtárral lenne

    ![Kerberos telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Néhány perc múlva a realmd csomag első telepítése után a virtuális gépen.

    ![Kerberos telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Vegye fel a Linux virtuális gép a felügyelt tartományba
Most, hogy a szükséges csomagok Linux virtuális számítógépre telepítve van, a következő feladata a virtuális gép csatlakozik a felügyelt tartományhoz.

1. Fedezze fel a AAD felügyelt tartomány. Írja be a következő parancsot a gitt Terminálszolgáltatások:

    sudo tartomány CONTOSO100.COM felfedezése

    ![Tartomány felfedezése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Ha a **tartomány felfedezése** nem található a felügyelt tartomány, ellenőrizze, hogy a tartomány érhető el a virtuális számítógépről (ping próbálja meg). Gondoskodjon arról, hogy a virtuális gép valóban lett telepítve, amelyben a felügyelt tartomány érhető el virtuális hálózatához is.

2. Kerberos inicializálni. Írja be a következő parancsot a gitt Terminálszolgáltatások. Győződjön meg arról, hogy adja meg a felhasználók, akik a "AAD Adatközpont rendszergazdák" csoporthoz tartozik. Csak ezek a felhasználók is csatlakozni tudnak a felügyelt tartományhoz számítógépek.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Győződjön meg arról, hogy a tartománynév nagybetűkkel adja meg, és egyéb kinit sikertelen lesz.

3. A számítógép csatlakozik a tartományhoz. Írja be a következő parancsot a gitt Terminálszolgáltatások. Adja meg az előző lépésben (kinit) megadott ugyanahhoz a felhasználóhoz.

    sudo tartomány illesztés – részletes CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Tartomány illesztés](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

("Sikeres beiktatott gép tartomány") üzenetet kapja, ha a számítógép sikeresen csatlakozik a felügyelt tartományhoz.


## <a name="verify-domain-join"></a>Ellenőrizze a tartomány illesztés
Gyorsan ellenőrizheti, hogy a gép van már sikeresen a tartományhoz felügyelt. Csatlakozás az újonnan a tartomány használata a SSH és a tartomány felhasználói fiókot, és jelölje be a megoldódott a felhasználói fiók megfelelően RHEL virtuális csatlakozott.

1. Csatlakozás a következő parancsot írja be a gitt Terminálszolgáltatások az újonnan a tartomány csatlakozott RHEL virtuális gép SSH használatával. A felügyelt tartományhoz tartozó domain fiókot használ (például 'bob@CONTOSO100.COM' ebben az esetben.)

    ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net

2. Írja be a gitt Terminálszolgáltatások tekintheti meg, ha az otthoni címtár megfelelően inicializált-e a következő parancsot.

    pwd

3. Írja be a gitt Terminálszolgáltatások tekintheti meg, ha a csoporttagság megfelelően lehet megoldani a következő parancsot.

    azonosító

Egy minta kimenet ezeket a parancsokat a következőképpen néz ki:

![Ellenőrizze a tartomány illesztés](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Tartomány illesztés – hibaelhárítás
[Hibaelhárítás tartomány illesztés](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) cikke nyújt útmutatást.


## <a name="related-content"></a>Kapcsolódó tartalom
- [Azure Active Directory tartományi szolgáltatások – első lépések útmutató](./active-directory-ds-getting-started.md)

- [A Windows Server virtuális gép vegye fel az Azure Active Directory tartományi szolgáltatások felügyelt tartományba](active-directory-ds-admin-guide-join-windows-vm.md)

- [Bejelentkezés Linux operációs rendszert futtató virtuális géphez](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Kerberos telepítése](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Piros kalap vállalati Linux 7 – útmutató a Windows-integráció](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
