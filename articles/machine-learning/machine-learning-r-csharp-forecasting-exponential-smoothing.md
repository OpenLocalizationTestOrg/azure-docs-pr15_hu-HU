<properties 
    pageTitle="Előrejelzés-exponenciális simítás |} Microsoft Azure" 
    description="Webszolgáltatás: előrejelzés-exponenciális simítás" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="xueshzha"/> 


#<a name="forecasting---exponential-smoothing"></a>Előrejelzés - exponenciális simítás 

A [webszolgáltatás]( https://datamarket.azure.com/dataset/aml_labs/ets) alkalmazza az exponenciális simítás modell (Trendérték) kapcsol az előrejelzések a korábbi, a felhasználó által megadott adatok alapján. Az igény szerinti egy adott termék növelik idén? Is lehet találnia a termék értékesítése a karácsonyi évszak az, hogy e is a készlet hatékony megtervezéséhez? Előrejelzési modellek alkalmasak az ilyen kérdésekkel foglalkozik. A múlt adatokat adni, ezek a modellek vizsgálja meg, rejtett trendek és a későbbi trendek előrejelzésére szezonalitás.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 
>Ez a webszolgáltatás sikerült által igénybe vehető felhasználók – esetleg keresztül mobilalkalmazásban, egy webhelyen keresztül, vagy akár a helyi számítógépre, például. De a webszolgáltatás célja is, hogyan Azure gépi tanulási webszolgáltatásokhoz R meg felvételével létrehozásához használható példaként szolgálnak. R néhány kódsorokat és Azure gépi tanulási Studio gomb gombra kattint egy kísérlet R kóddal létrehozható és közzétett webes szolgáltatásként. A webszolgáltatás majd a Microsoft Azure piactéren közzétett és felhasználó és eszköz elfogyasztott világszerte a szerző a webszolgáltatás infrastruktúra beállítás nélkül.
 
##<a name="consumption-of-web-service"></a>Webszolgáltatás felhasználása 
 
Ez a szolgáltatás fogadja el a 4 argumentumokat, és a Trendérték előrejelzések számítja ki.
A beviteli argumentumok:

* Gyakoriság - azt jelzi, hogy a gyakoriság (napi/heti/havi vagy negyedéves és éves) alapadatok.
* Horizont – a jövőben előrejelzési időkereten.
* Dátum - idő az idő új adatsor felvétele.
* Az érték – az új adatsor adatok időértékek beszúrása.

A szolgáltatás a kimenet a számított előre jelzett érték lesz.

Minta beviteli lehet: 

* Gyakoriság – 12
* Horizont – 12
* Dátum - 1/15/2012; 2/15/2012; 3/15/2012; 4/15/2012; 5/15/2012; 6/15/2012; 7/15/2012; 8 / 15/2012; 9/15/2012; 2012-10-15; 2012-11-15; 12/15/2012; 1/15/2013; 2/15/2013; 3/15/2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013; 9/15/2013; 10/15/2013; 11/15/2013; 12/15/2013; 1/15/2014-es; 2/15/2014-es; 3/15/2014-es; 4/15/2014-es; 5/15/2014-es; 6/15/2014-es; 7/15/2014-es; 8 / 15/2014-es; 9/15/2014-es
* Érték - 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509
 
>Ez a szolgáltatás, az Azure piactéren elérhető is egy OData-szolgáltatás; Ezek a előfordulhat, hogy keresztül POST vagy GET módszerek neve. 

Az automatikus módon szolgáltatás használata más több módja létezik (például alkalmazás van [Itt](http://microsoftazuremachinelearning.azurewebsites.net/etsForecasting.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>C# kód web service fogyasztási kezdési:
    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }



##<a name="creation-of-web-service"></a>A webszolgáltatás létrehozása 

>Ez a webszolgáltatás készült Azure gépi tanulási használatával. Az ingyenes próbaverzióra, valamint bevezető szintű videoklipek kísérletek és a [közzétételi webszolgáltatások](machine-learning-publish-a-machine-learning-web-service.md)létrehozásával kapcsolatban olvassa el [azure.com/ml](http://azure.com/ml). Az alábbi van a kísérlet a webes szolgáltatás és példa-kódot a kísérlet moduljainak minden egyes létrehozott látható.

Az Azure gépi tanulási, belül egy új üres kísérlet hozták létre. A bemeneti mintaadatokat egy előre definiált adatok séma használata feltöltése. Az adatok séma csatolva van az [R parancsfájl végrehajtása] [ execute-r-script] modul által generált az exponenciális Simítási modell előrejelzés a j "trendérték" és "előrejelzési függvények használatával 


![Kísérlet továbbításához][2]

####<a name="module-1"></a>A modul 1:
 
    # Add in the CSV file with the data in the format shown below 
![Adatok minta][3]   

####<a name="module-2"></a>A modul 2:
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- ets(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

 
##<a name="limitations"></a>Korlátozások 

Ez a rendkívül egyszerű példa: az exponenciális Simítási előrejelzéshez. A fenti példa kódból is láthatja, nem kifogására hiba történik, és a szolgáltatás feltételezi, hogy az összes változó figyelembevételével folyamatos/pozitív értékek, a gyakoriság 1-nél nagyobb egész szám kell lennie. A dátum és az érték pontok hossza ugyanúgy kell lennie. A dátum változó kell tartaniuk a "hh/nn/éééé" formátum.

##<a name="faq"></a>GYAKORI KÉRDÉSEK
A webszolgáltatás vagy a közzététel az Azure Piactérhez felhasználási a gyakori kérdések című [alábbi](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img1.png
[2]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img2.png
[3]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
