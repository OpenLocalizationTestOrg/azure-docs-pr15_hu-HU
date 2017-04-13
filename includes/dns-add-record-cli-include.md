#### <a name="create-an-a-record-set-with-single-record"></a>Az egyetlen olyan rekordot be A rekord létrehozása

Egy rekordkészletben hozhat létre `azure network dns record-set create`. Adja meg a zone nevét, az erőforráscsoport rekord relatív neve, rekordtípust, és az idő beállítása Live (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Az A létrehozása után beállítása esetben a IPv4-cím hozzáadása a rekord beállítása a `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Hozzon létre egy egyetlen olyan rekordot be AAAA rekordot

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot CNAME rekord létrehozása

CNAME rekordok engedélyezése csak egyetlen karakterlánc.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot az MX-rekord létrehozása

Ebben a példában a rekordkészletben neve használjuk "@" az MX rekord létrehozásához, a zóna csúcs (ebben az esetben "contoso.com"). Ez a közös MX-rekord esetében.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Egy egyetlen olyan rekordot az NS rekord létrehozása

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot PTR-rekord létrehozása  
Ebben az esetben "a-arpa – zónában (Zone.com)" a ARPA zónában, amely az IP-tartomány jelöli.  IP-címet a IP-tartományon belül minden beállítása a ebbe a zónába PTR-rekordja felel meg.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot a rekord létrehozása

Ha a rekord a legfelső szintű a zóna hoz létre, akkor a rekord neve "_service" és "_protocol" megadhatja. Nem szükséges, amelyet fel szeretne venni "@" a rekord nevét.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Az egyetlen olyan rekordot TXT rekord létrehozása

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
