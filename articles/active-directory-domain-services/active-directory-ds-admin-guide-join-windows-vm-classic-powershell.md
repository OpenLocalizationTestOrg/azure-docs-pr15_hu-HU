<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Felügyeleti útmutató |} Microsoft Azure"
    description="Virtuális gép Windows Azure PowerShell és a klasszikus telepítési modell felügyelt tartományhoz való csatlakozásra."
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>A Windows Server virtuális gép vegye fel a felügyelt tartományba PowerShell használatával

> [AZURE.SELECTOR]
- [Azure klasszikus portál – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [A PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure van két különböző telepítési modellekkel létrehozásáról és használatáról az erőforrások: [az erőforrás-kezelő és klasszikus](../resource-manager-deployment-model.md). Ez a cikk bemutatja, hogy a klasszikus telepítési modellt használja. Azure Active Directory tartományi szolgáltatások jelenleg nem támogatja az erőforrás-kezelő modell.

Ezeket a lépéseket megjelenítése egy sor olyan létre, és adja meg a Windows-alapú Azure virtuális gép egy építőelem megközelítés használatával előre Azure PowerShell-parancsait testreszabása. Az alábbi lépések segítségével összeállítása egy Azure virtuális Windows-alapú számítógépre, és vegye fel azt az Azure Active Directory tartományi szolgáltatások felügyelt tartományba.

Ezeket a lépéseket hajtsa végre a fill-a-a-üres megközelítés létrehozása az Azure PowerShell parancs úgy állítja be. Ez a módszer akkor lehet hasznos, ha új PowerShell vagy azt szeretné tudni, hogy milyen értékeket sikeres konfigurációs megadása. Speciális PowerShell-felhasználók készítése a parancsok és helyett a saját a változók értékeit (a "$" kezdetű vonalak).

Ha még nem tette meg, akkor [telepítése és konfigurálása az Azure PowerShell](../powershell-install-configure.md) használata a képernyőn megjelenő utasításokat Azure PowerShell telepítése a helyi számítógépen. Ezután nyissa meg a Windows PowerShell parancssor parancsot.

## <a name="step-1-add-your-account"></a>Lépés: 1: A fiók hozzáadása

1. A PowerShell parancssorba írja be a **Hozzáadás-AzureAccount** , majd kattintson a **meg az ENTER billentyűt**.
2. Írja be az e-mail címet az Azure-előfizetéséhez társított, és kattintson a **Tovább**gombra.
3. Írja be a fiókjához tartozó jelszót.
4. Kattintson a **Bejelentkezés**gombra.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Lépés: 2: Az előfizetést és a tárterület-fiók beállítása

Az Azure előfizetést és a tárterület-fiók beállítása a Windows PowerShell-parancssorában futtassa a ezek a parancsok. Mindent, ami az idézőjelekkel együtt cseréje a < és > karaktert, a megfelelő neveket.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

A kimenet: a **Get-AzureSubscription** parancs a SubscriptionName tulajdonságból elérheti a megfelelő előfizetés nevére. A megfelelő tároló fióknév érheti el a felirat tulajdonság a kimenet: a **Get-AzureStorageAccount** parancs, a **Kijelölés-AzureSubscription** parancs futtatása után.


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Lépés 3: Részletes útmutató – a virtuális gép kiépítése, és vegye fel azt a felügyelt tartományba
Az alábbiakban a megfelelő Azure PowerShell-parancsot a virtuális számítógéphez készített üres sorok között minden blokk olvashatóság érdekében.

Adja meg a Windows virtuális gépen kell építenie adatait.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

D-, DS vagy G-sorozat virtuális gépeken futó InstanceSize értékének olvassa el a [virtuális gép és felhőalapú szolgáltatást méretű az Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx)című témakört.

A felügyelt tartomány információt nyújtanak.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Adja meg a nevét a felhőalapú szolgáltatást.

    $svcname="Contoso100-test"

Adja meg a nevét a virtuális hálózat, amelyhez a virtuális összekapcsolhatók. Győződjön meg arról, hogy a AAD-felügyelt tartományi érhető el a virtuális hálózat.

    $vnetname="MyPreviewVnet"

Jelölje ki a virtuális képet a virtuális létrehozására használható.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Állítsa be a virtuális - virtuális nevét, a példány méret és a használandó képet.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

A virtuális szerezze be a rendszergazdai hitelesítő adataival. Válassza a helyi rendszergazdai erős jelszóval.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Szerezze be a "AAD Adatközpont rendszergazdák" csoport virtuális csatlakozik a felügyelt tartományhoz tartozó felhasználói fiók hitelesítő adatait. Nem adja meg a tartomány nevét – például ebben a példában, azt adja meg a "Péter" a felhasználó nevét.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Állítsa be a virtuális – adja meg a tartomány illesztés követelménynek és a szükséges hitelesítő adatokat.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

A virtuális alhálózat beállítása

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Nem kötelező: Mutasson a tartomány IP-címét. Ha az Azure Active Directory tartományi szolgáltatások felügyelt tartomány a DNS-kiszolgálóiról az virtuális hálózati kell az IP-címek, ebben a lépésben nincs szükség.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Most nyújtása a tartományhoz tartozó Windows virtuális.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>A Windows virtuális kiépítése, és automatikusan csatlakozzon az AAD felügyelt tartomány parancsfájl
A PowerShell-parancsot beállítása létrehoz egy virtuális gépet, a vállalati verzió-kiszolgáló, amely:

- A Windows Server 2012 R2 adatközponthoz képet használja.
- Van egy kis extra virtuális gépen.
- A név contoso-próba foglalja magában:
- Automatikusan tartomány tartományhoz a contoso100 felügyelt.
- Megjelenik a felügyelt tartomány azonos virtuális hálózaton.

Az alábbiakban a teljes mintaparancsfájl létrehozása a Windows virtuális gép, és automatikusan csatlakozzon az Azure Active Directory tartományi szolgáltatások felügyelt tartományt.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Kapcsolódó tartalom
- [Azure Active Directory tartományi szolgáltatások – első lépések útmutató](./active-directory-ds-getting-started.md)

- [Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány felügyelete](./active-directory-ds-admin-guide-administer-domain.md)
