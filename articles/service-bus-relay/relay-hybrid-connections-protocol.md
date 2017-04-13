<properties 
    pageTitle="Azure továbbítási hibrid kapcsolatok protokoll |} Microsoft Azure"
    description="Továbbítás hibrid kapcsolatok protokoll útmutató zure."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure továbbítási hibrid kapcsolatok Protocol (protokoll)

Azure továbbítási egyike, a fő videofunkcióinak oszlopok az Azure Service Bus platform. A továbbítási új "Hibrid kapcsolatok" lehetőség egy biztonságos, a Megnyitás-protocol fejlesztés HTTP és WebSockets alapján.

Az előbbi egyaránt nevű "BizTalk szolgáltatások" szolgáltatás, protokoll alapját készült változat azt. Azure alkalmazás szolgáltatások integrálása a hibrid kapcsolatok továbbra is szerepelni fognak működni-van.

"Hibrid kapcsolatok" lehetővé teszi, hogy a két hálózati alkalmazások, amellyel akár egyszerre mindkét fél találhatók címfordító vagy a tűzfalak mögött kétirányú, bináris adatfolyam kommunikációját létrehozásáról.

Ehhez a dokumentumhoz a hibrid kapcsolatok továbbítási történő csatlakozás figyelő és a feladó szerepkörök, és hogyan hallgatók fogadja el az új kapcsolatokat az ügyfelek ügyféloldali interakciók ismerteti.

## <a name="interaction-model"></a>Kapcsolati modell

A hibrid kapcsolatok továbbítási csatlakozik a két fél, mert az Azure felhőben, amelyek mindkét fél képesek felderíteni és csatlakoztatása a saját hálózaton szempontjából szinkronizálása pont. Szinkronizálása ponthoz neve "Hibrid kapcsolat" Ebben és egyéb dokumentumok a az API-k és az Azure-portálon. A hibrid kapcsolatok szolgáltatás végpontjának továbbiakban szolgáltatásként"" – a többség ezt a dokumentumot.

A kapcsolati modell leans számos más hálózati API-k által létrehozott meg:

Először azt jelzi, hogy miként kezelje a bejövő kapcsolatokkal készenléti, és ezt követően fogadja el őket, hogy a beérkező figyelő van. A másik oldalon van egy kapcsolódó ügyfél, amely a figyelő meglepő, hogy kapcsolat fogadja el a kétirányú kommunikációt elérési létrehozó fog csatlakozni. "Csatlakozás", "Meghallgatását", "Elfogadás" is látni fogja a legtöbb szoftvercsatornán API-khoz ugyanazokkal a feltételekkel.

Minden kommunikációs továbbítását modell van mindkét fél, ellenőrizze a szolgáltatás végpontjának, amely lehetővé teszi a "figyelő" is "ügyfél" köznyelvi használja, és más terminológia túlterhelések; okozhat felé kimenő kapcsolatok a pontos terminológia, ezért használjuk hibrid kapcsolatok a következő módon:

A program a kapcsolat mindkét oldalára úgynevezett "ügyfél", mivel ezek az ügyfelek számára, hogy a szolgáltatás. Az ügyfél, vár, és fogadja el a kapcsolatok a "figyelő" vagy "figyelő szerepkör" tekinthető. A Service figyelő felé új kapcsolatot kezdeményez ügyfél neve a "feladó" vagy a "feladó"szerepkört.

## <a name="listener-interactions"></a>Figyelő funkciók

A figyelő van a szolgáltatás; négy interakciók a hivatkozás szakaszban a dokumentumon belül minden átutalásra vonatkozó részletes témakörben olvashat.

### <a name="listen"></a>Meghallgatása

