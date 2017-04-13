<properties 
    pageTitle="Az Azure gépi tanulási túlélési Analysis |} Microsoft Azure" 
    description="Túlélési Analysis esemény előfordulása valószínűség" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Túlélési elemzése 

Sok esetben a fő a felmérés eredménye az érdeklődésre számot tartó esemény ideje. Más szóval a kérdés "Ha ez az esemény akkor fordul elő?" meg kell adnia. Példák, fontolja meg a azokról a helyzetekről, ahol a az adatok ismerteti az eltelt idő (nap, év, fogyasztási, stb.) addig, amíg az esemény (betegség relapse, doktori fokos kapott, berendezés billentyűivel hiba) a vizsgált fordul elő. Az adatokat az egyes példányok jelöl egy adott objektum (egy türelemmel, student, egy autós stb.).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

A [webszolgáltatás]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) választ ad a rendszer rákérdez, "Mi az a valószínűségét, hogy az érdeklődésre számot tartó esemény akkor fordul elő, az objektum x? idő n" Ez a webszolgáltatás túlélési elemzése adatmodell megadásával lehetővé teszi a felhasználóknak adja meg a modell képzése és tesztelje az adatokat. A fő témát a kísérlet az eltelt idő hosszának modell, mindaddig, amíg az érdeklődésre számot tartó esemény. 

>Ez a webszolgáltatás sikerült által igénybe vehető felhasználók – esetleg keresztül mobilalkalmazásban, egy webhelyen keresztül, vagy akár a helyi számítógépre, például. De a webszolgáltatás célja is, hogyan Azure gépi tanulási webszolgáltatásokhoz R meg felvételével létrehozásához használható példaként szolgálnak. R néhány kódsorokat és Azure gépi tanulási Studio gomb gombra kattint egy kísérlet R kóddal létrehozható és közzétett webes szolgáltatásként. A webszolgáltatás majd a Microsoft Azure piactéren közzétett és felhasználó és eszköz elfogyasztott világszerte a szerző a webszolgáltatás infrastruktúra beállítás nélkül.  

##<a name="consumption-of-web-service"></a>Webszolgáltatás felhasználása

A bemeneti adatok séma a webszolgáltatás az alábbi táblázatban látható. Hat információkat vehet van szükség, mint a bemeneti: képzés az adatok, adatok tesztelés, majd a fontos, az index "idő" idő dimenzió, az index és a "esemény" dimenzió változó fájltípusok (folyamatos vagy tényező). Az oktatóanyag adatok karakterlánccal, ahol az vesszővel elválasztott a sorok és oszlopok pontosvesszővel elválasztott jeleníti meg. A szolgáltatások az adatok egy rugalmas. A bemeneti karakterlánc összes elemének numerikus kell lennie. Az oktatóanyag adatok a "idő" dimenzió azt jelzi, addig, amíg az esemény (a türelemmel kábítószerrel használatát, a diák beszerzése a doktori mértékben a berendezés billentyűivel hiba problémákat autós Visszatérés a vizsgált a vizsgálat (türelemmel kábítószerrel kezelés programok, a diák kezdő doktori tanulmány, vezeti, stb kezdési autót fogadása) kiindulási pontként eltelt idő mennyiségek (nap, év, fogyasztási, stb.) stb.) akkor fordul elő. A "esemény" dimenzió azt jelzi, hogy az érdeklődésre számot tartó esemény a vizsgálat végén. Egy értéket a "esemény = 1" azt jelenti, hogy az érdeklődésre számot tartó esemény; "idő" dimenzió jelölt időben "esemény = 0" azt jelenti, hogy az érdeklődésre számot tartó esemény nem történt a "idő" dimenzió jelölt ideje szerint.

