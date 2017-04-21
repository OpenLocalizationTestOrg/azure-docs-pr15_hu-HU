# <a name="pull-request-etiquette-and-best-practices-for-microsoft-contributors-to-azure-documentation"></a>Húzza a kérelem etikett és a gyakorlati tanácsok Microsoft munkatársak Azure dokumentációjában olvasható az

Dokumentáció módosításainak közzététele, elküldését ki kérelmek a elágazás a. Minden ki kérelem előtt egyesítendő váró tartalmaz. Ebből a cikkből megértéséhez, hogy hogyan együtt kell működniük ki kérelem véleményezők, és hogyan hozhat létre kérések ki, hogy könnyebben és gyorsabban tekintse át, így mindenki számára jobban működik a leküldéses kérés várakozási sorban található.

## <a name="working-with-pull-request-reviewers"></a>Leküldéses kérelem véleményezők használata

Az alábbiakban kell tudnia ki kérelem véleményezők használatával kapcsolatos alapvető. 

- <b>Ismerje meg a szerepkör a leküldéses kérelem véleményező monogramjára cseréli. Véleményező:</b>
  - A tartalom egyszerű minőségének köszönhetően
  - Megakadályozza, hogy a tárban tárolnak hátrányait
  - Visszajelzés biztosít az egyesítés előtt

  Leküldéses kérelem véleményezők tartalom irányítási szerepkörbe szerepelnek. Az elsődleges célja nem egyszerűen egyesítése a lehető legrövidebb elküldése függetlenül. A várt, hogy végezze el a módosításokat, különösen olyan új és erősen javított cikkek igénylő visszajelzést.

- <b>Tervezés és a leküldéses kérelem véleményező:</b>
  - Fontosként ki kérések
  - Leküldéses kérelem időtúllépés és keltezés verziókban
  - Módosíthatja, vagy adjon hozzá sok fájlok ki összehívások

- <b>A leküldéses SLA kérése áttekintése</b>

  A magánjellegű adattárban minden alkalommal, amikor a leküldéses kérelem beírja a leküldéses kérés várakozási sorban található kész-egyesítés címkével a csapat próbálja olvassa el a leküldéses kérelem 12 üzleti órában (M-F, 8 AM az 5 PM), és visszajelzést, vagy egyesítése, ha nincs visszajelzés nem szükséges. E szolgáltatásiszint-szerződés az act véleményezési az ár, az egyesítés nem vonatkozik. Ha megfelelnek [űrlapegyesítéshez](contributor-guide-pr-criteria.md)PRs egyesítve lesznek. 

## <a name="make-the-pull-request-queue-work-better-for-everyone"></a>Ellenőrizze a leküldéses kérés várólista mindenki számára jobban tudjon dolgozni

Az ár várólista két alapvető realitásokat van:

- Leküldéses kérések kicsik, hatókörét és nagyon hasonlít a változásokat tartalmazó áttekintése kevesebb időt vesz. 
- Leküldéses kérelmeket, amelyek nagy hatókör és más, vegyes típusú változásokat tartalmazó a, tekintse át a több időt vesz igénybe.

Azt is megtudhatja, hogy a leküldéses kérés várólista jobban tudjon dolgozni, kövesse az alábbi ajánlott eljárást:

- Külön kisebb érkező frissítések a meglévő cikkekre mutató fő felülírja vagy új cikkek. A módosítások külön munka használja a munkát. 

- Ha törli a cikkek vagy a képeket, az új tartalom kiegészítéseinek és frissítések ne keverje a törölt elemek. A módosítások új tartalom egy külön munka ág kezelni.

- Verziókban vagy a tartalom újrabontása tervezés, az ár véleményező monogramjára cseréli. Előfordulhat, hogy hozzon létre egy megjelenés ág vagy koordinálása egyesítés időpontok közzétételi időpontokat, hogy a tartalom közzétételének a megfelelő időben partnerlistájukra segítséget.

- A változások az azure-tartalom-ár adattárban készít a ACOM repó (azaz bal oldali navigációs sávban jelenjenek meg a lapok, átirányítások vagy tanulási térképek módosításai) készült frissítéseket koordinálása próbál, ha időszakokra használható kell segítik az ár véleményező monogramjára cseréli. Egyéb esetben kockázat, a sérült hivatkozások sok problémákat.

## <a name="criteria-for-expedited-pull-requests"></a>Sürgős ki kérések feltételei

- Lépjen kapcsolatba a azdocprs való felgyorsításához PRs csak akkor, ha feltétlenül szükséges. Kérhet küldött kezelésére szolgáló piros zónában, a adatvédelmi, a jogi és a biztonsági problémákról; ár valódi törött felhasználói élmény; és a vezetői olyan. 
- Tartalom szolgáltatás verziókban nem felel meg gyorsított kezelése - szolgáltatás megjelenés tartalomhoz előzetes tervezési, vagy azt a normál prioritású várólista kell kezelni.


## <a name="in-a-hurry-submit-prs-that-can-be-accepted-automatically"></a>Egy siet? PRs, amely automatikusan elfogadja elküldése

Segítségével a PRMerger automatizálási szabályokat több egyesített automatikusan a napi szintű PRs.

PRMerger Ha automatikusan elfogadhatja az ár:
* 10 fájlokat vagy kevesebb hatással van.
* 15 véglegesítése vagy kevesebb tartalmaz.
* Kisebb, mint 20 %-a szöveget módosíthatja.
* Nem frissülnek a választók/switchers.
* Nincs a fájlok törlése vagy hozzáadott.
* Nincsenek képek újak, módosítása vagy törlése.

Leküldéses kérését nem felel meg az alábbi feltételek, ha a "PRmerger egyesítése nem" címke automatikusan így tudja, hogy Véleményezés igényel egy emberi ár véleményező.

### <a name="need-to-make-a-lot-of-little-changes"></a>Segítségre van szüksége, hogy sok kis módosítások?

A fenti PRMerger automatizálási szabályok a köteg készíthet, és tegye a következőket:
* Az egyszerűsített változásai együtt egy ár cikkek elküldése legfeljebb 10 fájlok.
* Hozzon létre egy külön ár cikkek mely képeket vagy választók módosítása. Ehhez az emberi áttekintése.
* Hozzon létre egy új vagy törölt cikkek külön ár. Ehhez az emberi áttekintése.

## <a name="related"></a>Kapcsolódó

- [Tekintse át a minőség feltétel ki kérés](contributor-guide-pr-criteria.md)

- [Húzza a kérelem Megjegyzés automatizálási](contributor-guide-pull-request-comments.md)
