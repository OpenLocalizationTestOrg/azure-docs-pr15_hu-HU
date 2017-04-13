<properties
   pageTitle="Példa DMZ – összeállítása NSGs együtt egy egyszerű DMZ |} Microsoft Azure"
   description="A DMZ hálózati a biztonsági csoportok (NSG) összeállítása"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>1 – például egy egyszerű DMZ a NSGs összeállítása

[Lépjen vissza a biztonsági oszlopazonosító ajánlott eljárások a lapra][HOME]

Ez a példa hoz létre egy egyszerű DMZ négy windows-kiszolgálók és hálózati biztonsági csoportokat. Minden egyes lépés egy mélyebb megértése megadására vonatkozó parancsok keresztül is azt ismerteti. Is van egy mélyreható megadására forgalom forgatókönyv szakasz részletes hogyan a forgalmat a DMZ a védelmet a rétegek végighalad. Végül a hivatkozás szakaszban az teljes kódot és útmutatás a környezet ellenőrzéséhez és kísérletezhet a különböző forgatókönyvekben össze. 

![A NSG bejövő DMZ][1]

## <a name="environment-description"></a>Környezet leírása
Ebben a példában az előfizetés, amely tartalmazza az alábbiakat:

- Két cloud services: "FrontEnd001" és "BackEnd001"
- Virtuális hálózati, "CorpNetwork", a két alhálózat; "FrontEnd" és "Kódmentes"
- Mindkét alhálózathoz alkalmazott hálózati biztonsági csoport
- A Windows Server, amely az alkalmazás webkiszolgáló ("IIS01")
- Vissza az alkalmazás megjelenítő két windows-kiszolgálók befejezése servers ("AppVM01", "AppVM02")
- A Windows server, amely a DNS-kiszolgáló ("DNS01")

A hivatkozások szakaszban alatti van egy PowerShell-parancsprogramot, amely a fentebb ismertetett környezet a legtöbb gyűjt. A VMs és virtuális hálózatok épület ugyan a példában parancsfájl által végzett nem témakörben olvashat részletesen a dokumentumban. 

A környezet; összeállításához

  1.    A hivatkozások szakaszban szereplő hálózati beállítások XML-fájl mentése (frissíti neve, hely és IP-címek a megfelelő az adott esetben)
  2.    A környezet a parancsfájl van (az előfizetések, szolgáltatásnevek stb.) futtassanak megfelelően a parancsfájl felhasználói változók frissítése
  3.    A parancsprogram végrehajtása a PowerShell

**Megjegyzés**: A régió esetében a PowerShell-parancsprogramot, meg kell egyeznie a hálózati konfigurációja XML-fájlban szereplő.

Miután sikeresen további fut, a parancsfájl választható lépések lehet venni, a hivatkozások szakaszban beállíthatja a webkiszolgáló és az alkalmazás kiszolgáló engedélyezése ebben a konfigurációban DMZ tesztelése egyszerű webalkalmazás két parancsfájlok.

A következő szakaszokban fog hálózati biztonsági csoportokat és működésének ebben a példában az útmutató a PowerShell-parancsprogramot kulcs sornyi alapján által részletes leírását.

## <a name="network-security-groups-nsg"></a>Hálózati biztonsági csoportok (NSG)
Ebben a példában NSG csoport összeállítása és majd betöltött hat szabályokkal. 

>[AZURE.TIP] Általánosságban elmondható először kell az adott "Engedélyezése" szabályok létrehozása, és kattintson a további általános "Megtagadás" szabályokat utolsó. A hozzárendelt prioritás megkövetel, amelyek szabályok értékeli ki az első. Miután forgalom kiderül, hogy egy adott szabályt szeretne alkalmazni, nincs további szabályok értékeli ki. NSG szabályok vagy a bejövő és kimenő irányba (az alhálózathoz szemszögéből) alkalmazhat.

Deklaratív a következő szabályok éppen készültek bejövő:

1.  Engedélyezve van a belső DNS-forgalmat (port 53)
2.  RDP-forgalmat (port 3389) az internetről bármely virtuális engedélyezett
3.  HTTP (80-as port) az internetről forgalom webkiszolgálóra (IIS01) engedélyezett.
4.  Bármely (az összes portok) érkező forgalmat IIS01 AppVM1 az engedélyezett
5.  Minden forgalom (az összes portok) az internetről a teljes megtagadva VNet (mindkét alhálózat)
6.  Bármely (az összes portok) érkező forgalmat a Frontend alhálózat Kódmentes az alhálózathoz van megtagadja

