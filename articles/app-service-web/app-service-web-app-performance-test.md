<properties
   pageTitle="Az Azure webalkalmazást teljesítmény tesztelése |} Microsoft Azure"
   description="Azure web app teljesítményét tesztek ellenőrizni, hogy hogyan kezeli az alkalmazás a felhasználó betöltés futtatása. Válaszidő mérésére, és keresse meg a hibák, amelyek jelezhetik problémákat."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Teljesítmény az Azure-terhelés alatt webalkalmazást tesztelése

Indítsa el, és munkakörnyezeti frissítések telepítéséhez, jelölje be a webalkalmazás teljesítményét. Úgy, hogy jobban felmérheti, hogy az alkalmazás készen áll a Megjelenés. Működés további benne, hogy az alkalmazás kezelheti a forgalmat, vagy a következő marketingtevékenység leküldéses csúcs használata során.

Nyilvános villámnézetben is vizsgálat az alkalmazás ingyenes az Azure-portálon.
Ezek a vizsgálatok az alkalmazás felhasználói terheltsége hasonlóan egy adott időszakban, és az alkalmazás válasz mérjük. Például a vizsgálati eredmények megjelenítése, hogy milyen gyorsan az alkalmazás válaszol a felhasználók adott számú. Ezenkívül mutatják hány kérések nem sikerült, amely jelezheti, hogy problémákat tapasztal az alkalmazással kapcsolatban.      

