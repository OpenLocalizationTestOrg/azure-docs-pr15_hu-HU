<properties
    pageTitle="Egy replika az Active Directory tartományi vezérlő telepítse az Azure-ban |} Microsoft Azure"
    description="Megtudhatja, hogy miként telepítheti a tartományvezérlőnek egy helyszíni Active Directory erdőből Azure virtuális-gépen oktatóprogram."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Egy replika az Active Directory tartományi vezérlő telepítse az Azure virtuális hálózatban

Ez a témakör bemutatja a további tartomány vezérlők (más néven replika DCs) telepítése Azure virtuális gépeken (VMs) az Azure virtuális hálózatban a helyszíni Active Directory tartomány esetén.

Is bizonyára kíváncsi, az alábbi kapcsolódó témakörök:

-  Másik lehetőségként telepítheti egy új Active Directory-erdőt Azure virtuális hálózaton. Ezek a lépések lásd: [Új Active Directory-erdő Azure virtuális hálózaton telepítése](../active-directory/active-directory-new-forest-virtual-machine.md).
-  Magyarázó rész Azure virtuális hálózaton Active Directory tartományi szolgáltatások (AD DS) telepítéséről, olvassa el [Útmutató telepítése a Windows Server Active Directory a Azure virtuális gépeken futó](https://msdn.microsoft.com/library/azure/jj156090.aspx).


## <a name="scenario-diagram"></a>Eset diagram

Ebben az esetben a külső felhasználók a tartományhoz tartozó kiszolgálón futó alkalmazások eléréséhez szükséges. A VMs, futtassa az alkalmazás-kiszolgálók, és a replika DCs az Azure virtuális hálózaton van telepítve. A virtuális hálózat kapcsolhat össze a helyszíni hálózaton, [webhely virtuális Magánhálózati](../vpn-gateway/vpn-gateway-site-to-site-create.md) kapcsolat által a következő ábrán látható módon, vagy gyorsabb kapcsolat [készült ExpressRoute](../expressroute/expressroute-locations-providers.md) is használhatja.

Az alkalmazás-kiszolgálók és a DCs telepítését, külön felhőszolgáltatások számítási feldolgozás terjesztheti és [elérhetőségének beállítása](../virtual-machines/virtual-machines-windows-manage-availability.md) a továbbfejlesztett hibatűrést belül.
A DCs bizonyos egymással, és a helyszíni DCs az Active Directory-replikáció használatával. Nem Szinkronizáló eszközök van szükség.

![Diagram pf replika az Active Directory tartományvezérlőnek az Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Az Azure virtuális hálózati az Active Directory-hely létrehozása

Érdemes webhely létrehozása az Active Directory, amely a hálózat terület megfelelő a virtuális hálózathoz. Amely segít a optimalizálhatja a hitelesítés, replikációs és egyéb Adatközpont hely műveleteket. A következő lépések bemutatják, hogy miként hozzon létre egy webhelyet, és további hátteréről [hozzáadása az új webhely](https://technet.microsoft.com/library/cc781496.aspx).

1. Nyissa meg az Active Directory-webhelyek és a szolgáltatások: **Kiszolgálókezelő** > **eszközök** > **az Active Directory-helyek és szolgáltatások**.
2. A régió, amelyen létrehozta az Azure virtuális hálózati ábrázolásához webhely létrehozása: kattintson a **webhelyek**elemre > **művelet** > **Új webhely** > írja be a nevét az új webhely, például az Azure US nyugati > jelöljön ki egy webhely hivatkozást > **OK gombra**.
3. Alhálózat létrehozása és az új webhely társítása: kattintson duplán a **webhelyek** > kattintson a jobb gombbal a **alhálózat** > **Új alhálózat** > írja be az IP-címtartományokat (például a forgatókönyv diagram 10.1.0.0/16) virtuális hálózat > jelölje be az új Azure webhely > **az OK gombra**.

## <a name="create-an-azure-virtual-network"></a>Hozzon létre egy Azure virtuális hálózat

1. Az [Azure klasszikus portált](https://manage.windowsazure.com), kattintson az **Új** > **Hálózati szolgáltatások** > **Virtuális hálózati** > **Egyéni létrehozása** és használata az alábbi értékeket, a varázsló.

    A varázsló lapján...  | Ezek az értékek megadása
    ------------- | -------------
    **Virtuális hálózati részletei**  | <p>Név: Írja be a virtuális hálózat, például WestUSVNet nevét.</p><p>Régió: Válassza a legközelebbi területhez tartozik.</p>
    **DNS- és a virtuális Magánhálózati kapcsolat**  | <p>A DNS-kiszolgálók: Adja meg, és a helyszíni DNS-kiszolgálók egy vagy több IP-címét.</p><p>Kapcsolat: Jelölje be a **beállítás a webhely VPN**.</p><p>Helyi hálózaton: Adjon meg egy új helyi hálózathoz.</p><p>Ha készült ExpressRoute virtuális Magánhálózattal helyett használja, a [konfigurálása Exchange szolgáltatón keresztül készült ExpressRoute származó](../expressroute/expressroute-locations-providers.md)című témakört.</p>
    **Hely közötti kapcsolat**  | <p>Név: Írja be egy nevet a helyszíni hálózaton.</p><p>Virtuális Magánhálózati eszköz IP-címe: Adja meg az eszközt, amelyet a virtuális hálózat fog csatlakozni a nyilvános IP-címét. A virtuális Magánhálózati eszköz nem található meg az mögött egy hálózati címfordítást.</p><p>Cím: Adja meg a címtartományokat (például a forgatókönyv diagram 192.168.0.0/16) a helyszíni hálózaton.</p>
    **Virtuális hálózati cím szóköz**  | <p>Címterület: Az IP-címtartományokat VMs, amelyet az Azure virtuális hálózatban (például a forgatókönyv diagram 10.1.0.0/16) futtatásához adja meg. A cím nem áll átfedésben a helyszíni hálózaton címtartományai.</p><p>Alhálózat: Adja meg a nevét és az alkalmazás-kiszolgálók alhálózat címét (például Frontend, 10.1.1.0/24) és a DCs (Kódmentes, például 10.1.2.0/24).</p><p>Kattintson a **átjáró alhálózat hozzáadása**gombra.</p>

2. Ezután beállíthatja a virtuális hálózati átjáró létrehozása a biztonságos webhely virtuális Magánhálózati kapcsolat fogja. Az utasításokért [konfigurálása a virtuális hálózati átjáró](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) című témakört.
3. A webhely virtuális Magánhálózati kapcsolat létrehozása az új virtuális hálózati és között egy helyszíni VPN eszközön. Az utasításokért [konfigurálása a virtuális hálózati átjáró](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) című témakört.


## <a name="create-azure-vms-for-the-dc-roles"></a>Azure VMs létrehozása az Adatközpont szerepkörök

Ismételje meg az alábbi lépésekkel hozhat létre szükség szerint az Adatközpont szerepkör tárolni VMs. A hibatűrést és a redundancia legalább két virtuális DCs kell telepítenie. Ha az Azure virtuális hálózati tartalmaz legalább két DCs hasonlóképpen konfigurált (Ez azt jelenti, hogy mindkét globális katalógusok futtatása a DNS-kiszolgálót, azok hosszan az egyiket bármely FSMO szerepkört, és így tovább) helyezze el a VMs futó azok DCs az elérhetőség, továbbfejlesztett hibatűrést beállítása.
A felhasználói felület helyett a Windows PowerShell használatával a VMs létrehozásához a [Azure PowerShell hozhat létre, és adja meg a Windows-alapú virtuális gépeken futó előre](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)című.

1. Az [Azure klasszikus portált](https://manage.windowsazure.com), kattintson az **Új** > **kiszámítania** > **virtuális gép** > **A gyűjteményben**. A következő értékeket használja a varázsló. Fogadja el a beállítás alapértelmezett értékét, kivéve, ha egy másik értékre szükséges vagy javasolt.

    A varázsló lapján...  | Ezek az értékek megadása
    ------------- | -------------
    **Válassza a kép**  | A Windows Server 2012 R2 adatközponthoz
    **Virtuális gép konfigurálása**  | <p>Virtuális számítógép neve: Írja be a egyetlen címke nevét (például AzureDC1).</p><p>Az új felhasználó neve: Írja be annak a felhasználónak a nevét. Ennek a felhasználónak a virtuális a helyi Rendszergazdák csoport tagjának lesz. Ez a név, hogy először jelentkezzen be a virtuális szüksége lesz. A rendszergazda nevű beépített fiók nem fognak működni.</p><p>Új jelszó megerősítése: Írjon be egy jelszót</p>
    **Virtuális gép konfigurálása**  | <p>Felhőbeli szolgáltatástól: Válassza a az első virtuális <b>Hozzon létre egy új, felhőalapú szolgáltatásba</b> , és további VMs, ugyanis az Adatközpont szerepkör létrehozásakor, jelölje ki, hogy egy felhőalapú szolgáltatás neve.</p><p>Felhőalapú szolgáltatás DNS-név: Adjon meg egy globálisan egyedi nevet</p><p>Régió/affinitás csoport/virtuális hálózati: Adja meg a virtuális hálózat nevét (például WestUSVNet).</p><p>Tároló fiók: Válassza <b>az automatikusan generált tárterület-fiók használata</b> az első virtuális, és válassza a tárhely azonos fióknév, ugyanis az Adatközpont szerepkör további VMs létrehozásakor.</p><p>Elérhetőség beállítása: Válassza ki, <b>Hozzon létre egy elérhetősége</b>.</p><p>Elérhetőség nevének megadása: a elérhetősége megadása esetén az első virtuális létrehozása, és válassza a, hogy azonos név további VMs létrehozásakor, írjon be egy nevet.</p>
    **Virtuális gép konfigurálása**  | <p>Jelölje ki <b>a virtuális ügynököt</b> és bármely más bővítmények szükséges.</p>
2. Lemezen csatolása minden virtuális az Adatközpont kiszolgálói szerepkör fog futni. A további lemezre mentheti az Active Directory adatbázis, naplók, és SYSVOL van szükség. Adja meg a méretet a lemez (például a 10 GB), és hagyja a **Host gyorsítótár preferencia** **nincs**értékre van állítva. A lépések című témakörben találja [hogyan csatolhat adatok lemezen a Windows virtuális gépet](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Miután első alkalommal jelentkezik a virtuális gép, nyissa meg a **Kiszolgálókezelő** > **fájl- és tárolása** NTFS használata lemezen kötet létrehozásához.
4. A statikus IP-cím foglalása VMs az Adatközpont szerepkör fog futni. Lefoglalásához a statikus IP-címet, töltse le a Microsoft webes Platform telepítő és [Azure PowerShell telepítése](../powershell-install-configure.md) és beállítása – AzureStaticVNetIP futtassa. Példa:

    "Get-AzureVM - ServiceName AzureDC1-AzureDC1 név |} Set-AzureStaticVNetIP - IP-cím 10.0.0.4 |} Frissítés-AzureVM

A statikus IP-cím beállításával kapcsolatos további tudnivalókért lásd: a [Configure egy virtuális statikus belső IP-címet](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Azure VMs Active Directory tartományi szolgáltatások telepítése

Jelentkezzen be egy virtuális, és ellenőrizheti, hogy csatlakozási erőforrásokat a webhely VPN vagy készült ExpressRoute kapcsolaton keresztül a helyszíni hálózaton. Ezután telepítse az Azure VMs Active Directory tartományi szolgáltatások. Ugyanezt az eljárást, amely egy további Adatközpont telepíti a helyszíni hálózaton (felhasználói felület, a Windows PowerShell vagy válasz kiterjesztésű) is használhatja. Módon telepítette az Active Directory tartományi szolgáltatások, feltétlenül megadhatja az új kötet, az Active Directory adatbázis, naplók és SYSVOL helyét. Ha Ön ismereteit az Active Directory tartományi szolgáltatások telepítésével, olvassa el a [Telepítése Active Directory tartományi szolgáltatások (szint 100)](https://technet.microsoft.com/library/hh472162.aspx) vagy [egy replika Windows Server 2012 tartományvezérlőnek a meglévő tartományban (szint 200) telepítése](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>A virtuális hálózati DNS-kiszolgáló átkonfigurálása

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a virtuális hálózat nevére, és kattintson a **beállítás** fülre [átkonfigurálása virtuális hálózatához DNS-kiszolgáló IP-címek](../virtual-network/virtual-networks-manage-dns-in-vnet.md) a statikus IP-címek rendelt a replika DCs az IP-címek egy helyszíni DNS-kiszolgálók helyett.

2. Győződjön meg arról, hogy minden a másolata Adatközpont VMs található a virtuális hálózat van konfigurálva a DNS-kiszolgálók használata a virtuális hálózaton, kattintson a **virtuális gépeken futó**, az Állapot oszlopban kattintson a minden virtuális, és kattintson az **Újraindítás**. Várja meg, amíg a virtuális **operációs rendszert futtató** állapot megjelenítése, mielőtt megpróbálná jelentkezhet be azt.

## <a name="create-vms-for-application-servers"></a>Hozzon létre VMs alkalmazáskiszolgálók

1. Ismételje meg az alábbi lépésekkel alkalmazáskiszolgálók futtatandó VMs létrehozásához. Fogadja el a beállítás alapértelmezett értékét, kivéve, ha egy másik értékre szükséges vagy javasolt.

    A varázsló lapján...  | Ezek az értékek megadása
    ------------- | -------------
    **Válassza a kép**  | A Windows Server 2012 R2 adatközponthoz
    **Virtuális gép konfigurálása**  | <p>Virtuális számítógép neve: Írja be a egyetlen címke nevét (például AppServer1).</p><p>Az új felhasználó neve: Írja be annak a felhasználónak a nevét. Ennek a felhasználónak a virtuális a helyi Rendszergazdák csoport tagjának lesz. Ez a név, hogy először jelentkezzen be a virtuális szüksége lesz. A rendszergazda nevű beépített fiók nem fognak működni.</p><p>Új jelszó megerősítése: Írjon be egy jelszót</p>
    **Virtuális gép konfigurálása**  | <p>Felhőbeli szolgáltatástól: Válassza a az első virtuális **Hozzon létre egy új, felhőalapú szolgáltatásba** , és további VMs, ugyanis az alkalmazás létrehozásakor, jelölje ki, hogy egy felhőalapú szolgáltatás neve.</p><p>Felhőalapú szolgáltatás DNS-név: Adjon meg egy globálisan egyedi nevet</p><p>Régió/affinitás csoport/virtuális hálózati: Adja meg a virtuális hálózat nevét (például WestUSVNet).</p><p>Tárterület-fiók: Válassza **az automatikusan generált tárterület-fiók használata** az első virtuális, és válassza a tárhely azonos fióknév, további VMs, ugyanis az alkalmazás létrehozásakor.</p><p>Elérhetőség beállítása: Válassza ki, **Hozzon létre egy elérhetősége**.</p><p>Elérhetőség nevének megadása: a elérhetősége megadása esetén az első virtuális létrehozása, és válassza a, hogy azonos név további VMs létrehozásakor, írjon be egy nevet.</p>
    **Virtuális gép konfigurálása**  | <p>Jelölje ki <b>a virtuális ügynököt</b> és bármely más bővítmények szükséges.</p>

2. Miután minden virtuális már kiépítve, jelentkezzen be, és a tartományhoz csatlakoztatni. A **Kiszolgálókezelő**, kattintson a **Helyi kiszolgáló** > **munkacsoport** > **Módosítás...** és válassza ki a **tartományt** , és írja be a helyszíni tartomány nevét. Adja meg a tartomány felhasználó hitelesítő adatait, és indítsa újra a virtuális a tartomány join befejezéséhez.

A felhasználói felület helyett a Windows PowerShell használatával a VMs létrehozásához a [Azure PowerShell hozhat létre, és adja meg a Windows-alapú virtuális gépeken futó előre](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)című.

A Windows PowerShell használatával kapcsolatos további tudnivalókért olvassa el az [Első lépések az Azure-parancsmagok](https://msdn.microsoft.com/library/azure/jj554332.aspx) és [Azure parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/jj554330.aspx)című témakört.

## <a name="additional-resources"></a>További források

-  [A Windows Server Active Directory Azure virtuális gépeken telepítési útmutatója](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Meglévő feltöltése a helyszíni tartomány vezérlők a Hyper-V Azure Azure PowerShell használatával](http://support.microsoft.com/kb/2904015)
-  [Egy új Active Directory-erdőt telepítse az Azure virtuális hálózaton](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure virtuális hálózati](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure informatikai szakembereknek IaaS: (01) virtuális gép alapjai](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure informatikai szakembereknek IaaS: (05) létrehozása virtuális hálózatok és idegen helyszíni kapcsolat](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure-kezelő parancsmagok](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
