<properties
    pageTitle="Azure App szolgáltatásban alkalmazás biztonságos"
    description="Megtudhatja, hogy miként biztonságos egy webalkalmazás, mobilalkalmazás kódmentes vagy API-alkalmazás Azure App szolgáltatásban."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="01/12/2016"
    ms.author="cephalin"/>


#<a name="secure-an-app-in-azure-app-service"></a>Azure App szolgáltatásban alkalmazás biztonságos

Ez a cikk segít az első lépések biztonságossá tétele a web App alkalmazásban, mobilalkalmazás kódmentes vagy API-alkalmazás Azure App szolgáltatásban. 

Azure App szolgáltatásban biztonsági kétszintű foglalja magában: 

- **Infrastruktúra- és a platform biztonsági** - megbízik a következőben Azure, hogy a szolgáltatások dolog, amit a felhőben biztonságosan ténylegesen futtatásához szükséges.
- **Alkalmazás biztonsági** - kell magát az alkalmazás biztonságos megtervezni. Ide tartoznak a integrálása az Azure Active Directory hogyan, hogyan kezelheti a tanúsítványok és hogyan akkor győződjön meg arról, hogy biztonságosan beszélgethet különböző szolgáltatásokat. 

#### <a name="infrastructure-and-platform-security"></a>Infrastruktúra- és a platform biztonsági
Alkalmazás szolgáltatás kezeli az Azure VMs, tárterület, hálózati kapcsolatok, webes keretek, kezelése és integrációs szolgáltatások és még sok mással, mert azt aktívan védett és ellenállóvá és megy erőteljes megfelelőségi keresztül, és győződjön meg róla, hogy folyamatosan ellenőrzi:

- Az alkalmazás szolgáltatás elkülöníteni, mind az internetről, és az Azure erőforrásoktól más felhasználók is.
- A titkos kulcsok (például a csatlakozási_karakterlánc), az alkalmazás szolgáltatás alkalmazás és a más erőforráscsoport Azure erőforrások (például SQL-adatbázis) közötti kommunikáció Azure belül maradjon, és nem cross bármelyik hálózati határai. Titkos kulcsok mindig a titkosított.
- Az alkalmazás szolgáltatási alkalmazás és a külső erőforrások, például a PowerShell-kezelés, parancssori felület, Azure SDK, REST API-k és hibrid kapcsolatok, minden kommunikációját megfelelően titkosított.
- adatkezelési védi az alkalmazás szolgáltatás erőforrások 24 órás veszélyt a rosszindulatú szoftverek ellen, elosztott megtagadását szolgáltatás (DDoS), man-a-a-középső (MITM), és más kockázatokkal szemben. 

További információt a infrastruktúra és a platform biztonsági Azure-ban olvassa el a [Azure az Adatvédelmi központ](/support/trust-center/security/)című témakört.

#### <a name="application-security"></a>Alkalmazás biztonsági

Noha az Azure biztonságossá tétele az infrastruktúra és a platform, amelyet az alkalmazás futtat a felelős, feladata, a biztonságos magát az alkalmazást. Más szóval kell fejlesztése, üzembe helyezéséhez és az alkalmazás kódját, és a tartalom kezelése biztonságos módon. Nélkül, az alkalmazás kódot vagy a tartalom is érinti a veszélyek például:

- SQL-beszúrás
- Munkamenet kihasználása
- Idegen webhely parancsfájlok
- Alkalmazásszintű MITM
- Alkalmazásszintű DDoS