- trainingdata - karakter karakterlánc. Vesszővel elválasztott sorok és oszlopok pontosvesszővel elválasztott. Minden egyes sorára dimenzió "idő", "esemény" dimenzió és előrejelző változót tartalmazza.
- testingdata – egy sornyi adatot, amely tartalmazza az adott objektum előrejelző változói.
- time_of_interest – az eltelt idő kamat n.
- index_time – a "idő" dimenzió (1-től) az oszlopindex.
- index_event – a "esemény" dimenzió (1-től) az oszlopindex.
- variable_types – akkor elválasztóként pontosvesszővel karaktereit. 0 folyamatos változók pedig 1 tényező változók.


A kimenet egy adott időpontig előforduló esemény valószínűsége. 

>Ez a szolgáltatás, az Azure piactéren elérhető is egy OData-szolgáltatás; Ezek a előfordulhat, hogy keresztül POST vagy GET módszerek neve. 

Az automatikus módon szolgáltatás használata más több módja létezik (például alkalmazás van [Itt](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C# kód web service fogyasztási kezdési:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




A tesztet értelmezését a következőképpen történik. Feltételezve, hogy az adatok a cél az eltelt idő modell addig, amíg a kábítószerrel használatát Visszatérés a két kezelés programok kapó betegek számára is. A kimenet: a webes szolgáltatás Olvasás: betegek 35 évesnek alatt, a problémát az előző kábítószer-kezelés 2 és, a hosszú helyi kezelés program véve és heroin és a kokain használatát, és visszatérés a kábítószerrel használatát az valószínűsége 95.64 %-a nap 500.

##<a name="creation-of-web-service"></a>A webszolgáltatás létrehozása

>Ez a webszolgáltatás készült Azure gépi tanulási használatával. Az ingyenes próbaverzióra, valamint bevezető szintű videoklipek kísérletek és a [közzétételi webszolgáltatások](machine-learning-publish-a-machine-learning-web-service.md)létrehozásával kapcsolatban olvassa el [azure.com/ml](http://azure.com/ml). Az alábbi van a kísérlet a webes szolgáltatás és példa-kódot a kísérlet moduljainak minden egyes létrehozott látható.

Azure gépi tanulási, belül egy új üres kísérlet készült és két [R parancsfájl végrehajtása] [ execute-r-script] modulok lekért alakzatot a munkaterületet. Az adatok séma egy egyszerű [Végrehajtása R parancsfájl]készült[execute-r-script], amely meghatározza, hogy a bemeneti adatok séma esetében a webszolgáltatás. Ez a modul majd csatolva van a második [R parancsfájl végrehajtása] [ execute-r-script] modul, amely jelentős munkát. Ez a modul adatok előfeldolgozása, épület adatmodell és az előrejelzések tartalmaz. Az adatok előfeldolgozási lépésben a bemeneti adatok – olyan hosszú karakterlánc átalakulnak, és adatok keret konvertálja. Az épület adatmodell lépésben egy külső "survival_2.37-7.zip" R csomag először telepítette túlélési elemzés elvégzéséhez. Kattintson a "coxph" függvény végrehajtása után egy adatsor adatkezelési feladatok. A "coxph" függvény túlélési elemzéshez részleteit az R dokumentációjában olvasható. Az előrejelzési lépésben a tesztelés példány adja meg a "surfit" függvénnyel képzett modellbe, és a tesztelés példány túlélési görbét készül, mint "görbe" változó. Végül a valószínűség az érdeklődésre számot tartó idő kapott. 

###<a name="experiment-flow"></a>Kísérlet folyamat:

![kísérletezés továbbításához][1]

####<a name="module-1"></a>A modul 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>A modul 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Korlátozások

Ez a webszolgáltatás szolgáltatás változók (oszlopokat) csak numerikus értékeket vehet igénybe. A "esemény" oszlopban csak értéke 0 vagy 1 is tarthat. A "idő" oszlopban kell lennie egy pozitív egész szám.

##<a name="faq"></a>GYAKORI KÉRDÉSEK
A webszolgáltatás vagy a közzététel az Azure Piactérhez felhasználási a gyakori kérdések című [alábbi](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
