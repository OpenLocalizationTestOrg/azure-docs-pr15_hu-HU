<properties
    pageTitle="Telepítse az Active Directory erdő Azure virtuális hálózaton |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre egy új Active Directory-erdőt virtuális gépen (virtuális) Azure virtuális hálózaton oktatóprogram."
    services="active-directory, virtual-network"
    keywords="az Active directory virtuális a számítógépen, telepítse az active directory erdőben azure active directory-videók "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Egy új Active Directory-erdőt telepítse az Azure virtuális hálózaton

Ez a témakör bemutatja az hogyan hozhat létre egy új Windows Server Active Directory-környezet (virtuális) virtuális gépen Azure virtuális hálózaton [Azure virtuális hálózati](../virtual-network/virtual-networks-overview.md). Ebben az esetben az Azure virtuális hálózati nem csatlakozik egy helyszíni hálózaton.

Is bizonyára kíváncsi, az alábbi kapcsolódó témakörök:

- A videóból megtudhatja, ezeket a lépéseket olvassa el a [egy új Active Directory-erdőt Azure virtuális hálózaton telepítése](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network) című témakört.
- Lehetősége van tetszés szerint [állítsa be a webhely VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) , vagy új erdő telepítse vagy bővít ki egy helyszíni erdőben Azure virtuális hálózathoz. Ezek a lépések lásd: [a replika Active Directory tartományvezérlőnek az Azure virtuális hálózat telepítése](../active-directory/active-directory-install-replica-active-directory-domain-controller.md).
-  Magyarázó rész Azure virtuális hálózaton Active Directory tartományi szolgáltatások (AD DS) telepítéséről, olvassa el [Útmutató telepítése a Windows Server Active Directory a Azure virtuális gépeken futó](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Forgatókönyv Diagram

Ebben az esetben a külső felhasználók a tartományhoz kiszolgálón futó alkalmazások eléréséhez szükséges. A VMs, hogy az alkalmazás-kiszolgálók futtatása és a tartomány vezérlők futó VMs is telepítve van telepítve van a saját felhőszolgáltatásában az Azure virtuális hálózat. Ezek egyben az elérhetőség, továbbfejlesztett hibatűrést beállítása részét képező.

![Active Directory erdő egy virtuális gépeken Azure virtuális hálózaton ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Hogyan Ez különbözik a helyszíni?

Nem áll sok és a helyszíni telepítésének a tartományvezérlőnek Azure különbségét. Az alábbi táblázat a főbb különbségek jelennek meg.

Konfigurálandó...  | A helyszíni  | Azure virtuális hálózati
------------- | -------------  | ------------
**A tartományvezérlőnek IP-címe**  | A hálózati adapteren tulajdonságok statikus IP-cím   | Futtassa a Set-AzureStaticVNetIP parancsmagot a statikus IP-cím
**DNS-feloldó-ügyfél**  | A tartomány tagok kártya tulajdonságainak beállítása elsődleges és másodlagos DNS-kiszolgáló címét a hálózaton   | DNS-kiszolgáló címét meg a a virtuális hálózati tulajdonságai
**Az Active Directory-adatbázis tárolási**  | Tetszés szerint módosítsa az alapértelmezett tárhelye C:\  | Át kell alapértelmezett tárolási helye a C:\



## <a name="create-an-azure-virtual-network"></a>Hozzon létre egy Azure virtuális hálózat

1. Jelentkezzen be az Azure klasszikus portálra.
2. Hozzon létre egy virtuális hálózaton. Kattintson a **hálózatok** > **létrehozása egy virtuális hálózaton**. Az értékek használata az alábbi táblázat a varázsló.

    A varázsló lapján...  | Ezek az értékek megadása
    ------------- | -------------
    **Virtuális hálózati részletei**  | <p>Név: Adjon egy nevet a virtuális hálózat</p><p>Régió: Válassza ki a legközelebbi régió</p>
    **DNS- és a virtuális Magánhálózati**  | <p>Hagyja üresen a DNS-kiszolgáló</p><p>Bármelyik VPN beállítást nem jelöli ki.</p>
    **Virtuális hálózati cím szóköz**  | <p>Alhálózat név: Adjon meg egy nevet alhálózathoz</p><p><b>10.0.0.0</b> kezdő IP:</p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Futtassa a tartományvezérlőnek és a DNS-kiszolgálói szerepkörök VMs létrehozása

Ismételje meg az alábbi lépésekkel hozhat létre szükség szerint az Adatközpont szerepkör tárolni VMs. A hibatűrést és a redundancia legalább két virtuális DCs kell telepítenie. Ha az Azure virtuális hálózati tartalmaz legalább két DCs hasonlóképpen konfigurált (Ez azt jelenti, hogy mindkét globális katalógusok futtatása a DNS-kiszolgálót, azok hosszan az egyiket bármely FSMO szerepkört, és így tovább) helyezze el a VMs futó azok DCs az elérhetőség, továbbfejlesztett hibatűrést beállítása.

A felhasználói felület helyett a Windows PowerShell használatával a VMs létrehozásához a [Azure PowerShell hozhat létre, és adja meg a Windows-alapú virtuális gépeken futó előre](../virtual-machines/virtual-machines-windows-classic-create-powershell.md)című.

1. Kattintson a klasszikus portál **Új** > **kiszámítania** > **virtuális gép** > **A gyűjtemény**. A következő értékeket használja a varázsló. Fogadja el a beállítás alapértelmezett értékét, kivéve, ha egy másik értékre szükséges vagy javasolt.

    A varázsló lapján...  | Ezek az értékek megadása
    ------------- | -------------
    **Válassza a kép**  | A Windows Server 2012 R2 adatközponthoz
    **Virtuális gép konfigurálása**  | <p>Virtuális számítógép neve: Írja be a egyetlen címke nevét (például AzureDC1).</p><p>Az új felhasználó neve: Írja be annak a felhasználónak a nevét. Ennek a felhasználónak a virtuális a helyi Rendszergazdák csoport tagjának lesz. Ez a név, hogy először jelentkezzen be a virtuális szüksége lesz. A rendszergazda nevű beépített fiók nem fognak működni.</p><p>Új jelszó megerősítése: Írjon be egy jelszót</p>
    **Virtuális gép konfigurálása**  | <p>Felhőbeli szolgáltatástól: Válassza a az első virtuális <b>Hozzon létre egy új, felhőalapú szolgáltatásba</b> , és további VMs, ugyanis az Adatközpont szerepkör létrehozásakor, jelölje ki, hogy egy felhőalapú szolgáltatás neve.</p><p>Felhőalapú szolgáltatás DNS-név: Adjon meg egy globálisan egyedi nevet</p><p>Régió/affinitás csoport/virtuális hálózati: Adja meg a virtuális hálózat nevét (például WestUSVNet).</p><p>Tároló fiók: Válassza <b>az automatikusan generált tárterület-fiók használata</b> az első virtuális, és válassza a tárhely azonos fióknév, ugyanis az Adatközpont szerepkör további VMs létrehozásakor.</p><p>Elérhetőség beállítása: Válassza ki, <b>Hozzon létre egy elérhetősége</b>.</p><p>Elérhetőség nevének megadása: a elérhetősége megadása esetén az első virtuális létrehozása, és válassza a, hogy azonos név további VMs létrehozásakor, írjon be egy nevet.</p>
    **Virtuális gép konfigurálása**  | <p>Jelölje ki <b>a virtuális ügynököt</b> és bármely más bővítmények szükséges.</p>
2. Minden egyes virtuális az Adatközpont kiszolgálói szerepkör által futtatott lemezen csatolhat. A további lemezre mentheti az Active Directory adatbázis, naplók, és SYSVOL van szükség. Adja meg a méretet a lemez (például a 10 GB), és hagyja a **Host gyorsítótár preferencia** **nincs**értékre van állítva. A lépések című témakörben találja [hogyan csatolhat adatok lemezen a Windows virtuális gépet](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Miután első alkalommal jelentkezik a virtuális gép, nyissa meg a **Kiszolgálókezelő** > **fájl- és tárolása** a lemezen NTFS használata kötet létrehozásához.
4. A statikus IP-cím foglalása VMs az Adatközpont szerepkör fog futni. Lefoglalásához a statikus IP-címet, töltse le a Microsoft webes Platform telepítő és [Azure PowerShell telepítése](../powershell-install-configure.md) és beállítása – AzureStaticVNetIP futtassa. Példa:

    "Get-AzureVM - ServiceName AzureDC1-AzureDC1 név |} Set-AzureStaticVNetIP - IP-cím 10.0.0.4 |} Frissítés-AzureVM

A statikus IP-cím beállításával kapcsolatos további tudnivalókért lásd: a [Configure egy virtuális statikus belső IP-címet](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-windows-server-active-directory"></a>Telepítse a Windows Server Active Directory

Az azonos gyakorlatának, [telepítse az Active Directory tartományi szolgáltatások](https://technet.microsoft.com/library/jj574166.aspx) , hogy használja-e a helyszíni használata (Ez azt jelenti, használhatja a felhasználói felület, a válasz-fájlként vagy a Windows PowerShell). Meg kell adnia a rendszergazdai hitelesítő adatok új erdő telepítéséhez. Az Active Directory-adatbázist, naplók és SYSVOL helyének megadásához módosítsa az alapértelmezett tárolási helye a operációs rendszer meghajtóról a virtuális csatolt további adatokat lemezre.

Az Adatközpont telepítés befejezése után újra csatlakozik a virtuális, és jelentkezzen be az Adatközpont. Ne feledje, hogy adja meg a tartomány hitelesítő adataival.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>A DNS-kiszolgáló Azure virtuális hálózatok alaphelyzetbe állítása

1. Állítsa alaphelyzetbe a DNS-továbbító beállítása az új Adatközpont/DNS-kiszolgálón.
  1. Kattintson az **Eszközök menü**a Kiszolgálókezelő > **DNS**.
  2. A **DNS-kezelőben**kattintson a jobb gombbal a DNS-kiszolgáló nevét, és válassza a **Tulajdonságok parancsot**.
  3. A **továbbítókat** lapon kattintson a továbbító IP-címét, és kattintson a **Szerkesztés**gombra.  Jelölje ki az IP-címét, és kattintson a **Törlés**gombra.
  4. Kattintson **az OK gombra** kattintva zárja be a szerkesztő és az **OK gombra** a DNS-kiszolgáló tulajdonságai bezárásához.
2. Frissítse a DNS-kiszolgáló beállítása a virtuális hálózathoz.
  1. Kattintson a **Virtuális hálózatok** > kattintson duplán a létrehozott virtuális hálózat > **konfigurálása** > **DNS-kiszolgálók**, írja be a név és a DIP a VMs az Adatközpont/DNS-kiszolgálói szerepkör futtató egyik, és kattintson a **Mentés**gombra.
  2. Jelölje ki a virtuális, és **Indítsa újra** a indíthatja el a DNS-feloldó beállítások konfigurálása a IP-címet az új DNS-kiszolgáló virtuális gombra.


## <a name="create-vms-for-domain-members"></a>VMs létrehozása tartományhoz csatlakoztatott

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


## <a name="see-also"></a>Lásd még:

-  [Egy új Active Directory-erdőt telepítése Azure virtuális hálózaton](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [A Windows Server Active Directory Azure virtuális gépeken telepítési útmutatója](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [A webhely VPN konfigurálása](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Egy replika az Active Directory tartományvezérlőnek telepítse az Azure virtuális hálózatban](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure informatikai szakembereknek IaaS: (01) virtuális gép alapjai](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure informatikai szakembereknek IaaS: (05) létrehozása virtuális hálózatok és idegen helyszíni kapcsolat](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md)
-  [Telepítse és állítsa be a Azure PowerShell hogyan](../powershell-install-configure.md)
-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Azure virtuális statikus IP-cím beállítása](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Azure virtuális statikus IP-cím hozzárendelése](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Új Active Directory-erdőben telepítése](https://technet.microsoft.com/library/jj574166.aspx)
-  [Az Active Directory tartományi szolgáltatások (AD DS) virtualizációs (szint 100) – bevezetés](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
