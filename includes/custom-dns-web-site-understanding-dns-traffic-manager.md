A Domain (DNS) segítségével keresse meg a műveletet az interneten. Például amikor írjon be egy címet a böngészőben, vagy az weblapon egy hivatkozásra, az DNS segítségével a tartomány lefordítása IP-címet. Az IP-cím használandó hasonlít a postai címét, de még nem nagyon emberi rövid. Például érdemes jelentősen megkönnyíti, például **: contoso.com** a tartománynév emlékezni fog, mint amilyen például 192.168.1.88 vagy 2001:0:4137:1f67:24a2:3888:9cce:fea3 IP-cím ne feledje.

A DNS-rendszerben *rekordok*alapul. Rekordok társítása egy egyedi *nevet*(például **contoso.com**), vagy IP-címet, vagy egy másik tartománynév. Ha egy alkalmazást, például egy webböngészőben, egy nevet a DNS-ben keres, megtalálja a rekordot, és használja a függetlenül mutat címként. Ha az érték mutat IP-címet, a böngészőben az, hogy az értéket kell használni. Ha egy másik tartománynév mutat, majd az alkalmazás azt végezze el ismét a felbontás. Végül minden névfeloldás érjen véget az IP-címet.

Amikor létrehoz egy Azure-webhelyet, a DNS-név automatikusan arra a webhelyre. Ez a név formájában történik ** &lt;yoursitename&gt;. azurewebsites.net**. A webhely-Azure forgalom Manager végpontjának hozzáadásakor a webhelyen keresztül érhető el majd a ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** tartományt.

> [AZURE.NOTE] Ha webhelyét a forgalom Manager végpontjának van konfigurálva, akkor használja a **. trafficmanager.net** cím a DNS-rekordok létrehozásakor.

> Csak-kezelővel CNAME rekordokat a forgalom

Is vannak többféle típusú rekordok saját függvények és a vonatkozó korlátozások, de a forgalom Manager végpontok konfigurált webhelyeket, hogy csak érdekli egy; *CNAME* rekordok.

###<a name="cname-or-alias-record"></a>Alias vagy a CNAME rekord

A CNAME rekord **mail.contoso.com** vagy **www.contoso.com**, például egy *adott* DNS-név (kanonikus) egy másik tartománynév rendeli hozzá. Az adatforgalom-kezelővel Azure webhelyek esetében a kanonikus tartománynév van a ** &lt;SajátPr >. trafficmanager.net** a forgalom Manager profil tartománynevet. Amikor létrejött, a CNAME alias hoz létre a ** &lt;SajátPr >. trafficmanager.net** tartomány nevét. A CNAME-bejegyzés fog úgy, hogy az IP-címét a ** &lt;SajátPr >. trafficmanager.net** tartománynév automatikusan, így ha megváltoztatja a webhely IP-címét, nem kell tennie semmit.

Miután irányítja a forgalmat Manager megérkezik, majd a forgalom az a webhelyét, a terheléselosztás módszer, hogy be van állítva az. Ez a teljesen átlátszó a látogatók a webhelyét. Csak az egyéni tartománynevet a böngészőjükben láthatók.

> [AZURE.NOTE] Néhány tartományregisztrálói csak teszi altartományokat feleltesse meg egy CNAME rekordot, például **www.contoso.com**, és nem legfelső szintű nevek (például **contoso.com)**használata esetén. További információt a CNAME rekordot a tartományregisztráló, <a href="http://en.wikipedia.org/wiki/CNAME_record">a CNAME rekord Wikipedia-bejegyzés</a>vagy a <a href="http://tools.ietf.org/html/rfc1035">IETF tartománynevek - végrehajtása és a specifikációja</a> dokumentum által biztosított dokumentációjában.
