# <a name="using-cdn-for-azure"></a>Azure CDN használata

Azure tartalom kézbesítési hálózati (CDN) felajánlja a fejlesztők BLOB- és a fizikai csomópontok az Amerikai Egyesült Államokban, Europe, Ázsia, Ausztrália és Dél-Amerika számítási példányainak statikus tartalommá gyorsítótárazás által a nagy sávszélességű tartalom kézbesítési globális megoldást. Aktuális CDN-csomópont helyeken, című témakör [Azure CDN-csomópont helyek].

Ezt a feladatot a következő lépésekből áll:

* [Lépés: 1: A tárterület-fiók létrehozása](#Step1)
* [Lépés: 2: A tárterület-fiókjának új CDN végpont létrehozásához](#Step2)
* [3 lépés: A CDN-tartalom elérése](#Step3)
* [Lépés: 4: A CDN-tartalom eltávolítása](#Step4)

A előnyei CDN Azure adatok gyorsítótárba a következők:

-   Jobb teljesítményt és a felhasználói élmény a végfelhasználók számára, akik távol tartalomforrás, és olyan alkalmazásokat használ, ahol számos "internet utakat" szükséges tartalom betöltése
-   Az elosztott hatékonyabb kezeléséhez pillanatnyi nagy terhelés, mondjuk elején egy eseményt, például egy termék bevezetése a nagyméretű

Meglévő CDN-ügyfelek most már használhatja az Azure CDN az [Azure klasszikus portálon]. A CDN-bővítmény funkció használata az előfizetéshez, és rendelkezik egy külön [számlázási csomagot].

<a id="Step1"> </a>
<h2>Lépés: 1: A tárterület-fiók létrehozása</h2>

A következő eljárással hozzon létre egy új tároló fiókot Azure előfizetéshez. A tároló fiók Azure tárolása hozzáférést biztosít. A tároló fiók jelöli a legmagasabb szintű egyes Azure tárolási szolgáltatás összetevők eléréséhez a névtér: Blob-szolgáltatások, a várólista szolgáltatások és a táblázat szolgáltatásokat. Az Azure tárterület-szolgáltatásokkal kapcsolatos további tudnivalókért olvassa el [a Azure tároló szolgáltatások használata](http://msdn.microsoft.com/library/azure/gg433040.aspx)című témakört.

Tárterület-fiók létrehozása, vagy a szolgáltatás rendszergazdája, vagy egy közös rendszergazda társítva az előfizetéshez kell lennie.

> [AZURE.NOTE] A művelet végrehajtása az Azure szolgáltatás felügyeleti API használatával kapcsolatos információkért témakört [Tárterület-fiók létrehozása](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) hivatkozást.

**Az Azure előfizetéshez tartozó tárterület fiók létrehozása**

1.  Jelentkezzen be az [Azure klasszikus portálon].
2.  A bal alsó sarkában kattintson az **Új**gombra. Az **Új** párbeszédpanelen válassza a **Data Services**pontra, majd kattintson a **tárolás** **Gyors létrehozása**parancsra.

    Megjelenik a **Tárterület-fiók létrehozása** párbeszédpanel.

    ![Tárterület-fiók létrehozása][create-new-storage-account]

4. Az **URL-cím** mezőbe írja be a altartomány nevét. Ez a bejegyzés a 3-24 kisbetűket és számokat is tartalmazhat.

    Ez az érték, amely az előfizetéshez tartozó Blob, várólista vagy táblázat erőforrások cím a URI belül az állomásnév lesz. A Blob szolgáltatásban tároló erőforrás megoldására az alábbi formátumban szeretné használni URI hol * &lt;StorageAccountLabel&gt; * az **Írja be az URL-CÍMEK**beírt érték hivatkozik:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Fontos:** Az URL-cím címke az altartomány a tárterület-fiók URI-űrlapok és Azure-ban tárolt szolgáltatások között egyedinek kell lennie.

    Ehhez a fiókhoz programozás útján elérésekor ezt az értéket is lesz a portálon vagy a tárterület-fióknak a nevét.

5.  A **Terület és affinitás csoport** legördülő listából a régió vagy affinitás csoport a tárterület-fiókom kijelölése. Ha azt szeretné, hogy a tárhely szolgáltatások el szeretné helyezni az azonos adatközpont más Windows Azure-szolgáltatásokhoz használni kívánt terület helyett egy affinitás csoport kijelölése Javíthatja a teljesítményt, és a kilépési felmerült nincs díjakat.  

    **Megjegyzés:** Egy affinitás csoport létrehozásához nyissa meg a **Beállítások** területen az adatkezelési portál **Affinitás csoportok**elemre, kattintson a **egy affinitás csoport hozzáadása** vagy a **Hozzáadás gombra**. Is létrehozása és kezelése a Windows Azure szolgáltatás felügyeleti API affinitás csoportok. További tudnivalókért lásd: [affinitás csoportok műveleteket].

6. Az **előfizetés** legördülő listából válassza ki az előfizetést, azzal a tárterület-fiók fogja használni.
7.  Kattintson a **tárterület-fiók létrehozása**gombra. A tároló fiók létrehozásának folyamata néhány percet igénybe vehet.
8.  Ellenőrizze, hogy a tárterület-fiókot sikeresen létrejött, győződjön meg arról, hogy a fiók megjelenik az **Online**állapotban **tárolásra** elemeket.

<a id="Step2"> </a>
<h2>Lépés: 2:, Hozzon létre egy új CDN végpontot a tárterület-fiók</h2>

Miután CDN a tárterület-fiók elérésének engedélyezése vagy szolgáltatás üzemeltetett, nyilvánosan elérhető összes objektumát aktiválhatják CDN él mentén-gyorsítótárazás. Ha módosítja egy objektumot, amely jelenleg tárolja a CDN-t, az új tartalom nem lesz a CDN kapható mindaddig, amíg a CDN frissíti a tartalmát, a gyorsítótárban tárolt tartalom time-to-live időszak lejártakor.

**Tárterület-fiókjának új CDN végpont létrehozása**

1. Az [Azure klasszikus portált]a navigációs ablakban kattintson a **CDN-t**.

2. A menüszalagon kattintson az **Új**gombra. Az **Új** párbeszédpanelen válassza az **Alkalmazás szolgáltatások** **CDN-t**, majd a **Gyors létrehozása**.

3. A **Origin tartomány** tartalmazó legördülő listára jelölje ki a tárterület-fiókot a rendelkezésre álló tárhely fiókok a listából az előző részben létrehozott. 

4. A **Létrehozás** gombra kattintva hozza létre az új végpontot.

5. Amikor létrejött a végpontot, az előfizetés végpontok listájának jelenik meg. A lista nézet eléréséhez a gyorsítótárban tárolt tartalmat, valamint az origin tartományt használni az URL-cím látható. 

    Az origin tartománya a helyre, ahonnan a CDN gyorsítótárát tartalmat. Az origin tartomány lehet egy tárterület-fiókot vagy egy felhőalapú szolgáltatásba; a tároló fiók ebben a példában az alkalmazásában használják. Tárterület-tartalom szerint szeretné rendezni, egy megadott gyorsítótár-vezérlők beállítást, vagy az alapértelmezett heurisztikus a gyorsítótár hálózat peremhálózat-kiszolgálói gyorsítótárba kerül. 


    > [AZURE.NOTE] A konfiguráció a végpont létrehozott azonnal nem érhető el; a regisztrációhoz az a CDN-hálózaton keresztül terjesztése legfeljebb 60 percig is eltarthat. A felhasználók, akik tartománynevet szeretne használni a CDN azonnal próbálja állapotkód (hibás kérés) 400 jelenhet meg mindaddig, amíg a tartalom a CDN keresztül érhető el.

<a id="Step3"> </a>
<h2>3 lépés: Az Access CDN-tartalom</h2> 

A CDN-t a gyorsítótárban tárolt tartalom eléréséhez használja az a CDN megadott URL-cím a portálon. A gyorsítótárban tárolt blob címe a következőhöz hasonló lesz:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Lépés: 4: Tartalom eltávolítása a CDN</h2>

Ha már nem szeretne a gyorsítótárban lévő az Azure tartalom kézbesítési hálózati (CDN) objektum, hajthatók végre a következő lépések egyikét:

-   A nyilvános tárolóból az Azure blob-törölheti a blob.
-   A tároló fel, hogy magánjellegű helyett. [Tárolók és BLOB való hozzáférés korlátozása](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) talál további információt.
-   Az adatkezelési portálon az CDN-végpontot törlése vagy letiltása.
-   A szolgáltatott szolgáltatás már nem válaszol az objektum kérelmekre módosíthatók.

Egy objektum már szerepel a gyorsítótárban az a CDN gyorsítótárazott marad, amíg a time-to-live az objektum lejárta. A time-to-live időszak lejárta, az CDN ellenőrzi, hogy az a CDN-végpontot továbbra is érvényes, és az objektum névtelenül továbbra is elérhető. Ha nem, majd az objektum lesz már nem gyorsítótárazott.

Az azt jelenti, hogy közvetlenül a tartalom törlése jelenleg nem támogatja a Azure adatkezelési portálon. Kérjük, forduljon az [Azure támogatja a](https://azure.microsoft.com/support/options/) Ha módosítani szeretné, hogy közvetlenül a tartalom törlése. 

## <a name="additional-resources"></a>További források

-   [Hogyan affinitás csoport létrehozása az Azure-ban]
-   [Hogyan: tároló fiókokat az Azure előfizetés]
-   [Tudnivalók a Szolgáltatáskezelés API]
-   [Hogyan CDN-tartalom hozzárendelése az egyéni tartomány]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN-csomópont helyek]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure klasszikus portál]: https://manage.windowsazure.com/
  [számlázási terv]: /pricing/calculator/?scenario=full
  [Hogyan affinitás csoport létrehozása az Azure-ban]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Tudnivalók a Szolgáltatáskezelés API]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [Hogyan CDN-tartalom hozzárendelése az egyéni tartomány]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