A következő szabályok kötött minden alhálózathoz, ha a HTTP-kérelmek volt az internetről bejövő az érintett webkiszolgálóra, mindkét szabályok 3 (lehetővé teszi) és 5 (megtagadja) szeretne alkalmazni, de mivel szabály 3 prioritása egy újabb csak ezt szeretné alkalmazni, és 5 szabály nem kellene kerülhet play. Így a HTTP-kérés szeretné tenni az érintett webkiszolgálóra. Ha ugyanazt a forgalmat a DNS01 kiszolgáló elérésére próbált, alkalmazásához az első szabály 5 (Megtagadás) a következő lesz, és szeretné átadni a kiszolgáló nem engedélyezi a forgalmat. Szabály 6 (Megtagadás) blokkolja a Frontend alhálózat (kivéve az 1 és 4 szabályok engedélyezett forgalom) Kódmentes alhálózathoz beszél, ez védi az Kódmentes hálózati esetben egy támadó kompromisszumok lenne a webalkalmazás on Frontend, a támadó korlátozott hozzáférést a Kódmentes "védett" hálózathoz (csak az erőforrások, a AppVM01 kiszolgálón elérhetővé tett).

Van olyan alapértelmezett kimenő szabályt, amely lehetővé teszi, hogy a forgalmat ki az internethez. Ebben a példában azt is lehetővé teszi a kimenő forgalmának és nem az minden kimenő szabályok módosításával. Mindkét irányba forgalom zárolásához, felhasználó által definiált útválasztás szükséges, ez az megismerkedett a"3" alatt.

Szabályok tárgyalt részletesebben az alábbi képlettel történik (**Megjegyzés**: bármelyik eleménél a egy dollárjelet kezdődő listája alatt (pl.: $NSGName) a dokumentum hivatkozás szakaszában a forgatókönyv a felhasználó által definiált változó):

