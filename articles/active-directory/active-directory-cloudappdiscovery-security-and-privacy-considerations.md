<properties
    pageTitle="Cloud App feltárás biztonsági és adatvédelmi szempontokat |} Microsoft Azure"
    description="Ez a témakör a Cloud App feltárás kapcsolatos biztonsági és adatvédelmi szempontokat ismerteti."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App feltárás biztonsági és adatvédelmi kapcsolatos szempontok

A Microsoft kötelességének tekinti, a személyes adatok védelme és az adatok védelme, lejátszásakor szoftverek és -szolgáltatások, amelyek segítséget nyújtanak a biztonsági a szervezet kezelése. <br>
Azt ismerik fel, amely akkor bízza meg az adatok másokkal, amikor, hogy az adatvédelmi szigorú biztonsági mérnöki befektetések és biztonsági másolat szakértelmét kell lennie.
A Microsoft nakhttp://go.microsoft.com/fwlink/?LinkId=311864 megfelelően igazodik szigorú megfelelőség és biztonsági irányelveket a biztonságos szoftver fejlesztési életciklus tanácsok a szolgáltatás működik. <br>
Biztonságossá tétele és az adatok védelme fontosságú felső a Microsoftnál.

Ebből a témakörből megtudhatja, adatainak összegyűjtött, feldolgozott és Azure Active Directory Cloud App feltárás belül védelmének beállítása




##<a name="overview"></a>– Áttekintés

Cloud App feltárás funkciója, az Azure Active Directory, és a Microsoft Azure-ban üzemelteti. <br>
A Cloud App feltárás végpont ügynök alkalmazás feltárás adatokat gyűjt a felügyelt informatikai gépek használják. <br>
Gyűjtött adatokat a rendszer elküldi biztonságosan egy titkosított csatornán keresztül az Azure Active Directory Cloud App keresőszolgáltatás. <br>
Egy szervezet Cloud App feltárás adata majd látható az Azure-portálon. <br>