Biztonsági megfontolások webes alkalmazáshoz teljes vitafórum van, a jelen dokumentum túlra. Az alkalmazás biztonságossá tétele további útmutatást a kiindulási pontként lásd: a [Nyitott webes alkalmazás biztonsági Project (OWASP)](https://www.owasp.org/index.php/Main_Page), kifejezetten a [10 legjobb project.](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project), amely megjeleníti az aktuális felső 10 kritikus webes alkalmazás biztonsági hibákat, OWASP tagok által meghatározott.

## <a name="perform-penetration-testing-on-your-app"></a>Végezze el a alkalmazásban tesztelje behatolási

A legegyszerűbb módja még az alkalmazás-szolgáltatási alkalmazás a biztonsági vizsgálata lépések egyik [Tinfoil biztonsági integrációja](/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) használatával végezze el az alkalmazást a beolvasás egyetlen kattintással rés. A vizsgálati eredmények megtekintéséhez egy könnyen érthető jelentést, és megtudhatja, hogy miként javíthatja az egyes biztonsági a lépésenkénti útmutatást.

Ha inkább végezze el a saját behatolási teszteket vagy egy másik képolvasó csomagja vagy szolgáltatót szeretne használni, kövesse az [Azure behatolási tesztelés jóváhagyási folyamat](https://security-forms.azure.com/penetration-testing/terms) , és végezze el a kívánt behatolási teszteket a előzetes jóváhagyás beszerzése.

##<a name="https"></a>Biztonságos vevőkkel folytatott kommunikáció

Ha a ** \*. azurewebsites.net** tartománynevet az alkalmazás szolgáltatás számára létrehozott azonnal használható HTTPS, mint amilyet összes SSL-tanúsítvány ** \*. azurewebsites.net** tartománynevek. Ha a webhely használja az [Egyéni tartomány neve](web-sites-custom-domain-name.md), feltöltheti SSL-tanúsítvány [HTTPS engedélyezése](web-sites-configure-ssl-certificate.md) az egyéni tartomány.

[HTTPS](https://en.wikipedia.org/wiki/HTTPS) engedélyezésével biztosíthatja az között az alkalmazás és a felhasználókat a kommunikációt MITM támadások ellen.

## <a name="secure-data-tier"></a>Réteg adatok védelme

Alkalmazás szolgáltatást, hogy minden kapcsolat-karakterlánc a titkosított végig a falat, és csak visszafejtése történik a virtuális futó az alkalmazást *, és* csak akkor, amikor az alkalmazás elindul erősen integrálódik SQL-adatbázishoz. Ezeken kívül Azure SQL-adatbázis számos biztonsági szolgáltatásai a számítógépes veszélyek, beleértve [a többi titkosítási](https://msdn.microsoft.com/library/dn948096.aspx), [Mindig titkosított](https://msdn.microsoft.com/library/mt163865.aspx), [Dinamikus adatok maszkolás](../sql-database/sql-database-dynamic-data-masking-get-started.md)és [Veszélyt észlelési](../sql-database/sql-database-threat-detection-get-started.md)alkalmazás adatainak védelme érdekében. Ha van bizalmas adatokat vagy a megfelelőségi követelmények betartását, olvassa el a [biztonságossá tétele az SQL-adatbázis](../sql-database/sql-database-security.md) további információt az adatok védelme.

Ha egy külső adatbázis szolgáltatója például ClearDB, nézze át közvetlenül a biztonsággal kapcsolatos gyakorlati tanácsok a szolgáltató dokumentációt.  

##<a name="develop"></a>Biztonságos fejlesztés és telepítéséhez

### <a name="publishing-profiles-and-publish-settings"></a>Közzététel profilok és a Közzététel beállításai

Amikor fejlesztett alkalmazások, a felügyeleti feladatok végrehajtása, illetve például **A Visual Studio**, **Webes mátrix**, **Azure PowerShell** vagy az **Azure parancssori kezelőfelületről Azure**segédprogramok feladatok automatizálása, *közzétételi beállításokat* fájl vagy a *Közzétételi profil*is használhatja. Mindkét fájltípusok hitelesítést végezni, akkor az Azure, és kell-e titkosítani megakadályozva ezzel az illetéktelen hozzáférést.

* A **közzétételi beállításokat** fájl tartalmaz

    * Az Azure előfizetés azonosítója

    * Adatkezelési tanúsítvány, amely lehetővé teszi, hogy az adatkezelési feladatok végrehajtása az előfizetés *anélkül, hogy adja meg a felhasználónév és jelszó*.

* A **Közzétételi profil** fájl tartalmaz

    * Közzététel az alkalmazást az adatait

A Közzététel beállításai vagy közzététel profil fájl használó segédprogram használata esetén profil vagy a közzétételi beállításokat tartalmazó a segédprogramot, majd **törölje** a fájlt a fájl importálása Akkor, ha kell a fájlt, megoszthatja másokkal, például a projekten dolgozó tárolni biztonságos helyen, például egy *titkosított* könyvtár engedélykorlátozásokkal.

Ezenkívül győződjön meg arról, hogy az importált hitelesítő adatok be vannak-e csatlakoztatva. Például **Azure PowerShell** és **Azure parancssori kezelőfelületről Azure** mindkét tárolni az importált adatokat az **otthoni címtár** (*~* Linux vagy OS X rendszer és */users/yourusername* Windows rendszeren.) A felesleges biztonság érdekében érdemes **titkosítása** ezekre a helyekre eszközeivel titkosítási érhető el az operációs rendszerének.

### <a name="configuration-settings-and-connection-strings"></a>Konfigurációs beállítások és a kapcsolati karakterláncot
Általános gyakorlat tárolása a kapcsolati karakterláncot, hitelesítő és más bizalmas információkat a konfigurációs fájl. Sajnos ezeket a fájlokat is elérhetővé tett a webhelyen, vagy történő egy nyilvános tárházba, ez az információ közzéteszi ellenőrizni. Egyszerű keresési a [GitHub](https://github.com), például is Kihúzás számtalan konfigurációs fájlok elérhető titkos kulcsok a nyilvános tárházakban található.

Célszerű, megtartása ezt az információt az alkalmazás konfigurációs fájl ki. Alkalmazás-szolgáltatás tárolását lehetővé konfigurációs adatok a futtatókörnyezet részeként **alkalmazás beállításainak** és a **kapcsolati karakterláncot**. Az értékeket az alkalmazás, a legtöbb programnyelv *környezeti változók* keresztül futásidőben szerepelnek. .NET-alkalmazásokhoz ezeket az értékeket a .NET configuration futásidőben be vannak beékelt. Ezekben az esetekben mellett a konfigurációs beállítások marad titkosított kivéve, ha a megtekintéséhez, vagy állítsa be őket az [Azure-portálon](https://portal.azure.com) vagy például PowerShell vagy az Azure CLI segédprogramok. 

Konfigurációs adatok tárolása alkalmazás szolgáltatás lehetővé teszi az alkalmazás rendszergazdáját, hogy a bizalmas információkat a gyártási alkalmazásai zárolásához. Fejlesztők konfigurációs beállítások külön halmazának használható alkalmazások fejlesztéséhez, és a beállítások automatikus lecserélése által megadott beállítások App szolgáltatásban. A fejlesztők még nem kell, hogy a titkos kulcsok be van állítva a gyártási alkalmazást. Alkalmazás szolgáltatás alkalmazás beállításainak és a kapcsolati karakterláncot beállításáról további tudnivalókért olvassa el a [konfigurálása web Apps alkalmazások](web-sites-configure.md)című témakört.

### <a name="ftps"></a>FTPS

Azure alkalmazás szolgáltatás biztonságos FTP hozzáférést biztosít a fájlrendszerben az alkalmazás **FTPS**keresztül. Ez lehetővé teszi, hogy keresztül biztonságosan hozzáférhet az alkalmazás kódja a web app, valamint a diagnosztikai naplók. Javasoljuk, hogy mindig FTPS helyett használhatja FTP. 

Az alkalmazás a FTPS hivatkozás található hajtsa végre a következő lépéseket:

1. Nyissa meg az [Azure-portálon](https://portal.azure.com).
2. Jelölje ki **az összes böngészése elemre**.
3. Kattintson a **Tallózás** lap **Alkalmazás szolgáltatásokkal**.
4. Az **Alkalmazás szolgáltatások** lap jelölje ki a kívánt alkalmazást.
5. Jelölje ki az alkalmazást a lap **összes beállítást**.
6. A **Beállítások** lap kattintson a **Tulajdonságok parancsot**.
7. Az FTP- és FTPS hivatkozások találhatók, kattintson a **Beállítások** lap. 

FTPS kapcsolatos további tudnivalókért olvassa el a [Fájlt Transfer Protocol](http://en.wikipedia.org/wiki/File_Transfer_Protocol)című témakört.

## <a name="next-steps"></a>Következő lépések

További információt az Azure platform információt a jelentés egy **biztonsági eseményt vagy visszaélés**, vagy értesíti a Microsoft, hogy Ön fog hajt végre **behatolási teszteli** a webhelyen, a biztonsági témakör a [Microsoft Azure az Adatvédelmi központ](https://azure.microsoft.com/support/trust-center/security/)security című.

Az alkalmazás-szolgáltatás alkalmazások **web.config** vagy a **következő** fájlokat olvashat olvassa el a [konfigurációs beállítások zárolását Azure alkalmazás Service web Apps alkalmazások](https://azure.microsoft.com/blog/2014/01/28/more-to-explore-configuration-options-unlocked-in-windows-azure-web-sites/)című témakört.

A naplózási adatainak alkalmazás szolgáltatás alkalmazásokat, amely lehet hasznos támadások, című témakörből [diagnosztikai naplózás engedélyezéséhez](web-sites-enable-diagnostic-log.md).

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter alkalmazásban az alkalmazás szolgáltatás. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="whats-changed"></a>Mi változott

* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)
