#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Egy Azure webhelyet egyéni tartománynév beállítása

A webhely létrehozásakor Azure Ez a témakör rövid altartományt azurewebsites.net tartomány, a felhasználói hozzáférhetnek a webhely egy URL-CÍMÉT, például a http://&lt;saját webhelyek >. azurewebsites.net. Jó helyen jár Ha úgy állítja be a webhely megosztott vagy szabványos üzemmódban, képezhető a webhely át tartománya nevére.

Tetszés szerint kezelővel Azure forgalom egyenleg bejövő forgalom webhelyére való betöltéséhez. [Azure webhelyek forgalom szabályozása Azure forgalom Manager]témakörben található további tájékoztatást a forgalom Manager működése-webhely[trafficmanager].

> [AZURE.NOTE] Ebben a feladatban eljárásokat alkalmazni az Azure webhelyek; Felhőszolgáltatások olvassa el az <a href="/develop/net/common-tasks/custom-dns/">Azure az egyéni tartománynév beállítása</a>című témakört.

> [AZURE.NOTE] A lépéseket, ebben a feladatban csak-webhelyek konfigurálása megosztott vagy szabványos üzemmódban, ami előfordulhat, hogy mennyi számlázható, az előfizetéséhez. <a href="/pricing/details/web-sites/">Webhelyek árak részletek</a> talál további információt.

Ebben a cikkben:

