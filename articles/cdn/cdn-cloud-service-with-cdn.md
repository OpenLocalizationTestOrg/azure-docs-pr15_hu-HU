<properties
    pageTitle="Egy felhőalapú szolgáltatásba integrálása az Azure CDN |} Microsoft Azure"
    description="Útmutatást ad meg egy felhőalapú szolgáltatásba, amely a tartalmak az integrált Azure CDN zárólap szolgál telepítéséről, oktatóanyagok"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Egy felhőalapú szolgáltatásba integrálása az Azure CDN

Egy felhőalapú szolgáltatásba is integrálódik a Azure CDN, minden olyan tartalom, a felhőbeli szolgáltatástól helyről felszolgálásához. Ez a módszer is biztosít a következő előnyökkel jár:

- Egyszerűen üzembe helyezéséhez és képek, a parancsprogramokat és a felhőalapú szolgáltatás project könyvtárak a stíluslapok
- A felhőszolgáltatásba, például a jQuery vagy betöltő verziók NuGet csomagjait egyszerűen frissítése
- A webes alkalmazás és a CDN-felszolgált tartalom minden ugyanazt a Visual Studio felületet a kezelése
- A webes alkalmazás és a CDN-felszolgált tartalom egységesített telepítési munkafolyamata
- ASP.NET hozzákapcsolása és minification integrálása az Azure CDN

## <a name="what-you-will-learn"></a>Mi ismerkedhet meg ##

Ebből az oktatóanyagból megtanulhatja, hogy miként:

