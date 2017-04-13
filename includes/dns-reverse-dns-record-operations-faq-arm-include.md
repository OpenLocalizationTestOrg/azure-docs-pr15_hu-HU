<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Gyakori kérdések – a Azure kiosztott IP-címének fordított DNS

### <a name="how-much-do-reverse-dns-records-cost"></a>Mennyi fordított DNS-rekordok költség?
Ingyenes is legyenek!  Fordított DNS-rekordok vagy lekérdezések külön költség nélkül nem.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Az internetről megoldja a Azure-hozzárendelt nyilvános IP-cím fordított DNS-rekordjait?
igen. Amikor a fordított DNS tulajdonság a nyilvános IP-cím, Azure kezeli a DNS-küldöttségek és annak érdekében, hogy a fordított DNS-rekord feloldása az összes internet-felhasználók szükséges DNS-zónák.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>A nyilvános IP-címek létrejön egy alapértelmezett fordított DNS-rekordot?
nem. Fordított DNS fiókfunkció funkcióról. Nem alapértelmezett fordított DNS-rekordok jönnek létre, ha úgy dönt, hogy nem kell konfigurálni őket.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Mi az a formátumot a teljes tartománynevét (FQDN)?
Teljes tartományneve előre sorrend meg vannak adva, és be kell fejezni egy ponttal (például "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Mi történik, ha nem a érvényességi ellenőrzi a korábban megadott fordított DNS-Rekordokat?
Hol az érvényesítési fordított a DNS ellenőrzése sikertelen, a szolgáltatás felügyeleti művelet sikertelen lesz. Kérjük, javítsa szükség szerint a fordított DNS értéket, és újra.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Lehet kezelni fordított DNS Azure webhelyem?
Fordított DNS nem támogatott a Azure-webhelyeket. Fordított DNS-Azure virtuális gépeken futó használata támogatott.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Lehet-e beállítása több fordított DNS-rekordok saját nyilvános IP-cím?
nem. Azure egyetlen fordított DNS-rekord támogat egyes nyilvános IP-cím. Minden egyes nyilvános IP-cím azonban beállíthatja, hogy a saját fordított DNS-rekordot.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Lehet-e konfigurálása fordított DNS-rekordok IPv6 nyilvános IP-címének?
nem.  Ebben az esetben a fordított DNS-rekordok támogatott IPv4 nyilvános IP-címek csak.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Is állítható be a DNS-rekord fordított saját nyilvános IP-cím egy megadott DomainNameLabel nélkül?
nem. Fordított DNS-rekordok használhatja a nyilvános IP-címek, meg kell adnia a DomainNameLabel tulajdonságot.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Lehet küldeni e-mailek külső tartományok a saját Azure kiszámítania szolgáltatások?
nem. [Azure kiszámítania szolgáltatások nem támogatja a külső tartományok küldése e-mailekhez](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