Felkészülés a szolgáltatás, amely egy figyelő jelzése készen áll a kapcsolatok, fogadja el hoz létre egy kimenő webes szoftvercsatorna kapcsolat. A kapcsolat kézfogás hajtja egy hibrid kapcsolat konfigurálva a továbbítási névtér, és a biztonsági jogkivonat származó tovább státuszt biztosító "Meghallgatását" nevet a nevét. A webes szoftvercsatorna elfogadta a szolgáltatás, amikor a regisztráció befejeződött, és a létrehozott webhely szoftvercsatorna legyen az összes azt követő tevékenységek engedélyezése "vezérlő csatorna" életben. A szolgáltatás lehetővé teszi, hogy legfeljebb 25 egyidejű hallgatók hibrid kapcsolat. Ha a 2-es vagy több aktív hallgatók, bejövő kapcsolatokkal fog kell meghatározni át őket tetszőleges sorrendű; Előfordulhat, hogy valós terjesztési.

### <a name="accept"></a>Fogadja el

Egy feladót a szolgáltatás új kapcsolatot nyitja meg, amikor a szolgáltatás kiválasztása, és értesítést a hibrid kapcsolatban az aktív hallgatók közül. A rendszer értesítést küld a figyelő a megnyitott vezérlő csatornán a webes szoftvercsatorna végpontot, amely a figyelő csatlakoznia kell a kapcsolatot fogadásához URL-CÍMÉT tartalmazó JSON üzenetként.

Az URL-címet is, és közvetlenül a figyelő anélkül további; kell használni kódolt adatai csak érvényes egy rövid ideig, lényegében, az addig, amíg a feladó létrehozott-végpont kell a kapcsolatot, de legfeljebb 30 másodperc várakozás hajlandó. Az URL-cím csak egy sikeres kapcsolódás használható. Amint szinkronizálása URL-címét a webhely szoftvercsatorna-kapcsolat létrejött, a webes szoftvercsatornán minden további tevékenység továbbítását van, a és a feladónak vagy a szolgáltatás által értelmezését és beavatkozás nélkül.

### <a name="renew"></a>Megújítása 

A biztonsági jogkivonat, a figyelő regisztrálhat, és a vezérlő csatorna kezelése használandó előfordulhat, hogy lejár, amíg a figyelő működik. Jogkivonat lejárta nem érinti a folyamatban lévő kapcsolatokat, de a vezérlő csatorna szolgáltatás, illetve hamarosan azonnali lejárta után megszakítható okoz. A "megújítása" kézmozdulat a figyelő küldhet, hogy a vezérlő csatorna karbantarthatók az hosszabb ideig, cserélje le a jogkivonat, a vezérlő csatorna társított JSON üzenet.

### <a name="ping"></a>Ping 

Ha olyan helyzetbe, közvetítő, ahogyan a vezérlő csatorna maradjon üresjárati betöltés például balancers vagy címfordító előfordulhat, hogy legördülő a TCP-kapcsolatot. A "ping" kézmozdulat elkerülhető, amely a csatornát, amely emlékezteti, mindenki, a hálózati útvonalon, amely a kapcsolat az célja, hogy kell életben, és azt is figyelmezteti arra liveness vizsgálatot a értesülnie a kis mennyiségű adattal küldésével. Ha nem sikerül a ping, a vezérlő csatorna kell figyelembe venni használhatatlanná válik, és a figyelő újra kell.

## <a name="sender-interaction"></a>Feladó kapcsolati

A feladó csak egyetlen kapcsolati tevékenység a szolgáltatással, akkor kapcsolódik.

### <a name="connect"></a>Csatlakozás

A "Csatlakozás" kézmozdulat megnyílik egy webes szoftvercsatorna az szolgáltatása, kezeléséről a hibrid kapcsolat nevét és meglévő (nem kötelező, de alapértelmezés szerint szükséges) a lekérdezési karakterláncban "Küldés" engedély feljogosító biztonsági jogkivonat. A szolgáltatás majd a fentebb ismertetett módon figyelő együttműködhet, és a webes szoftvercsatorna képező szinkronizálása kapcsolat létrehozása a figyelő van. A webes szoftvercsatorna elfogadása után a webes szoftvercsatornán minden további interakciók ezért lesz egy csatlakoztatott figyelő.

