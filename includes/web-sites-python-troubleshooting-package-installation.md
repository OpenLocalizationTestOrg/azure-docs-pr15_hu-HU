Előfordulhat, hogy néhány csomag nem telepíthető a futtatásakor a Azure mezőpont használatával.  Egyszerűen lehet, hogy a csomag nem áll rendelkezésre az Python csomag Index.  Lehet, hogy szükség-e egy fordításkor (egy fordításkor nem érhető el a számítógépen a web App alkalmazást futtató Azure App szolgáltatásban).

Ebben a részben áttekintjük módokon való megoldásával probléma.

### <a name="request-wheels"></a>Kerekek kérése

A csomag telepítéséhez a fordító, ha megpróbál kell kapcsolatba lépne a csomag tulajdonosával kerekek tenni a csomagban érhető el.

A [Microsoft Visual C++ fordító Python 2.7][]a legutóbbi elérhetőségét célszerűbb most, hogy telepítve van a natív kód Python 2.7 csomagok létrehozásához.

### <a name="build-wheels-requires-windows"></a>Kerekek összeállítása (Windows igényel)

Megjegyzés: Amikor ezt a beállítást, ügyeljen arra, hogy az megegyezzen a platform/építészet vagy a web App alkalmazás Azure szolgáltatásban használt Python környezet használatával csomag lefordítani (Windows/32-bit/2.7 vagy 3.4).

A csomag nem telepíthető, mert a fordító igényel, ha a fordító telepítéséhez a helyi számítógépen, és egy ikon, majd tartalmazni fogja a tárházba csomagjára összeállítása.

A Mac vagy Linux-felhasználók: Ha nem egy Windows számítógépre való hozzáférést, lásd: a [virtuális gépen futó Windows létrehozása][] , hogyan készíthet egy virtuális a Azure.  A kerekek összeállítása, felveheti a tárházba és ha szeretné vetni a virtuális felhasználhatja azt. 

[Microsoft Visual C++ fordító Python 2.7][]Python 2.7, telepítheti.

Python 3.4 telepítheti [A Microsoft Visual C++ 2010 Express][].

Kerekek létrehozásához, az ikon csomag lesz szüksége:

    env\scripts\pip install wheel

Kell megadnia `pip wheel` függőség összeállításához:

    env\scripts\pip wheel azure==0.8.4

Ezzel létrehoz egy .whl \wheelhouse mappában.  A \wheelhouse mappát és ikon fájlok hozzáadása a tárházba.

A requirements.txt hozzáadása szerkesztése a `--find-links` lehetőség a képernyő tetején. Ez azt jelenti, a helyi mappában pontos egyezést keres, mielőtt a python csomag index mezőpont.

    --find-links wheelhouse
    azure==0.8.4

Ha szeretné minden függőség szerepeltetni a \wheelhouse mappát, és nem használja fel a python csomag index egyáltalán kényszerítheti figyelmen kívül hagyja a csomag index hozzáadásával mezőpont `--no-index` a requirements.txt tetején.

    --no-index

### <a name="customize-installation"></a>Telepítés testreszabása

Testre szabhatja a telepítési parancsfájlt, egy csomag telepítéséhez alternatív telepítőt használ, például: egyszerű virtuális környezetben\_telepítése.  Lásd: az megjegyzéseket fűzhet például egy deploy.cmd.  Győződjön meg arról, hogy ilyen csomagok nem szerepel requirements.txt mezőpont megakadályozásához telepíti őket.

Adja hozzá a telepítési parancsfájlt:

    env\scripts\easy_install somepackage

Is használhatja az egyszerű\_exe telepítő telepítés történő telepítés (némelyike zip-kompatibilis, így könnyen\_telepítés támogatja őket).  A telepítő hozzáadása a tárházba és könnyen meghívása\_megkerülhetők, ha az elérési utat a végrehajtható fájl telepítése.

Adja hozzá a telepítési parancsfájlt:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>A virtuális környezet szerepeltetni a tárházba (Windows igényel)

Megjegyzés: Ez a beállítás használata esetén ügyeljen arra, hogy virtuális megegyezzen a platform/építészet vagy a web App alkalmazás Azure szolgáltatásban használt környezetet használata (Windows/32-bit/2.7 vagy 3.4).

Ha a virtuális környezet be a tárat, megakadályozhatja, hogy a telepítési parancsfájlt módon Azure virtuális környezet-kezelés: hozzon létre egy üres fájlt:

    .skipPythonDeployment

Azt javasoljuk, hogy a meglévő virtuális környezet meg az alkalmazást, hogy mikor virtuális lett felügyelt környezet automatikusan a maradék fájlok törlése.


[Windows operációs rendszert futtató virtuális gép létrehozása]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ fordító Python 2.7.]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
