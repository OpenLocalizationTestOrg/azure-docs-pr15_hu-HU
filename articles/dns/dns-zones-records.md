<properties 
   pageTitle="DNS-zónák és a rekordok |} Microsoft Azure" 
   description="DNS-zónák és a Microsoft Azure DNS-rekordok elhelyezésére támogatás áttekintése." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS-zónák és rekordok

Ezen az oldalon a tartományok, a DNS-zónák, és a DNS-rekordok és a rekord beállítása és a hogyan támogatja őket az Azure DNS-alapfogalmak ismerteti.

## <a name="domain-names"></a>A tartománynevek
 
A Domain Name System-tartományok hierarchia. A hierarchia elindítja a "" gyökértartomány, amelynek neve egyszerűen**.**.  Alatta a legfelső szintű tartományok, például "com", "nettó", "szervezeti", "Egyesült Királyság" vagy "jp" származnak.  Ezek alatti második szintű tartományok, például "org.uk" vagy "co.jp" is. A tartomány DNS-hierarchiában globálisan terjesztette, a világon a DNS-névkiszolgálók által üzemeltetett.

Tartományregisztrálótól a szervezet, amely lehetővé teszi, hogy egy tartománynevet (például "contoso.com)" vásárolhat.  Tartománynév beszerzése lépve kattintva állítsa be a DNS-hierarchiában a névvel például lehetővé teszi, hogy közvetlenül a neve "www.contoso.com" a vállalati webhely jobbra. A tartományregisztráló a tartományt az Ön nevében a saját névkiszolgálók is tartalmazhat, vagy azt is megteheti adhatja meg helyettesítő névkiszolgálók.

Azure DNS egy kiszolgáló tárolni a tartomány használható elosztott globálisan, a magas rendelkezésre állás nevet infrastruktúrát biztosít. A tartományok Azure DNS-szolgáltatója, kezelheti a többi Azure szolgáltatás, a DNS-rekordjait a ugyanazokkal a hitelesítő adatokkal, API, eszközök, számlázási és támogatási.

Azure DNS jelenleg nem támogatja vásárlásával tartománynevet. Ha meg szeretné vásárolni a tartománynév, szüksége egy külső tartománynév-regisztrálót. A tartományregisztráló fog általában díjat kis éves. A tartományok a Azure DNS majd tárolni a DNS-rekordok kezelésére. Lásd: [a tartomány Azure DNS meghatalmazott](dns-domain-delegation.md) további információt.

## <a name="dns-zones"></a>DNS-zónák 

A DNS-zóna használható egy adott tartomány DNS-rekordjait üzemelteti. Indítsa el a tartomány Azure DNS-szolgáltatója, az adott tartománynév DNS-zóna létrehozásához szükséges. A tartomány összes DNS-rekord kattintson a DNS-zóna belül jön létre. 

A tartomány "a contoso.com például több DNS-rekordjait, például"mail.contoso.com"(a levelezési kiszolgáló) és a"www.contoso.com"(a webhely) is tartalmazhatnak.

A DNS-zóna létrehozása az Azure DNS-ben, a zóna nevét az erőforráscsoport belül egyedinek kell lennie. A zone azonos nevű felhasználhassa őket egy másik erőforráscsoport vagy egy másik Azure-előfizetést. Ha több zónák megosztása ugyanazt a nevet, egyes példányok van-e hozzárendelve más néven kiszolgálói címeket. Címek csak egy csoportja a tartományregisztrálónál beállíthatók.

>[AZURE.NOTE] Nem kell a saját tartománynév létrehozásához a DNS-zóna Azure DNS-ben a tartománynevet. Azonban kell Öné a tartomány konfigurálása az Azure DNS-névkiszolgálók, a megfelelő névkiszolgálóinak a tartomány nevét a tartományregisztrálónál.

További tudnivalókért olvassa el a [meghatalmazott Azure DNS a tartomány](dns-domain-delegation.md)című témakört.

## <a name="dns-records"></a>DNS-rekordok

### <a name="record-types"></a>Bejegyzéstípusok

Minden DNS-rekordot a név és típus tartalmaz. Rekordok különböző típusú adatokat tartalmaznak szerint vannak tagolva. A leggyakoribb típus is az "A" rekord, amely IPv4-cím nevét. Egy másik közös típus egy "MX" rekord, amely a levelezési kiszolgáló nevét.

Azure DNS közös DNS-bejegyzés típusainak támogatja: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV és TXT.

### <a name="record-names"></a>Rekord neve

Az Azure DNS-rekordok relatív nevük segítségével adható. Egy *teljes* tartománynevét (FQDN) tartalmazza a zóna neve, mivel azonban nem *relatív* nevét. A relatív rekord neve "www" a "contoso.com" zónában, például a teljesen minősített rekord neve "www.contoso.com" adja vissza.

Egy *csúcs* rekordot a DNS-rekordot a legfelső szintű (vagy a *csúcs*) áll, a DNS-zóna. Például a DNS-zóna "contoso.com" egy csúcs rekord szintén teljesen minősített neve (is nevezik *nyílt* tartomány) contoso.com.  Kiállítás, a relatív név szerint '@' csúcs rekordok létrehozására szolgál.

### <a name="record-sets"></a>Rekord beállítása
Előfordul, hogy meg kell megadott név és típus egynél több DNS-rekord létrehozásához. Tegyük fel, hogy a "www.contoso.com" webhelyére a két különböző IP-címek üzemelteti. A webhely szükséges két különböző A rekordok, egyik, IP-címeket. Ez a példa egy rekordkészletben:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS kezeli a DNS-rekordjait *rekord állítja be*. A rekord beállítása (más néven egy *erőforrás* rekordkészletben) a DNS-rekordjait a zónán lehet ugyanaz a neve, amelyek azonos típusú gyűjteménye. A legtöbb rekord beállítása egyetlen rekordot tartalmazó, de ehhez hasonló, amelyben egy rekordkészletben tartalmaz egynél több rekord, például nem ritka.

Ha például tegyük fel, hogy már létrehozott egy A rekordot "www" a "contoso.com" zónában, mutató IP-cím "134.170.185.46" (az első rekordban a fenti).  A második rekord létrehozására, módon adhat hozzá, a rekordot a meglévő rekord helyett hozzon létre egy új rekord.

Az SOA és CNAME rekordtípust alól kivételt képező. A DNS-szabványokat nem teszi lehetővé az ilyen azonos nevű több rekordot, így a következő rekord beállítása tartalmazhatnak egyetlen olyan rekordot.

### <a name="time-to-live"></a>Time-to-live

Élettartam, vagy a TTL (élettartam), adja meg, mennyi ideig az egyes rekordok gyorsítótárazott ügyfelek újra lekérdezett előtt. A fenti példában a TTL (élettartam), 3600 másodpercet vagy 1 órát.

Az Azure DNS a TTL (élettartam) van megadva rekordkészletben nem az egyes rekordok, így az azonos értékkel használják, hogy rekordkészletben belül minden rekordhoz.  Megadhatja, hogy minden olyan TTL értéke 1 és 2 147 483 647 másodperc között.

### <a name="wildcard-records"></a>Helyettesítő rekordok

Azure DNS [helyettesítő rekordok](https://en.wikipedia.org/wiki/Wildcard_DNS_record)támogatja. Helyettesítő rekordot ad vissza egyező nevű lekérdezés válaszként (kivéve, ha egy nem helyettesítő rekordkészletben származó, amely közelebb van). Helyettesítő rekord beállítása támogatott NS és SOA kivételével az összes rekordtípust.  

Hozzon létre egy helyettesítő rekordkészletben, használja az rekordkészletben nevet "\*". Másik lehetőségként is használhatja a név, amelynek az "\*", a bal szélső, például felirat"\*.foo".

### <a name="cname-records"></a>CNAME rekordok

CNAME rekord beállítása más rekord azonos nevű bíró nem használható. Ha például nem hozhatók létre egy CNAME rekordot, a "www" relatív nevének megadása és a-rekord relatív nevű "www" egyszerre.

Mivel a zóna csúcs (neve = ‘@’) mindig tartalmazni fogja az NS és SOA rekord beállítása a zóna létrehozásakor készített, nem hozható létre egy CNAME rekordot, a zóna csúcs értéken.

Ezek a korlátok az a DNS-szabványoknak felmerülő, nem Azure DNS korlátai.

### <a name="ns-records"></a>A Névkiszolgálói rekordok

Egy NS rekord megadása: a csúcs zónák automatikusan létrejön (neve = ‘@’), és a rendszer automatikusan törli a zóna törlésekor (nem lehet törölni külön-külön).  Módosíthatja a rekordkészlet TTL (élettartam), de nem tudja módosítani a feljegyzéseket, amelyeket előre beállított kell utalnia az Azure DNS névkiszolgálóinak a zónába.

Létrehozhat és más Névkiszolgálói rekordok a zónában, nem pedig a zóna csúcs törlése.  Ez lehetővé teszi, hogy állítsa be a gyermek zones (lásd: [delegálása alszint tartományok Azure DNS-ben](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns).)

### <a name="soa-records"></a>SOA rekordok

Egy rekordkészletben SOA automatikusan létrejön a zónák a csúcs (neve = ‘@’), és a zóna törlésekor a rendszer automatikusan törli.  SOA rekordok nem létrehozott és nem külön-külön törölhető. 

A SOA rekord, amely egy előre konfigurált kell utalnia az Azure DNS által biztosított elsődleges neve-kiszolgáló neve "host" jellemző kivételével az összes tulajdonságai módosíthatók.

### <a name="spf-records"></a>Az SPF rekordok

Feladó házirend keretrendszer (SPF) rekordok használhatók megadása milyen levelezési kiszolgálókon folytató küldhet e-mailt egy adott tartománynevet nevében.  Az SPF rekordok helyes beállításokkal fontos, hogy a címzettek e-mailek megjelölése "Levélszemét" jelölés.

A DNS-RFC eredetileg egy új "SPF" rekordtípus támogatják ezt a helyzetet jelent meg. Régebbi névkiszolgálók támogatja, azok is engedélyezni a TXT record type megadhatja az SPF rekordok használatát.  A ellentmondás vezetett fejetlenséget, amely [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1)lett megoldásához.  Ezt a szolgáltatást SPF rekordok csak létrejöjjön használja a TXT-rekord típusát, és kiadásában megszűnt az SPF rekord típusa. 

**SPF rekordok Azure DNS által támogatott, és használja a TXT-rekord típusa kell létrehozni.** A feleslegessé vált SPF record type nem támogatott. Amikor [fájl importálása a DNS zone](dns-import-export.md)bármely SPF-rekordok segítségével az SPF-rekord típusa alakulnak át a TXT-rekord típusa. 

### <a name="srv-records"></a>SRV rekordok

[SRV rekordok](https://en.wikipedia.org/wiki/SRV_record) különböző szolgáltatások által használt kiszolgáló helyének megadásához. A rekord megadása Azure DNS-ben:

- A *szolgáltatás* és a *protokoll* meg kell adni a rekordkészletben név előtaggal aláhúzással részeként.  Ha például "\_sip. \_tcp.name ".  A zone csúcs a rekord, nincs szükség az adja meg, hogy '@' a rekord nevét az egyszerűen használja a szolgáltatás és a protokollt, például "\_sip. \_tcp ".
- A *Prioritás*, *súly*, *portokkal*és *cél* határozza meg az egyes rekordok a rekordkészletben vannak megadva. 

## <a name="tags-and-metadata"></a>Címkék és metaadatok

### <a name="tags"></a>Címkék
Címkék neve – érték párokká listáját, és Azure-kezelő által használt címke erőforrásokat.  Azure erőforrás-kezelő címkék ahhoz, hogy az Azure számlájának szűrt nézetben, és így is lehetővé teszi, hogy állított be egy házirendet, címkék szükség. További információt a címkék [használata címkék rendszerezheti az Azure erőforrások](../resource-group-using-tags.md)megtekintése

Azure DNS támogatja az erőforrás-kezelő Azure-címkék használata a DNS-zóna erőforrásokat.  Címkék nem támogatja a DNS-rekord beállítása.

### <a name="metadata"></a>Metaadatok

Rekordkészletben címkék alternatívájaként Azure DNS támogatja jegyzetet fűz "metaadatokkal" rekord beállítása.  Címkék hasonlóan metaadat-alapú lehetővé teszi, hogy minden rekordkészletben neve – érték párokká társítani.  Ez akkor lehet hasznos, ha például minden rekordkészletben célját rekordot.  Eltérően a címkéket metaadatok nem használhatók az Azure számla szűrt nézet számára, és az erőforrás-kezelő Azure házirend nem kell megadni.

## <a name="limits"></a>Korlátai

Azure DNS használatával az alábbi alapértelmezett korlátozások érvényesek:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Következő lépések

- Azure DNS használatának megkezdéséhez megtudhatja, hogyan hozhat [létre a DNS-zóna](dns-getstarted-create-dnszone-portal.md) , és a [DNS-rekordok létrehozása](dns-getstarted-create-recordset-portal.md).
- Egy meglévő DNS-zóna áttelepítéséhez megtudhatja, hogyan [importálása és a DNS-zónafájl](dns-import-export.md).