1. Először hálózati biztonsági csoport kell beépített tartása a szabályok:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  Ebben a példában az első szabály lehetővé teszi a DNS-forgalmat a DNS-kiszolgálóhoz, a kódmentes alhálózat belső hálózatok között. A szabály néhány fontos paraméter foglalja magában:
  - "Írja be a" azt jelzi, hogy milyen irányban forgalom Ez a szabály lép érvénybe, hogy; Ez a szemszögéből az alhálózathoz vagy a virtuális gép (attól függően, hogy hol e NSG van kötve). Így ha típus "Bejövő" és forgalom az alhálózathoz ír be, a szabályt szeretne alkalmazni és a forgalom elhagyja a alhálózathoz nem érinti a szabály által.
  - "Prioritás" állítja be a sorrendben, amelyben a forgalom adatfolyam kiértékelésre kerül. Minél kisebb számot annál nagyobb a prioritás. Amint egy adott adatforgalom folyamat egy szabály vonatkozik, nincs további szabályok feldolgozásának. Így ha prioritású 1 szabály lehetővé teszi, hogy a forgalmat, és a 2 prioritású szabály letiltja a forgalmat, és mindkét szabályok vonatkoznak a forgalmat, majd a forgalom flow szeretné tenni (1 szabály volna magasabb prioritást óta effektus tartott, és nincsenek további szabályok alkalmazott).
  - "Művelet" karakter, ha ez a szabály által érintett forgalom tiltott vagy engedélyezett.

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  Ez a szabály lehetővé teszi az internetről vagy a VNET a alhálózat bármely kiszolgáló RDP-portjához flow RDP-forgalmat. Ez a szabály cím prefixumokban; speciális kétféle használ. "VIRTUAL_NETWORK" és "INTERNET". Az egyszerűen címet a cím prefixumokban nagyobb kategóriát.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  Ez a szabály lehetővé teszi, hogy az érintett webkiszolgálóra találati bejövő internetes forgalmat. Ez nem szabályalkalmazási útválasztási; csak IIS01 átadni adatforgalmat teszi lehetővé. Így ha az internetről forgalom az érintett webkiszolgálóra volna, mint a címzetthez ezt a szabályt szeretne engedélyezze azt, és további szabályok feldolgozásának leállítása. (A szabály prioritású 140 az összes többi bejövő internetes forgalmat le van tiltva). HTTP-forgalmat feldolgozása csak akkor esetén, ha ez a szabály sikerült további korlátozódik Tevékenységfrissítések engedélyezése csak a 80-as Port cél.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  Ez a szabály lehetővé teszi, hogy a forgalom a IIS01 kiszolgálóról számára a AppVM01 kiszolgáló, egy újabb szabály megakadályozza az összes többi Frontend Kódmentes forgalmat. Ez a szabály javítása, ha a port ismert fel kell venni. Ha például az IIS-kiszolgálón található szerezze AppVM01 csak SQL Server, ha a célként megadott porttartományt módosítani kell a "*" (bármely) 1433 (SQL port), így a AppVM01 kisebb bejövő homonimaszerű webcímmel felület kell a webalkalmazás minden eddiginél kell veszélyezteti az.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Ez a szabály azokat a kiszolgálókat, a hálózaton történő forgalom megtagadja az internetről. A szabály prioritású 110 és 120 együtt lehetővé teszi, hogy csak a bejövő internetes forgalmat a tűzfal és más kiszolgálókon és megakadályozza RDP-portok milyen egyéb. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  A végleges szabály forgalmat a Frontend alhálózat Kódmentes az alhálózathoz letiltja. Mivel ez egy bejövő csak szabályt, fordított forgalom engedélyezett (a a Kódmentes a Frontend szeretné).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Adatforgalom felhasználási területei
#### <a name="allowed-web-to-web-server"></a>(*Engedélyezett*) Webes való webkiszolgálón
1.  Internetes felhasználói kérések HTTP lap a FrontEnd001.CloudApp.Net (internetes szemben lévő felhőalapú szolgáltatás)
2.  Szolgáltatás fázisai forgalom felé IIS01 80-as port nyitva végponton az a felhő (az érintett webkiszolgálóra)
3.  Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (DNS) nem vonatkoznak, Ugrás a következő szabályok
  2.    NSG szabály 2 (RDP) nem alkalmazható, Ugrás a következő szabályok
  3.    NSG szabály 3 (IIS01 interneten) alkalmazhat, forgalom engedélyezett, leállítása a szabály feldolgozása
4.  Adatforgalom találatok belső IP-címét az érintett webkiszolgálóra IIS01 (10.0.1.5)
5.  IIS01 figyel az internetes forgalmat, megkapja a kérelmet, és elindítja a kérelem feldolgozása
6.  IIS01 kéri az SQL Server AppVM01 vonatkozó információk
7.  Nincs kimenő szabályok Frontend alhálózat forgalom engedélyezve
8.  A Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (DNS) nem vonatkoznak, Ugrás a következő szabályok
  2.    NSG szabály 2 (RDP) nem alkalmazható, Ugrás a következő szabályok
  3.    NSG szabály 3 (a tűzfal Internet) nem vonatkoznak, Ugrás a következő szabályok
  4.    NSG szabály 4 (IIS01 AppVM01 való) alkalmazása forgalom van engedélyezve, szabály feldolgozás leállítása
9.  AppVM01 az SQL-lekérdezés fogadása, és válaszoljon
10. Mivel nincsenek kimenő szabályok az Kódmentes alhálózat engedélyezve van-e az adott válasz
11. Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    Nincs bejövő alkalmazott NSG szabály Frontend az alhálózathoz, így a NSG egyik szabály alkalmazása a Kódmentes alhálózat érkező forgalmat
  2.    Az alapértelmezett rendszer szabály lehetővé teszi a forgalom alhálózat között lehetővé válik a forgalom, így engedélyezi a forgalmat.
12. Az IIS-kiszolgáló megkapja a SQL választ, és a HTTP-válasz befejeződik, és elküldi a lekérdező
13. Mivel nincsenek olyan nem kimenő szabályok az Frontend alhálózat a kérdésre adott választ engedélyezve van, és az Internet felhasználó megkapja az weblapon kért.