<center>![Hogyan működik a Cloud App feltárás](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


A következő szakaszok hajtsa végre az adatáramlást az, és ismertetik, hogyan, Ugrás a szervezetbe Cloud App keresőszolgáltatás titkosítása és végül a Cloud App feltárás portálra.



## <a name="collecting-data-from-your-organization"></a>Adatok gyűjtése a szervezetbe

Annak érdekében, hogy az Azure Active Directory Cloud App keresési lehetőség használatával az összefüggéseket az alkalmazottaknak a szervezet által használt alkalmazásokat az első, kell először telepítenie az Azure Active Directory Cloud App feltárás végpont ügynök gépek a szervezet számára.

A rendszergazdák az Azure Active Directory-ös bérlői webhelyet (vagy a meghatalmazott) ügynök telepítőcsomag letölthető az Azure-portálra. A agent vagy kézzel telepíthető vagy SCCM vagy csoportházirendet alkalmaz a szervezet több számítógépen telepíteni.

További telepítési beállítások, tanulmányozza [Cloud App feltárás csoport házirend telepítési útmutatóját](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).
<br>

### <a name="data-collected-by-the-agent"></a>A agent által gyűjtött adatokat

A tagolt a listában, az alábbi információkat gyűjti a ügynök, ha a kapcsolat webalkalmazás. Az adatokra csak azokat, amelyek a az ügyvezető van konfigurálva feltárás alkalmazások. <br>
Módosíthatja a listájának felhő, amely a agent figyeli a Cloud App feltárás fel a Microsoft [Azure portálon](https://portal.azure.com/), a **Beállítások**között->**Adatgyűjtés**->**alkalmazás gyűjteménylista**. További részletekért olvassa el [Az első lépések a Cloud App feltárás](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Információk kategória**: felhasználói adatok <br>
**Leírás**: <br>
A Windows-felhasználónevet, a folyamat, amely a célba webalkalmazás kérelmet (pl.: TARTOMANY\felhasznalonev) és a Windows biztonsági azonosító (biztonsági AZONOSÍTÓK) annak a felhasználónak.


**Információk kategória**: adatok feldolgozásához <br>
**Leírás**: <br>
A folyamat, amely a kérelmet a cél webalkalmazás nevét (például: "iexplore.exe")

**Információk kategória**: számítógép adatai <br>
**Leírás**: <br>
A gépi NetBIOS nevére, amelyen telepítve van a agent.

**Információk kategória**: alkalmazás forgalmi adatai <br>
**Leírás**: <br>

A következő kapcsolat információkat:

- A forrás (helyi számítógép) és a cél IP-címek és a portszámokat

- A szervezet keresztül, amely a kérést, megy nyilvános IP-címét.

- A kérés időpontja

- A hangerő forgalom küldése és fogadása

- Az IP-verziót (4-es és 6)

- Csak a kapcsolatok TLS: A céllista állomásnév a kiszolgáló neve megjelölése bővítmény vagy a kiszolgálói tanúsítvány.

A következő HTTP információkat:

- A módszer (GET, bejegyzés stb.)

- Protocol (a HTTP/1.1 stb.)

- Felhasználói ügynökök karakterlánc

- Hostname (állomásnév)

- Cél URI (kivéve a lekérdezési karakterlánc)

- Tartalomtípus adatai

- Referrer URL-cím adatait (kivéve a lekérdezési karakterlánc)



> [AZURE.NOTE] A fenti információk HTTP kell felvenni az összes nem titkosított kapcsolatot.
Az TLS-kapcsolatot ezt az információt csak rögzíthető, amikor a "Mély ellenőrzés" beállítás be van kapcsolva a portálon. A beállítás alapértelmezés szerint "Tovább".
További információ, alább találja és [Első lépések a Cloud App feltárás](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Mellett az adatokat, hogy a hálózati tevékenységről gyűjt a ügynök, azt is névtelen adatokat gyűjt a szoftver és hardverkonfiguráció hibajelentések és a ügynök használatának információt.

<br><br>
### <a name="how-the-agent-works"></a>A agent működése

A ügynök telepítés két összetevőket tartalmazza:

- A felhasználói módú komponens

- Egy kernelmódú illesztőprogram összetevőt (Windows szűrés Platform illesztőprogram)



Amikor először telepítette a az ügynök a számítógépen, amely ezután használja a Cloud App keresőszolgáltatás biztonságos kapcsolatot létesíteni tárolja megbízható számítógép-specifikus tanúsítvány. <br>
A agent rendszeres átveszi házirend beállítása a Cloud App keresőszolgáltatás a biztonságos kapcsolaton keresztül. <br>
A házirend monitor és automatikus frissítése kell lennie engedélyezve van-e, többek között felhő alkalmazásokat információkat tartalmaz.

Internetes forgalmat küldi és fogadja az Internet Explorer, a Chrome és a számítógépen, a Cloud App feltárás ügynök elemzi a forgalmat, és az első karaktertől a megfelelő metaadatokat (lásd a fenti **a agent által gyűjtött adatok** szakaszt). <br>
Percenként, a agent feltölti a összegyűjtött metaadatok Cloud App keresőszolgáltatás egy titkosított csatornán keresztül.

Az illesztőprogram összetevő a titkosított forgalom elfogja, és beilleszti magát a titkosított adatfolyam. További részletek az alábbi **Intercepting adatainak titkosított kapcsolatot (mély vizsgálat)** szakaszt.


### <a name="respecting-user-privacy"></a>Felhasználói adatvédelmi tiszteletben

Célunk az eszközök állíthatja be az alkalmazás használatát és a felhasználó adatvédelmi tetszés szerint a szervezet által használt részletes optika közötti egyensúly rendszergazdák számára. Emiatt kínálunk, amely a következő belül, a beállítások lapon a portálon:

- **Adatgyűjtés**: rendszergazdák választhat, mely alkalmazások vagy szeretnének feltárás adatok beolvasása kategóriák megadását.

- **Mélyreható vizsgálati**: rendszergazdák is választotta, hogy a adja meg, ha a ügynök gyűjt a HTTP-forgalom az SSL/TLS-kapcsolatot (más néven **"mély ellenőrzés"**). Bővebben a következő szakaszban.

- **Beleegyezés beállítások**: rendszergazdák a Cloud App feltárás portal segítségével válassza ki, hogy a felhasználók tájékoztatását a ügynök adatgyűjtési, és hogy a felhasználó-e a felhasználói adatok gyűjtése ügynök megkezdése előtt beleegyezés.

A Cloud App feltárás végpont agent csak részben leírt **az ügynök által gyűjtött adatokat** a fenti információk gyűjti össze.


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Titkosított kapcsolatot (mély vizsgálat) adatainak objektummodelljéhez
Amint azt már említettük, a rendszergazda a Lync-titkosított kapcsolatot ("mély ellenőrzés") adatainak agent konfigurálhatja. TLS ([Átviteli réteg biztonsági](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) az egyik azok a leggyakoribb protokollok az interneten használt ma. Hogy titkosítja a TLS való kommunikáció, egy ügyfél webkiszolgáló; biztonságban kommunikációs csatorna hozhat létre TLS hitelesítő áthaladó alapvető védelmet biztosít, és megakadályozhatja, hogy a bizalmas információkat közzétételét.

A végpontok közötti biztonságos titkosított csatorna TLS által biztosított lehetővé teszi a fontos biztonság és adatvédelem, miközben a protokoll gyakran visszaélnek rosszindulatú vagy nefarious céljából. Így sokkal így valójában adott TLS gyakran nevezik "egyetemes tűzfal-figyelmen kívül hagyása protokoll". A probléma gyökerének, hogy a legtöbb tűzfalak nem tudja TLS-kapcsolati vizsgálja meg, mert az alkalmazásszintű adatok titkosítva van az SSL használatával. Ennek ismeretében támadók gyakran kihasználhatja TLS benne, hogy még a legtöbb intelligens alkalmazásszintű tűzfalak TLS teljesen vak és egyszerűen kell továbbítani a hosts TLS kommunikációját felhasználó végzi a kártékony tartalmak számára. A végfelhasználók gyakran kihasználhatja a TLS lehetőséget, ha vannak érvényben vállalati tűzfal és a proxykiszolgálók, az access-vezérlők figyelmen kívül szeretné csatlakozni a nyilvános proxyk lehetőséget, és nem TLS használata a tűzfalon, egyéb esetben blokkolhatja a házirend-használatával.

Mély vizsgálati lehetővé teszi, hogy a Cloud App feltárás agent használni kívánt egy megbízható ember-a-a-középső. Amikor egy ügyfél kérést egy HTTPS védett erőforrás eléréséhez, a végpontot Agent illesztőprogram elfogja a kapcsolatot, és létrehozza az új kapcsolatot a cél-kiszolgálót, hogy olvassa be az SSL-tanúsítványt az ügyfél nevében. A agent ellenőrzi, hogy a tanúsítvány is megbízható (ellenőrzése, hogy a program nem visszavont, és egyéb tanúsítvány ellenőrzések végrehajtásához), és ha a következő fázis, majd a végpontot ügynök átmásolja az adatokat a kiszolgálói tanúsítvány, és hoz létre saját kiszolgálói tanúsítvány – egy hozzáférés tanúsítvány – amelyekre neve. A hozzáférés-tanúsítvány, amely aláírt a azonnali által a legfelső szintű tanúsítványok, amelyen telepítve van az megbízható tanúsítvány a Windows áruházban végpont munkatársának. E önaláírt legfelső szintű tanúsítvány nem exportálható van megjelölve, és vezérlés rendszergazdái számára is. Célja, hogy soha ne hagyja meg a számítógépen, amelyen létrehozták. Amikor a végfelhasználó ügyfélalkalmazás megkapja a hozzáférés-tanúsítvány, azt fog a megbízható gombra, mert azt sikeresen ellenőrizheti a tanúsítvány-lánc teljes mértékben a legfelső szintű tanúsítvány való. Ez a folyamat elsősorban a végfelhasználó szempontjából az alábbiakban leírtak szerint néhány Ellenjavallat átlátszó.

Mély ellenőrző engedélyezésével a Cloud App feltárás végpont Agent visszafejteni és vizsgálata titkosított TLS kommunikáció, csökkentheti a zajforrásoktól, és adja meg a titkosított felhő alkalmazások használatáról az összefüggéseket a szolgáltatás lehetővé teszi.

#### <a name="a-word-of-caution"></a>Egy szót, amire érdemes ügyelni
Mély vizsgálati bekapcsolása, előtt erősen ajánlott a tervezett a jogi és szervezeti HR kommunikációt, és a beszerzése egyetértését. Személyes titkosított kommunikációt végfelhasználó vizsgálata lehet a bizalmas tárgyat, egyértelmű okok miatt. Egy gyártási bevezetési mély ellenőrzés, mielőtt érdekében, hogy bizonyos, hogy a vállalati biztonsági, és jelzi, hogy titkosított kapcsolati ellenőrizni kell a használati irányelvek frissített. Felhasználói értesítés és a webhelyek ítélt bizalmas (például a banki és orvosi webhelyek) alóli is szükség, ha úgy állítja be Cloud App feltárás Lync-őket. Fent említett rendszergazdái a Cloud App feltárás portálon válassza ki, hogy a felhasználók tájékoztatását a ügynök adatgyűjtési-e, és csak a felhasználó hozzájárulása az ügynök a felhasználói adatok gyűjtése megkezdése előtt-e.

### <a name="known-issues-and-drawbacks"></a>Ismert problémák és hátrányai
Létezik néhány olyan esetek, amikor TLS hozzáférés hatással lehetnek a végfelhasználók számára:

- Bővített érvényességi (lé) tanúsítványok jeleníti meg a böngésző címsorába zöld használni kívánt egy vizuális clue, hogy meglátogatott egy megbízható webhelyen. TLS-ellenőrző nem ismétlődő EV a tanúsítványban általa az ügyfélnek, így EV-tanúsítványok használó webhelyek a szokásos módon fog működni, de a címsorban nem jelennek meg a zöld.  

- Nyilvános kulcs rögzítése (más néven tanúsítvány rögzítése) felhasználók megóvni a férfi középső célú támadásokkal és engedélyezetlen hitelesítésszolgáltatók készültek. A legfelső szintű tanúsítvány rögzített webhely nem egyezik meg az ismert jó hitelesítésszolgáltató közül, amikor a böngésző elutasítja a a hiba-kapcsolatot. Mivel a TLS hozzáférés, valójában egy férfi-a-a-középről, a kapcsolatok sikertelen lesz.

- Ha a felhasználók a böngészőben a böngésző cím sáv vizsgálja meg a webhely adatai lakatikon gombra, nem láthatják a jelentkezzen be a webhely tanúsítvány használt hitelesítő végződésű lánc, de inkább a egy a Windows-es tanúsítvány-lánc tanúsítvány tároló megbízható.

Ha csökkenteni szeretné a jelen hibák előfordulását, azt nyomon követheti a felhőszolgáltatások és ügyfélalkalmazásokban bővített érvényességi vagy nyilvános kulcs rögzítése, és kérje meg a végpontot Agent ismert probléma által sújtott kapcsolatok objektummodelljéhez elkerülése érdekében. Még ezekben az esetekben azonban a felhőben az alkalmazások és az átvitt adatok mennyisége jelentések kap, de még nem mély ellenőrzött, mivel nincs olvashat, hogy hogyan használt alkalmazások érhetők el.


## <a name="sending-data-to-cloud-app-discovery"></a>Adatok küldése a Cloud App feltárás

Metaadat-alapú a agent által gyűjtött meg, miután gyorsítótárban van a számítógépen egy percig, illetve amíg a gyorsítótárban tárolt adatokat eléri a méret 5MB. Ezután tömörített és biztonságos kapcsolatot létesíteni a Cloud App keresőszolgáltatás keresztül küldött.

Az ügynök nem tud kommunikálni a Cloud App keresőszolgáltatás valamilyen okból, ha csak a számítógépen (például a Rendszergazdák csoport) jogosultsággal rendelkező felhasználók számára is elérhető helyi fájl gyorsítótárat a összegyűjtött metaadat-alapú vannak tárolva. <br>
A agent megpróbálja automatikusan küldje el a gyorsítótárban lévő metaadatok, amíg sikeresen megérkeztek Cloud App feltárás szolgáltatás.



## <a name="receiving-the-data-at-the-service-end"></a>Az adatok szolgáltatás végén fogadása

Szolgáltatás a számítógépen meghatározott ügyfél-hitelesítési tanúsítványt használja a fentiekben, és továbbítja az adatokat egy titkosított csatornán Cloud App felfedezése hitelesíteni a ügynökök. <br>
A Cloud App feltárás szolgáltatás analytics folyamat dolgozza fel metaadatok ügyfelek külön-külön logikailag szétválasztás az keresztül az analitikai folyamat minden szakaszát.
Elemzett metaadat-alapú meghajtók a különböző jelentések, a portálon.

A feldolgozatlan metaadatok és elemzett metaadatokat 180 napig tárolja. Ezeken kívül ügyfelek rögzítése a saját kiválasztásának Azure blob tároló fiók elemzett metaadat-alapú adhatja meg.
Az offline elemzésének metaadatok, valamint az adatok hosszabb adatmegőrzési esetében hasznos lehet.

## <a name="accessing-the-data-using-the-azure-portal"></a>Az adatok az Azure portálon elérése

A metaadatok gyűjtött biztonsága annak érdekében alapértelmezés szerint a a bérlő csak a globális rendszergazdák hozzáférést a Cloud App keresési lehetőség az Azure-portálon. <br>
A rendszergazdák azonban más felhasználóknak vagy csoportoknak a hozzáférés delegálása választhat.



> [AZURE.NOTE] További részletekért olvassa el [Az első lépések a Cloud App feltárás](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Bármelyik felhasználó eléréséhez a portálon Azure Active Directory prémium licenccel rendelkező licenccel kell rendelkezniük.



##<a name="additional-resources"></a>További források


* [Hogyan, észleljék a szervezeten belüli felhasznált unsanctioned felhő alkalmazások](active-directory-cloudappdiscovery-whatis.md)
* [Az alkalmazások kezelése az Azure Active Directory cikk indexben](active-directory-apps-index.md)