-   [Azure CDN zárólap integrálása a felhőalapú szolgáltatásba, és statikus tartalommá szolgálnak a weblapokon az Azure CDN](#deploy)
-   [A felhőszolgáltatásában statikus tartalommá gyorsítótár beállításainak konfigurálása](#caching)
-   [A vezérlő műveletek Azure CDN keresztül tartalmat](#controller)
-   [Szolgálják kapcsolt és Azure CDN keresztül tartalom minified a parancsfájl, Visual Studio rendszerének hibakeresési megőrzése mellett](#bundling)
-   [Állítsa be Visszalépés Stíluslap és parancsfájlok, amikor az Azure CDN offline állapotban.](#fallback)

## <a name="what-you-will-build"></a>Mit fog összeállítása ##

Fogja telepíteni felhőalapú szolgáltatás webes szerepkör az alapértelmezett sablon ASP.NET MVC, tartalom kiszolgálására a kép vezérlő művelet eredménye és az alapértelmezett JavaScript- és CSS fájlokat, például egy beépített Azure CDN-kód hozzáadása és konfigurálása a visszalépési mechanizmusa az abban az esetben, ha a CDN nem elérhető kapcsolat felszolgált kötegeket is kódírás.

## <a name="what-you-will-need"></a>Amire szüksége lesz ##

Ez az oktatóanyag az alábbi előfeltételek foglalja magában:

-   Az aktív [Microsoft Azure-fiók](/account/)
-   Az [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) Visual Studio 2015

> [AZURE.NOTE] Az oktatóprogram elvégzéséhez Azure-fiók van szüksége:
> + [Nyissa meg az ingyenes Azure-fiók](/pricing/free-trial/) is - jóváírások kap próbálja ki az fizetett Azure szolgáltatások is használhatja, és a megszokott ezek után akár tarthatja a fiók, és használata ingyenes Azure szolgáltatások, például webhelyek.
> + [MSDN előfizetői előnyeinek aktiválása](/pricing/member-offers/msdn-benefits-details/) is – az MSDN-előfizetés lépve credits havonta fizetett Azure szolgáltatások használható.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Egy felhőalapú szolgáltatásba terjesztése ##

Ebben a részben fogja telepíteni az alapértelmezett ASP.NET MVC alkalmazás sablon a Visual Studio Skype 2015 vállalati webhely szerepkörbe felhőalapú szolgáltatást, és integrálása egy új CDN-végpontot. Az alábbi lépéseket:

1. A Visual Studio Skype 2015 vállalati, hozzon létre egy új Azure felhő a menüsor megjelenítése az ismételt megjelenítéséhez **Fájl > Új > a Project > felhő > Azure Felhőszolgáltatásba**. Adja meg egy nevet, és kattintson az **OK gombra**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. Jelölje ki **Az ASP.NET webes szerepkört** , és kattintson a **>** gombra. Kattintson az OK gombra.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. Jelölje ki a **MVC** , és kattintson az **OK gombra**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Most tegye közzé a weben szerepkör az Azure felhőszolgáltatásba. Kattintson a jobb gombbal a felhőalapú szolgáltatás projekt, és válassza a **Közzététel**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Ha még nem jelentkezett be a Microsoft Azure, kattintson a **… fiók hozzáadása** tartalmazó legördülő listára, és a **fiók hozzáadása** menüpontra.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. A bejelentkezési lapon jelentkezzen be aktiválásához az Azure-fiók használt Microsoft-fiókjával.
7. Miután bejelentkezett,, kattintson a **Tovább**gombra.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Feltételezve, hogy egy felhőalapú szolgáltatás, illetve a tárhely fiók eddig nem hozott létre, a Visual Studio segítségével hozhat létre is. A **felhőalapú szolgáltatás hozzon létre és a fiókadatok** párbeszédpanelen írja be a kívánt szolgáltatás nevét, és válassza ki a kívánt területet. Kattintson a **Létrehozás**parancsra.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. A Közzététel beállításai lap konfigurációjának ellenőrzése, és kattintson a **Közzététel**gombra.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] A közzétételi folyamat cloud Services hosszú ideig tart. A engedélyezése webes telepítése az összes szerepkörök beállítás teheti a sokkal gyorsabban felhőszolgáltatásában hibakeresési, mert a Web szerepkörök (de ideiglenes) a gyors frissítéseket. Ezt a beállítást a további tudnivalókért olvassa el a [közzétételi egy felhőalapú szolgáltatásba, az Azure-eszközök használatával](http://msdn.microsoft.com/library/ff683672.aspx)című témakört.

    A **Microsoft Azure tevékenységnapló** azt mutatja, hogy közzétételi állapota **kész**, amikor egy CDN-végpontot, amely a felhőalapú szolgáltatás integrálva van hoz létre.

    >[AZURE.WARNING] Ha, a közzététel után a telepített felhőszolgáltatásában egy hiba képernyő jelenik meg, annak oka valószínűleg a felhőszolgáltatásában már telepítette a [vendégként való bekapcsolódáshoz OS, amely nem foglal .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates)használja.  A probléma megoldásához [.NET indítási feladatként 4.5.2](../cloud-services/cloud-services-dotnet-install-dotnet.md)üzembe helyezésével működik.

## <a name="create-a-new-cdn-profile"></a>Hozzon létre egy új CDN-profil

CDN-profil CDN-végpontok gyűjteménye.  Az egyes profilok tartalmaz egy vagy több CDN-végpontok.  Előfordulhat, hogy használni kívánt több profil a CDN-végpontok internetes tartomány, webalkalmazás, vagy néhány egyéb feltétel szerinti rendszerezésére.

> [AZURE.TIP] Ha már van egy CDN-profilt, ebben az oktatóanyagban használni kívánt, folytassa [Új CDN végpont létrehozása](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Hozzon létre egy új CDN-végpontot

**Tárterület-fiókjának új CDN végpont létrehozása**

1. Nyissa meg az [Azure Kezelőportálja](https://portal.azure.com)CDN-profilját.  Előfordulhat, hogy rendelkezik a kiemelt azt az előző lépésben az irányítópult.  Ha nem, akkor megtalálja: kattintson a **Tallózás gombra**, majd a **CDN-profilokat**, és a profiljában szeretne felvenni a végpontot.

    A CDN-profil lap jelenik meg.

    ![CDN-profil][cdn-profile-settings]

2. **Végpont hozzáadása** gombra.

    ![Végpont gomb felvétele][cdn-new-endpoint-button]

    A **Hozzáadás zárólap** lap jelenik meg.

    ![Végpont lap hozzáadása][cdn-add-endpoint]

3. Írja be egy **nevet** a CDN-végpont.  Ez a név szolgálnak a tartományban a gyorsítótárban tárolt forrásokat `<EndpointName>.azureedge.net`.

4. Válassza az **Origin típusa** legördülő *felhőszolgáltatásában*.  

5. A **Origin hostname (állomásnév)** tartalmazó legördülő listára válassza a felhőalapú szolgáltatás.

6. Hagyja az alapértelmezett beállítások az **Origin elérési útját**, **Origin host élőfej**és **protokoll/Origin port**.  Meg kell adnia egy protocol (HTTP vagy HTTPS).

7. A **Hozzáadás** gombra kattintva hozza létre az új végpontot.

8. Amikor létrejött a végpontot, jelennek meg a profil végpontok listáját. A lista nézet eléréséhez a gyorsítótárban tárolt tartalmat, valamint az origin tartományt használni az URL-cím látható.

    ![CDN-végpontok][cdn-endpoint-success]

    > [AZURE.NOTE] Az endpoint nem azonnal lesz használható.  A regisztrációhoz az a CDN-hálózaton keresztül szülőtől 90 percig is eltarthat. A felhasználók, akik tartománynevet szeretne használni a CDN azonnal próbálja állapotkód 404 jelenhet meg mindaddig, amíg a tartalom a CDN keresztül érhető el.

## <a name="test-the-cdn-endpoint"></a>Az a CDN-végpontot tesztelése

Ha a közzétételi állapota **kész**, nyisson meg egy böngészőablakot, és kattintson az * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. A beállítás Ez az URL a következő:

    http://camservice.azureedge.net/Content/bootstrap.css

Amely megfelel az alábbi origin URL-címet a CDN-végpont:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Amikor visszalép * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, Testreszabásuk a böngészőben kéri töltse le vagy nyissa meg a bootstrap.css az közzétett webalkalmazásból kapott.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Hasonlóképpen érheti el minden nyilvánosan elérhető URL-CÍMÉT a * *http://*&lt;serviceName >*.cloudapp.net/** egyenesen az a CDN-végpontot. Példa:

-   A Script elérési .js fájl
-   Minden olyan tartalom a szülőszakasz/Content címkéje fájl elérési útja
-   Bármely vezérlő művelet
-   Ha a lekérdezési karakterlánc engedélyezve van a CDN végpont, a lekérdezési karakterlánc bármely URL-címe

Erre valójában a fenti konfiguráció üzemeltetheti a teljes felhőalapú szolgáltatást * *http://*&lt;cdnName >*.azureedge.net/**. Ha e navigáljon **http://camservice.azureedge.net/ ** tudom bekapcsolni a művelet eredménye kezdőlap/Index.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Ez nem jelent, azonban, hogy az mindig célszerű (vagy általában célszerű) keresztül Azure CDN-teljes felhőszolgáltatásba szolgálnak. A telepítés által okozott problémák vannak:

-   Eljárás a teljes webhely nyilvánosak, mivel a Azure CDN magánjellegű tartalmakat nem szolgál, most szükséges.
-   Ha valamilyen okból offline állapotba kerül az a CDN-végpontot, e ütemezett karbantartás vagy felhasználói hiba, a teljes felhőszolgáltatásba offline állapotba kerül kivéve, ha a vevők átirányíthatja az origin URL-cím * *http://*&lt;serviceName >*.cloudapp.net/**.
-   Még az egyéni beállításokkal gyorsítótár-vezérlők (lásd: [konfigurálása gyorsítótár-beállításai a felhőszolgáltatásában statikus fájlokat](#caching)) a CDN-végpont nem javítja a nagyon dinamikus tartalom. Ha a Kezdőlap lap betöltése a CDN-végpontot, a fenti, értesítés, amely legalább 5 másodperc tartott betöltése alapértelmezett kezdőlapja meglehetősen egyszerű lap első alkalommal próbált. Tegyük fel, hogy mi történne ügyfélprogram az ezen az oldalon dinamikus tartalommal percenként frissítenie kell. Megfelelője a CDN-végpont gyakori gyorsítótár mért sikertelen találatok rövid gyorsítótár lejárat felszolgálásához egy CDN-végpontot dinamikus tartalom van szükség. A felhőalapú szolgáltatás teljesítményének hurts, és a defeats a CDN célját.

Ez esetben milyen tartalmakat szolgáló az Azure CDN-eseti alapon a felhőszolgáltatásában határozza meg. Emiatt már rendelkezik látott egyes tartalom fájlok elérése az a CDN-végpontot. E kell követnie egy adott vezérlő művelet keresztül az a CDN-végpontot kiszolgálására a [vezérlő műveletek Azure CDN-től a tartalmat](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>A felhőszolgáltatásában statikus fájlok gyorsítótár beállításainak konfigurálása ##

Azure CDN-integráció a felhőszolgáltatásában megadhatja, hogy gyorsítótárba helyezhető az a CDN-végpont statikus tartalommá módját. Ehhez nyissa meg a *Web.config* a webes szerepkör projektből (pl. WebRole1), és adja hozzá a `<staticContent>` elem `<system.webServer>`. Az alábbi XML konfigurálja a gyorsítótár 3 nap múlva lejár.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Miután ezt megtette, az összes statikus fájlokat a felhőszolgáltatásában megfigyelheti az a CDN-gyorsítótár ugyanezt a szabályt. Pontosabban szabályozhatja a gyorsítótár beállításainak hozzáadása *egy fájlt* egy mappába, és adja hozzá a beállításokat. Például hozzáadása *egy fájlt* a *\Content* mappába, és a tartalom cserélje ki az alábbi XML-fájl:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

E beállítás hatására a *\Content* mappából, hogy gyorsítótárba helyezhető-15 nap összes statikus fájlokat.

További információt, hogy miként kell beállítani a `<clientCache>` elem, lásd: [ügyfél gyorsítótárának &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

A [vezérlő műveletek Azure CDN-től a tartalmat](#controller), az e is megtudhatja, hogyan konfigurálása a gyorsítótár beállításainak vezérlő művelet eredménye az a CDN-gyorsítótár.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>A vezérlő műveletek Azure CDN keresztül tartalmat ##

Ha egy felhőalapú szolgáltatás webes szerepkör integrálása Azure CDN, is viszonylag könnyen vezérlő műveletek az Azure CDN-től a tartalmat. Eltérő szolgáló Azure CDN (igazolni felett) közvetlenül a felhőalapú szolgáltatás, [Martin Balliauw](https://twitter.com/maartenballiauw) megtudhatja, hogy miként egy kis játék a [csökkentése késés az Azure CDN a weben](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN)MemeGenerator vezérlő tesz. E fog egyszerűen Reprodukálja, itt.

Tegyük fel, hogy a felhőszolgáltatásában szeretne előállítani memes alapján fiatal tokmányba Norris képként ( [Zsolt](http://www.flickr.com/photos/alan-light/218493788/)fény fényképpel) jelennek meg:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Van egy egyszerű `Index` műveletet, amely lehetővé teszi a felhasználóknak a felsőfokú melléknevek adhat meg a képet, majd hozza létre a meme után azok bejegyzéseket tehet közzé a műveletet. Mivel ez éppen tokmányba Norris, várható válhat menet népszerű globálisan a lapon. Ez a jó példa félig dinamikus tartalom felszolgálásához az Azure CDN.

Kövesse a fenti vezérlő művelet beállítása:

1. A *\Controllers* mappában .cs *MemeGeneratorController.cs* nevű új fájl létrehozása, és cserélje le a tartalom a következő kódot. Ügyeljen arra, hogy a kijelölt rész cserélje le a CDN nevére.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Kattintson a jobb gombbal az alapértelmezett `Index()` művelet, és válassza a **Nézet hozzáadása**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Fogadja el az alábbi beállításokat, és kattintson a **Hozzáadás**gombra.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Nyissa meg az új *Views\MemeGenerator\Index.cshtml* , és cserélje le a tartalom a következő egyszerű HTML a felsőfokú melléknevek elküldése:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Tegye közzé újra a felhőalapú szolgáltatásba, és kattintson az * *http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** a böngészőben.

Amikor az űrlapértékek elküldése `/MemeGenerator/Index`, a `Index_Post` művelet módszer mutató hivatkozást ad vissza az `Show` művelet módszer a megfelelő beviteli azonosítójú. Ha a hivatkozás gombra kattint, érhető el a következő kódot:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Ha a helyi hibakereső van csatolva, majd fog kapni a normál hibakeresési felület és a helyi átirányítást. Ha a felhőbeli szolgáltatástól fut, majd azt úgy irányítja át a:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Amely megfelel az alábbi origin URL-címet a CDN-végpont:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Ezután felhasználhatja a `OutputCacheAttribute` a attribútum a `Generate` használatával adja meg, hogyan művelet eredménye gyorsítótárba, amely az Azure CDN akkor figyelembe veszi. Az alábbi kód adja meg a gyorsítótár lejárati 1 óra (3600 másodpercet).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Hasonlóképpen, szolgálhat olyan vezérlő tevékenységtől tartalmat a felhőszolgáltatásában keresztül az Azure CDN a kívánt gyorsítótárazási lehetőség.

A következő szakaszban lehet megtudhatja, hogy miként kiszolgálására a kötegelt és minified parancsprogramokat és a CSS Azure CDN keresztül.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>ASP.NET hozzákapcsolása és minification integrálása az Azure CDN ##

Parancsfájlok és CSS stíluslapok ritkán módosíthatja, és az Azure CDN-gyorsítótár elsődleges vár. A teljes webhely szerepkör az Azure CDN keresztül szolgáló legkönnyebben hozzákapcsolása és minification integrálása az Azure CDN. Azonban előfordulhat, hogy nem kívánt művelet, e megtudhatja, hogy miként adunk, amíg a kívánt develper felület ASP.NET hozzákapcsolása és minification, például: megőrzése:

-   Nagy hibakeresési mód élmény
-   Egyszerűsített telepítési
-   Az ügyfelek számára a parancsfájlt vagy egyéni Stíluslap verziófrissítések azonnali frissítések
-   Ha a CDN-végpontot nem sikerül alaplekérdezések mechanizmusa
-   Kód módosításának kisméretűvé alakítása

A projektben **WebRole1** [Integrate az Azure webhely és a szolgálják statikus tartalommá a weblapokon az Azure CDN az Azure CDN zárólap](#deploy)létrehozott, nyissa meg a *App_Start\BundleConfig.cs* , és tekintse meg a `bundles.Add()` módszer hívásokat.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

Az első `bundles.Add()` utasítás hozzáad egy parancsprogramot az első lépésekhez a virtuális könyvtár `~/bundles/jquery`. Ezután nyissa meg *Views\Shared\_Layout.cshtml* hogyan script az első lépésekhez címke leképezésének módját megjelenítéséhez. Látnia kell a következő sort Razor kód megkeresése:

    @Scripts.Render("~/bundles/jquery")

Razor kód Azure webes szerepkör futtatásakor azt meg egy `<script>` címkét a parancsprogram köteget, az alábbihoz hasonló:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Azonban, hogy futtatásakor a Visual Studióban beírásával `F5`, akkor fogja jeleníti meg a minden parancsfájl az első lépésekhez egyenként (fenti esetben csak egy parancsprogram jelenik meg az első lépésekhez):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Ez lehetővé teszi, hogy a fejlesztői környezet JavaScript-kód hibakeresési, miközben csökkentése egyidejű ügyfélkapcsolatok (hozzákapcsolása), és javítása a fájl letöltése teljesítmény (minification) gyártási. Azure CDN-integrációval megtartása remek lehetőség. Ezenkívül leképezett az első lépésekhez már tartalmazza az automatikusan generált verzió karakterlánc, mivel kívánt való replikáció funkció tehát a amikor frissíti a NuGet jQuery verzióból, akkor frissíthető az ügyfél oldalán minél korábban beállítást.

Kövesse az alábbi integrációs ASP.NET hozzákapcsolása és minification a CDN végponttal.

1. Vissza a *App_Start\BundleConfig.cs*, módosítsa a `bundles.Add()` módszereket, amelyekkel használja egy másik [az első lépésekhez konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), egy CDN címet ad meg. Ehhez cserélje le a `RegisterBundles` módszer definition kódot a következő:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Helyére ne felejtse el `<yourCDNName>` az Azure CDN a nevet.

    Az egyszerű szavakat, állít be `bundles.UseCdn = true` gondosan megfogalmazott CDN URL-címe hozzáadott minden az első lépésekhez és. Ha például az első konstruktort be a kódot:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    megegyezik a következővel:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Ez a konstruktor Ez az ASP.NET hozzákapcsolása és minification jeleníti meg, amikor helyileg indítja egyéni parancsfájlok, de a megadott CDN-cím használata a szóban forgó parancsfájl eléréséhez. Megjegyzendő, két fontos jellemzőit gondosan megfogalmazott CDN URL-címe:

    -   Az a CDN URL-CÍMÉT az origin mező is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, amely olyan, hogy a parancsprogram az első lépésekhez a felhőszolgáltatásában ténylegesen a virtuális könyvtár.
    -   CDN-konstruktor használja, mivel a CDN parancsprogram-az első lépésekhez már nem címkében az automatikusan generált verzió karakterlánc leképezett URL-címét. Egyedi verzió karakterlánc manuálisan kell létrehozni, minden alkalommal, amikor a parancsprogram az első lépésekhez gyorsítótár mért sikertelen az Azure CDN a találatok kényszerítése módosul. Egy időben Ez a verzió egyedi karakterlánc állandónak kell a telepítés után a kötegben telepíti, teljes méret az Azure CDN a gyorsítótár-találatok élettartama keresztül.
    -   A lekérdezési karakterlánc v = < W.X.Y.Z > illesztéssel a *Properties\AssemblyInfo.cs* a webes szerepkör projektben. A telepítési munkafolyamatot, amely tartalmazza az összeállítás-verzió növekvő, minden alkalommal, amikor az Azure segítségével teszi közzé is. Vagy *Properties\AssemblyInfo.cs* csak módosíthatja a projekt automatikusan növeléséről a verziószámot tartalmazó karakterlánc, minden alkalommal, amikor hoz létre, a helyettesítő karakter használata "*". Példa:

            [assembly: AssemblyVersion("1.0.0.*")]

        Egyszerűsítheti a telepítés élettartama egy egyedi karakterláncot létrehozása más stratégia itt működni fog.

3. Ismét közzéteheti a felhőalapú szolgáltatást, és a Kezdőlap lap eléréséhez.

4. A HTML-kódot az oldal megtekintése. Látnia kell CDN URL-címe egy egyedi verzió karakterlánccal, minden alkalommal, amikor a módosítások ismét közzéteheti a felhőalapú szolgáltatáshoz való leképezésének módját megjelenítéséhez. Példa:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. A Visual Studióban, hibakeresési beírásával a felhőalapú szolgáltatást a Visual Studióban `F5`.,

6. A HTML-kódot az oldal megtekintése. Minden egyes külön-külön való megjelenítését, hogy egy egységes hibakeresési felület a Visual Studióban lehet parancsfájl továbbra is látni fogja.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>CDN-URL-címek alaplekérdezések mechanizmusa ##

Bármilyen okból sikertelen az Azure CDN-végpontot, ha azt szeretné, a weblap és ügyeljen arra, elég a JavaScript- és betöltő megoldásként origin webkiszolgálón eléréséhez. Elég komoly elveszíti képek CDN elérhetetlenség, de sokkal több nagyon gyenge hálózati minőség elveszíti a parancsprogramokat és a stíluslapok kulcsfontosságú lap funkcióit miatt a webhelyen.

Az [első lépésekhez](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) osztály [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , amely lehetővé teszi, hogy az a CDN-hiba alaplekérdezések eljárás konfigurálása nevű tulajdonságot tartalmazza. Ez a tulajdonság használatához kövesse az alábbi lépéseket:

1. A webes szerepkör projekt nyissa meg a *App_Start\BundleConfig.cs*, ahová felvette a CDN URL-címet az egyes [az első lépésekhez konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), és a következő módosításokat kiemelve az alapértelmezett kötegeket alaplekérdezések mechanizmusa hozzáadása:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Ha `CdnFallbackExpression` van nem null parancsfájl van beékelt ellenőrizze, hogy a köteg sikeresen betöltött-e, és, ha nem, az első lépésekhez eléréséhez közvetlenül a webes forráskiszolgálóval a HTML-be. Ez a tulajdonság kell a JavaScript-kifejezés, amely azt vizsgálja, hogy a megfelelő CDN az első lépésekhez megfelelően töltődik be kell állítani. A kifejezés minden az első lépésekhez ellenőrzéséhez szükséges működik aszerint, hogy a tartalmat. A fenti alapértelmezett kötegeket:

    -   `window.jquery`jquery-{verzió} .js könyvjelzőnév
    -   `$.validator`a könyvjelzőnév jquery.validate.js
    -   `window.Modernizr`a könyvjelzőnév .js modernizer-{verzió}
    -   `$.fn.modal`bootstrap.js könyvjelzőnév

    Akkor lehet, hogy láthatta, hogy e nem adta meg az CdnFallbackExpression a `~/Cointent/css` az első lépésekhez. Ennek oka, hogy jelenleg csak egy [System.Web.Optimization hibáját](https://aspnetoptimization.codeplex.com/workitem/104) beszúrhatja egy `<script>` helyett a várható a visszalépési CSS-címke `<link>` címke.

    Van, akkor jó helyen jár, egy jó [Stílus az első lépésekhez Visszalépés](https://github.com/EmberConsultingGroup/StyleBundleFallback) [Decemberéig tanácsadási csoport](https://github.com/EmberConsultingGroup)által kínált.

2. Használni szeretné a megoldáshoz CSS, .cs új fájl létrehozása a webes szerepkör projekt *App_Start* mappába *StyleBundleExtensions.cs*, és cserélje le a tartalmát a [GitHub kódot](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).

4. A *App_Start\StyleFundleExtensions.cs*nevezze át a névtér a webes szerepkör nevét (például a **WebRole1**).

3. Való visszatéréshez `App_Start\BundleConfig.cs` és módosítása az utolsó `bundles.Add` utasítás kiemelt kódot a következő:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Az új bővítmény módszert használja az azonos arról ellenőrizni a DOM-a HTML-kód parancsfájl a egy egyező osztálynév, a szabály neve és a szabály érték, ha nem sikerül az egyezés a CSS-fájlok az első lépésekhez, és visszatérhet a forráskiszolgálóval webes esik definiálva.

4. Tegye közzé újra a felhőalapú szolgáltatást, és a Kezdőlap lap eléréséhez.

5. A HTML-kódot az oldal megtekintése. Keresse meg a parancsfájlok bejuttatott az alábbihoz hasonló:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Figyelje meg, hogy a CSS-fájlok az első lépésekhez bejuttatott parancsfájl továbbra is tartalmaz az errant tartós a a `CdnFallbackExpression` tulajdonság a sorban lévő:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    De óta első része a. kifejezés mindig true (a közvetlenül fölötte, amely a vonalra) ad vissza, a document.write() függvény soha nem futtathatók.

## <a name="more-information"></a>További információ ##
- [Az Azure Tartalomkézbesítési hálózatai (CDN) – áttekintés](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Azure CDN használatával](cdn-create-new-endpoint.md)
- [ASP.NET hozzákapcsolása és Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