#### <a name="allowed-rdp-to-backend"></a>(*Engedélyezett*) A Kódmentes RDP
1.  Internetes kiszolgáló rendszergazdája kér AppVM01 a BackEnd001.CloudApp.Net:xxxxx xxxxx esetén a véletlenszerűen hozzárendelt portszámot (a hozzárendelt port az Azure-portálon vagy található PowerShell) AppVM01 való RDP-munkamenet RDP
2.  Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (DNS) nem vonatkoznak, Ugrás a következő szabályok
  2.    NSG szabály 2 (RDP) alkalmazhat, forgalom engedélyezett, leállítása a szabály feldolgozása
3.  Nincs kimenő szabályokkal alapértelmezett szabályokat alkalmazni, és visszatérő forgalom engedélyezett
4.  RDP-munkamenet engedélyezése
5.  AppVM01 kér, felhasználónév jelszó

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Engedélyezett*) Webes Server DNS keresési DNS-kiszolgálón
1.  Webes kiszolgálón, IIS01, egy adatcsatorna www.data.gov az igényeinek, de a cím feloldásához igényeinek.
2.  A hálózati konfigurálásról a VNet listák DNS01 (az Kódmentes alhálózat 10.0.2.4), az elsődleges DNS-kiszolgálót, IIS01 küld a DNS-kérés DNS01
3.  Nincs kimenő szabályok Frontend alhálózat forgalom engedélyezve
4.  Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.     NSG szabály 1 (DNS) alkalmazhat, forgalom engedélyezett, leállítása a szabály feldolgozása
5.  DNS-kiszolgáló megkapja a kérelmet
6.  DNS-kiszolgáló nem tartozik a gyorsítótárazott címet és a program rákérdez a legfelső szintű DNS-kiszolgálót az interneten
7.  Nincs kimenő szabályok Kódmentes alhálózat forgalom engedélyezve
8.  Az Internet a DNS server válaszol, mivel ebben a munkamenetben belső kezdeményezett, a válasz engedélyezve van
9.  DNS-kiszolgáló gyorsítótárát a választ, és az eredeti értekezlet-összehívást vissza IIS01 válaszol
10. Nincs kimenő szabályok Kódmentes alhálózat forgalom engedélyezve
11. Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    Nincs bejövő alkalmazott NSG szabály Frontend az alhálózathoz, így a NSG egyik szabály alkalmazása a Kódmentes alhálózat érkező forgalmat
  2.    Az alapértelmezett rendszer szabály lehetővé teszi a forgalom alhálózat közötti lehetővé teszi a forgalom, hogy engedélyezi-e a forgalmat
12. IIS01 a kérdésre adott választ kap a DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Engedélyezett*) Webkiszolgáló access-fájlt a AppVM01
1.  Egy fájlt a AppVM01 kér IIS01
2.  Nincs kimenő szabályok Frontend alhálózat forgalom engedélyezve
3.  A Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (DNS) nem vonatkoznak, Ugrás a következő szabályok
  2.    NSG szabály 2 (RDP) nem alkalmazható, Ugrás a következő szabályok
  3.    NSG szabály 3 (IIS01 interneten) nem vonatkoznak, Ugrás a következő szabályok
  4.    NSG szabály 4 (IIS01 AppVM01 való) alkalmazása forgalom van engedélyezve, szabály feldolgozás leállítása
4.  AppVM01 megkapja a kérelmet, és válasza egy fájlt (feltételezve, hogy a hozzáférés engedélyezése)
5.  Mivel nincsenek kimenő szabályok az Kódmentes alhálózat engedélyezve van-e az adott válasz
6.  Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    Nincs bejövő alkalmazott NSG szabály Frontend az alhálózathoz, így a NSG egyik szabály alkalmazása a Kódmentes alhálózat érkező forgalmat
  2.    Az alapértelmezett rendszer szabály lehetővé teszi a forgalom alhálózat között lehetővé válik a forgalom, így engedélyezi a forgalmat.
7.  Az IIS-kiszolgáló megkapja a fájlt

