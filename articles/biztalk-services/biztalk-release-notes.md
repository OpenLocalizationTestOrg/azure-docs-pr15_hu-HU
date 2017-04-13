<properties
    pageTitle="Kibocsátási megjegyzések az Azure BizTalk szolgáltatások |} Microsoft Azure BizTalk szolgáltatások"
    description="Az ismert problémák az Azure BizTalk szolgáltatások listák" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Kibocsátási megjegyzések az Azure BizTalk szolgáltatások

A Microsoft Azure BizTalk szolgáltatásokra kibocsátási megjegyzések az ismert problémák az ebben a kiadásban tartalmazzák.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>A November frissítés BizTalk szolgáltatások újdonságai
* A többi titkosítási engedélyezhető az BizTalk szolgáltatások portálon. Lásd: [a többi BizTalk Services portál titkosítás engedélyezése](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Frissítési előzmények

### <a name="october-update"></a>Október frissítése

* Szervezeti fiókok támogatottak:  
 * **Forgatókönyv**: regisztrálta a BizTalk szolgáltatás telepítési egy Microsoft-fiókkal (például user@live.com). Ebben az esetben csak a Microsoft-Account felhasználók hogyan kezelhetik az BizTalk Services portál BizTalk szolgáltatás. Szervezeti fiókkal nem használhatók.  
 * **Forgatókönyv**: a szervezeti fiókkal történő az Azure Active Directory BizTalk szolgáltatás telepítési regisztrálta (például user@fabrikam.com vagy user@contoso.com). Ebben az esetben csak a szervezeten belül Azure Active Directory felhasználók kezelhetik a BizTalk Services portál BizTalk szolgáltatás. A Microsoft-fiók nem használhatók.  
* Az Azure klasszikus portálon BizTalk szolgáltatás létrehozásakor automatikusan bejegyzett a BizTalk szolgáltatások portálon.
 * **Forgatókönyv**: Jelentkezzen be az Azure klasszikus portal, akkor a BizTalk szolgáltatás hozzon létre, és válassza a **Manage** az első alkalommal. Amikor megnyílik a BizTalk Services portál, a BizTalk szolgáltatás automatikusan regisztrálja, és készen áll a központi telepítés a.  
 Lásd: [regisztrálása és frissítése a BizTalk szolgáltatás telepítése a BizTalk szolgáltatások portálon](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>Augusztus 14 frissítése
* A BizTalk Services portál szerződés, valamint a szétválasztás – kereskedelmi partner rendelkezést és hidakat híd most leválasztott. Most már létrehozhat rendelkezést és hidakat külön-külön, és futásidőben hidakat úgy, hogy létrejött a szerkesztése üzenetben szereplő értékek alapján. Lásd: [Rendelkezést létrehozása az Azure BizTalk szolgáltatásokban](https://msdn.microsoft.com/library/azure/hh689908.aspx), [Hozzon létre egy szerkesztése híd BizTalk Services portál használatával](https://msdn.microsoft.com/library/azure/dn793986.aspx), [egy BizTalk szolgáltatások portálon AS2 híd létrehozása](https://msdn.microsoft.com/library/azure/dn793993.aspx), és [hogyan hidakat megoldani a futásidőben rendelkezést?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* A sablonok rendelkezést a létrehozásra szolgáló parancs megszakad.  
* A Küldés melletti szerződés most megadhatja az egyes séma különböző elválasztó beállítása. Ez a beállítás van megadva, a Küldés egymás megállapodás protocol beállításai. További tudnivalókért lásd: [egy X12 létrehozása az Azure BizTalk Services szerződés](https://msdn.microsoft.com/library/azure/hh689847.aspx) és [létrehozása az Azure BizTalk Services EDIFACT megállapodást](https://msdn.microsoft.com/library/azure/dn606267.aspx). Két új entitás az TPM OM API-nak is megjelenik a ugyanerre a célra. Lásd: [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) és [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Szabványos XSD szerkezeteket, származtatott típusú, például most támogatottak. Lásd: [használata szabványos XSD szerkezetek a a térképek](https://msdn.microsoft.com/library/azure/dn793987.aspx) és a [Használati származtatott típusok az esetek megfeleltetése és példát tartalmaz](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 új Mikrofon az üzenet aláírása és új titkosítási algoritmusok támogatja. Lásd: [az Azure BizTalk Services AS2 megállapodást létrehozása](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Ismert problémák

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Csatlakozási problémákat tapasztal a BizTalk Services portál frissítése után

  Ha a BizTalk Services portál megnyitni, miközben a szolgáltatás a módosítások görgetheti BizTalk szolgáltatások frissítését, akkor előfordulhat, hogy arcra csatlakozási problémákat tapasztal a BizTalk szolgáltatások Portal segítségével.  
  Kerülő indítsa újra a böngészőt, törölheti a böngésző gyorsítótárának kiürítése, vagy a portálon nyissa meg a személyes módban.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE nem találja a eltérés, ha a hiba vagy figyelmeztetés BizTalk szolgáltatások projekt kattint
A Visual Studio 2012 frissítés 3 RC 1 a probléma megoldásához telepítse.  

### <a name="custom-binding-project-reference"></a>Egyéni kötés projekt hivatkozás
Fontolja meg a Visual Studio megoldás BizTalk szolgáltatások projekttel az alábbi esetekben:  
* Ugyanabban a Visual Studio megoldásban van BizTalk szolgáltatások projekt és egyéni kötés projekt. A BizTalk szolgáltatás project a egyéni kötési projektfájlt hivatkozást tartalmaz.
* A BizTalk szolgáltatási projekt egyéni kötési/viselkedés DLL hivatkozást tartalmaz.

"Generál" a Visual Studióban a megoldás sikeres. Ezután újraépítéséhez"vagy"Tiszta"a megoldást. Ezt követően újraépítéséhez vagy tiszta újra, az alábbihoz hasonló hibaüzenet:  
  Nem lehet fájl másolása <Path to DLL> való "bin\Debug\FileName.dll". A folyamat nem tudja elérni a fájlt "bin\Debug\FileName.dll", mert azt egy másik folyamat használja.  

#### <a name="workaround"></a>Megoldás:
* Ha [a Visual Studio 2012 frissítés 3](https://www.microsoft.com/download/details.aspx?id=39305) telepítve van, az alábbi két lehetőség van:

  * Indítsa újra a Visual Studióban, vagy

  * Indítsa újra a megoldást. Építés csak ezt követően végezze el a megoldást:  

* Ha nincs telepítve a [Visual Studio 2012 frissítés 3](https://www.microsoft.com/download/details.aspx?id=39305) , nyissa meg a Feladatkezelőt, a folyamatok lapon kattintson, és kattintson a MSBuild.exe folyamat, majd kattintson a folyamat leállítása gombra.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Továbbítás BasicHttpRelay végpontok hidakat és BizTalk Services portál használata esetén nem támogatott nem nyomtatható karaktereket belekerülnek HTTP fejlécként

Használatakor nem nyomtatható karaktereket Előléptetett tulajdonságok részeként üzenetek azokat az üzeneteket nem kell továbbítási helyre irányítva a BasicHttpRelay kötés használó. Az előléptetett tulajdonságai, amely is érhető el, a nyomon követés részét URL-címként kódolt BLOB- és a célok nem kódolt.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN küldi aszinkron még akkor is, ha a Küldés aszinkron MDN beállítás bejelölve  
Képzelje el – Ha, jelölje be a **aszinkron MDN küldése** jelölőnégyzetet, és adjon meg egy URL-CÍMÉT az aszinkron MDN küldés, majd törölje a jelet a **aszinkron MDN küldése** jelölőnégyzetet, és ismét a MDN továbbra is küld a megadott URL-cím annak ellenére, hogy nincs bejelölve a aszinkron MDNs küldése lehetőséget.  
Kerülő törölje a jelet a megadott URL-cím, mielőtt a **Küldés aszinkron MDN** jelölőnégyzet jelölésének törlése és telepítheti a AS2 találja.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Az elválasztó karakterek túl egy érvényes interchange okozó egy üres üzenetet küldeni szeretné a felfüggesztett végpont  
Ha túl-IEA szakasz szóközök, a kicsomagoló tekinti meg interchange jelenlegi végét, és vizsgálja meg a következő üzenetként szóközök következő készlete. Mivel ez nem érvényes interchange, erőforrásigények előfordulhat, hogy az útvonal cél egy sikeres üzenetet küld, és egy üres üzenetet küld az felfüggesztett végpontot.  
### <a name="tracking-in-biztalk-services-portal"></a>A BizTalk Services portál nyomon követése  
A nyomon követés események szerkesztése üzenet feldolgozása és bármely korrelációs rögzítésének. Ha egy üzenetet nem sikerül kívül az Protocol (protokoll) szakaszban, a nyomon követés sikeres jeleníti meg. Ebben az esetben a olvassa el a NAPLÓT részt a **Részletek** oszlop alatti **követés** hibaadatok.
A X12 beállítások küldhet és fogadhat ([egy X12 létrehozása az Azure BizTalk Services szerződés](https://msdn.microsoft.com/library/azure/hh689847.aspx)) adja meg a protokoll szakasz adatokat.  

### <a name="update-agreement"></a>Szerződés frissítése  
A BizTalk Services portál lehetővé teszi a minősítő identitás módosítása az, ha olyan megállapodást van beállítva. Ez a eredményezhet inconsistence tulajdonságait. Például a ZZ:1234567 és a minősítő ZZ:7654321 megállapodás van. A BizTalk Services portál profilbeállítások módosítsa ZZ:1234567 01:ChangedValue lesz. A szerződés nyitja meg, és 01:ChangedValue ZZ:1234567 helyett jelenik meg.
Az identitás minősítő módosítása, törlése a szerződés, **identitások** a partner profil frissítése, és hozza létre a szerződést.  
> AZURE. Ez a jelenség figyelmeztetés X12 és AS2 hatással van.  

### <a name="as2-attachments"></a>Mellékletek AS2  
Az üzenetek nem támogatottak a AS2 mellékletek küldéséhez és fogadásához. Pontosabban mellékletek meg automatikusan figyelmen kívül hagyja, és az üzenet törzsébe normál AS2 üzenetként feldolgozása.  
### <a name="resources-remembering-path"></a>Források: Az elérési út megjegyzése  
**Erőforrások**hozzáadása, a párbeszédpanelt nem emlékszik előfordulhat, hogy az elérési út hozzáadása egy erőforrás korábban használt. Ne feledje, hogy a korábban használt elérési útját, próbálja meg a BizTalk Services portál webhely felvétele a **Megbízható helyek** az Internet Explorerben.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Nevezze át az szervezet hidat, és zárja be a projekt a módosítások mentése nélkül, ha a szervezet újra megnyitja a hibát eredményez
Fontolja meg egy forgatókönyv a következő sorrendben:  
* BizTalk szolgáltatási projekt hidat (például egy XML-One-Way-híd) hozzáadása  

* Nevezze át a híd megadásával a szervezet neve tulajdonság értékét. Ez a parancs átnevezi a társított .bridgeconfig fájlt a megadott név.  

* A módosítások mentése nélkül zárja be a .bcs fájl (a Visual Studio lap bezárásával).  

* Nyissa meg a .bcs fájlt ismét a megoldást Intézőből.  
Megfigyelheti, hogy a fájlhoz társított .bridgeconfig megadott új nevét, miközben a személy nevét a tervezési területen még mindig a régi nevet. Ha megpróbálja megnyitni a híd konfiguráció duplán kattintva a híd összetevő, akkor a következő hibaüzenet:  
  "<old name>"Személy adatait a kapcsolódó fájl"<old name>.bridgeconfig" nem létezik  
Elkerülje ebben az esetben, győződjön meg arról, hogy a módosítások mentéséhez, a személyek BizTalk szolgáltatási projekt átnevezése után.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>BizTalk szolgáltatás project sikeresen hozza létre, akkor is, ha egy eltérés zárni Visual Studio projekt
Fontolja meg az ahová felvette-eltérés (például XSD-fájl) Példa BizTalk szolgáltatás projektté a híd konfiguráció (például megadásával azt egy kérés üzenet típusa), hogy eltérés szerepeltetni és zárni a Visual Studio projekt. Ebben az esetben a projekt létrehozása nem ad bármilyen hiba mindaddig, amíg a törölt eltérés áll rendelkezésre, ugyanazon a helyen, ahol tartalmazta a Visual Studio projektből származó lemezen.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>A BizTalk szolgáltatás project nem ellenőrzi a séma elérhetőségének beállítása a hidakat közben
BizTalk szolgáltatási projekt a projekt hozzáadott séma importálása másik séma, ha a BizTalk szolgáltatás project nem ellenőrzi, hogy az importált séma bekerül a projekt. Próbál meg, például a projekt létrehozása, ha nem kap a build hibák.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>A válaszüzenetet egy XML-kérés / válasz híd értéke mindig a UTF-8 karakterkészlet
Ebben a kiadásban a válaszüzenetet egy XML-kérés / válasz híd karakterkészletet értéke mindig UTF-8.
### <a name="user-defined-datatypes"></a>Felhasználó által definiált adattípusok
A BizTalk kártya csomag kártyák belül az BizTalk kártya szolgáltatás használhat a felhasználó által definiált adattípusok kártya műveletekhez.
Felhasználó által definiált adattípusok használatakor, másolja a fájlokat (dll) meghajtó: \Program Files\Microsoft BizTalk kártya Service\BAServiceRuntime\bin\ vagy való a globális összeállítási gyorsítótár (GAC) a BizTalk kártya szolgáltatás üzemeltető kiszolgálón. Egyéb esetben a következő hiba akkor fordulhat elő az ügyfélgépen:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] A GACUtil.exe telepítheti egy fájlt a globális összeállítási gyorsítótárba ajánlott. A GACUtil.exe dokumentumok ezzel az eszközzel, és a Visual Studio parancssori kapcsolók használatával.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>A webhely szolgáltatás BizTalk kártya újraindítása
A **BizTalk kártya szolgáltatás futtatókörnyezet** telepítése* hoz létre a * *BizTalk kártya szolgáltatás* * webhely, amely tartalmazza az IIS a * *BAService* * alkalmazás.* *BAService** alkalmazás belső továbbító kötés használja kiterjesztése a helyszíni szolgáltatás végpontjának a felhőben vannak. A tárolt szolgáltatás helyszíni telepítésű a megfelelő továbbítási végpont fog lehet regisztrálni a szolgáltatás Bus csak akkor, amikor a helyszíni szolgáltatás elindul.  

Ha leállítása és indítson el egy alkalmazást, az alkalmazás automatikus indítása a beállításokat nem fogadja van. Így **BAService** leállítása, meg kell mindig indítsa újra a **BizTalk kártya szolgáltatás** webhelyet helyette. Ne indítása és leállítása a **BAService** alkalmazás.
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Speciális karakterek nem használható az üzleti összetevők címét és a szervezet neve
Speciális karakterek nem üzleti összetevők címét és a személy nevét kell használni. Ha így tesz, az BizTalk szolgáltatási projekt telepítése során hiba lép fel. Az egyes karakterek, például "%", a BizTalk kártya szolgáltatás webhely előfordulhat, hogy lépjen leállítva állapotba és be kell kézi indítása.
### <a name="test-map-with-get-context-property"></a>Tesztelje a térkép Get-környezet tulajdonság
Ha átalakító tartalmaz egy **Helyi tulajdonság beolvasása** térkép műveletet, a **Próba-leképezés** sikertelen lesz. Ideiglenes megoldásként cserélje le a **Helyi tulajdonság beolvasása** térkép művelet egy karakterlánc ÖSSZEFŰZ térkép művelet üres adatokat tartalmazó. Ezzel a cél séma feltöltése és tesztelni egyéb átalakítás funkciók engedélyezése.
### <a name="test-map-property-does-not-display"></a>Teszt térkép tulajdonság nem jelenít meg
A **Próba-megfeleltetés** tulajdonságai ne jelenjen meg a Visual Studióban. Ez akkor fordulhat elő, ha a **Tulajdonságok** és a **Megoldás Explorer** ablakban vannak nem egyidejű rögzítve. Megoldásához rögzítése az a **tulajdonságokat** és a **Megoldás Explorer** -ablakot.  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Dátum és idő újraformázni legördülő szürkén jelenik meg
Ha egy DateTime újraformázni térkép művelet a tervezési felületre hozzáadott és konfigurálva, a Formátum legördülő listában szereplő szürkítve jelennek meg. Ez akkor fordulhat elő, ha a számítógép megjelenítési van beállítva, **Közepes – 125 %** vagy a **nagyobbak – 150 % -os**. Úgy tudja megoldani, állítsa a Megjelenítés **kisebb – 100 % -os (alapértelmezett)** , használja az alábbi lépéseket:  
1. Nyissa meg a **Vezérlőpultot** , és kattintson a **Megjelenés és személyes beállítások elemre**.
2. A **Megjelenítés**gombra.
3. Kattintson a **kisebb – 100 % -os (alapértelmezett)** , és kattintson az **Alkalmaz**gombra.

A **Formátum** legördülő lista célszerű most vártnak megfelelően.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Ismétlődő rendelkezést a BizTalk Services portál
Vegye figyelembe az alábbiakat:
1. Hozzon létre egy olyan megállapodást, a kereskedési Partner Management OM API.
2. Nyissa meg a szerződés a BizTalk Services portál két különböző lapokon.
3. Telepítse a szerződés mindkét lapját.
4. Következtében mind a rendelkezést üzemelnek eredményezi az ismétlődő elemeket a BizTalk Services portál

**Megoldás:**. Nyissa meg az ismétlődő rendelkezést közül bármelyik a BizTalk Services portál, és eltávolítása.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Hidakat tanúsítvány nem használható, frissített után egy tanúsítványt az eltérés áruházban változásáról.
Vegye figyelembe az alábbi esetekben:  

**Alkalmazási helyzetek 1: Tanúsítványokkal ujjlenyomat-alapú biztonságossá tehető az üzenet áthelyezése egy azt az egy szolgáltatás végpontjának**  
Fontolja meg, ahol ujjlenyomat-alapú tanúsítványok használja a BizTalk szolgáltatási projekt példa. Frissítse a tanúsítvány a BizTalk Services portál ugyanazt a nevet, de a másik ujjlenyomat, de nem a BizTalk szolgáltatási projekt frissítése lehetőséget. Az ilyen esetben a híd feldolgozása az üzeneteket, mivel a régebbi tanúsítvány adatainak előfordulhat, hogy továbbra is a csatorna gyorsítótárban továbbra is. Ezt követően a következő üzenet feldolgozási sikertelen lesz.  

**Megoldás**: a tanúsítványt a BizTalk szolgáltatási projekt frissítsen és telepítsen újra a a project.  

**2 alkalmazási helyzetek: Segítségével név sokaság viselkedése biztonságossá tehető az üzenet áthelyezése egy azt az egy szolgáltatás végpontjának tanúsítványok azonosítása**

Fontolja meg, ahol név sokaság viselkedése azonosítására használt tanúsítványokat a BizTalk szolgáltatási projekt példa. A tanúsítvány a BizTalk Services portál frissítése, de nem frissíti a BizTalk szolgáltatás project ennek megfelelően. Az ilyen esetben a híd feldolgozása az üzeneteket, mivel a régebbi tanúsítvány adatainak előfordulhat, hogy továbbra is a csatorna gyorsítótárban továbbra is. Ezt követően a következő üzenet feldolgozási sikertelen lesz.  

**Megoldás**: a tanúsítványt a BizTalk szolgáltatási projekt frissítsen és telepítsen újra a a project.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Hidakat folytatja a üzeneteket akkor is, ha az SQL-adatbázis offline állapotban.
A BizTalk szolgáltatások hidakat továbbra is üzenetek folyamata egy ideig, még akkor is, ha a Microsoft Azure SQL-adatbázis (ami például telepített eltérések és folyamatok futó adatokat tárol), offline állapotban. Ennek az oka BizTalk szolgáltatás használja, az eltérések a gyorsítótáras és híd konfigurációs.
Ha nem szeretné, hogy a hidakat feldolgozása esetleges üzeneteket, amikor az SQL-adatbázis offline állapotban, a BizTalk szolgáltatások PowerShell-parancsmagok leállítása vagy felfüggesztése a BizTalk szolgáltatás is használhatja. Lásd: [Azure BizTalk szolgáltatás felügyeleti minta](http://go.microsoft.com/fwlink/p/?LinkID=329019) a Windows PowerShell-parancsmagok műveletek kezelésére.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Egy további AJ-karaktert tartalmaz egy azt egyéni kódot összetevő belül az XML-üzenet olvasása
Fontolja meg az esetet, amelyhez el egy XML-üzenetet egy azt az egyéni kód belül szeretne. Ha a .NET API System.Text.Encoding.UTF8.GetString(bytes) egy további AJ karakter szerepel az eredményben a az üzenet elejére. Igen, ha nem szeretné, hogy a kimenet, ha meg szeretné jeleníteni a felesleges AJ karakter, kell használnia ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Üzeneteket küld egy WCF használja azt nem méretezni.
WCF használatával hidat küldött üzenetek nem méretezni. Ha azt szeretné, hogy egy méretezhető ügyfél HttpWebRequest inkább kell használni.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>FRISSÍTÉS: Jogkivonat szolgáltató hiba az általános elérhetőségű kiadás BizTalk szolgáltatások előzetes verzióról
Nem egy szerkesztése vagy AS2 megállapodás az aktív kötegekben történik. Amikor a BizTalk szolgáltatás frissítését a Előnézettől kezdve, ám a fordulhat elő, az alábbi:
* Hiba: A jogkivonat-szolgáltató nem tudott adja meg a biztonsági jogkivonat. Jogkivonat-szolgáltató üzenet vissza: A távoli neve nem oldható.

* Megszakítja a köteg feladatokat.

**Megoldás**: az BizTalk szolgáltatás után az általános elérhetőségű kiadás frissül, telepítsen újra a szerződést.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>FRISSÍTÉS: Eszközkészlet jeleníti meg a régi híd ikonok a BizTalk szolgáltatások SDK történő frissítés után
A szolgáltatások egy régi ikonjai a hidakat, BizTalk SDK korábbi verzióiban történő frissítése után az eszközkészlet továbbra is a hidakat a régi ikonjának megjelenítése. Azonban BizTalk szolgáltatás project tervezési felületre hidat vesz fel, ha a felület látható az új ikonra.  

**Megoldás:**. A probléma megoldásához dolgozhat a .tbd fájlok törlésével <system drive>: \Users\<felhasználói > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>FRISSÍTÉS: BizTalk portál frissítése az előnézeti Georgia előfordulhat, hogy látható egy hiba, jelezve, hogy az szerkesztése lehetősége nem érhető el
Miközben a BizTalk szolgáltatásokat az előnézeti Georgia frissített be van jelentkezve a BizTalk Services portál be, ha az alábbi hibaüzenet jelenhet meg a portálon:  

Ez a lehetőség nem érhető el a Microsoft Azure BizTalk Services edition részeként. Használandó e ezekre a lehetőségekre kiadásának megfelelő váltani.  

**Megoldás**: napló meg az a portálon, bezárása és megnyitása a böngészőben, majd jelentkezzen be a portálon.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>FRISSÍTÉS: Új követési adatok nem jelennek meg a Georgia BizTalk szolgáltatások frissítése után
Tegyük fel, ahol egy XML-híd BizTalk szolgáltatások előzetes előfizetés telepítve van példa. Üzeneteket küldeni a hidat, és a megfelelő adatok nyomon követése a BizTalk Services portál elérhető-e. Most Ha frissíti a BizTalk Services portál és -szolgáltatások BizTalk futtatókörnyezet bittel Georgia, és üzenetet küldeni az azonos híd végpontot korábban telepített, a nyomon követési adatok nem jelennek meg a frissítés után elküldött üzenetek.  

### <a name="pipelines-vs-bridges"></a>Folyamatok v/s hidakat
Ebben a dokumentumban a kifejezés "folyamatok" és "hidakat" azonos értelemben használhatók. Mindkét lényegében jelent, a célt szolgálja, amely, egy üzenet egység BizTalk szolgáltatások rendszerre telepíthető.  

### <a name="concepts"></a>Fogalmak  

[BizTalk szolgáltatások](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
