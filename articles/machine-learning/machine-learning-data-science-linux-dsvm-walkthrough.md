<properties 
    pageTitle="Adatok tudományos a Linux adatok tudományos virtuális gépen |} Microsoft Azure" 
    description="Hogyan lehet több általános adatok tudományos feladatok elvégzéséhez a Linux adatok tudományos virtuális." 
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Adatok tudományos a Linux adatok tudományos virtuális gépen

Ez a forgatókönyv bemutatja, hogyan néhány gyakori adatok tudományos feladatok elvégzéséhez a Linux adatok tudományos virtuális. A Linux adatok tudományos virtuális gép (DSVM) elérhető, amely az adatok analitikai és gépi tanulási a gyakran használt eszközök gyűjteménye előtelepített Azure virtuális gép képként. A fő szoftver összetevőket a [rendelkezést Linux adatok tudományos virtuális gép](machine-learning-data-science-linux-dsvm-intro.md) témakörben vannak részletezve. A virtuális kép megkönnyíti az első lépések az adatok tudományos módon perc alatt anélkül, hogy telepítse és állítsa be az egyes eszközökről egyenként. Egyszerűen méretezése felfelé a virtuális, ha szükséges, és állítsa le, amikor nincs használatban. Ez az erőforrás így rugalmas és a költség hatékony. 

A tudományos adatokkal kapcsolatos feladatok Ez a forgatókönyv kövesse az igazolni a [Csapat adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/)lépések. Ezt a folyamatot biztosít az adatok tudomány, amely lehetővé teszi az adatok tudósok fölé intelligens alkalmazások létrehozásába életciklusának hatékony együttműködés a csoportoknak a rendszeres megközelítés. Az adatok tudományos folyamat egy közelítéses keretet is biztosít egy személy által követett adatok tudományos.

Az útmutató a [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) adatkészlet elemzése azt. Ez egy olyan e-mailek levélszemét vagy a sonka (tehát, még nem levélszemét), megjelölt és meg az e-mailek tartalmának statisztikai adatokat is tartalmaz. A statisztikai adatokat tartalmazza a következő, de egy szakasz tárgyalja. 


## <a name="prerequisites"></a>Előfeltételek

Virtuális gép Linux adatok tudományos használata előtt az alábbiakat kell rendelkeznie:

