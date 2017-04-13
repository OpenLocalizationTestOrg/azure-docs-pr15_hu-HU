Azure határozza meg, hogy az alkalmazást használ-e a Python **mindkét alábbi feltétel teljesülése esetén**:

- a legfelső szintű mappa Requirements.txt fájl
- bármely .py fájl a legfelső szintű mappa, illetve nem runtime.txt, amely meghatározza az python

Ez a helyzet, amikor egy speciális telepítési hajtja végre a fájlokat, valamint a további Python műveletek, például a szokásos szinkronizálás Python parancsfájl használandó:

- Virtuális környezet automatikus kezelése
- Mezőpont használatával requirements.txt szereplő csomagok telepítése
- A kijelölt Python verziójától függően megfelelő web.config létrehozása.
- Statikus fájlok Django alkalmazások összegyűjtése

Megadhatja, hogy az alapértelmezett telepítési lépéseket bizonyos aspektusait anélkül, hogy a parancsprogram testreszabása.

Ha meg szeretné ugorja át az összes Python speciális telepítési, a üres fájl hozhat létre:

    \.skipPythonDeployment

Ha szeretne Django alkalmazás statikus fájlokat a webhelycsoport hagyhat ki:

    \.skipDjango 

Telepítési pontosabban bírálja felül az alapértelmezett telepítési parancsfájlt: hozzon létre a következő fájlokat:

    \.deployment
    \deploy.cmd

A fájlok létrehozása az [Azure parancssor][] is használhatja.  Ez a parancs használata a project mappából:

    azure site deploymentscript --python

Ha ezeket a fájlokat nem léteznek, az Azure létrehoz egy ideiglenes telepítési parancsfájlt, és futtatása.  Megegyezik egy hoz létre, a paranccsal.

[Azure parancssor]: http://azure.microsoft.com/downloads/
