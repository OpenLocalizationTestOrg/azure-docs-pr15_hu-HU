<properties
    pageTitle="Az első Windows virtuális IIS telepítése |} Microsoft Azure"
    description="Az első Windows virtuális gép kísérletezni IIS és az Azure portálon 80-as port megnyitása telepítésével."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Kísérletezés a szerepkörbe telepítése a Windows virtuális
    
Ha befejezte az első virtuális gép (virtuális) felfelé és fut, áthelyezheti telepítésének szoftvereinek és szolgáltatásainak. Az ebben az oktatóanyagban fogjuk telepítse az IIS használja a Windows Server virtuális Kiszolgálókezelő. Ezután azt hoz létre egy hálózati biztonsági csoport (NSG) kattintva nyissa meg a 80-as port IIS forgalom az Azure portál használatával. 

Az első virtuális még nem hozott létre, ha meg kell térjen vissza [az első Windows virtuális gép az Azure-portálon létrehozása](virtual-machines-windows-hero-tutorial.md) ebből az oktatóanyagból a továbblépés előtt.

## <a name="make-sure-the-vm-is-running"></a>A virtuális futásának ellenőrzése

1. Nyissa meg az [Azure-portálon](https://portal.azure.com).
2. A központi menüben kattintson a **virtuális gépeken futó**. Jelölje ki a virtuális gép a listából.
3. Ha az állapot **Leállítva (Deallocated)**, a **Start** gombra, a virtuális a **Essentials** lap a. Ha az Állapot oszlop értéke **fut**, áthelyezhetők a következő lépéssel.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>A virtuális gép csatlakozzon, és bejelentkezés

1.  A központi menüben kattintson a **virtuális gépeken futó**. Jelölje ki a virtuális gép a listából.

3. A virtuális gép lap kattintson a **Csatlakozás**gombra. Ez a hoz létre, és egy távoli asztali Protocol (fájl .rdp), amely olyan, mintha egy helyi számítógépre való kapcsolódáshoz letöltések. Érdemes lehet, hogy mentse a fájlt az asztalra, hogy megkönnyítse az elérésüket. **Nyissa meg** a fájl a virtuális csatlakozni.

    ![Az Azure portált a virtuális csatlakoztatása ábrázoló kép](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Megjelenik egy figyelmeztetés, amely a .rdp egy ismeretlen közzétevőtől származó. Ez a normál. A távoli asztali ablakában kattintson a **Csatlakozás** továbbra is.

    ![Az ismeretlen publisher kapcsolatos figyelmeztetés képe](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. A Windows biztonsági ablakban írja be a felhasználónevét és jelszavát a virtuális létrehozott létrehozott helyi fiók. A felhasználónév így kell megadni *vmname*& #92; a *felhasználónév*, majd kattintson az **OK gombra**.

    ![Képernyőkép: a virtuális név, felhasználónév és jelszó beírása](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Megjelenik egy figyelmeztetés, hogy a tanúsítvány nem lehet ellenőrizni. Ez a normál. Kattintson az **Igen gombra** a virtuális gép az identitás ellenőrzése és a Befejezés a Bejelentkezés gombra.

    ![Képernyőkép a területről, egy üzenet verziók a virtuális azonosítása](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Ha, probléma történik, amikor megpróbál kapcsolódni, lásd: a [távoli asztali kapcsolatos hibák elhárítása kapcsolatok a Windows-alapú Azure virtuális gép](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>A virtuális IIS telepítése

Most, hogy be van jelentkezve a virtuális, azt, hogy több kísérletezhet telepíti a kiszolgálói szerepkörök.

1. Nyissa meg **A Kiszolgálókezelő** , ha nem látható. Kattintson a **Start** menüre, és kattintson a **Kiszolgáló elemre**.
2. **A Kiszolgálókezelő**jelölje ki a bal oldali ablaktáblában **Helyi kiszolgáló** . 
3. A menüben válassza a **kezelés** > **hozzáadása szerepkörök és szolgáltatások**.
4. A szerepkörök hozzáadása és szolgáltatások varázslót, a **Telepítési típus** lapon válassza a **szerepköralapú vagy a szolgáltatás-alapú telepítés**, és kattintson a **Tovább gombra**.

    ![Képernyőkép a szerepkörök hozzáadása és szolgáltatások varázsló lapon a telepítés típusát](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Jelölje ki a virtuális a kiszolgáló készletből, és kattintson a **Tovább**gombra.
6. A **Kiszolgálói szerepkörök** lapon jelölje be a **Webkiszolgáló (IIS)**.

    ![Képernyőkép: a szerepkörök hozzáadása és szolgáltatások varázsló fülre a kiszolgálói szerepkörök](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. Az előugró ablakban az IIS-hez szükséges szolgáltatások hozzáadásával kapcsolatban győződjön meg arról, hogy **eszközeit tartalmazza** legyen kijelölve, és kattintson a **Szolgáltatások hozzáadása**gombra. Ha bezárja az előugró ablakot, kattintson a **Tovább** gombra a varázslóban.

    ![Képernyőkép: a IIS szerepkört hozzáadásának megerősítését előugró](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. A szolgáltatások lapon kattintson a **Tovább**gombra.
9. A **Webhely kiszolgálói szerepkör (IIS)** lapon kattintson a **Tovább**gombra. 
10. A **Szerepkör-szolgáltatások** lapon kattintson a **Tovább**gombra. 
11. A **Megerősítés** lapon kattintson a **telepítés**gombra. 
12. A telepítés befejeződött, kattintson a **Bezárás** gombra a varázsló.



## <a name="open-port-80"></a>Nyissa meg a 80-as port 

A virtuális fogadja el a bejövő fölé 80-as port sorrendben kell a hálózati biztonsági csoport hozzáadása egy bejövő szabályt. 

1. Nyissa meg az [Azure-portálon](https://portal.azure.com).
2. Jelölje ki a virtuális létrehozott **virtuális gépeken futó** .
3. A virtuális gépeken futó beállításait jelölje ki a **felületek hálózati** , és válassza a a meglévő hálózati kapcsolaton.

    ![Képernyőkép: a virtuális gép beállításai a hálózati kapcsolat esetén](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. **Alapvető tudnivalók** a hálózati kapcsolat kattintson a **hálózati biztonsági csoport**.

    ![Képernyőkép: a Essentials szakaszban a hálózati kapcsolat](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. A NSG az **alapvető tudnivalók** lap rendelkeznie kell egy meglévő alapértelmezett bejövő szabály lehetővé teszi, hogy jelentkezzen be a virtuális **alapértelmezett engedélyezése rdp** . Egy másik bejövő szabály IIS forgalmának engedélyezésére ad hozzá. **Bejövő biztonsági szabály**gombra.

    ![Képernyőkép a Essentials szakaszban a NSG számára](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. A **Bejövő szabályok biztonsági**kattintson a **Hozzáadás**gombra.

    ![Biztonsági szabály hozzáadása az gomb képe](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. A **Bejövő szabályok biztonsági**kattintson a **Hozzáadás**gombra. Írja be a **80-as** port tartomány, és győződjön meg arról, hogy **engedélyezve** van-e jelölve. Ha végzett, kattintson az **OK gombra**.

    ![Biztonsági szabály hozzáadása az gomb képe](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
További információt a NSGs, a bejövő és kimenő szabályokat, lásd: a [külső hozzáférés engedélyezése a virtuális az Azure portál használatával](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>Csatlakozás az IIS alapértelmezett webhelyéhez

1. Az Azure-portálon kattintson a **virtuális gépeken futó** , és válassza ki a virtuális.
2. Az **alapvető tudnivalók** lap a **nyilvános IP-címet**másolja.

    ![Képernyőkép: a virtuális nyilvános IP-címe megkeresése](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Nyissa meg a böngészőt, és be a címsorba, írja be a nyilvános IP-cím jelennek meg: http://<publicIPaddress> , és kattintson a címre kattintva nyissa **meg az ENTER billentyűt** .
3. A böngészőben nyíljon meg a IIS alapértelmezett lap. Hogy néz ki:

    ![Képernyőkép: az alapértelmezett IIS lap néz ki a böngészőben](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Következő lépések

- Is kísérletezhet [feltüntetésével lemezen adatok](virtual-machines-windows-attach-disk-portal.md) a virtuális géphez. Adatok lemezt a virtuális gép további tárhelyet biztosít.