![A web app teljesítményproblémákat keresése](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Előzetes teendők

* Ha nincs telepítve egyik már szüksége lesz az [Azure előfizetés](https://account.windowsazure.com/subscriptions). Megtudhatja, hogyan dolgozhat, [Nyissa meg az ingyenes Azure-fiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* A teljesítmény próba előzményekre egy [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -fiókot kell. A megfelelő fiók automatikusan létrejön, a teljesítmény vizsgálata beállításakor. Vagy pedig hozzon létre egy új fiókot, illetve egy meglévő-fiókot használja, ha Ön a tulajdonosa. 

* Telepítse az alkalmazást nem munkakörnyezetben vizsgálathoz. Az alkalmazás használata egy alkalmazás szolgáltatáscsomagja nem a terv előállítására használt van. Úgy, hogy Ön nem érinti bármelyik meglévő ügyfelek vagy az alkalmazás gyártási lassíthatja. 

## <a name="set-up-and-run-your-performance-test"></a>Állítsa be, és futtassa a teljesítmény vizsgálata

0.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com). A saját Visual Studio Team Services-fiók használata esetén a fióktulajdonos rendszergazdaként jelentkezzen be.

0.  Nyissa meg a web App alkalmazásban.

    ![Nyissa meg az összes böngészése, Web Apps alkalmazások, a web App alkalmazásban](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Nyissa meg a **teljesítményét vizsgálatot**.

    ![Válassza az eszközök, teljesítmény vizsgálata](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. Most fog csatol egy [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) fiókot a teljesítmény próba előzményekre.

    A csapat Services-fiók használata esetén válassza a fiók. Ha nem, hozzon létre egy új fiókot.

    ![Jelölje ki a meglévő Team Services-fiókot vagy hozzon létre egy új fiókot](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  A teljesítmény vizsgálata létrehozása. Állítsa be részleteit, és futtassa a tesztet. 

Nézze meg az eredményt valós idejű, a vizsgálat futása közben.

Tegyük fel, hogy az alkalmazás, amely az adott az előző év ünnepi eladási kuponokat ki van. Ebben az esetben a 100 egyidejű ügyfelek csúcs terhelést 15 percet tartott. Duplán a vevők számát idén szeretnénk. Is szeretnénk javítsa a felhasználói elégedettséget lap betöltés idejének csökkentése 5 másodperc 2 másodpercig. Igen azt fogja tesztelje a frissített alkalmazás teljesítményének 15 percet 250 felhasználókkal.

Azt fogja szimulálhatja betöltése a alkalmazásba létrehozása (ügyfelek) virtuális felhasználók által ki látogasson el a webhelyére, egy időben. Ezzel jelenik us hány kéréseket adatkapcsolat vagy lassan válaszol.

  ![Hozzon létre, beállítása és a teljesítmény teszt futtatása](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  A web app alapértelmezett URL-CÍMÉT a rendszer automatikusan hozzáadja. 
   Módosíthatja, hogy az URL-címet, tesztelje a többi oldal (csak HTTP GET kérelmek).

   *  Helyi feltételek szimulálhatja késés csökkentése, és válasszon a felhasználók legközelebb helyet a betöltés létrehozása.

  Az alábbiakban a folyamatban lévő a tesztet. Az első perc alatt az oldal szeretnénk, mint lassabban betölti.

  ![Valós idejű adatokat a folyamatban lévő teljesítmény vizsgálata](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Miután végzett a vizsgálat, az azt megtudhatja, hogy az oldal sokkal gyorsabb betöltése az első perc után. Ezzel az elemzéssel azonosítása, ahol azt előfordulhat, hogy szeretne kezdeni a probléma elhárításához.

  ![Bejegyzett vizsgálat eredményeit mutatja, többek között a sikertelen kérelmek](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Több URL-cím tesztelése

Visual Studio Web Test fájl feltöltésekor szerint jelenítik meg egy záró végfelhasználói forgatókönyv több URL-címeit tartalmazó teljesítmény vizsgálatok is futtathatók. Néhány módszert hozhat létre a Visual Studio Web Test fájlban található:

* [Forgalom Fiddler használatával rögzítése és a Visual Studio Web Test-fájlként](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Visual Studio betöltés próba fájl létrehozása](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Tölthet fel, és futtassa a Visual Studio Web Test fájlt:
 
0. Kövesse a fenti az **Új teljesítményét tesztelje** a lap megnyitásához.
   Ez a lap válasszon a CONFIGFURE használatával TESZTELÉSE lehetőséget a **Configure vizsgálat használatával** lap megnyitásához.  

    ![A beállítás betöltés, tesztelje a lap megnyitása](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Ellenőrizze, hogy a **Visual Studio Web Test** a próba-típus van beállítva, és válassza a HTTP archív mappák.
    A "mappát" ikon használata beállítások a fájl Adatkijelölő párbeszédpanel megnyitásához.

    ![Visual Studio webes URL-cím Test több fájl feltöltésekor](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Miután feltöltötte a fájlt, megjelenik a lista URL-cím adatai csoportban vizsgálandó URL-címet.
 
0. Adja meg a felhasználó terhelést tesztelje az időtartam, majd válassza a **Futtatás próba**.

    ![A felhasználó betöltése és időtartam](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    A próba után látható az eredmény a két ablaktáblát. A bal oldali ablaktáblában a diagramok sorozata performnace adatokat jeleníti meg.

    ![A teljesítmény eredménye ablaktábla](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    A jobb oldali sikertelen kérelmek, a hiba és történt, hogy hányszor listáját jeleníti meg.

    ![A kérés hibák ablaktábla](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Futtassa újra a tesztet a **ismétlése** ikonra a jobb oldali ablaktábla tetején kiválasztásával.

    ![A próba újrafuttatása](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>A kérdések és válaszok

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>Kérdés: van korlátozás a mennyi ideig lehet futtatását is lehetővé teszi a vizsgálat? 

**A**: Igen, akár egy óra a próba futtathatja az Azure-portálon.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>Kérdés: hogyan mennyi időt kapok teljesítmény vizsgálatok futtatása? 

**A**: után a minta nyilvános akkor 20 000 felhasználó virtuális perc (VUMs) ingyenes havonta a Visual Studio Team Services-fiókjával. Egy VUM megszorozza a próba percben virtuális felhasználók számát. Ha a művelet túllépi a szabad értéket az igényeinek, több időt vásárolhat, és fizetni csak akkor használja.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>Kérdés: hol ellenőrizheti lehet használtam még eddig hány VUMs?

**A**: ellenőrizheti, hogy az összeg, az Azure-portálon.

![Nyissa meg a Team Services-fiók](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Jelölje be a használt VUMs](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>Kérdés: Mi az alapértelmezett beállítás a és a meglévő azt vizsgálja hatással vannak?

**A**: az alapértelmezett beállítás a teljesítmény betöltés vizsgálatok egy kézi próba - ugyanaz, mint előtt több URL-címe tesztelése lehetőséget hozzá lett adva a portálra.
A meglévő vizsgálatot hasonlóan működnek, és az URL-cím használata is.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>Kérdés: milyen szolgáltatások nem támogatja a Visual Studio Web tesztfájl?

**A**: jelenleg ez a funkció nem támogatja a webes próba bővítmények, adatforrások és kivonása szabályokat. Törölje ezeket a webes vizsgálatot fájl szerkesztése Azt a projektet támogatásához az ezek szolgáltatásai jövőbeli frissítéseit.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>Kérdés: támogatja bármely más webes próba fájlformátumok?
  
**A**: A bemutató csak Visual Studio Web Test formátumú fájlok használata támogatott.
Örömmel vesszük észrevételeit, ha más fájlformátumokat segítségre lenne. A kapcsolatfelvételi e-mail [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>Kérdés: milyen egyéb teheti a Visual Studio Team Services fiókkal?

**A**: az új fiók megkereséséhez használja a ```https://{accountname}.visualstudio.com```. A kód megosztása, összeállítása, tesztelje, nyomon követheti a munkahelyi telefonszámán és a szállítási szoftver – a felhőben, bármilyen eszközzel, vagy a nyelv a mind. További arról, hogy [A Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) funkciók és szolgáltatások csapata együttműködését, és üzembe folyamatosan.

## <a name="see-also"></a>Lásd még:

* [Egyszerű felhő teljesítmény vizsgálatok futtatása](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Apache Jmeter teljesítmény vizsgálatok futtatása](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Rekord és felhőalapú ismétléses betöltése vizsgálatok](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Teljesítmény tesztelje az alkalmazás a felhőben](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