- **Azure előfizetés**. Ha még nem rendelkezik egy, olvassa el a [ma az ingyenes Azure fiók létrehozása](https://azure.microsoft.com/free/)című témakört.
- [**Linux adatok tudományos virtuális**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). A kiépítési a virtuális további tudnivalókért lásd [a Linux adatok tudományos virtuális gép rendelkezni](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) telepítve van a számítógépén, és egy XFCE munkamenet megnyitni. Való telepítéséről és konfigurálásáról egy **X2Go ügyfél**szeretne olvassa el a [telepítése és beállítása X2Go ügyfél](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client)című témakört. 
- Egy **AzureML fiókot**. Ha még nem rendelkezik egy újat a [AzureML kezdőlapján](https://studio.azureml.net/)a regisztrálhat. Van egy ingyenes használatát réteg segítünk az első lépések.


## <a name="download-the-spambase-dataset"></a>Töltse le a spambase adatkészlet

A [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) adatkészlet egy olyan viszonylag kis csak 4601 példák adatait. Ez a kényelmes méretét, hogy néhány, ahogy az adatok tudományos virtuális főbb funkciói továbbra is a erőforrásigények mérsékelt igazoló használja.

>[AZURE.NOTE] Ez a forgatókönyv a D2 v2 méretű Linux adatok tudományos virtuális gép hozták létre. Ez a méret DSVM is alkalmas kezelése az Útmutató lépéseit.

Ha további tárterületet, további lemez létrehozása, és csatolja azokat a virtuális. Ezek a lemez használatával állandó Azure tárolására, hogy a adataik megmarad, akkor is, ha a kiszolgáló van reprovisioned miatt átméretezése vagy leáll. Adja hozzá a lemezen, és csatolja a virtuális, kövesse a [lemez egy Linux virtuális gép hozzáadása](../virtual-machines/virtual-machines-linux-add-disk.md). Ezeket a lépéseket az Azure parancssori kezelőfelületről Azure, amely már telepítve van a DSVM használja. Így ezeket a műveleteket történik teljes mértékben a virtuális magát. Egy másik tároló növelése érdekében, hogy [Azure-fájlok](../storage/storage-how-to-use-files-linux.md)használata.

Az adatok letöltéséhez Terminálszolgáltatások ablak megnyitása, és ez a parancs futtatásával:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

A letöltött fájl nincsenek fejlécsor, így érdemes hozzon létre egy másik fájlt tartalmazó élőfej. Ezzel a paranccsal hozzon létre egy fájlt a megfelelő fejlécek futtatása:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Ezután ÖSSZEFŰZ együtt a parancsot a két fájlokat:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Az adathalmaz minden egyes e-mailek statisztika különböző típusú foglalja magában: 

- Például az oszlopok ***word\_freq\_WORD*** szavakat az e-mailben, amelyek megegyeznek a *WORD*százalékos jelzik. Ha például ha *word\_freq\_győződjön* értéke 1, 1 %-át az e-mailt az összes szót volt, *Ellenőrizze*, majd. 
- Például az oszlopok ***karakter\_freq\_karakter*** az e-mailben minden karaktert, amelyekre *karakter*százalékos jelzik. 
- ***tőke\_futtatása\_hossza\_leghosszabb*** leghosszabb hossza nagybetűvel sorozatát. 
- ***tőke\_futtatása\_hossza\_átlagos*** nagybetűvel az összes sorozatok átlagos hossza. 
- ***tőke\_futtatása\_hossza\_teljes*** nagybetűvel az összes sorozatok összes hossza. 
- ***levélszemét*** azt jelzi, hogy az e-mailt volt a levélszemétnek ítélteket-e (1 = 0, a levélszemét nem levélszemétként =).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>A Microsoft R nyissa meg az adatkészlet feltárása

Vegyük vizsgálja meg az adatokat, és végezze el a néhány egyszerű gépi tanulási az r Az adatok tudományos virtuális megtalálható [A Microsoft R megnyitott](https://mran.revolutionanalytics.com/open/) előtelepítve. Az R verziójában többszálas matematikai tárak különféle egyetlen összefűzött verziók jobb teljesítményt nyújtanak. A Microsoft R megnyitott is tartalmaz reprodukálhatósági a CRAN csomag tárházba pillanatképét használatával.

Úgy juthat az mintakódok, használja az útmutató másolatait, klónozhatja az **Azure-gépi – oktatás – adatok-tudományos** tárházba segítségével mely számjegy, amely a virtuális előre telepítve. A mely számjegy parancssorból futtasson:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Nyisson meg egy Terminálszolgáltatások ablakot, és indítása új R az R interaktív konzolt.

>[AZURE.NOTE] Az alábbi eljárások az RStudio is használhatja. RStudio telepítéséhez a egy terminált parancs végrehajtása:`./Desktop/DSVM\ tools/installRStudio.sh`

Szeretné importálni az adatokat, és állítsa be a környezetet, futtassa:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Minden egyes oszlopát összefoglaló statisztikájának megtekintése:

    summary(data)

Az adatok másik megjelenítése:

    str(data)

Ez jeleníti meg, milyen mindegyik változó és az első néhány értékeket az adatkészlet. 

A *levélszemét* oszlop egy egész elolvasva, de valójában egy kockák változó (vagy tényező). A típus beállítása:

    data$spam <- as.factor(data$spam)

Néhány felderítő elemzés, használja a [ggplot2](http://ggplot2.org/) csomagot, a népszerű nyújtó grafikus tárban, amely már telepítve van a virtuális r. Vegye figyelembe az összesített adatok jelennek meg a korábbi verziójú összesítő statisztika felkínálunk felkiáltójel karakter gyakoriságát. Vegyük ábrázolni e gyakoriságok itt az alábbi parancsok:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Mivel a nulla sáv van döntés az ábra, nézzük volna meg:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Van egy nem trivial sűrűségfüggvény eredménye 1 érdekesnek tűnő felett. Tekintsük át a csak az adatokat:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Majd ossza levélszemét viewben sonka szerint:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Ezek a példák kell lehetővé teszik, hogy a hasonló felvételt a többi oszlop a bennük szereplő adatait.


## <a name="train-and-test-an-ml-model"></a>Képzése, és egy Machine Learning modell tesztelése

Most vegyük láthatja el ismeretekkel a gépi tanulási modellek osztályozásához az adathalmazban másként, lett létrehozva vagy a sonka tartalmazó e-mailt, néhány. Hogy láthatja el ismeretekkel a döntési fa modell és a véletlen erdő modell ebben a részben, és tesztelje a azok az előrejelzések pontosságának. 

>[AZURE.NOTE] A rpart (rekurzív szétválasztás és regressziós fák) csomag, amelyet a következő kódot már telepítve van a adatok tudományos virtuális.


Először nézzük ossza szét az adatkészlet képzés és próba beállítása:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

És hozza létre az e-mailt osztályozásához döntési fa.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Az alábbiakban az eredmény:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Annak megállapításához, hogy mennyire oktatás beállítása hajt végre, használja a következő kódot:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Annak megállapítása, hogy mennyire végrehajtott próba beállítása:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Lássunk még egy véletlen erdő modell. Véletlen erdők láthatja el ismeretekkel a döntési fák rengeteg, és a kimeneti, hogy az minden az egyes döntési fák a minősítések üzemmód osztály. Egy nagyobb teljesítményű gépi tanulási megközelítés, ahogy azok javítása, a döntési fa modell overfit oktatás adatkészletet szeretne tendencia figyelhető biztosítanak. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Azure Machine Learning rendszerbe állítják a modellt

[Azure gépi tanulási Studio](https://studio.azureml.net/) (AzureML) egy felhőalapú szolgáltatásba, amely megkönnyíti létre és helyezhetnek üzembe a cserélendő analytics modellek. AzureML szép funkciói egyik az azt jelenti, hogy az R funkció webes szolgáltatásként közzététele. A AzureML R csomag közvetlenül a DSVM az R munkamenetet az elvégzendő egyszerűen telepítési. 

A döntési fa kódot az előző szakaszétól üzembe helyezéséhez szeretne jelentkezzen be az Azure gépi tanulási Studio. A munkaterület-azonosító, és a egy jóváhagyási jogkivonat a sigh van szüksége. Ezek az értékek megkeresése és a velük AzureML változók inicializálni:

A bal oldali menüben válassza a **Beállítások** . Megjegyzés: a **MUNKATERÜLET azonosítója**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

A rezsiköltség menüből válassza az **Engedélyezés tokenek** , és jegyezze fel a **Elsődleges engedélyezési Token**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

A **AzureML** csomag betöltése, és adja meg az értékeket a változók jogkivonat, és a munkaterület-azonosítóval a R munkamenete a DSVM a:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Vegyük leegyszerűsíti a modellt, így ez a bemutató könnyebben végrehajtása. Válassza a három változóval a legfelső szintű legközelebb döntési fa, és hozza létre az új fa csak az adott három változók használata:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Szükséges, amely a szolgáltatások egy bemeneteként adja eredményül a becsült értékek előrejelzése függvény:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

A predictSpam függvény a **publishWebService** függvénnyel AzureML közzététele: 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Ez a funkció megnyitja a **predictSpam** függvény, **spamWebService** definiált ráfordítások és a kimeneti értékeket nevű webszolgáltatás hoz létre, és az új végpontot adatokat ad eredményül..

A közzétett webes szolgáltatás, többek között az API végpontját részletes adatainak megjelenítéséhez, és a hívóbetűk paranccsal:

    spamWebService[[2]]

Próbálja ki az első 10 sorok a próba beállítása:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Használhat más eszközöket érhető el

A hátralévő szakaszok bemutatják, hogyan használja az eszközök a Linux adatok tudományos virtuális telepítve. Az alábbiakban ismertetett eszközök listája:

- XGBoost
- Python
- Jupyterhub
- Rattle
- PostgreSQL & erdeimókus SQL
- SQL Server adatraktár


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) segédprogram gyors és pontos csillapítja fa valósítja.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost is felhívhatja python vagy a parancssor.

## <a name="python"></a>Python

Fejlesztési Python használ a Anaconda Python terjesztését 2.7 és 3.5-ös a DSVM a telepítve van. 

>[AZURE.NOTE] A Anaconda eloszlás [Condas](http://conda.pydata.org/docs/index.html), használt egyedi környezetek létrehozása, amelyek különböző verzióival, illetve azokat a telepített csomagok Python tartalmazza.

Vegyük egyes az spambase adatkészlet olvashatók, és az e-mailek osztályozásához támogatási vektoros gépekhez scikit a – Ismerje meg:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Az előrejelzések tétele:

    clf.predict(X.ix[0:20, :])

Szeretné megjeleníteni, hogy miként teheti közzé AzureML zárólap, akkor kezdje hosszúak egyszerűbb modell a három változóval azt jelent, ha a korábban közzétett azt az R modell. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

A modell AzureML közzététele:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] Ez a lehetőség csak a python 2.7, és a 3.5-ös még nem támogatott. Futtassa a **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

A Anaconda eloszlását a DSVM megtalálható Jupyter jegyzetfüzet, a platformok környezet Python, R vagy Ágnes kódot és elemzési megosztására. A Jupyter jegyzetfüzet JupyterHub keresztül érhető el. Jelentkezzen be a helyi Linux felhasználónevet és jelszót az Ön ***https://\<virtuális DNS-nevét és IP-cím\>: 8000 /***. JupyterHub az összes konfigurációs fájl címtár **/etc/jupyterhub**találhatók.

Néhány példa jegyzetfüzetek már telepítve vannak a virtuális:

- Lásd: a [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) egy minta Python jegyzetfüzet.
- Lásd: minta **R** Jegyzetfüzet [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) .
- Lásd: a [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) egy másik példa **Python** jegyzetfüzet.

>[AZURE.NOTE] A Ágnes nyelvet a Linux adatok tudományos virtuális a parancssorból is érhető el.


## <a name="rattle"></a>Rattle

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) az adatok adatbányászati grafikus R eszköz (az R analitikai eszköz kattintva megtudhatja, egyszerűen). Egyszerűen betöltése, és adatokat a és összeállítása és az adatmodellek felmérése intuitív felületet rendelkezik.  A cikk [Rattle: A adatok Mining grafikus r](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) , mely szemlélteti szolgáltatásai útmutató nyújt.

Telepítse és Rattle kezdje az alábbi parancsokat:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] A DSVM a telepítés nem szükséges. De Rattle kérhet további csomagokat telepítésekor betölti.

Rattle egy lap-alapú kapcsolatot használja. A lapokat a legtöbb felel meg az [Adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), például adatok betöltése, vagy azt felfedezése című témakör lépéseit. Az adatok tudományos folyamat szövegdobozról balról jobbra a lapokra. De az utolsó lapon látható a naplózási Rattle futtasson R parancsot. 


Betöltése, és állítsa be az adatkészlet:

- Ha szeretné tölteni a fájlt, jelölje be az **adatok** lap, majd 
- Válassza ki a **fájlnév** melletti sorkijelölőre, és válassza a **spambaseHeaders.data**. 
- A fájl betöltése. Jelölje ki a felső sor gombok **végrehajtás** . Ekkor megjelennek az egyes oszlopok, beleértve az azonosított adattípusát, hogy egy beviteli, a cél vagy más típusú változókban és egyedi értékek számát összefoglalását.
- Rattle a célként megfelelően észlelt a **levélszemét** oszlopban. Jelölje ki a levélszemét oszlopot, majd állítsa a **Cél adattípus** **Categoric**.

Az adatok vizsgálatára: 

- Kattintson a **Tallózás** fülre. 
- Kattintson **összefoglaló**, majd a **végrehajtás**, a változó típusú információkat és összefoglaló statisztikai adatokat. 
- Más típusú mindegyik változó statisztikájának megtekintéséhez válassza a további beállításokat, például **Adja meg** vagy **alapjai**.

A **Tallózás** lap is lehetővé teszi, hogy hány osztályon felvétel készítése. Egy hisztogramot az adatok ábrázolása:


- Jelölje ki a **Felosztás**.
- Jelölje be a **hisztogram** **word_freq_remove** és **word_freq_you**.
- Jelölje ki, **hajtsa végre**. Meg kell jelennie egy egyetlen graph ablakban, és hol található a világos, hogy a word "," sokkal több gyakran megjelenik az e-mailben, mint "eltávolítása" mindkét sűrűség felvétel.

A korrelációs felvétel is érdekes. Létrehozásához:


- Válassza a **korrelációs** **típusát**, majd 
- Jelölje ki, **hajtsa végre**. 
- Rattle figyelmeztetést jelenít meg, hogy legfeljebb 40 változót ajánl. Válassza az **Igen gombra** az ábra megtekintéséhez. 

Vannak bizonyos érdekes összefüggések, amely hárommal: "technológia" van erősen összefüggésbe "HP" és "labs", például. Akkor is nagyon összefüggésbe "650", mivel a körzetszámot, az adatkészlet adományozók 650.

Az Intéző-ablak az összefüggések szavak között a numerikus értékeket érhetők el. Érdemes a megjegyzést, például, hogy a "technológia" negatív összefüggésben, "a" és "pénzt" segítségével.

Rattle alakíthatják át az adatkészlet kezelje a gyakori problémákat. Ha például lehetővé teszi átméretezése funkciók, imputálására hiányzó értékek, kiugró kezelni, és távolíthat el változók és észrevételek hiányzó adatokkal. Rattle azonosítását társítási szabályok megfigyelések és/vagy változók között is. Ezen kívül alapján való bevezető útmutató alkalmazás.

Rattle fürt analysis is végezhet. Nézzük kizárni bizonyos szolgáltatások, így könnyebben olvassa el a kimenet. Az **adatok** lapon válassza ki a **figyelmen kívül hagyása** a változók, kivéve a következő tíz elem mindegyike mellett:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- Levélszemét

Ezután lépjen vissza az **fürt** lapján válassza a **KMeans**, és adja meg, *hány fürt* 4-es. Ezután **hajtsa végre**. Az eredmények megjelennek a kimeneti ablakban. Egy fürt "György" és "hp" max gyakorisága és valószínűleg a jogos üzleti e-mailt.

Egy egyszerű döntési fa gépi tanulási modell létrehozásához: 

- Jelölje ki a **modell** lap 
- Válassza a **fastruktúrájú** lehetőséget a **típus**. 
- Jelölje ki a **végrehajtás** szöveg űrlap a kimeneti ablakban a fastruktúra megjelenítéséhez. 
- A **Rajzolás** gombra kattintva grafikus verzió megtekintése. Ez így néz igazán a fa azt kapott *rpart*korábbi használatával.

Rattle szép funkciói egyik az azt jelenti, hogy több gépi tanulási módszerek futtatása és a gyors kiértékelésében. Az alábbiakban a lépéseket:

- Válassza az **összes** **típusát**. 
- Jelölje ki, **hajtsa végre**. 
- Befejezése után kattintson bármelyik egyetlen **típus**, például **SVM**, és tekintse meg az eredményeket. 
- Kattintson az érvényesítés meg a **kiértékelés** lap modellek a teljesítmény is hasonlíthatja. Például a **Hiba mátrix** kijelölés fejetlenséget mátrix, általános hiba, és láthatók átlagolt osztály hiba, az egyes modell érvényesítése beállítása. 
- ROC görbék ábrázolni, magánjellegű elemzést végezhet, és végezze el az egyéb típusú modell értékelést.

Miután végzett modellek összeállítását, jelölje ki a **napló** lapján megtekintheti a R kód futtatásához Rattle a munkamenet közben. Kijelölhet mentse az **Exportálás** gombra. 

>[AZURE.NOTE] Van egy hiba Rattle a jelenlegi kiadásába. Módosítsa a parancsfájlt, vagy ismételje meg a később használatával, a szövegben a napló kell *a napló... exportálása* elé # karakter beszúrása. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & erdeimókus SQL

A DSVM telepített PostgreSQL megtalálható. PostgreSQL az összetett, a Megnyitás-forrás relációs adatbázisból. Ebben a szakaszban a levélszemét adatkészlet betöltése PostgreSQL, és kattintson a lekérdezés mutatja.

Adatok betöltése, előtt engedélyeznie kell a localhost jelszó-hitelesítést. A parancssorablakban:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Alsó részén a konfigurációs fájl több sort, amely az engedélyezett kapcsolatok részletesen a következők:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

A "IPv4-helyi kapcsolat" vonal md5 helyett ident, így azt bejelentkezni felhasználónevével és jelszavával módosítása:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

És indítsa újra a postgres szolgáltatást:

    sudo systemctl restart postgresql

Psql elindítására egy interaktív Terminálszolgáltatások PostgreSQL, a beépített postgres felhasználóként a parancssorból futtassa a következő parancsot:

    sudo -u postgres psql

Hozzon létre egy új felhasználói fiókot, amely segítségével az ugyanazon felhasználónév van jelenleg bejelentkezve, és adjon meg egy jelszót a Linux fiókként:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Jelentkezzen be psql a felhasználók:

    psql

És az adatok importálása az új adatbázis esetén:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Most vizsgáljuk meg az adatokat, és az adatbázisok via JDBC-illesztőprogram használata bizonyos lekérdezések **Erdeimókus SQL**, grafikus eszköz, amellyel futtatni.

Első lépésként nyissa meg a erdeimókus SQL lévő az alkalmazások menü. A vezető beállítása:

- Jelölje be a **Windows**, majd a **Nézet illesztőprogramok**. 
- Kattintson a jobb gombbal a **PostgreSQL** , és válassza a **Illesztőprogram módosítása**. 
- Jelölje ki a **felesleges Osztályjegyzetfüzet elérési útját**, majd **adja hozzá**. 
- A **fájl nevét** adja meg a ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** és 
- Kattintson a **Megnyitás**.
- Válassza a lista-illesztőprogramok, majd válassza a **org.postgresql.Driver** **Osztálynév**, és kattintson **az OK gombra**.

A helyi kiszolgálóhoz való csatlakozás beállítása:
 
- Jelölje be a **Windows**, majd **megtekintése aliasok.** 
- Válassza a **+** , hogy új aliast gombra. 
- Nevezze el *a levélszemét-adatbázist*, válassza a **PostgreSQL** **illesztőprogram** legördülő.
- Állítsa az URL-cím *jdbc:postgresql://localhost/spam*. 
- Írja be a *felhasználónevét* és *jelszavát*. 
- Kattintson az **OK gombra**. 
- Nyissa meg a **kapcsolat** ablakban, kattintson duplán a ***levélszemét adatbázis*** alias. 
- Jelölje be a **Kapcsolódás**.

Bizonyos lekérdezések futtatása:

- Jelölje ki az **SQL** -lapon.
- Írja be például egy egyszerű lekérdezés `SELECT * from data;` a lekérdezés rendelés_részletei.mennyiség az SQL-lap tetején. 
- Nyomja le a **Ctrl + Enter** futtatni. Alapértelmezés szerint erdeimókus SQL a lekérdezésből az első 100 sorát adja vissza. 

Vannak-e adatait esetleges számos további lekérdezések. Például hogyan *Ellenőrizze* a word gyakoriságának különbözik a levélszemét és sonka között?

    SELECT avg(word_freq_make), spam from data group by spam;

És Mik azok a jellemzőit *3d*gyakran tartalmazó e-mailek?

    SELECT * from data order by word_freq_3d desc;

A legtöbb e-maileket, amelyek egy nagy előfordulás *3D* látszólag spam, így lehet hasznos funkciója az e-mailek osztályozásához prediktív modell készítéséhez.

Ha a PostgreSQL-adatbázisban tárolt adatokkal gépi tanulási végrehajtásához megy végbe, fontolja meg [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server adatraktár

Azure SQL-adatraktár alkalmas nagy mennyiségű relációs és a nem relációs adatok feldolgozásának felhőalapú, méretezési adatbázis. További tudnivalókért lásd: [Mi az Azure SQL-adatraktár?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

A adatraktár kapcsolódni, és a tábla létrehozása, futtassa a parancssorból a következő parancsot:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Kattintson a sqlcmd parancssorba:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Másolja az adatokat tartalmazó bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] A letöltött fájl a sorvégződések Windows stílusú, de bcp UNIX stílusú vár, hogy szükség van bcp, amely a - r jelölővel.

És a lekérdezés, amelynek sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Tudta is lekérdezés SQL erdeimókus. Hajtsa végre az PostgreSQL, használja a Microsoft MSSQL kiszolgáló JDBC illesztőprogramját, amely megtalálható a ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***hasonló lépéseket.

## <a name="next-steps"></a>Következő lépések

Témakörök, amelyek végigvezetik Önt a feladatokat foglalja az Azure-ban az adatok tudományos folyamat áttekintése látható [Csapat tudományos adatfeldolgozás](http://aka.ms/datascienceprocess).

Más végpontok közötti forgatókönyvek, amelyek bemutatják a lépéseket a csapat adatok tudományos folyamat a különböző forgatókönyvekben leírását olvassa el a [csapat adatok tudományos folyamatok bemutatása](data-science-process-walkthroughs.md)című témakört. A forgatókönyvek is bemutatják, hogyan lehet a felhőben, és a helyszíni eszközök és szolgáltatások egyesítése egy munkafolyamat vagy a folyamat hozhat létre új intelligens alkalmazást.

