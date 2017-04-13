<properties 
    pageTitle="Az Azure API-kezelési házirendek |} Microsoft Azure" 
    description="Megtudhatja, hogy miként létrehozása, szerkesztése és az API-kezelési házirendek beállítása." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


#<a name="policies-in-azure-api-management"></a>Az Azure API adatkezelési házirendek

Azure API-kezelése házirendek olyan hatékony lehetőséget, a rendszer, amelyek lehetővé teszik az API keresztül konfigurációs viselkedésének módosítása a publisher. Házirendek olyan gyűjteménye, kimutatások, egymás után a kérésre végrehajtott vagy a válasz egy API. Népszerű kimutatások vegye fel a konvertálás XML-ből JSON-ba, és hívja fel a bejövő hívásait a fejlesztők mennyiségét korlátozni korlátozása ráta. Számos további házirendek beépített érhetők el.

A [Házirend-hivatkozás][] teljes listáját a házirendi és beállításaik talál.

Házirendek alkalmazza a program az átjáróban, amelyek között az API-ügyfél és a felügyelt API helyezkedik el. Az átjáró összes kéréseket fogad, és általában úgy továbbítja az alapul szolgáló API-nak változatlan. Azonban a házirend is módosításainak alkalmazása a bejövő kérelem és a kimenő választ.

Házirend-kifejezések is alkalmazható attribútum vagy szöveges értékek közül az API-kezelési házirendek kivéve, ha a házirend mást ad meg. Néhány irányelvet, például a [Control flow][] és [-változó beállítása][] házirendek házirend kifejezések alapulnak. További tudnivalókért lásd: a [Speciális házirendek][] és a [házirend kifejezések][].

## <a name="scopes"> </a>Házirendek beállítása
Házirendek beállíthatók globálisan vagy egy [termék][], [API][] vagy [művelet][]körét. Állítsa be egy házirendet, nyissa meg a házirend-szerkesztő a publisher-portálon.

![Házirendek menü][policies-menu]

A házirend-szerkesztő három fő részből áll: a házirend hatálya (felső), a házirend-definícióhoz, ahol házirendek szerkesztett (balra), és a kimutatásokban (jobbra) listája:

![Házirend-szerkesztő][policies-editor]

A kezdéshez házirend beállítása ki kell választania a hatókör, amelynél a házirend kell alkalmazni. Az alábbi a **Starter** képernyőképen termék be van jelölve. Látható, hogy a szabály neve mellett a négyzetes jelek jelzik, hogy, hogy a házirend már alkalmazott e szintjén.

![Hatókör][policies-scope]

A házirend már telepítve van, mivel a konfiguráció a definíció nézetben jelenik meg.

![Konfigurálása][policies-configure]

A házirend jelenik meg, csak olvasható először. Kattintson a definíció **Házirendek beállítása** művelet szerkesztéséhez.

![Szerkesztése][policies-edit]

A házirend-definícióhoz egy egyszerű XML-dokumentum, amely leírja a bejövő és kimenő kimutatások sorozatát. Az XML szerkeszthető közvetlenül a definíció ablakában. Kimutatások listájának jobb hiányzik, és az aktuális hatókör alkalmazandó kimutatások vannak engedélyezve van, és a kiemelt; ahogy a fenti képernyőképet **Korlát hívás ráta** utasításában.

Kattintás az engedélyezett utasítás hozzáadja a megfelelő XML azon a helyen, a kurzor a definíció nézetben. 

>[AZURE.NOTE] Ha nincs engedélyezve a házirendet, amely a hozzáadni kívánt, ellenőrizze, hogy helyes, hogy a házirend hatálya alá. Minden egyes házirendi készült az egyes tartományok és a szakaszok házirend. Tekintse át a házirend szakaszokat és a keresési tartományok egy házirendet, jelölje be a **használatát** a [Házirend-hivatkozás][]házirendhez szakaszát.

Egy teljes listát az házirendi és beállításaik érhetők el a [Házirend-hivatkozás][].

Például egy új kimutatást, bejövő felkérést korlátozhatja a megadott IP-címek hozzáadásához helyezze a kurzort arra elé tartalmát a `inbound` XML-elem, és kattintson az **IP-címei szűkítése hívó** utasítás.

![Korlátozás házirendek][policies-restrict]