-   [CNAME és A rekordok funkcióinak bemutatása](#understanding-records)
-   [A webhelyek megosztott vagy szabványos üzemmódban konfigurálása](#bkmk_configsharedmode)
-   [A webhelyek forgalom kezelő hozzáadása](#trafficmanager)
-   [A CNAME a saját tartomány hozzáadása](#bkmk_configurecname)
-   [Az egyéni tartományát A rekord létrehozása](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Dátumtáblázatok ismertetése és az A CNAME rekordok</h2>

CNAME (vagy a rekordok alias) és a rekordok is lehetővé teszi, hogy a tartománynév hozzárendelése a webhelyhez, azonban mindegyik másképp működnek.

###<a name="cname-or-alias-record"></a>Alias vagy a CNAME rekord

Egy CNAME rekordot a *adott* tartományt, például: **contoso.com** vagy **www.contoso.com**, megfelelteti a kanonikus tartománynév. Ebben az esetben a kanonikus tartománynév, a vagy a ** &lt;SajátPr >. azurewebsites.net** tartománynevemet az Azure webhelyén vagy a ** &lt;SajátPr >. trafficmgr.com** a forgalom Manager profil tartománynevet. Amikor létrejött, a CNAME alias hoz létre a ** &lt;SajátPr >. azurewebsites.net** vagy ** &lt;SajátPr >. trafficmgr.com** tartomány nevét. A CNAME-bejegyzés fog úgy, hogy az IP-címét a ** &lt;SajátPr >. azurewebsites.net** vagy ** &lt;SajátPr >. trafficmgr.com** tartománynév automatikusan, így ha megváltoztatja a webhely IP-címét, nem kell tennie semmit.

> [AZURE.NOTE] Néhány tartományregisztrálói csak teszi altartományokat feleltesse meg egy CNAME rekordot, például www.contoso.com, és nem legfelső szintű nevek (például contoso.com) használata esetén. További információt a CNAME rekordot a tartományregisztráló, <a href="http://en.wikipedia.org/wiki/CNAME_record">a CNAME rekord Wikipedia-bejegyzés</a>vagy a <a href="http://tools.ietf.org/html/rfc1035">IETF tartománynevek - végrehajtása és a specifikációja</a> dokumentum által biztosított dokumentációjában.

###<a name="a-record"></a>Rekord létrehozása

Az A rekord rendeli hozzá a tartományt, például: **contoso.com** vagy **www.contoso.com**, *vagy egy helyettesítő tartomány* például ** \*. contoso.com**, IP-cím. Az Azure webhely esetében, vagy a szolgáltatás a virtuális IP-címének vagy egy adott IP cím, amikor a webhelyhez vásárolt. A fő az A rekord fölé egy CNAME rekordot előnye, hogy egy bejegyzést, például egy helyettesítő karaktert használó lehet úgy * **. contoso.com**, több alárendelt tartomány kérelem amely szeretné kezelni * *mail.contoso.com**, * *login.contoso.com**, vagy * *www.contso.com**.

> [AZURE.NOTE] Az A rekord van rendelve egy statikus IP-cím, mivel azt nem lehet automatikusan változtatásainak feloldása a IP-címére a webhely. Egyéni tartománynév-beállítások a webhelyére; konfigurálásakor megadva IP-címet A rekordok való használatra Ezt az értéket változhat jó helyen jár, ha törli, és hozza létre a webhely vagy webhely felszabadításához biztonsági módjának megváltoztatása.

> [AZURE.NOTE] A rekordok terheléselosztás forgalom kezelője nem használható. További tudnivalókért lásd: [Azure webhelyeket forgalom szabályozása Azure forgalom Manager][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>A megosztott vagy szabványos üzemmódban-webhely konfigurálása</h2>

A saját tartománynév beállítása egy webhelyen lehetőség csak a megosztott és a Azure webhelyeket szabványos módban érhetők el. Előtte a megosztva mód ingyenes webhely vagy a webhely szabványos üzemmódban webhelye, el kell távolítani kiadások CAPS LOCK helyen webhely-előfizetéséhez. További információt a megosztott és a szokásos mód árak, részletek [Árak][PricingDetails].

1. A böngészőben nyissa meg az [Adatkezelési portál][portal].
2. A **webhelyek** lapon kattintson a webhely nevét.

    ![][standardmode1]

3. Kattintson a **méret** fülre.

    ![][standardmode2]


4. **Általános** szakaszában állítsa be a webhely mód **MEGOSZTVA**gombra kattintva.

    ![][standardmode3]

    > [AZURE.NOTE] Ha fogja használni, akkor forgalom Manager a webhelyhez, jelölje be a szabványos üzemmódban helyett a megosztott kell használnia.

5. Kattintson a **Mentés**gombra.
6. Amikor a rendszer kéri, a megosztott üzemmód (vagy ha úgy dönt, hogy Standard szabványos üzemmódban) növekedése kapcsolatban, kattintson az **Igen** gombra, ha elfogadja.

    <!--![][standardmode4]-->

    **Megjegyzés:**<br />
   A "Konfigurálása skála webhely"webhely neve"nem sikerült" hibaüzenet jelenik meg, ha további információk a Részletek gomb segítségével.

<a name="trafficmanager"></a><h2>(Nem kötelező) A webhely forgalom kezelő hozzáadása</h2>

Ha a webhely forgalom Manager használni kívánt, hajtsa végre az alábbi lépéseket.

1. Ha Ön nem rendelkezik a forgalom Manager profilok, használja az információkat [használatával gyors létrehozása forgalom Manager profil] létrehozása[ createprofile] hozhat létre egyet. Megjegyzés: a **. trafficmgr.com** társított forgalom Manager profilját. Ez a leendő lesz.

2. Használja az információkat [hozzáadása] vagy törlése a végpontok[ addendpoint] a webhely, amelyet kíván hozzáadni a forgalom Manager profilhoz zárólap.

    > [AZURE.NOTE] Ha a webhely nem szerepel a végpont hozzáadása, ellenőrizze, hogy van-e állítva az szabványos üzemmódban. A webhely szabványos üzemmódban annak érdekében, hogy a forgalmat Manager munka kell használnia.

3. Jelentkezzen be a DNS tartományregisztráló webhelyén, és nyissa meg a DNS kezelése lapot. Keresse meg a hivatkozásokat, vagy a **Tartománynevet**, a **DNS-**vagy a **Név kiszolgáló kezelése**felirata webhely részeinek.

4. Ha válassza ki vagy adja meg a CNAME rekordok most megkeresése Előfordulhat, hogy ki kell válassza a rekord típusát a legördülő listában, vagy nyissa meg a Speciális beállítások lapot. **Alias**vagy **altartományokat**a szavak **CNAME**, meg kell kinéznie.

5. A tartomány és altartomány alias CNAME is meg kell adnia. Ha például **www** Ha létre szeretne hozni egy aliast **www.customdomain.com**.

5. Meg kell adnia a kanonikus tartománynév a CNAME alias állomásnév is. Ez a **. trafficmgr.com** a webhely neve.

Ha például a következő CNAME rekord továbbítja minden forgalom **www.contoso.com** **contoso.trafficmgr.com**, a tartomány nevét a webhelyre:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias vagy állomásnév oszlopban név/altartomány</strong></td>
<td><strong>Kanonikus tartomány</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.trafficmgr.com</td>
</tr>
</table>

A látogatók a **www.contoso.com** soha nem jelenik meg igaz a host (contoso.azurewebsite.net), ezért a továbbítás folyamat nem látható a végfelhasználó.

> [AZURE.NOTE] A webhelye forgalom Manager használja, ha nem szükséges hajtsa végre az alábbi szakaszok, "**a CNAME a saját tartomány hozzáadása**" és "**egy A-rekord az egyéni tartomány felvétele**" című témakör lépéseit. A CNAME rekord az előző lépésekben létrehozott bejövő adatforgalmat irányítja a forgalmat Manager, majd a webhely endpoint(s) irányítja a forgalmat, amely.

<a name="bkmk_configurecname"></a><h2>A CNAME a saját tartomány hozzáadása</h2>

A CNAME rekord létrehozására, hozzá kell adnia egy új bejegyzés a DNS-táblázat az egyéni tartományához tartozó a szolgáltató által biztosított eszközök segítségével. Minden tartományregisztráló még egy CNAME rekordot tartalmazó hasonló, de némileg eltérő módszer, de a fogalmak megegyeznek.

1. Az alábbi módszerek egyikét segítségével keresse meg a **. azurewebsite.net** a webhely rendelt tartománynevet.

    * Jelentkezzen be az [Azure Kezelőportálja][portal], jelölje be a webhely, jelölje be az **Irányítópult**és a **fontos** szakaszban keresse meg a **Webhely URL-címe** tételt.

    * Telepítse és állítsa be a [Azure Powershell](/manage/install-and-configure-windows-powershell/), és használja a következő parancsot:

            get-azurewebsite yoursitename | select hostnames

    * Telepítse és állítsa be az [Azure parancssor](/manage/install-and-configure-cli/), és használja a következő parancsot:

            azure site domain list yoursitename

    A mentése **. azurewebsite.net** nevét, ahogy az fog szerepelni az alábbi lépéseket.

3. Jelentkezzen be a DNS tartományregisztráló webhelyén, és nyissa meg a DNS kezelése lapot. Keresse meg a hivatkozásokat, vagy a **Tartománynevet**, a **DNS-**vagy a **Név kiszolgáló kezelése**felirata webhely részeinek.

4. Ha jelölje ki vagy adja meg a CNAME rekordok most megkeresése Előfordulhat, hogy ki kell válassza a rekord típusát a legördülő listában, vagy nyissa meg a Speciális beállítások lapot. **Alias**vagy **altartományokat**a szavak **CNAME**, meg kell kinéznie.

5. A tartomány és altartomány alias CNAME is meg kell adnia. Ha például **www** Ha létre szeretne hozni egy aliast **www.customdomain.com**. Ha szeretne a gyökértartomány alias létrehozása, akkor előfordulhat, hogy fog szerepelni a "**@**' a tartományregisztráló DNS-eszközök a szimbólumot.

5. Meg kell adnia a kanonikus tartománynév a CNAME alias állomásnév is. Ez a **. azurewebsite.net** a webhely neve.

Ha például a következő CNAME rekord továbbítja minden forgalom **www.contoso.com** **contoso.azurewebsite.net**, a tartomány nevét a webhelyre:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias vagy állomásnév oszlopban név/altartomány</strong></td>
<td><strong>Kanonikus tartomány</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.azurewebsite.NET</td>
</tr>
</table>

A látogatók a **www.contoso.com** soha nem jelenik meg igaz a host (contoso.azurewebsite.net), ezért a továbbítás folyamat nem látható a végfelhasználó.

> [AZURE.NOTE] A fenti példában csak a __www__ altartomány forgalom vonatkozik. CNAME rekordok nem használhat helyettesítő karaktereket, mivel létre kell hoznia egy CNAME minden tartomány és altartomány. Ha azt szeretné, irányítsa át a forgalmat a altartományokat, mint például *., akkor a contoso.com azurewebsite.net címére, állítsa be a DNS-beállítások egy __URL-átirányítási__ vagy a __Továbbítás URL__ -bejegyzést, vagy hozzon létre egy A rekordot.

> [AZURE.NOTE] Eltarthat egy kis időt, a szülőtől megtörtént a DNS-rendszerben CNAME. A webhely CNAME nem lehet beállítani, amíg a CNAME propagálása. Ellenőrizze, hogy a CNAME elérhető egy szolgáltatásba, például <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> is használhatja.

###<a name="add-the-domain-name-to-your-website"></a>A tartománynév elhelyezése a webhelyen

Miután a CNAME rekordot a tartománynévhez propagálása, a webhely össze kell kapcsolnia. Az egyéni tartománynevet határozza meg a CNAME rekordot a webhelyre vagy az Azure parancssori kezelőfelületről Azure használatával vagy az Azure adatkezelési portál használatával is hozzáadhat.

**Parancssori eszközökkel tartománynév hozzáadása**

Telepítse és állítsa be az [Azure parancssori felületet](/manage/install-and-configure-cli/), és használja a következő parancsot:

    azure site domain add customdomain yoursitename

Például a következőket fogja hozzáadása egyéni tartománynevet a **www.contoso.com** **contoso.azurewebsite.net** webhelyhez:

    azure site domain add www.contoso.com contoso

Győződhet meg, hogy az egyéni tartománynevet hozzá lett adva a webhelyet a következő parancs használatával:

    azure site domain list yoursitename

A listában, ezzel a paranccsal vissza kell tartalmaznia az egyéni tartomány neve, valamint az alapértelmezett **. azurewebsite.net** bejegyzést.

**Az Azure adatkezelési portál használatával tartománynév hozzáadása**

1. A böngészőben nyissa meg az [Azure Kezelőportálja][portal].

2. A **webhelyek** lapon kattintással jelölje ki azt a webhelyet, válassza az **Irányítópult**, és válassza **A tartományok kezelése** a lap alján.

    ![][setcname2]

6. A **DOMAIN NAMES** szöveg mezőbe írja be a beállította a tartományt.

    ![][setcname3]

6. Kattintson a pipa ikonra koppintva fogadja el a tartomány nevét.

Konfigurációs befejeződése után az egyéni tartomány neve megjelenik **a webhely lap** **tartománynevek** szakaszában.

<a name="bkmk_configurearecord"></a><h2>Az egyéni tartományát A rekord létrehozása</h2>

Az A rekord létrehozásához először meg kell keresnie a IP-címét a webhely. Ezután szöveg beszúrása az egyéni tartományához tartozó DNS táblázatban a szolgáltató által biztosított eszközeivel. Minden tartományregisztráló egy A rekordot megadni a hasonló, de némileg eltérő módszer van, de a fogalmak megegyeznek. Az A rekord létrehozásán is létre kell hoznia egy CNAME rekordot, amely Azure használ, ellenőrizze az A rekordot.

1. A böngészőben nyissa meg az [Azure Kezelőportálja][portal].

2. A **webhelyek** lapon kattintással jelölje ki azt a webhelyet, válassza az **Irányítópult**, és válassza a **Tartományok kezelése** a képernyőn lentről felfelé.

    ![][setcname2]

5. Az **egyéni tartományok kezelése** párbeszédpanelen keresse meg **A rekordok konfigurálásakor használt az IP-címet**. Az IP-cím másolása Ez az A rekordot létrehozásakor lesz.

5. Az **egyéni tartományok kezelése** párbeszédpanelen figyelje awverify tartománynév végén található a szöveget a párbeszédpanel tetején. Meg kell **awverify.mysite.azurewebsites.net** hol található a **saját webhely** az a webhely nevét. Másolása, mivel az a tartomány nevét használja, ha az igazolás CNAME-rekord létrehozása

6. Jelentkezzen be a DNS tartományregisztráló webhelyén, és nyissa meg a DNS kezelése lapot. Keresse meg a hivatkozásokat, vagy a **Tartománynevet**, a **DNS-**vagy a **Név kiszolgáló kezelése**felirata webhely részeinek.

6. Ha jelölje ki vagy írja be- és CNAME-rekordok megkeresése Előfordulhat, hogy ki kell válassza a rekord típusát a legördülő listában, vagy nyissa meg a Speciális beállítások lapot.

7. A következő lépésekkel hozhat létre az A rekordot:

    1. Jelölje ki vagy adja meg a tartomány és altartomány, amelyekkel az A rekordot. Például válassza a **www-t** , ha létre szeretne hozni egy aliast **www.customdomain.com**. Ha az összes altartományokat helyettesítő szöveg létrehozása szeretne, adja meg a "__*__". Ez az összes alárendelt tartományok, például **mail.customdomain.com** **login.customdomain.com**és **www.customdomain.com**kiterjed.

        Ha létrehoz egy A rekordot a gyökértartomány szeretne, akkor előfordulhat, hogy fog szerepelni a "**@**" a tartományregisztráló DNS-eszközök a szimbólumot.

    2. A megadott mezőben írja be a felhőalapú szolgáltatás IP-címét. Ez a a tartomány bejegyzés használt az A rekordot a felhőalapú szolgáltatás üzembe az IP-címmel társít.

        Ha például az egy rekordot minden forgalom továbbítja **contoso.com** **137.135.70.239**, a telepített alkalmazások IP-címét az alábbi:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>A Host név/altartomány</strong></td>
        <td><strong>IP-cím</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        Ez a példa bemutatja, hogy hozzon létre egy A rekordot a legfelső szintű tartomány. Ha ki szeretne, hogy pontosan illeszkedjen összes altartományokat helyettesítő szöveg létrehozása, írná be "__*__", az altartomány.

7. Ezután hozzon létre egy CNAME rekordot a **awverify**alias és korábbi szerezte be **awverify.mysite.azurewebsites.net** kanonikus tartomány.

    > [AZURE.NOTE] Alias awverify az egyes tartományregisztrálókkal is működik, míg mások megkövetelheti awverify.www.customdomainname.com vagy awverify.customdomainname.com teljes alias tartomány nevére.

    Ha például a következő hoz létre egy CNAME rekordot, A rekord konfigurációjának ellenőrzése Azure használható.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias vagy állomásnév oszlopban név/altartomány</strong></td>
    <td><strong>Kanonikus tartomány</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Eltarthat egy kis időt, hogy a awverify CNAME szülőtől megtörtént a DNS-rendszerben. Az egyéni tartománynevet, az A rekordot a webhelyhez határozza meg, amíg a awverify CNAME propagálása nem állítható. Ellenőrizze, hogy a CNAME elérhető egy szolgáltatásba, például <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> is használhatja.

###<a name="add-the-domain-name-to-your-website"></a>A tartománynév elhelyezése a webhelyen

A tartománynév **awverify** CNAME rekord propagálása, miután társíthatja az egyéni tartomány határozza meg az A rekordot a webhelyhez. Az egyéni tartománynevet határozza meg az A rekordot, hogy a webhely bármelyik az Azure CLI használatával vagy az Azure adatkezelési portál használatával is hozzáadhat.

**A tartománynév használata az Azure parancssori kezelőfelületről Azure hozzáadása**

Telepítse és állítsa be az [Azure CLI](/manage/install-and-configure-cli/), és használja a következő parancsot:

    azure site domain add customdomain yoursitename

Például a következőket fogja hozzáadása egyéni tartománynevet, **akkor a contoso.com** **contoso.azurewebsite.net** webhelyhez:

    azure site domain add contoso.com contoso

Győződhet meg, hogy az egyéni tartománynevet hozzá lett adva a webhelyet a következő parancs használatával:

    azure site domain list yoursitename

A listában, ezzel a paranccsal vissza kell tartalmaznia az egyéni tartomány neve, valamint az alapértelmezett **. azurewebsite.net** bejegyzést.

**Az Azure adatkezelési portál használatával tartománynév hozzáadása**

1. A böngészőben nyissa meg az [Azure Kezelőportálja][portal].

2. A **webhelyek** lapon kattintással jelölje ki azt a webhelyet, válassza az **Irányítópult**, és válassza **A tartományok kezelése** a lap alján.

    ![][setcname2]

6. A **DOMAIN NAMES** szöveg mezőbe írja be a beállította a tartományt.

    ![][setcname3]

6. Kattintson a pipa ikonra koppintva fogadja el a tartomány nevét.

Konfigurációs befejeződése után az egyéni tartomány neve megjelenik **a webhely lap** **tartománynevek** szakaszában.

> [AZURE.NOTE] Miután hozzáadta a az A rekordot, hogy a webhely által megadott egyéni tartománynevet, érdemes lehet eltávolítani a awverify CNAME rekordot, a szolgáltató által biztosított eszközeivel. Jó helyen jár Ha egy másik A hozzáadni kívánt rekord kapcsolatban, akkor a awverify újra rekordot, mielőtt az új tartománynevet társítható határozza meg az új rekordot a webhelyhez.

## <a name="next-steps"></a>Következő lépések

-   [Webhelyek kezelése](/manage/services/web-sites/how-to-manage-websites/)

-   [Webhelyek SSL-tanúsítvány beállítása](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
