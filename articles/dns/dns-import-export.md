<properties
   pageTitle="Importálása és exportálása egy tartomány zónafájl Azure DNS használatával CLI |} Microsoft Azure"
   description="Megtudhatja, hogy miként importálása és exportálása egy DNS-zónafájl Azure DNS Azure CLI használatával"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importálás és exportálás DNS zone fájlt az Azure CLI


Ez a cikk azt ismerteti, hogy miként fájlok importálásához és exportálásához DNS zone Azure DNS használata az Azure CLI keresztül.

## <a name="introduction-to-dns-zone-migration"></a>DNS-zóna áttelepítése – bevezetés

A DNS-zónafájl egy szövegfájlt, amely tartalmazza az összes tartománynév (DNS) rekord a zónában részletes adatait. Egy szabványos formátum, így átvitele a DNS-rekordjait DNS-rendszer között alkalmas követi azt. Zóna fájl használatával módja a kész, megbízható és kényelmes és Azure DNS elhalványítása adhatja át a DNS-zóna.

Azure DNS támogatja az importálás és exportálás zóna fájlokat az Azure parancssori kezelőfelületről használatával. Az Azure CLI segédprogram platformok parancssori Azure szolgáltatások kezeléséhez használható. Érhető el a Windows, Mac és Linux platformokhoz készült [Azure letöltési oldalon](https://azure.microsoft.com/downloads/). Platformok támogatás különösen fontos az importálás és exportálás zóna fájlok, mivel a leggyakrabban használt név kiszolgálószoftver, [KÖTÉS](https://www.isc.org/downloads/bind/), általában az alábbiakon Linux.

## <a name="obtain-your-existing-dns-zone-file"></a>A meglévő DNS zone file beszerzése

Mielőtt a DNS-zónafájl importálása Azure DNS, szüksége lesz a zónafájlba másolatának beszerzése. Ez a fájl forrását, ahol jelenleg üzemeltetik a a DNS-zóna függ.

- Ha a DNS-zóna (például egy tartományregisztrálónál, a saját DNS-szolgáltatója vagy a alternatív felhő szolgáltató) partner szolgáltatás üzemelteti, szolgáltatás biztosítania kell az azt jelenti, hogy a DNS-zónafájl letöltése.

- A DNS-zóna Windows DNS üzemelteti, a zóna fájlok alapértelmezett mappája esetén **%systemroot%\system32\dns**. A teljes elérési útját, minden egyes zónafájlba is láthatók lesznek a DNS-szolgáltatás kezelőkonzol az **Általános** lapon.

- A DNS-zóna KÖTÉS használatával üzemelteti, ha zónák zóna-fájl helyét a KÖTÉS konfigurációs fájl **named.conf**van megadva.

**A go Daddynél zóna fájlok használata**<BR>
A go Daddynél letöltött zóna fájlokat kissé nem szabványos formátuma. A probléma elhárításához a zóna-fájlok importálása az Azure DNS előtt szükség. A rekordadat az egyes DNS-rekordot a DNS-nevek adható teljesen minősített neve, de nincs telepítve a egy végződő "." Ez azt jelenti, hogy más DNS-rendszerben, relatív névként értelmezi azokat. A záró hozzáfűzni a zónafájlba módosítani szeretne "." nevüket az Azure DNS importálás előtt.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>A DNS-zónafájl importálása az Azure DNS


Egy zónafájl importálása új zóna Azure DNS-ben Ha létrehoz egy még nem létezik. Ha a zóna már létezik, a rekord beállítása a zónafájlba kell a meglévő rekord beállítása egyesíti.

### <a name="merge-behavior"></a>Működés egyesítése

- Alapértelmezés szerint a meglévő és új rekord beállítása egyesíti. Egy egyesített rekordkészletben belül elemekről vonja ismétlődő.

- Másik lehetőségként megadásával a `--force` beállítást választja, az importálási folyamat lecseréli a meglévő rekord beállítása az új rekord beállítása. Meglévő rekord beállítása, hogy nem rendelkezik a megfelelő rekord beállítása a az importált zónafájlja nem távolítja el.

- Rekord beállítása egyesítésekor a time to live (TTL) a korábban létrehozott rekord beállítása használják. Ha `--force` van használt, használja a TTL (élettartam) az új rekord megadása.

- Hitelesítésszolgáltató (SOA) paraméterek elejére (kivéve `host`) kell venni az importált zónafájl, függetlenül attól, hogy mindig `--force` használják. Hasonlóképpen a névkiszolgáló-rekord beállítása a zóna csúcs, az a TTL (élettartam) mindig kérdezi le az importált zónafájl.

- Az importált CNAME rekord nem lecseréli egy meglévő CNAME rekord azonos nevű kivéve, ha a `--force` paramétert.

- Ütközés akkor történik, egy CNAME rekordot, és egy másik rekordhoz ugyanazt a nevet, de a különböző típusú (függetlenül attól, amely meglévő vagy új) között, ha a meglévő rekordba megőrződnek. Ez a független e `--force`.

### <a name="additional-information-about-importing"></a>További információ a importálása

A következő megjegyzések adja meg, további technikai részleteket a zóna importálási folyamat.

- A `$TTL` irányelv nem kötelező, és támogatja. Ha nincs `$TTL` irányelv számítható ki, nélküli egy explicit TTL (élettartam) rekordok importált beállítása alapértelmezett TTL (élettartam) 3600 másodpercet lesz. Ha két rekord azonos rekord megadása másik TTL értékkel adja meg, a kisebb értéket használja.

- A `$ORIGIN` irányelv nem kötelező, és támogatja. Nem `$ORIGIN` állítva, a használt alapértelmezett érték a zóna nevére a parancssorból a megadott módon (a záró plusz ".").

- A `$INCLUDE` és `$GENERATE` irányelvek nem támogatottak.

- E bejegyzéstípusok támogatottak: A, AAAA, CNAME, MX, NS, SOA, SRV és TXT.

- A SOA rekord automatikusan létrejön Azure DNS-zóna létrehozásakor. Zóna fájl importálásakor összes SOA paramétert kell venni a zóna fájl, *kivéve* a `host` paraméter. Ez a paraméter Azure DNS által megadott értéket használja. Ennek oka az, a paraméter az Azure DNS által biztosított elsődleges névkiszolgáló kell hivatkozniuk.

- A névkiszolgáló-rekord beállítása a zóna csúcs is automatikusan létrejön Azure DNS a zóna jön létre. Csak a TTL (élettartam) a rekordkészlet importálhatók. Ezeket a rekordokat Azure DNS által megadott név kiszolgálónevek tartalmazzák. A rekord adatainak nem írja felül az importált zónafájlja szereplő értékeket.

- Nyilvános villámnézetben Azure DNS támogatja a TXT rekordok csak egyetlen-karakterlánc. Karakterláncsoros TXT rekordok fog tömb összefűzéséből, és a program csak 255 karakterből állhat.

### <a name="cli-format-and-values"></a>CLI formátum és értékek


A DNS-zóna importálása az Azure CLI parancs formátuma:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Értékek:

- `<resource group>`az erőforráscsoport a nevét, az Azure DNS-zóna van.
- `<zone name>`az a név, a zóna.
- `<zone file name>`van az importálni kívánt zónafájlja elérési útját és nevét.

Ha egy ilyen nevű zóna az erőforráscsoport nem létezik, azt meg létrejön. Ha a zóna már létezik, az importált rekord beállítása egyesíti a meglévő rekord beállítása. Írja felül a meglévő rekord beállítása, használja a `--force` lehetőséget.

Ellenőrizheti egy zónafájl formátumának anélkül, hogy valójában importálja a fájlt, a `--parse-only` lehetőséget.

### <a name="step-1-import-a-zone-file"></a>Lépés: 1. A zone-fájl importálása

A zone **contoso.com**zóna fájl importálása.

1. Jelentkezzen be az Azure-előfizetése az Azure CLI használatával.

        azure login

2. Jelölje ki azt az előfizetést, amelyhez az új DNS-zóna létrehozásához.

        azure account set <subscription name>

3. Azure a DNS-az Azure csak erőforrás-kezelő szolgáltatást, így az Azure CLI kell lehet másikra váltani, erőforrás-kezelő módra.

        azure config mode arm

4. Az Azure DNS-szolgáltatást használ, regisztrálnia kell az előfizetés a Microsoft.Network erőforrás-szolgáltatót szeretne használni. (Ez a egyszeri műveletet az egyes előfizetések.)

        azure provider register Microsoft.Network

5. Ha nem egy már, is kell hozzon létre egy erőforrás-kezelő erőforrás csoportot.

        azure group create myresourcegroup westeurope

6. A parancsot a fájl **contoso.com.txt** a zóna **contoso.com** importálhat egy új, az erőforrás csoport **myresourcegroup**a DNS-zóna, `azure network dns zone import`.<BR>Ez a parancs a zónafájlba betöltése és elemzéséhez. A parancs a zóna létrehozása az Azure DNS-szolgáltatása hajtja végre parancsokat, és az összes a rekord beállítása a zónában. A parancs akkor is jelentést a, a konzol ablakban, és az esetleges hibákat vagy figyelmeztetések előrehaladását. Rekord beállítása az adatsor hoz létre, mert azt egy nagy zónafájlja importálása néhány percig is eltarthat.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Lépés: 2. Ellenőrizze a zóna

Ha ellenőrizni szeretné a DNS-zóna a fájl importálása után, használhatja az alábbi módszerek egyikét:

- A rekordok jeleníthet meg a következő Azure CLI parancs használatával.

        azure network dns record-set list myresourcegroup contoso.com

- A PowerShell-parancsmag használatával megtekintheti a rekordok listáját `Get-AzureRmDnsRecordSet`.

- Használható `nslookup` névfeloldás a rekordok ellenőrzése. A zone még nem meghatalmazott, mert szüksége lesz explicit módon adja meg a megfelelő Azure DNS-névkiszolgálókat. Az alábbi példa bemutatja, hogyan beolvasni a név kiszolgálónevek a zónába. Informatikai is szemlélteti, hogyan lehet a "www" rekord lekérdezés használatával `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>3 a lépést. Frissítse a DNS-delegálás

Hogy a zóna megfelelően importált ellenőrzését követően szüksége lesz frissítése a DNS-delegálás, hogy az Azure DNS névkiszolgálóira mutassanak. További információ a [frissítése a DNS-delegálás](dns-domain-delegation.md)témakört.

## <a name="export-a-dns-zone-file-from-azure-dns"></a>A DNS-zónafájl exportálása Azure DNS

A DNS-zóna importálása az Azure CLI parancs formátuma:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Értékek:

- `<resource group>`az erőforráscsoport a nevét, az Azure DNS-zóna van.
- `<zone name>`az a név, a zóna.
- `<zone file name>`az exportálandó a zónafájl elérési út/név.

A zone importálással, először kell bejelentkezni, válassza az előfizetés, és állítsa be az erőforrás-kezelő üzemmód Azure CLI.

### <a name="to-export-a-zone-file"></a>A zone fájl exportálása


1. Jelentkezzen be az Azure-előfizetése az Azure CLI használatával.

        azure login

2. Jelölje ki azt az előfizetést, amelyhez az új DNS-zóna létrehozásához.

        azure account set <subscription name>

3. Azure DNS egy Azure csak erőforrás-kezelő szolgáltatást. Az Azure CLI erőforrás-kezelő módra lehet másikra váltani.

        azure config mode arm

4. Erőforrás csoport **myresourcegroup** meglévő Azure DNS zone **contoso.com** (az aktuális mappában) fájl **contoso.com.txt** exportálásához futtatása `azure network dns zone export`. Ez a parancs az Azure DNS-szolgáltatás felsorolása rekord beállítása a zónában, és az eredmények exportálása egy kötési-kompatibilis zónafájl visszahívja.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

