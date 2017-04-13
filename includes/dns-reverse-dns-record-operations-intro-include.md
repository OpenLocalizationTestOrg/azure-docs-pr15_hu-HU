## <a name="what-is-reverse-dns"></a>Mi az, fordított DNS?

Hagyományos DNS-rekordok engedélyezése a megfeleltetés DNS nevét (például "www.contoso.com") IP-címet (például 64.4.6.100).  Fordított DNS lehetővé teszi, hogy a fordítás IP-cím (64.4.6.100) vissza az nevét (www.contoso.com).

Fordított DNS-rekordjait a helyzetek számos használják. Fordított DNS-rekordok például körben használt e-mail levélszemét elleni e-mail üzenet feladója ellenőrzésével.  A fogadó levelezési kiszolgáló beolvasni a kimenő levelek kiszolgálója IP-címének a fordított DNS-rekordot, és ellenőrizze, hogy ha tároló jogosult tartományból származó e-mailt küld. (Ne feledje azonban, hogy [Azure kiszámítania szolgáltatások nem támogatja a külső tartományok küldése e-mailekhez](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Hogyan fordított DNS működése

Fordított DNS-rekordjait a speciális DNS-zónák, úgynevezett "ARPA" zónák van közzétéve.  Ezek a zónák egy külön DNS-hierarchiában a tartományt (például "contoso.com)" üzemeltet normál hierarchiát párhuzamosan beillesztő

Például a DNS record "www.contoso.com" segítségével működik együtt a "www" név "A" DNS rekordot a contoso.com zónában.  Ez A rekord a megfelelő IP-címére, ebben az esetben 64.4.6.100 mutat.  A fordított keresés alkalmazása külön-külön, egy "PTR" bejegyzés "100" a "6.4.64.in-addr.arpa" zónában (IP-címek felcseréli ARPA zónák megjegyzés.) nevű használatával  A PTR-rekordja, ha helyesen konfigurált mutat a www.contoso.com nevét.

Amikor egy szervezet egy címet blokkolt IP-Címek, azok is szerezheti be a megfelelő ARPA zóna kezelése jobbra. A megfelelő Azure által használt IP-cím blokkok ARPA zónák üzemeltetett és a Microsoft kezeli. Az Internetszolgáltató meg saját IP-címek a ARPA zóna is tartalmazhat, vagy előfordulhat, hogy engedélyezi a ARPA zóna lehetőség, például Azure DNS egy DNS-szolgáltatás üzemelteti.

>[AZURE.NOTE] Előre DNS keresések és fordított DNS keresések külön, a párhuzamos DNS hierarchiák szerepelni fog. A fordított keresés a "www.contoso.com" **nem** is az "contoso.com" zónában, inkább helyezkedik el a megfelelő címet blokkolt IP-Címek ARPA zónában.

Fordított DNS további tudnivalókért lásd a [Fordított DNS keresés](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Fordított DNS Azure támogatása

Azure vonatkozó DNS fordított két külön forgatókönyv támogatja:

1. Az a cím blokkolt IP-Címek megfelelő ARPA zóna szolgáltatója.
2. Lehetővé teszi az Azure szolgáltatás IP-cím fordított DNS-rekordjainak beállítása.

A támogatási volt, Azure DNS-Rekordokat használható a ARPA zónák tárolásához és az egyes fordított DNS keresés PTR-rekordjának kezelését.  A folyamat a ARPA zónát hoz létre, a meghatalmazás beállítása és PTR-rekordok konfigurálása ugyanúgy történik, mint normál DNS-zónák.  A csak különbségek, hogy a meghatalmazás kell beállítania a DNS-szolgáltató, hanem az Internetszolgáltató keresztül, és csak a PTR rekordtípus kell használni.

A támogatási az utóbbi, a Azure lehetővé teszi a fordított keresés rendelt a szolgáltatás IP-címek konfigurálása.  A fordított keresés PTR-rekordja a megfelelő ARPA zónában Azure úgy van beállítva.  Ezek a ARPA zónák, a megfelelő Azure-által használt IP-címtartományai Microsoft van közzétéve. **Ez a cikk hátralévő ebben az esetben részletesen ismerteti.**
