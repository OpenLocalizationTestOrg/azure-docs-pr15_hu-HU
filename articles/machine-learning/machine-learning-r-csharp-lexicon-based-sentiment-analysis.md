<properties 
    pageTitle="Eljárásán alapú Sentiment Analysis |} Microsoft Azure" 
    description="Eljárásán alapú Sentiment elemzése" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Eljárásán alapú Sentiment elemzése 

Hogyan is, a felhasználók vélemények és mértékegysége szokások felé márka vagy témakörök online közösségi hálózatok, például Facebook közzétesz, twitterre, véleményezések stb.? Sentiment analysis ilyen kérdéseket elemzéséhez módszert kínál.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Kétféleképp általában sentiment elemzéshez. Egy felügyelt tanuló algoritmus sem használja, és a többi felügyelet tanulási lehet tekinteni. A felügyelt tanulási algoritmus egy nagy jelekkel ellátott corpus általában épül besorolás modell. Pontosságát főként alapján a széljegyzet minőségének, és általában a képzési folyamat hosszú időt vesz igénybe. Amellett, hogy a algoritmus alkalmazása azt egy másik tartományt, amikor a eredménye általában nem jó. Felügyelt tanulási, eljárásán felügyelet tanulási felhasználási sentiment szótár, amely egy nagy adatok corpus tárolásához és képzési használatához nincs szükség összehasonlítva – a teljes folyamat sokkal gyorsabb, ezáltal. 

A [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) a MPQA-szubjektivitás eljárásán (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), amely a leggyakrabban használt szubjektivitás lexikonok egyike épül. MPQA 5097 2533, negatív és pozitív szavak szerepelnek. És az összes a következő szavak vonatkozóan kiegészítések értelmében erős vagy gyenge polaritás szerint. A teljes corpus GNU általános nyilvános licenc csoportban található. A webszolgáltatás minden rövid mondatot, például twitterre és a Facebook-bejegyzések alkalmazhatók. 

>Ez a webszolgáltatás sikerült által igénybe vehető felhasználók – esetleg mobilalkalmazásban, egy webhelyen keresztül, vagy akár a helyi számítógépre – például. De a webszolgáltatás célja is, hogyan Azure gépi tanulási webszolgáltatásokhoz R meg felvételével létrehozásához használható példaként szolgálnak. R néhány kódsorokat és Azure gépi tanulási Studio gomb gombra kattint egy kísérlet R kóddal létrehozható és közzétett webes szolgáltatásként. A webszolgáltatás majd a Microsoft Azure piactéren közzétett és felhasználó és eszköz elfogyasztott világszerte a szerző a webszolgáltatás infrastruktúra beállítás nélkül.

##<a name="consumption-of-web-service"></a>Webszolgáltatás felhasználása

A bemeneti adatok lehet a szöveget, de a webszolgáltatás jobban működik-e a rövid mondatok. A kimenet numerikus értéke 1 és 1 közötti. Bármilyen érték alatti 0 azt jelzi, hogy a szöveg a sentiment negatív; Ha a fenti 0 pozitív. Az eredmény abszolút értékét azt jelzi, hogy a társított sentiment erőssége. 

>Ez a szolgáltatás, az Azure piactéren elérhető is egy OData-szolgáltatás; Ezek a előfordulhat, hogy keresztül POST vagy GET módszerek neve. 

Az automatikus módon szolgáltatás használata más több módja létezik (például alkalmazás van [Itt](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>C# kód web service fogyasztási kezdési:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



A bemeneti érték "ma jó nap." A kimenet "1", a bemeneti mondat társított egy pozitív sentiment jelző. 

##<a name="creation-of-web-service"></a>A webszolgáltatás létrehozása
>Ez a webszolgáltatás készült Azure gépi tanulási használatával. Az ingyenes próbaverzióra, valamint bevezető szintű videoklipek kísérletek és a [közzétételi webszolgáltatások](machine-learning-publish-a-machine-learning-web-service.md)létrehozásával kapcsolatban olvassa el [azure.com/ml](http://azure.com/ml). Az alábbi van a kísérlet a webes szolgáltatás és példa-kódot a kísérlet moduljainak minden egyes létrehozott látható.


Az Azure gépi tanulási, belül egy új üres kísérlet hozták létre. Az alábbi ábrán a kísérlet folyamat eljárásán-alapú sentiment elemzését. A "sent_dict.csv" fájlt a MPQA szubjektivitás eljárásán, és van beállítva, mint a bemeneti adatok [R]parancsfájl végrehajtása[execute-r-script]. Egy másik bemeneti érték az Amazon Véleményezés adatkészlet vizsgálathoz, ahol azt kijelölés, akkor oszlop neve módosítását, elvégzett, és ossza szét a műveleteket a mintában szereplő ismerkedést. A szubjektivitás eljárásán mentheti a memóriában, és a pontszámhoz számítási folyamat felgyorsítsa használjuk kivonat csomagot. A teljes szöveget fog tokenekre bontott "tm" csomag, és a sentiment szótár a szó képest. Végül egy pontszám alapján számolja a szöveg minden szubjektív szó vastagságának hozzáadásával. 

###<a name="experiment-flow"></a>Kísérlet folyamat:

![kísérletezés továbbításához][2]


####<a name="module-1"></a>A modul 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Kivonat csomag telepítéséhez
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Korlátozások

A egy algoritmus szempontjából eljárásán-alapú sentiment analysis egy általános sentiment analysis eszközt, amely nem végezhetnek jobban, mint az osztályozási módszer az adott mezőkhöz. A Ellentett képzése probléma nem is foglalkozik. Azt a program a szavak több Ellentett képzése megoldás, de hatékonyabb módszerre van egy Ellentett képzése szótár és hozhat létre néhány szabály. A webszolgáltatás végez jobban rövid és egyszerű mondatok, például twitterre és a Facebook-bejegyzéseket, mint a hosszú és bonyolult mondatok, például az Amazon véleményezések. 

##<a name="faq"></a>GYAKORI KÉRDÉSEK
A webszolgáltatás vagy a közzététel az Azure Piactérhez felhasználási a gyakori kérdések című [alábbi](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
