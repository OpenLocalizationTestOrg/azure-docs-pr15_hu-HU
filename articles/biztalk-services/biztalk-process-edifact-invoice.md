<properties
   pageTitle="Oktatóprogram: Azure BizTalk szolgáltatások használata EDIFACT számlák folyamat |} Microsoft Azure BizTalk szolgáltatások"
   description="Hogyan hozhat létre és konfigurálása a mezőben, összekötő vagy API alkalmazást, és vele az Azure alkalmazás szolgáltatás összefüggés-alkalmazásban"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Oktatóprogram: Folyamat EDIFACT számlák Azure BizTalk szolgáltatások használata
A BizTalk Services portál használatával állítsa be és és üzembe X12 EDIFACT rendelkezést. Ebben az oktatóanyagban megnézi, hogyan hozhat létre egy EDIFACT megállapodást cseréjére számlák kereskedelmi partnerei között. Ebben az oktatóprogramban egy végpont az üzleti megoldás EDIFACT üzenetváltásra Northwind és Contoso két kereskedelmi partnerek érintő körül íródott.  

## <a name="sample-based-on-this-tutorial"></a>Ebben az oktatóanyagban alapján minta
Ebben az oktatóanyagban írott körül minta **Küldése EDIFACT számlák BizTalk szolgáltatások használatával**, amely az [MSDN-kód gyűjtemény](http://go.microsoft.com/fwlink/?LinkId=401005)töltse le a rendelkezésére áll. Esetleg a minta használja, és ebben az oktatóanyagban megértéséhez, hogy hogyan a minta készült folyamatát. Vagy, ha ez az oktatóanyag segítségével hozzon létre saját megoldás alapokról felfelé. Ebben az oktatóanyagban cél a második módszer, hogy megismeri hogyan a megoldás készült. Is lehető, az oktatóprogram mintát követik és használja (például sémák, átalakítások) eltérések ugyanaz a neve a mintában használt.  

>[AZURE.NOTE] Ez a megoldás érint egy EAI híd üzenetet küld egy szerkesztése hidat, mert a [BizTalk szolgáltatások híd láncolás minta](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) minta azt használja.  

## <a name="what-does-the-solution-do"></a>Mit jelent a megoldást?

Ez a megoldás a Northwind a Contoso EDIFACT számlák kap. A számlák nem szabványos szerkesztése formátumban vannak. Igen küld az adott számla Northwind, mielőtt azt kell átalakítását egy EDIFACT számla (más néven INVOIC) dokumentumot. Visszaigazolás, a Northwind kell a EDIFACT számla folyamat, és vissza (más néven CONTRL) vezérlő üzenet Contoso.

![][1]  

Ebben az esetben üzleti eléréséhez Contoso használja a Microsoft Azure-BizTalk szolgáltatásokkal szolgáltatásait.

*   A Contoso EAI hidakat használja az eredeti számlát EDIFACT INVOIC átalakítása.

*   A EAI híd majd az üzenetet küld egy szerkesztése küldés híd konfigurálva a BizTalk Services portál megállapodás keretében rendszerbe.

*   A szerkesztése küldés híd a EDIFACT INVOIC folyamatok, és a Northwind továbbítja.

*   Után az adott számla érkezik, a Northwind visszatér a CONTRL üzenet a szerkesztése megállapodás keretében rendszerbe híd kap.  

> [AZURE.NOTE] Tetszés szerint a megoldás is bemutatja, hogyan küldhet kötegelés a számlák kötegekben egyes számlát külön-külön küldő helyett.  

Elvégzéséhez az alkalmazási példát, azt használ szolgáltatási Bus sorban várakozó Contoso számla küldése Northwind és fogadásához nyugtázó a Northwind. Ezek a sorok ügyfélalkalmazás, amely letölthető és a minta-csomagban elérhető található létrehozható részeként ebben az oktatóanyagban.  

## <a name="prerequisites"></a>Előfeltételek

*   A szolgáltatás Bus névtér kell rendelkeznie. A névtér létrehozása című cikkben olvashat [Útmutató: létrehozása vagy módosítása a szolgáltatás Bus szolgáltatás Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Tudassa velünk feltételezik, hogy már kiépítve, szolgáltatás Bus névteret **edifactbts**neve.

*   BizTalk szolgáltatások előfizetés kell rendelkeznie. Című cikkben olvashat [BizTalk szolgáltatás létrehozása az Azure klasszikus portál használatával](http://go.microsoft.com/fwlink/?LinkID=302280). Az ebben az oktatóanyagban tudassa velünk feltételezik **contosowabs**nevű BizTalk szolgáltatások előfizetése van.

*   Regisztráljon az BizTalk Services portál BizTalk szolgáltatások előfizetését. Című cikkben olvashat [a BizTalk szolgáltatás telepítése az BizTalk Services portál rögzítése](https://msdn.microsoft.com/library/hh689837.aspx)

*   Visual Studio telepítve kell rendelkeznie.

*   BizTalk szolgáltatások SDK telepítve kell rendelkeznie. A SDK letöltheti a [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Lépés: 1: A szolgáltatás Bus sorban várakozó létrehozása  
A megoldás használja a szolgáltatás Bus sorban várakozó üzenetváltásra kereskedelmi partnerei között. A Contoso és a Northwind üzeneteket küldeni az sorban várakozó származó, ahol a EAI és/vagy szerkesztése hidakat felhasználása őket. Ehhez a megoldáshoz három szolgáltatás Bus sorban várakozó van szüksége:

*   **northwindreceive** – Northwind megkapja a számlát a Contoso a várólista fölé.

*   **contosoreceive** – Contoso megkapja a visszaigazoló a Northwind a várólista keresztül.

*   **felfüggesztett** – az összes felfüggesztett üzenetek továbbítása a sorba. Üzenetek felfüggesztett vannak, ha nem tudnak feldolgozása közben.

A szolgáltatás Bus sorban várakozó a minta csomagban ügyfélalkalmazás használatával hozhat létre.  

1.  Nyissa meg a helyet, ahol a minta letöltött, **Oktatóanyag küldése számlák használatával BizTalk szolgáltatások szerkesztése Bridges.sln**.

2.  Nyomja le az **F5** létre, és indítsa el az **Oktatóprogram** ügyfélalkalmazás.

3.  A képernyőn adja meg a szolgáltatás Bus ACS névtér, a kibocsátó neve és a kibocsátó billentyűt.

    ![][2]  
4.  Egy üzenetben kéri, hogy három sorban várakozó a szolgáltatás Bus névtér jön létre. Kattintson az **OK gombra**.

5.  Az oktatóprogram ügyfél operációs rendszert futtató hagyja. Nyissa meg a, kattintson a **Szolgáltatás Bus** > **_a szolgáltatás Bus névtér_** > **sorban várakozó**, és ellenőrizze, hogy létrejönnek-e a három sorokat.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Lépés: 2: Hozzon létre, és helyezhetnek üzembe a kereskedelmi partner szerződés
Kereskedelmi partner és egy megállapodást Contoso Northwind létrehozása. A kereskedelmi partner szerződés között két partnerei melyik üzenet séma szeretne használni, mely üzenetek protokoll használatát, stb például kereskedelmi szerződés határozza meg. A kereskedelmi partner szerződés két szerkesztése hidakat, egy üzeneteket küldeni (más néven a **híd szerkesztése küldése**) kereskedelmi partnerek pedig (más néven a **híd szerkesztése fogadása**) kereskedelmi partnerek érkező üzenetek fogadására tartalmazza.

A megoldás környezetben a a szerkesztése küldés híd felel meg a Küldés oldalán a szerződést, és a EDIFACT számla küldi a Contoso a Northwind. Hasonlóképpen a szerkesztése fogadási híd fogadási, a szerződés oldalán lévő felel meg, és a Northwind fogadási nyugták szolgál.  

### <a name="create-the-trading-partners"></a>A kereskedelmi partner létrehozása

Kezdésként létrehozása kereskedelmi partnerek Contoso és a Northwind.  

1.  A BizTalk Services portál, a **partnerek** lapon kattintson a **Hozzáadás**gombra.

2.  Az új partner lapon adja meg a **Contoso** a partner neveként, és kattintson a **Mentés**gombra.

3.  Ismételje meg a második partner, a **Northwind**létrehozásához.  

### <a name="create-the-agreement"></a>A szerződés létrehozása
Kereskedelmi partner rendelkezést közötti kereskedelmi partnerek üzleti profilok jönnek létre. Ez a megoldás használja az alapértelmezett partner profilokat, amelyek automatikusan létrejönnek, ha a partnerek létrehozott.  

1.  Kattintson a BizTalk Services portál **rendelkezést** > **Hozzáadás gombra**.

2.  Az új megállapodás **Az általános beállítások** lapon adja meg az értékeket, az alábbi képen látható módon, és kattintson a **Folytatás**gombra.

    ![][3]  

    **Folytatás**gombra kattintás után két füle, fogadása és **Beállítások** **Küldése** elérhetővé válnak.

3.  A Contoso és a Northwind küldés megállapodást létrehozása. A jelen szerződés szabályozza, hogy hogyan Contoso küld a EDIFACT számla Northwind.

    1.  Kattintson a **Küldés a beállítások**gombra.

    2.  Megtartja az alapértelmezett értékeket, a **Bejövő URL-CÍMÉT**, **átalakítási**és **Batching** lapokon.

    3.  A **Protocol (protokoll)** lapon a **sémák** csoportban töltse fel a **EFACT_D93A_INVOIC.xsd** sémában. Ez a séma a minta csomag érhető el.

        ![][4]  
    4.  Az **átviteli** lapon adja meg a szolgáltatás Bus sorban várakozó részleteit. A Küldés melletti szerződés használjuk, a EDIFACT számla küldeni a Northwind **northwindreceive** várakozási sorban található, és a **felfüggesztett** várólista és az üzenetek, a nem sikerül feldolgozása közben, és fel van függesztve irányítja. Ezek a sorok létrehozott **lépés 1: a szolgáltatás Bus sorban várakozó létrehozása** (az ebben a témakörben).

        ![][5]  

        A **átviteli beállításai > típus átviteli** és **felfüggesztése Üzenetbeállítások > típus átviteli**, jelölje be az Azure Service Bus, és adja meg az értékeket a képen látható módon.

4.  A Contoso és a Northwind fogadási megállapodást létrehozása. A jelen szerződés szabályozza, hogy hogyan Contoso megkapja a visszaigazoló Northwind.

    1.  Kattintson a **fogadás beállításai**gombra.

    2.  Megtartja az alapértelmezett értékeket a **szállítási** és **Átalakítás** lap.

    3.  A **Protocol (protokoll)** lapon a **sémák** csoportban töltse fel a **EFACT_4.1_CONTRL.xsd** séma. Ez a séma a minta csomag érhető el.

    4.  Az **útvonal** lapon győződjön meg arról, hogy csak a Northwind nyugták Contoso vannak-e irányítva szűrő létrehozása **Útvonal beállítások**csoportban kattintson a **Hozzáadás** a útválasztási szűrő létrehozása lehetőséget.

        ![][6]  
        1.  Adjon meg értéket a **Szabály nevét**, **útvonal szabály**és **útvonal cél** , a képen látható módon.

        2.  Kattintson a **Mentés**gombra.

    5.  Az **útvonal** lapon újra, adja meg, ahol felfüggesztett nyugták (nyugták sikertelen feldolgozása közben), a rendszer továbbítja. Állítsa a átviteli típusra Azure Service Bus, irányítja a cél típusának **várólista**, hitelesítés típusa az **Access aláírás megosztott** (Társítások), a szolgáltatás Bus névtér a szükséges a Társítások kapcsolati karakterláncot, és írja be a várólista neve **felfüggesztett**.

5.  Végül kattintson a **Deploy** bevezetését tervezi a szerződést. Megjegyzés: a végpontok hol küldése és fogadása rendelkezést üzemelnek.

    *   **Küldési beállítások** lapján a **Bejövő URL-CÍMÉT**vegye figyelembe az végpontot. Üzenet küldése egy Contoso való használatáról a szerkesztése küldés híd Northwind, egy üzenet kell küldenie a végpont.

    *   **Fogadási beállítások** lapján a **átviteli**vegye figyelembe az végpontot. Üzenet küldése egy Northwind consotóhoz használata a szerkesztése kap híd kell üzenetet küldeni a végponttal.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Lépés 3: Hozzon létre, és telepítse az BizTalk szolgáltatások projektet

Az előző lépésben szerkesztése küldés rendszerbe, és fogadhat rendelkezést EDIFACT számlák és nyugták feldolgozása. Ezek a rendelkezést csak az olyan folyamat üzeneteket, amelyek megfelelnek a szabványos EDIFACT üzenet séma is. Azonban a megoldás esetben egy Contoso küld számla Northwind házon belüli saját sémában. Igen mielőtt a Küldés szerkesztése híd az üzenetet küld, azt kell átalakítását a belső séma a szabványos EDIFACT számla séma. A BizTalk szolgáltatások EAI projektet tartalmaz, amely.

A BizTalk szolgáltatások projekt **InvoiceProcessingBridge**, hogy az üzenet átalakítások egyben a letöltött minta tartalmazza. A projekt magában foglalja a következő eltérések:

*   **INHOUSEINVOICE. XSD** – a Northwind küldött házon belüli számla séma.

*   **EFACT_D93A_INVOIC. XSD** – standard EDIFACT számla séma.

*   **EFACT_4.1_CONTRL. XSD** – a EDIFACT visszaigazolás, amely a Contoso küld a Northwind adatbázis sémája.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – az átalakítás, amely a belső számla séma megfelelteti a szabványos EDIFACT számla sémának.  

### <a name="create-the-biztalk-services-project"></a>A szolgáltatások BizTalk projekt létrehozása
1.  A Visual Studio megoldás bontsa ki a InvoiceProcessingBridge projektet, és nyissa meg a **MessageFlowItinerary.bcs** fájlt.

2.  Kattintson bárhová a vászonra, majd adja meg a **BizTalk szolgáltatás URL-CÍMÉT** szeretne, adja meg az BizTalk szolgáltatások előfizetése nevére. Ha például `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Az eszközkészlet, az egérrel húzza az **Xml-One-Way híd** a vásznat. Állítsa a **Személy neve** és a **Relatív cím** tulajdonságok hidat **ProcessInvoiceBridge**. Kattintson duplán a **ProcessInvoiceBridge** kattintva nyissa meg a híd konfigurációs felület.

4.  A **Üzenet típusa** mezőben kattintson a plusz (**+**) gombra kattintva adja meg a bejövő üzenet a sémában. Mivel a bejövő üzenetek a EAI híd mindig a belső számla, állítsa a **INHOUSEINVOICE**.

    ![][8]  
5.  Kattintson az **XML-átalakító** alakzatra, és tulajdonságmezőben a **térképek** tulajdonság, kattintson a három pontra (**…**) gombra. **Térképek kiválasztása** párbeszédpanelen jelölje ki a **INHOUSEINVOICE_to_D93AINVOIC** átalakítás fájlt, és kattintson **az OK**gombra.

    ![][9]  
6.  Térjen vissza az **MessageFlowItinerary.bcs**, és az eszköztárról, és húzza a **Kétirányú külső szolgáltatás végpontjának** jobb oldalán a **ProcessInvoiceBridge**. A **Szervezet neve** tulajdonságot állítsa **EDIBridge**.

7.  A megoldás Explorer bontsa ki a **MessageFlowItinerary.bcs** , és kattintson duplán a **EDIBridge.config** fájlra. Cserélje le a **EDIBridge.config** tartalmát a következő.

    > [AZURE.NOTE] Miért van szükség a .config fájlok szerkesztésekor? A külső szolgáltatás végpontjának, amelyek a híd Lekérdezéstervező vászon azt hozzá a szerkesztése hidakat, amely a korábban telepített azt jelöli. Szerkesztése hidakat kétirányú hidakat, a Küldés és fogadás, oldal. A EAI hidat, amely a híd Tervező jelöltük azonban egy egyirányú azt. Igen a két hidakat másik üzenetet exchange szokások kezelésére, használjuk egy egyéni híd viselkedés konfigurációját felvételével a .config fájlban. Emellett az egyéni működés is képes a hitelesítési az szerkesztése küldés híd végpontot. Egyéni mindez a [BizTalk szolgáltatások híd láncolás minta - EAI való szerkesztése](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104)külön mintaként érhető el. Ez a megoldás a minta használja.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Konfigurációs kapcsolatos részleteket a EDIBridge.config fájl frissítése

    *   A _<behaviors>_, adja meg a ACS névtér és a BizTalk Services-előfizetéséhez társított billentyűt.

    *   A _<client>_, adja meg a végpontot, ahol szerkesztése küldés létrejött telepítve van.

    Mentse a módosításokat, és zárja be a konfigurációs fájl.

9.  Az eszközkészlet lapon kattintson az **összekötő** , és csatlakozzon az **ProcessInvoiceBridge** és **EDIBridge** összetevők. Jelölje ki az összekötőt, és tulajdonságai párbeszédpanelen állítsa be **Szűrőfeltétel** **Egyezés összes**. Ez biztosítja, hogy a EAI híd által feldolgozott összes üzenetet, a rendszer továbbítja a szerkesztése híd.

    ![][10]  
10.  Mentse a módosításokat a megoldáshoz.  

### <a name="deploy-the-project"></a>A projekt telepítése

1.  Azon a számítógépen, amelyen létrehozta a BizTalk szolgáltatások projekt töltse le és telepítse az SSL-tanúsítvány BizTalk szolgáltatások előfizetéséhez. A forrás BizTalk szolgáltatások csoportban kattintson az **Irányítópult**, és válassza az **SSL-tanúsítvány letöltése**parancsra. Kattintson duplán a tanúsítványra, és kövesse a kérdésre, a telepítés befejezéséhez. Ellenőrizze, hogy telepíteni a tanúsítványt, a **Megbízható legfelső szintű hitelesítésszolgáltatók** tanúsítvány áruházból.

2.  A Visual Studio megoldás Intézőben kattintson a jobb gombbal a **InvoiceProcessingBridge** projekt, és válassza a **központi telepítés**.

3.  Adja meg az értékeket a képen látható módon, és kattintson a **központi telepítés**gombra. Kattintson a **Kapcsolat adatait** az BizTalk szolgáltatások irányítópult BizTalk szolgáltatások ACS hitelesítő adatok válik elérhetővé.

    ![][11]  

    Másolja az végpontot, ahol a EAI híd telepítve van, például a kimeneti ablakban `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. A végpont URL-CÍMÉT később szüksége lesz.  

## <a name="step-4-test-the-solution"></a>Lépés: 4: Tesztelje a megoldás


Ez a témakör azt megnézheti hogyan ellenőrizheti a megoldást az **Oktatóprogram** ügyfélalkalmazás a minta részeként használatával.  

1.  A Visual Studióban nyomja le az F5 billentyűparancs hatására az **Oktatóprogram ügyfél**indításához.

2.  A képernyő kell rendelkeznie a lépés előre megadott értékeket a szolgáltatás Bus sorban várakozó létrehozott hol. Kattintson a **Tovább**gombra.

3.  A következő ablakra, adja meg a BizTalk szolgáltatások előfizetés ACS hitelesítő adatait, és a végpontok hol EAI és szerkesztése (kap) hidakat van telepítve.

    Az előző lépésben másolta volt az EAI híd végpontot. Szerkesztése fogadása híd végpontot, a BizTalk Services portál, nyissa meg a szerződés > fogadási beállítások > átviteli > végpontot.

    ![][12]  
4.  A következő ablakra Contoso, csoportban kattintson a **Belső számla elküldése** gombra. A fájlban párbeszédpanel megnyitásához nyissa meg a INHOUSEINVOICE.txt fájlt. Vizsgálja meg a fájl tartalmát, és kattintson az **OK gombra** a számla elküldése.

    ![][13]  
5.  Néhány másodperc alatt a számla érkezik, a Northwind. Az **Üzenet megtekintése** hivatkozásra a Northwind megkapta számla megjelenítéséhez. Figyelje meg hogyan a Northwind megkapta számla szabványos EDIFACT sémában Contoso által küldött egy belső séma állapotában.

    ![][14]  
6.  Válassza ki a számlát, és kattintson a **Küldés nyugtázó**. Előugró párbeszédpanelen figyelje meg, hogy a interchange azonosító ugyanazt a kapott számla és a visszaigazolás küldött. A **Visszaigazoló küldése** párbeszédpanelen az OK gombra.

    ![][15]  
7.  A Contoso sikeresen megérkezett a néhány másodperc alatt, a visszaigazolás.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>(Nem kötelező) 5 lépésben: kötegekben küldése EDIFACT számla 
BizTalk szolgáltatások szerkesztése hidakat is lehetővé teszi a kimenő üzenetek kötegelés. Ez a funkció akkor hasznos, ha a partnereket, hogy egy köteg az üzenetek (a bizonyos feltételeknek megfelelő) egyes üzeneteket helyett célszerű érkeznek.

A legfontosabb méretarány kötegekben való munkavégzés során a tényleges verzióját a köteget, más néven a Megjelenés feltétel. A Megjelenés feltétel alapulhatnak a fogadó partner üzeneteket fogadni szeretné, hogyan. Ha kötegelés engedélyezve van, a szerkesztése híd nem a kimenő üzenet küldése a fogadó partner mindaddig, amíg a Megjelenés feltétel teljesül. Például a eljárásnak feltétel alapján üzenet mérete kiszállításának egy köteg csak ha "n" üzeneteket a rendszer kötegekbe foglalja. A köteg feltételeket is időalapú, úgy, hogy egy köteg érkezik naponta rögzített egyszerre. A megoldás akkor próbáljon meg a üzenetméret alapú feltételt.

1.  Kattintson a BizTalk Services portál a korábban létrehozott találja. Kattintson a Küldés beállítások > kötegelés > hozzáadása a köteget.

2.  A köteg nevét adja meg a **InvoiceBatch**, adjon meg egy leírást, és kattintson a **Tovább gombra**.

3.  Adja meg, hogy egy köteg feltételeket, amely meghatározza, hogy mely üzenetekkel kell lennie kötegelt. Ebben a megoldásban azt a köteg minden üzenet. Igen válassza a speciális definíciók beállítás használata, és írja be az **1 = 1**. Ez egy feltétel, amely a program mindig true, és így összes üzenet kötegekbe foglalja. Kattintson a **Tovább**gombra.

    ![][17]  
4.  Adja meg, hogy egy köteg megjelenés feltételeket. A legördülő listából válassza ki a **MessageCountBased**, és a **darab**, adja meg a **3**. Ez azt jelenti, hogy egy köteg három üzenetek Northwind küld. Kattintson a **Tovább**gombra.

    ![][18]  
5.  Tekintse át az összefoglaló, és kattintson a **Mentés**gombra. Kattintson a **Deploy** újratelepítenie a szerződést.

6.  Térjen vissza az **Oktatóprogram ügyfél** **Belső számla elküldése**kattintson, és kövesse a képernyőn megjelenő utasításokat a számla elküldése. Megfigyelheti, hogy számla nem kapott a Northwind, mert nem teljesül a tétel méretét. Ismételje meg ezt a lépést még kétszer, hogy a Northwind küldött három számla üzenetei. Ez a 3 üzenetek köteg megjelenés feltételeknek eleget, és ekkor a Northwind a számla kell jelennie.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