#### <a name="denied-web-to-backend-server"></a>(*Megtagadva*) Kódmentes Server webhely
1.  Internetes felhasználó megpróbál egy fájlt a AppVM01 hozzáférni a BackEnd001.CloudApp.Net szolgáltatáson keresztül
2.  Vannak nyitva fájlmegosztás végpontok, mivel ez nem kellene át a felhőalapú szolgáltatást, és nem jutnak el a kiszolgálóra
3.  Ha valamilyen okból a végpontok megnyitása, NSG szabály 5 (VNet interneten) megakadályozza a forgalom

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Megtagadva*) Webes DNS keresése a DNS-kiszolgálón
1.  Az Internet-felhasználó megpróbálja megkeresni egy belső DNS-rekordot a DNS01 a BackEnd001.CloudApp.Net szolgáltatáson keresztül
2.  Mivel nincsenek olyan nyissa meg a DNS-végpontok, ez nem kellene át a felhőalapú szolgáltatást, és nem jutnak el a kiszolgálóra
3.  Ha valamilyen okból a végpontok megnyitása, NSG szabály 5 (VNet interneten) megakadályozza a forgalom (Megjegyzés: két okból nem kell alkalmazni, hogy a szabály 1 (DNS), először a forrás címét-e az internethez, ez a szabály csak a helyi VNet forrásaként vonatkozik, ez a egy engedélyezése szabály, így soha nem letiltsa azt a forgalom)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Megtagadva*) Web Access SQL a tűzfalon keresztül
1.  Internetes felhasználó SQL-adatokat kér FrontEnd001.CloudApp.Net (internetes szemben lévő felhőalapú szolgáltatás)
2.  Mivel nincsenek végpontok olyan nyissa meg az SQL, ez nem kellene át a felhőalapú szolgáltatást, és nem éri a tűzfalon keresztül
3.  Ha valamilyen oknál fogva végpontok megnyitott, a Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (DNS) nem vonatkoznak, Ugrás a következő szabályok
  2.    NSG szabály 2 (RDP) nem alkalmazható, Ugrás a következő szabályok
  3.    NSG szabály 3 (IIS01 interneten) alkalmazhat, forgalom engedélyezett, leállítása a szabály feldolgozása
4.  Adatforgalom találatok belső IP-címét a IIS01 (10.0.1.5)
5.  IIS01 nem figyel port 1433, így nem válaszol a kérés

## <a name="conclusion"></a>Elfogadásáról
A bejövő vissza vége alhálózat elkülönítése viszonylag egyszerű és egyenes előre módja: az.

További példákat és a hálózati biztonsági határai áttekintése megtalálható [Itt][HOME].

## <a name="references"></a>Hivatkozások
### <a name="main-script-and-network-config"></a>Fő parancsfájl, és a hálózati beállítások
Mentse a teljes parancsfájl PowerShell parancsprogramot fájlban. Mentse a hálózati Config "NetworkConf1.xml" nevű fájlt.
Szükség szerint módosítsa a felhasználó által definiált változók. Futtassa a, majd kövesse a fenti példa 1 szakaszban lévő tűzfal szabály beállítási útmutatás.

#### <a name="full-script"></a>Teljes parancsfájl
Ez a parancsfájl fog a felhasználó által definiált változók; alapján

1.  Csatlakozás az Azure előfizetéssel
2.  Tárterület új fiók létrehozása
3.  Hozzon létre egy új VNet és a hálózati konfigurációs fájl a megadott két alhálózat
4.  4 a windows server VMs összeállítása
5.  Állítsa be a NSG együtt:
  - Egy NSG létrehozása
  - Ez a szabályok feltöltése
  - A megfelelő alhálózat a NSG kötése

A PowerShell-parancsprogramot kell futtatni a helyi meghajtóra internetes PC-re vagy a kiszolgáló csatlakozik.

>[AZURE.IMPORTANT] Ha ezt a parancsfájlt, lehetnek figyelmeztetések vagy más PowerShell videogaléria tájékoztató üzenetek. Piros csak hibaüzenetek aggodalomra.


    <# 
     .SYNOPSIS
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.5
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"

    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #   

      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
      

#### <a name="network-config-file"></a>Hálózati konfigurációs fájl
Mentse az XML-fájl frissített helyét, és a hivatkozás elhelyezése a $NetworkConfigFile változó a fenti parancsfájl ezt a fájlt.
    
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Minta alkalmazás parancsfájlok
Ha szeretne egy minta alkalmazás telepítése a jelenlegi és más DMZ példák egyet az alábbi hivatkozásra kapott: [Alkalmazás mintaparancsfájl][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "A NSG bejövő DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