## <a name="interaction-summary"></a>Kapcsolati összefoglaló tevékenység

A kapcsolati modell eredménye, hogy megtalálható-e a feladó ügyfél ki a "tiszta" webes szoftvercsatornát figyelő kapcsolódik, és, amely van szüksége, nincs további preambulumok vagy előkészítése az kézfogás. Ezzel gyakorlatilag bármilyen meglévő webes szoftvercsatorna ügyfél megvalósítás könnyen kihasználhatja a hibrid kapcsolatok szolgáltatás csak elküldésével megfelelően épített URL-címet a webes szoftvercsatorna ügyfél rétegbe.

A szinkronizálása kapcsolat webes szoftvercsatorna, amely a figyelő beolvassa az elfogadás kapcsolati keresztül is könnyen áttekinthető, és bármelyik meglévő webes szoftvercsatorna kiszolgáló végrehajtása néhány további minimális hardverabsztrakciós, amely a keret helyi hálózaton hallgatók műveletek "elfogadás" és a hibrid kapcsolatok távoli "elfogadás" műveletek között megkülönbözteti átadandó.

## <a name="protocol-reference"></a>Protocol (protokoll) hivatkozás

Ez a szakasz ismerteti a fentebb ismertetett protokoll kapcsolati adatait.

Az összes webhely szoftvercsatorna kapcsolatok hozhatók létre 443-as port frissítésként a HTTPS 1.1-es, amelyeket gyakran néhány webes szoftvercsatorna keretrendszer vagy API. A leírás Itt legyen végrehajtása semleges, anélkül, hogy egy adott keretrendszer javasolja.

## <a name="listener-protocol"></a>Figyelő Protocol (protokoll)

A figyelő protokoll két kapcsolat kézmozdulatok és három üzenet műveletből áll.

### <a name="listener-control-channel-connection"></a>Figyelő vezérlő csatornán keresztül

A vezérlő csatorna megnyitása a webes szoftvercsatorna kapcsolat létrehozása:

