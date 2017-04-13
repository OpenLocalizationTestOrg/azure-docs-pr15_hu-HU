
<properties
    pageTitle="Létrehozás RemoteApp hibrid gyűjtemények elhárítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként RemoteApp hibrid webhelycsoport létrehozása hibák elhárítása"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-azure-remoteapp-hybrid-collections"></a>Létrehozás Azure RemoteApp hibrid gyűjtemények – problémamegoldás

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

A hibrid gyűjtemény a üzemelteti, és tárolja az adatokat az Azure felhőben, de a lehetővé teszi, hogy felhasználók access-adatok és a erőforrásokat a helyi hálózaton tárolt. Felhasználók hozzá tudnak férni alkalmazások jelentkezik be a vállalati hitelesítő adatok szinkronizált vagy az Azure Active Directory címtárral összevont. Telepítheti a meglévő Azure virtuális hálózati használó hibrid gyűjteménye, vagy létrehozhat egy új virtuális hálózathoz. Azt javasoljuk, hogy hoz létre, vagy egy virtuális hálózati alhálózat használata a elég nagy CIDR tartománnyá az Azure RemoteApp várható jövőbeli növekedését.

A webhelycsoport még eddig nem hozott létre? Lásd: [a hibrid gyűjtemény létrehozása](remoteapp-create-hybrid-deployment.md) lépések.

Ha problémái vannak a webhelycsoport létrehozása, vagy ha a webhelycsoport nem működik a módon úgy gondolja, hogy meg kell, olvassa el az alábbi információkat.

## <a name="your-image-is-invalid"></a>A kép érvénytelen. ##
Ha megjelenik a következőhöz hasonló "GoldImageInvalid" üzenet, amikor Ön várakozik Azure kiépítése a webhelycsoport, az azt jelenti, hogy a sablon képe nem felel meg a [kép követelmények definiált](remoteapp-imagereqs.md). Igen lépjen, olvassa el az adott [követelményeket](remoteapp-imagereqs.md), javítása a képet, és próbálja meg ismét a webhelycsoport létrehozása.



## <a name="does-your-vnet-have-network-security-groups-defined"></a>Rendelkezik a VNET hálózati biztonsági csoportok definiált? ##
Ha használja a webhelycsoport-alhálózat definiált hálózati biztonsági csoportokat, ellenőrizze ezen [URL-EK és a portokra](remoteapp-ports.md) elérhetők alhálózathoz belül.

A VMs a alhálózat szorosabb vezérlő az Ön által rendszerbe is adhat további hálózati biztonsági csoportokat.

## <a name="are-you-using-your-own-dns-servers-and-are-they-accessible-from-your-vnet-subnet"></a>A saját DNS-kiszolgálók használja? És elérhetők azok VNET alhálózathoz? ##
>[AZURE.NOTE] Győződjön meg arról, hogy a DNS-kiszolgálók a VNET mindig felfelé, és mindig tudja oldani a virtuális gépeken futó a VNET a szolgáltatott van. Ne használja a Google DNS.


Hibrid gyűjtemények használja a saját DNS-kiszolgálók. Megadhatja őket a hálózati konfigurációja sémában, illetve az adatkezelési portálon keresztül a virtuális hálózat létrehozásakor. A DNS-kiszolgálók használják, hogy azok (nem pedig a ciklikus) feladatátvevő módon meghatározott sorrendben.  
Olvassa el a [Névfeloldás VMs és szerepkör-példányok](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) , győződjön meg arról, hogy a DNS-kiszolgálók konfigurált correcly.

Ellenőrizze, hogy érhetők el a DNS-kiszolgálóiról a webhelycsoport és a VNET alhálózat elérhető megadott ebben a gyűjteményben.

Példa:

    <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="" IPAddress=""/>
      </DnsServers>
    </Dns>
    </VirtualNetworkConfiguration>

![A DNS-meghatározása](./media/remoteapp-hybridtrouble/dnsvpn.png)

## <a name="are-you-using-an-active-directory-domain-controller-in-your-collection"></a>Használja az Active Directory tartományvezérlőnek a gyűjteményben? ##
Jelenleg csak az Active Directory-tartománya Azure RemoteApp társítható. A hibrid gyűjtemény csak Azure Active Directory-fiókok, amely a Windows Server Active Directory-telepítés; a DirSync eszköz használatával szinkronizált támogatja pontosabban vagy szinkronizálja a jelszó-szinkronizálás beállítást, vagy beállította az Active Directory összevonási szolgáltatások (AD FS) összevonási szinkronizálja. Hozzon létre egy egyéni tartományt, amely megfelel az egyszerű Felhasználónévi tartomány utótag a helyszíni tartomány, és beállíthatja a címtár-integrációs szüksége.

További információt talál [Azure RemoteApp Active Directory konfigurálása](remoteapp-ad.md) .

Győződjön meg arról, hogy a tartomány megadottakat érvényesek, és a tartományvezérlőnek a a virtuális az Azure Remote alkalmazáshoz használt alhálózat létrehozott érhető el. Győződjön meg arról is számítógépek hozzáadása a megadott tartomány engedélye nyújtott szolgáltatás fiók hitelesítő adatait, és, hogy az Active Directory megadott név hozzárendelhető a DNS-Rekordokat a VNET megadott.

## <a name="what-domain-name-did-you-specify-when-you-created-your-collection"></a>Milyen tartománynevet adta meg a webhelycsoport létrehozása után? ##

A tartomány nevét, az Ön által létrehozott vagy hozzáadott belső tartománynév (nem Azure Active Directory tartományi neve) kell lennie, és felbontási DNS-formátumban (contoso.local) kell lennie. Például az Active Directory belső név (contoso.local) és az Active Directory UPN (contoso.com), – kell használnia a belső név, a webhelycsoport létrehozásakor.