Ez egy XML-kódrészlet, hogy hozzáadja a `inbound` nyújt útmutatást, hogy miként kell beállítani a kimutatás elemet.

    <ip-filter action="allow | forbid">
        <address>address</address>
        <address-range from="address" to="address"/>
    </ip-filter>

Bejövő felkéréseit, és fogadja el, csak azokat a 1.2.3.4 IP-címének módosítása az XML az alábbi képlettel történik:

    <ip-filter action="allow">
        <address>1.2.3.4</address>
    </ip-filter>

![Mentés][policies-save]

Teljes konfigurálása a kimutatások házirend, kattintson a **Mentés** gombra, és a módosítások fog kell propagálja az API az adatkezelési átjáró azonnal.

##<a name="sections"> </a>Ismertetése házirend beállítása

A házirend ahhoz, hogy a kérelem és a válasz végrehajtása kimutatásokat sorozata. A konfiguráció oszlik megfelelően `inbound`, `backend`, `outbound`, és `on-error` szakaszok, ahogy az alábbi beállításokkal.

    <policies>
      <inbound>
        <!-- statements to be applied to the request go here -->
      </inbound>
      <backend>
        <!-- statements to be applied before the request is forwarded to 
             the backend service go here -->
      </backend>
      <outbound>
        <!-- statements to be applied to the response go here -->
      </outbound>
      <on-error>
        <!-- statements to be applied if there is an error condition go here -->
      </on-error>
    </policies> 

Ha egy kérelmet feldolgozása közben hibát jelez, lépéseit a további a `inbound`, `backend`, vagy `outbound` kimaradnak a szakaszok és a végrehajtás ugrik az utasításokat a `on-error` szakaszban. Helyezze a házirendi a `on-error` szakasz használatával áttekintheti a hiba a `context.LastError` tulajdonság, nézze meg, és a hiba válasz használatával testreszabása a `set-body` házirendet, és konfigurálása, hogy mi történik, ha hiba történik. Vannak olyan hibakódok házirendi feldolgozása során fellépő hibák és a beépített lépéseket. További tudnivalókért olvassa el a [Hiba az API-kezelési házirendek kezelése](https://msdn.microsoft.com/library/azure/mt629506.aspx)című témakört.

Mivel házirendek különféle szintű (globális, termék, API-val és a művelet) adható meg a konfigurációs teszi lehetővé, hogy adja meg a sorrendben, amelyben a házirend-definícióhoz kimutatások hajtsa végre a szülő-házirend részletez. 

A következő sorrendben házirend hatókörök értékeli ki.

1. Globális hatóköre
2. Termék hatóköre
3. A hatókör API
4. A művelet hatókör

A kimutatások határértékeket helyzetének szerint értékeli ki az `base` elemet, ha a bemutató.

Például a globális szinten, és az API konfigurált házirend Ha egy házirendet, majd amikor használják, hogy az adott API mindkét házirend fog vonatkozni. API-kezelés lehetővé teszi, hogy az Alap elem keresztül kombinált házirendi mérvadó rendezési. 

    <policies>
        <inbound>
            <cross-domain />
            <base />
            <find-and-replace from="xyz" to="abc" />
        </inbound>
    </policies>

A fenti példa házirend-definícióban az `cross-domain` utasítás szeretne végrehajtani, mielőtt bármilyen magasabb házirendek viszont, amely követi a `find-and-replace` házirend.

Ugyanaz a házirend kétszer a házirendi szabály jelenik meg, ha a program alkalmazza a legutóbb kiértékelt szabályt. Használhatja ezt a magasabb hatókör definiált házirendek felülbírálják. A házirendek az aktuális hatókör a csoportházirend-szerkesztőben megtekintéséhez kattintson a **kijelölt hatókör hatékony házirend újraszámítása**gombra.

Ne feledje, hogy globális házirend nem szülő házirendet, és használja a `<base>` elemet, akkor nem befolyásolja. 

## <a name="next-steps"></a>Következő lépések

Nézze meg a videó követő házirend kifejezésekre.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Házirend-hivatkozás]: api-management-policy-reference.md
[Termék]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Művelet]: api-management-howto-add-operations.md

[Speciális házirendek]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Változó beállítása]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Házirend-kifejezések]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