*wss: / / {névtér-cím} /* *$servicebus* */* *hybridconnection /**{path}? a művelet hc sb =... & sb-hc-id =... & sb-hc-jogkivonat =... "*

A *névtér-cím* a tartománynevét az Azure továbbítási névtér hibrid csatlakozni, általában az űrlap üzemeltető {*sajatNev}. servicebus.windows.net.*

A lekérdezési karakterlánc paraméter beállítások a következők

| Paraméter        | Szükség? | Leírás                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| a művelet hc SB | igen       | A paraméter kell lennie a figyelő szerepkör **a művelet hc sb meghallgatását =**                                                                                                                                |
| {path}       | igen       | Az előre beállított hibrid kapcsolatokat a figyelő regisztrálhatja a névtér URL-címként kódolt elérési útját. Ez a kifejezés van ellátva a rögzített * **$servicebus**/**hybridconnection /*** elérési út része. |
| SB-hc-jogkivonat  | igen\*     | A figyelő kell adnia egy érvényes, az URL-címként kódolt szolgáltatás Bus megosztott hozzáférési jogkivonat a névtér vagy **meghallgatása** jogosít hibrid kapcsolatot.                                           |
| SB-hc-azonosító     | nem        | Az ügyfél által megadott választható azonosító lehetővé teszi, hogy a végpontok közötti diagnosztikai nyomkövetési.                                                                                                                             |

A webes szoftvercsatorna-kapcsolat a hibrid kapcsolat elérési út nem regisztrált, eltávolított vagy egy érvénytelen vagy hiányzó jogkivonat, vagy más hiba miatt sikertelen, ha a hiba visszajelzés nyújtanak a normál HTTP 1.1 állapot visszajelzés modellt használja. Az állapot leírása fogja tartalmazni a hiba a nyomon követés-jelző azonosítóra kell juttatni Azure támogatása:

| Kód | Hibaüzenet          | Leírás                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nem található      | A hibrid kapcsolat **elérési út** nem érvényes, vagy az alap URL-cím hibás |
| 401  | Ezzel az illetéktelen   | A biztonsági jogkivonat a hiányzó vagy hibás vagy érvénytelen                  |
| 403  | Tiltott      | A biztonsági jogkivonat esetén nem érvényes az elérési út ehhez a művelethez          |
| 500  | Belső hiba | Hiba történt a szolgáltatásban                                    |

Ha a webes szoftvercsatorna-kapcsolat a szolgáltatás után kezdetben állított be, így a megfelelő Web szoftvercsatorna protokoll hibakód együtt is tartalmazni fogja a nyomon követés azonosítóját leíró hibaüzenet fog kommunikálandó az az oka szándékosan leáll. A szolgáltatás nem leáll a vezérlő csatorna nélkül, ha a hibát jelez. Bármely TISZTÍT leállítása ellenőrzött ügyfél.

| WS állapota | Leírás                                                                        |
|-----------|------------------------------------------------------------------------------------|
| az 1001      | A hibrid kapcsolat elérési törölték vagy tiltható le.                           |
| 1008      | A biztonsági jogkivonat lejárt, és ezért sérül a hitelesítési házirend. |
| 1011      | Hiba történt a szolgáltatás belül.                                           |

### <a name="accept-handshake"></a>Fogadja el a handshake 

Az elfogadás rendszer értesítést küld a szolgáltatás által a figyelő a korábban létrehozott vezérlő csatornán webes szoftvercsatorna szöveg keret JSON üzenetként. Válasz az üzenetre nem.

Az üzenet egy JSON "elfogadásához" nevű, amely meghatározza a következő tulajdonságokat adott időben objektum tartalmazza:

-   **cím** – használandó létrehozásáról a szolgáltatás a webhelyen szoftvercsatorna fogadja el a bejövő kapcsolatot az URL-karakterlánc.

-   **azonosító** – Ez a kapcsolat egyedi azonosítója. Ha a feladó ügyfél által megadott azonosítója, a feladó megadott érték, különben a rendszer által létrehozott érték.

-   **connectHeaders** – a feladó, amely magában foglalja a Sec-WebSocket-protokollt, mind a Sec-WebSocket-bővítmények fejlécek által a továbbítási végpontra megadott összes HTTP rovatfejek.

| Üzenet elfogadása                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

A JSON üzenetben megadott cím URL-CÍMÉT a webhely szoftvercsatorna elfogadásához vagy elutasításához a feladó szoftvercsatorna létrehozásához használt a figyelő.

#### <a name="accepting-the-socket"></a>A szoftvercsatorna elfogadása

Ha szeretne elfogadni, a figyelő egy WebSocket kapcsolatot létesít a megadott cím.

Figyelmét arra, hogy az előnézeti időszakra, URI-cím használhat egy egyszerű IP-címet, és a TLS-tanúsítvány, a kiszolgáló által biztosított meghiúsul, hogy a cím érvényességi. Ez a villámnézetben finomított lesz.

Ha a "fogadja el" üzenet "Sec WebSocket, protokoll" Élőfej folytat, várhatóan, hogy a figyelő csak elfogadása a WebSocket, ha adott protokoll támogat, és, hogy a WebSocket meghatározott állítja a fejlécen.

Ugyanaz a "Sec-WebSocket-bővítmények" fejléc vonatkozik. Ha keretében támogatja a kiterjesztése, azt a fejléc a *kiszolgáló* egymás válasz a szükséges "Sec-WebSocket-bővítmények" kézfogás a bővítmény kell beállítania.

Az URL-címet kell használni – az elfogadás szoftvercsatorna megállapítási, de a következő paraméterek tartalmazza:

| Paraméter        | Szükség? | Leírás                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| a művelet hc SB | igen       | Szoftvercsatornát elfogadása a paraméter csak akkor **a művelet hc sb = elfogadása**                                                                                                                                                                                                                          |
| {path}       | igen       | Az előre beállított hibrid kapcsolatokat a figyelő regisztrálhatja a névtér URL-címként kódolt elérési útját. Ez a kifejezés van ellátva a rögzített * **$servicebus**/**hybridconnection /*** elérési út része.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB-hc-id |} No        | A fenti **azonosító** leírása megjelenítéséhez.                                                                                                                                                                                                                                                              |

Ha hiba lép fel, a szolgáltatás lehet válaszolni az alábbi képlettel történik:

| Kód | Hibaüzenet          | Leírás                         |
|------|----------------|-------------------------------------|
| 403  | Tiltott      | Az URL-formátuma nem érvényes.               |
| 500  | Belső hiba | Hiba történt a szolgáltatásban |

Miután a kapcsolat létrejött, a kiszolgáló állítsa le a webes szoftvercsatorna történik, ha a feladó webes szoftvercsatorna leállítja a le vagy a következő állapotú

| WS állapota | Leírás                                                                        |
|-----------|------------------------------------------------------------------------------------|
| az 1001      | A feladó ügyfél leállítja a kapcsolat                                        |
| az 1001      | A hibrid kapcsolat elérési törölték vagy tiltható le.                           |
| 1008      | A biztonsági jogkivonat lejárt, és ezért sérül a hitelesítési házirend. |
| 1011      | Hiba történt a szolgáltatás belül.                                           |

#### <a name="rejecting-the-socket"></a>A szoftvercsatorna elutasítása

A hasonló kézfogás a szoftvercsatorna elvetésével után az "fogadja el" üzenet vizsgálata van szükség, így a állapotkódja és az állapot leírása, a visszautasítást okát kommunikáció a feladónak flow is.

A protocol tervezés választás az alábbi környezetbe a WebSocket kézfogás (vagyis egy definiált hibás állapotú végén készült), hogy figyelő ügyfél megvalósítás WebSocket ügyfél támaszkodhat továbbra is, és nem kell alkalmaznia egy plusz, a HTTP-ügyfél bA.

A szoftvercsatorna elutasításához az ügyfél tart az "fogadja el" üzenet URI-cím, és két lekérdezési karakterlánc paraméterei hozzáfűzi:

| Paraméter             | Szükség? | Leírás                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | igen       | Numerikus HTTP állapotkód                |
| statusDescription | igen       | Az elutasítás emberi olvasható oka |

Az eredményül kapott URI majd használt; WebSocket kapcsolatot létesíteni ismét szem előtt, hogy a TLS tanúsítvány előfordulhat, hogy nem egyezik meg a címet a villámnézetben, adatérvényesítési is tiltható le kell.

Helyesen hajt végre, ha a handshake szándékosan meghiúsul, és a HTTP-hibakódot 410, mivel nincs WebSocket létrejött. Ha hiba történik, ezek a lehetőségek:

| Kód | Hibaüzenet          | Leírás                         |
|------|----------------|-------------------------------------|
| 403  | Tiltott      | Az URL-formátuma nem érvényes.               |
| 500  | Belső hiba | Hiba történt a szolgáltatásban |

### <a name="listener-token-renewal"></a>Figyelő jogkivonat megújítása

A figyelő token lejárta, amikor azt lecserélheti azt a szolgáltatást a kijelölt ellenőrzési csatornán keresztül küld a keret szöveges üzenet. Az üzenet egy "renewToken", amely meghatározza a következő tulajdonság adott időben nevű JSON-objektum tartalmazza:

-   **jogkivonat** – egy érvényes, az URL-címként kódolt szolgáltatás Bus megosztott hozzáférési jogkivonat a névtér vagy **meghallgatása** jogosít hibrid kapcsolatot.

| Üzenet renewToken                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Ha jogkivonat hibás, hozzáférés megtagadva és felhőbeli szolgáltatástól bezárul a vezérlő csatorna websocket hiba, egyéb esetben nem érkezik válasz.

| WS állapota | Leírás                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | A biztonsági jogkivonat lejárt, és ezért sérül a hitelesítési házirend. |

<a name="sender-protocol"></a>Feladó Protocol (protokoll)
---------------

A feladó protokoll megegyezik hatékony hogyan figyelő jön létre. A cél, a végpontok közötti webes szoftvercsatorna maximális áttetszőség. A cím való csatlakozáshoz ugyanaz, mint a figyelő, de a "művelet" eltér, és a token egy másik engedélyre van szüksége:

*wss: / / {névtér-cím} /* *$servicebus* */* *hybridconnection /**{path}? a művelet hc sb =... & sb-hc-id =... \[és sbc-hc-jogkivonat =... \]*

A *névtér-cím* a tartománynevét az Azure továbbítási névtér hibrid csatlakozni, általában az űrlap üzemeltető {*sajatNev}. servicebus.windows.net.*

A kérés tetszőleges további HTTP fejlécek alkalmazás által definiált is tartalmazhatnak. Az összes megadott élőfej a figyelő való flow, és a vezérlő "fogadja el" üzenet "connectHeader" tárgya találhatók.

A lekérdezési karakterlánc paraméter beállítások a következők

| Paraméter        | Szükség? | Leírás                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| a művelet hc SB | igen       | A paraméter kell lennie a figyelő szerepkör **művelet = csatlakozás**                                                                                                                                                    |
| {path}       | igen       | Az előre beállított hibrid kapcsolatokat a figyelő regisztrálhatja a névtér URL-címként kódolt elérési útját.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB-hc-jogkivonat |} Yes\*     | A figyelő kell adnia egy érvényes, az URL-címként kódolt szolgáltatás Bus megosztott hozzáférési jogkivonat a névtér vagy a **Küldés** jogosít hibrid kapcsolatot.                                                            | | SB-hc-id |} No        | Nem kötelező azonosítója, amely lehetővé teszi a végpontok közötti diagnosztikai nyomkövetési és számára hozzáférhető a figyelő az elfogadás kommunikációkezdéskor.                                                                                       |

A webes szoftvercsatorna-kapcsolat a hibrid kapcsolat elérési út nem regisztrált, eltávolított vagy egy érvénytelen vagy hiányzó jogkivonat, vagy más hiba miatt sikertelen, ha a hiba visszajelzés nyújtanak a normál HTTP 1.1 állapot visszajelzés modellt használja. Az állapot leírása fogja tartalmazni a hiba a nyomon követés-jelző azonosítóra kell juttatni Azure támogatása:

| Kód | Hibaüzenet          | Leírás                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nem található      | A hibrid kapcsolat **elérési út** nem érvényes, vagy az alap URL-cím hibás |
| 401  | Ezzel az illetéktelen   | A biztonsági jogkivonat a hiányzó vagy hibás vagy érvénytelen                  |
| 403  | Tiltott      | A biztonsági jogkivonat esetén nem érvényes az elérési út ehhez a művelethez          |
| 500  | Belső hiba | Hiba történt a szolgáltatásban                                    |

Ha a webes szoftvercsatorna-kapcsolat a szolgáltatás után kezdetben állított be, így a megfelelő Web szoftvercsatorna protokoll hibakód együtt is tartalmazni fogja a nyomon követés azonosítóját leíró hibaüzenet fog kommunikálandó az az oka szándékosan leáll.

| WS állapota | Leírás                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1000      | A figyelő a szoftvercsatorna leállt.                                                 |
| az 1001      | A hibrid kapcsolat elérési törölték vagy tiltható le.                           |
| 1008      | A biztonsági jogkivonat lejárt, és ezért sérül a hitelesítési házirend. |
| 1011      | Hiba történt a szolgáltatás belül.                                           |

## <a name="next-steps"></a>A következő lépéseket:

- [Továbbítás – gyakori kérdések](relay-faq.md)
- [Névtér létrehozása](relay-create-namespace-portal.md)
- [Első lépések a .NET rendszerhez](relay-hybrid-connections-dotnet-get-started.md)
- [Csomópont – első lépések](relay-hybrid-connections-node-get-started.md)