<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Gyakori kérdések – a ARPA zóna Azure DNS-szolgáltatója.

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Lehet-e tárolni ARPA zónák a Szolgáltató által kiosztott IP-blokkok Azure DNS?
igen. A saját IP-címtartományai Azure DNS-zónák ARPA szolgáltatója teljesen támogatott.

Egyszerűen [létrehozása az Azure DNS-zóna](dns-getstarted-create-dnszone.md), majd az Internetszolgáltató átadni, [a zóna](dns-domain-delegation.md)használata.  Kattintson az egyes fordított keresés PTR rekordokat kezelheti ugyanúgy, mint a többi rekordtípusokat.

Akkor is [importálhatja egy meglévő fordított keresés zóna az Azure CLI használatával](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Mennyibe kerül a ARPA zóna költség szolgáltatója?
A ARPA zónát tároló az Internetszolgáltató rendelt IP-blokk Azure DNS alapdíjával díjazott [szabványos Azure DNS mértékeket](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Üzemeltetheti ARPA zónák IPv4 és az IPv6-címek Azure DNS-ben?
igen.