<properties
   pageTitle="Hozzon létre egy DNS-zóna CLI használatával |} Microsoft Azure"
   description="Megtudhatja, hogy miként Azure DNS DNS-zónák indítása CLI használatával tartománya DNS-annak részletes létrehozása"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Hozzon létre egy Azure DNS-zóna CLI használatával


> [AZURE.SELECTOR]
- [Azure portál](dns-getstarted-create-dnszone-portal.md)
- [A PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


Ez a cikk azt ismerteti, hogy a műveletek elvégzéséhez hozhat létre a DNS-zóna CLI használatával. A DNS-zóna PowerShell alrendszerrel vagy az Azure-portálra is létrehozhat.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Első lépések

Ezeket az utasításokat a Microsoft Azure CLI használja. Ügyeljen arra, hogy a legújabb Azure CLI frissítése (0.9.8 vagy újabb) az Azure DNS parancsok használata. Típus `azure -v` ellenőrizni, hogy melyik Azure CLI verziót a számítógépre telepítve van.

## <a name="step-1---set-up-azure-cli"></a>Lépés: 1 – Azure CLI beállítása

### <a name="1-install-azure-cli"></a>1. Telepítse az Azure CLI

A Windows Azure CLI, Linux vagy Mac telepítése Az alábbi lépéseket kell elvégezni, mielőtt Azure DNS Azure CLI használatával kezelheti. További információ [telepítse az Azure CLI](../xplat-cli-install.md)címen érhető el. A DNS-parancsok megkövetelése Azure CLI verzió 0.9.8 vagy újabb verziójában.

Az összes a hálózati szolgáltató vonatkozó parancsok CLI lehet található, a következő parancsot:

    azure network

### <a name="2-switch-cli-mode"></a>2. a kapcsoló CLI mód

Azure DNS Azure erőforrás-kezelő használja. Ellenőrizze, hogy CLI mód ARM parancsok használata vált.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. Jelentkezzen be az Azure-fiók

A rendszer kéri a hitelesítő adatokkal hitelesítést végezni. Ne feledje, hogy csak használhatja fejlesztés fiókok.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Válassza azt az előfizetést

A használandó Azure előfizetések kiválasztása.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. erőforráscsoport létrehozása

Azure erőforrás-kezelő igényel, hogy az összes erőforráscsoport adjon meg egy helyet. Ez az erőforrások erőforráscsoport az alapértelmezett helye szolgál. Jó helyen jár, mivel összes DNS-erőforrás a globális és nem regionális, erőforrás csoport helyét választható nincs hatással van Azure DNS.

Kihagyhatja ezt a lépést, ha egy meglévő erőforráscsoport használja.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. register

Az Azure DNS-szolgáltatás a Microsoft.Network erőforrás szolgáltató kezeli. Azure-előfizetése van szüksége, ez az erőforrás-szolgáltató használata az Azure DNS használata előtt regisztrálását. Az egyes előfizetés egyszeri műveletet.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Lépés: 2 - hozzon létre egy DNS-zóna

A DNS-zóna alapján hoz létre a `azure network dns zone create` parancsot. Tetszés szerint hozhat létre a DNS-zóna címkék együtt. Címkék neve – érték párokká listáját, és Azure-kezelő által címke erőforrásokhoz számlázási vagy csoportosítási célokat szolgálnak. További információt a címkék [használata címkék rendszerezheti az Azure erőforrások](../resource-group-using-tags.md)megtekintése

Az Azure DNS zone nevét kell megadni, egy megszakítása nélkül **"."**. Például: "**a contoso.com**" helyett "**contoso.com.**".


### <a name="to-create-a-dns-zone"></a>A DNS-zóna létrehozása

Az alábbi példában hoz létre a DNS-zóna *contoso.com* nevű *MyResourceGroup*néven erőforrás csoportjában található.

A példa használatával hozzon létre a DNS-zóna, helyettesítése a saját értékeit.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>A DNS-zóna és a címkék létrehozása.

Azure DNS CLI támogatja a DNS-zónák a megadott használatával a választható címkék *-címke* paraméter. A következő példa bemutatja, hogyan szeretné a DNS-zóna két címkék, a project = bemutató és boríték = teszt.

Használja az alábbi példában hozhat létre a DNS-zóna és a címkék, helyettesítése a saját értékeit.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Rekordok megtekintése

Is létrehozásához a DNS-zóna hozza létre a következő DNS-rekordokat:

- A "Start a SOA" (jogosultság) rekord. Ez a bemutató minden DNS-zóna gyökérszintjén.

- A mérvadó névkiszolgálói (NS) rekordok. Ezek megjelenítése, mely névkiszolgálók üzemeltet a zóna. Azure DNS névkiszolgálók erőforráskészlethez tartozik használja, és így más névkiszolgálók rendelhetők különböző zónákba Azure DNS-ben. [A tartomány Azure DNS meghatalmazott](dns-domain-delegation.md) talál további információt.

Ezeket a rekordokat tekintheti `azure network dns-record-set show`.<BR>
*Szintaxis: hálózati dns rekordkészlet megjelenítése <-erőforráscsoport >< dns-zóna-neve > <name><type>*


Az alábbi példában, ha a parancsot futtatja az erőforrás csoport *myresourcegroup*rekord nevének megadása *"@"* (a legfelső szintű rekordot), és írja be a *SOA*, eredményez, hogy a következő kimenet:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
A Névkiszolgáló-rekordok létrehozása a zóna a megtekintéséhez használja az alábbi parancsot:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Rekord beállítása a legfelső szintű (vagy a *csúcs*) a DNS-zóna használati **@** szerint a rekord nevét.

## <a name="test"></a>Teszt

A DNS-zóna tesztelheti nslookup, ALAPOSABBAN meg, például a DNS-eszközök használatával vagy az `Resolve-DnsName` PowerShell-parancsmag.

Hogy Ön még nem még a tartomány használata az új zóna Azure DNS-ben, irányítsa át a DNS-lekérdezés közvetlenül az egyik a Névkiszolgálók a zóna szüksége. A Névkiszolgálók a zóna a Névkiszolgáló-rekordok szerepelnek, mint "azure hálózati dns-rekordkészlet megjelenítése" fent felsorolt. Ügyeljen a helyette az alábbi parancsot a zóna vonatkozó helyes értékek.

Az alábbi példában ALAPOSABBAN meg a névkiszolgáló a DNS-zóna rendelt használatával tartomány contoso.com lekérdezéséhez. A lekérdezés tartalmaz, mutasson a névkiszolgáló, amely azt használni * @ * és ALAPOSABBAN zóna nevét.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Következő lépések

Miután létrehozta a DNS-zóna, létrehozása [rekord beállítása és a rekordok](dns-getstarted-create-recordset-cli.md) névfeloldás tartománya internetes indításához.
