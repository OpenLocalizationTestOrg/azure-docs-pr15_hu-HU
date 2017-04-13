<properties
    pageTitle="Épület adatmodell Recommnendations felhasználói felülettel |} Microsoft Azure"
    description="Azure gépi tanulási javaslatok - épület adatmodell javaslatok felhasználói felülettel"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Javaslatok felhasználói felülettel adatmodell létrehozása

A dokumentum részletes útmutatásra. A célja, hogy végigvezetik Önt a [Javaslatok felhasználói felület](https://recommendations-portal.azurewebsites.net/)használatával modell képzése szükséges lépéseket.
Gyakorlásához keresztül lehetővé teszi a folyamat épület adatmodell, előtt programozás útján tegye megértése. Azt is familiarizes, felhasználói felülettel, amely olyan hasznos, a szolgáltatás használatba vételekor.

A gyakorlat körülbelül 30 percet vesz igénybe.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Lépés: 1 – bejelentkezési a felhasználói felület ajánlások ##

1. Ha még nem tette meg, akkor kell [előfizetési](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) egy új [Javaslatok API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) -előfizetés. Iratkozzon fel a **Azure** Service [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) , és jelentkezzen be a Azure-fiókjával. Részletes útmutatást a bejelentkezési folyamatot pontjait *tevékenység 1* [– Első lépések](cognitive-services-recommendations-quick-start.md) 

1. Miután beszerzett egy **kulcsot** javaslatok API-előfizetéshez tartozó, ugorjon a [Javaslatok felhasználói felület](https://recommendations-portal.azurewebsites.net/). 

1. Jelentkezzen be a portálon írjon be a termékkulcsot a **Fiókkulcs** mezőbe, és kattintson a **Bejelentkezés** gombra.

    ![Felhasználói felület javaslatok: Bejelentkezési párbeszédpanelt.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Lépés: 2 – oktatás adatokat vegyük gyűjtése ##

Építés hozhat létre, mielőtt a motor van szüksége a két csatolt információk: a katalógus fájl- és fájlokat használatát. 

A katalógus fájlt, az ügyfélnek vannak kínálnak cikkekre vonatkozó információkat tartalmazza. Használatát fájl ezeket az elemeket használata vagy a tranzakciók az üzleti adatokat tartalmazza.

Általában a az alábbi információkat vehet tároló adatbázisban volna lekérdezés. Ma nyújtott mintaadatokkal meg, hogy a javaslatok API használata talál.

Az adatok letölthető [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Másolja a vágólapra, és csomagolja ki az **Books.Zip** fájl egy mappába a helyi számítógépre. Ha például **c:\data**.

A katalógus és használati fájlokat a séma részletes tájékoztatást [a modell betanítása Collecting adatok](cognitive-services-recommendations-collecting-data.md) című témakörben találhatók.
 
Ebben a gyakorlatban fogjuk használata az, hogy nem kell nagyon megvárja, amíg túl sokáig oktatás egy kis fájlt. Ha meg szeretné több reálisan fájl, azt is elhelyezte **MsStoreData.zip** , amely tartalmazza az [adott helyen](http://aka.ms/RecoSampleData)a Microsoft Store minta tranzakciók.

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Lépés a 3 - projekt létrehozása, és töltse fel a katalógus és használati adatainak ##

A [Javaslatok felhasználói felület](https://recommendations-portal.azurewebsites.net/)való bejelentkezés után megjelenik a projektek lap. Ha a korábban létrehozott minden olyan projektek, itt kell megjelennek.
Projekt (más néven *egy a modellben* az API-hivatkozások) az katalógus és használati adatainak tároló. Több *hozza létre* a Project hozhat létre. Fogja ismerni azon a folyamaton, a következő lépésekben.

1. Új projekt létrehozása, írja be a nevét a szövegmezőbe (valami hasonló működő "MyFirstModel"), és kattintson a **Projekt hozzáadása**gombra.
 
    ![Felhasználói felület javaslatok: Projektek lapon.][reco_projects]

1. A projekt kapja a létrehozása után a **Tallózás** gombra a fájl **hozzáadása katalógus fájl** szakasznál. Töltse fel a katalógus szerezte be a 2. Ha már mentette a *c:\data*, akkor keresse meg azt a mappát.

    ![Felhasználói felület javaslatok: Adatok felvétele a projekt.][reco_firstmodel]

1. A katalógus feltöltése, után a **Tallózás** gombra a fájl **Használatát fájlok hozzáadása** szakasznál. Adja hozzá a usage_large.txt fájlt.

> **Mi történik, ha van egy nagyméretű fájlt használati adatok?**
>
> 200-MB-nál kisebb csak használatát fájlokat tölthet fel. Említett, a rendszer is tartsa lenyomva az ujját 2 GB-os értékű tranzakció adatokat, hogy úgy is feltöltheti egynél több fájlt, ha szükséges.
> Nem előfordulhat, hogy mennyi adatot a hatékony adatmodell létrehozása, nem csak a keresni az épp szükséges adatokat, de az adatok minőségének méretét. Érdemes a közös, ahol a tranzakciók többsége csak a legnépszerűbb elemek néhány, és nincs "kissé jel" egyéb elemek használati adatok megtekintéséhez.

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Lépés: 4 – vegyük végezze el az egyes oktatás! ##

Most, hogy a katalógus, mind a használati adatok feltöltött azt, hogy azt is megtudhatja, az adatokból mintázatok, a program betanítása készen áll.

1.  Kattintson az **Új Szerkesztés** gombra.

1.  Jelölje ki a **javaslatok** a Szerkesztés típusa. Látható, hogy támogatjuk rangsorolási összeállítása egy FBT (gyakran vásárolt együtt), valamint típusok összeállítása.

    ![Felhasználói felület javaslatok: Összeállítása párbeszédpanelen.][reco_build_dialog.png]


    Egy FBT build lehetővé teszi, amelyek általában vásárolt/felhasznált ugyanazon tranzakció termékek azonosításához.
    Rangsorolási építés azonosítja az érdeklődésre számot tartó funkciók használják. 
    Nagyon mély FBT be nem megyünk vagy rangsorolási hozza létre az e workshop, de ha érdeklik kell érdemes [típusok és a modell minőség dokumentáció lap összeállítása](cognitive-services-recommendations-buildtypes.md).

1. Kattintson a **Szerkesztés** gombra. A Szerkesztés folyamat körülbelül öt percig tart, ha használja az könyvek adatokat. Hosszabb ideig tart a nagyobb adatkészleteket.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Lépés az 5 - nézzük meg, hogy a gép tanultakhoz! ##

A Szerkesztés befejezése után, láthatja, egy új build buildjeiben listában. A Szerkesztés elemre, és a felhasználó ajánlások lekérdezhető.

1. A Szerkesztés befejezése után kattintson az **eredmény**. Így játszhat le, és a modellt, és milyen elemek ajánlott.

    ![Felhasználói felület javaslatok: Pontszámhoz gomb][reco_score_button]

1. Válasszon egy elemet az adott elem javaslatok a visszaadott elemek megtekintéséhez. Figyelje meg, ha nem elég tranzakciók előrejelzésére egy adott tételt ajánlást, a rendszer nem adja vissza az adott elem ajánlások.  Ha valamiért nem javaslatok visszaadó elem, próbálja meg más elemek pontozási.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Lépés a 6 - lépések ##
Gratulálok! modell képzett van, és kattintson a modell javaslatok kapott.  A javaslatok felhasználói felületének rendkívül hasznos eszköz, amely lehetővé teszi a jelenítheti meg a projekt állapotát, és hozza létre. 

Most, hogy van egy adatmodellt, előfordulhat, hogy kívánt megtudhatja, hogy miként programozás útján végezze el a fenti lépéseket. Ahhoz, hogy megtudhatja, hogy miként programozás útján hívja az API-t, azt felkérést, hogy ellenőrizze a [Javaslatok API-hivatkozás](http://go.microsoft.com/fwlink/?LinkId=759348) , és töltse le a [Javaslatok minta alkalmazás](http://go.microsoft.com/fwlink/?LinkID=759344).


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
