<properties 
    pageTitle="Arányainak próba különbség |} Microsoft Azure" 
    description="Az arányok próba különbség" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Az arányok próba különbség


Különböznek két arányainak statisztikailag? Tegyük fel, hogy egy felhasználó szeretne hasonlítani két filmek határozza meg, ha több filmet van-e egy lényegesen nagyobb részét "tetszésnyilvánítások" Ha a másik képest. A nagyméretű minta lehet az arányok 0,50 és 0,51 statisztikailag jelentős eltérése. Kis mintával nem lehet elég adat határozza meg, ezeket az arányok ténylegesen eltérőek. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

A [webszolgáltatás]( https://datamarket.azure.com/dataset/aml_labs/prop_test) t-próba a felhasználó által a siker küszöbértéke és a 2 összehasonlító csoportok kísérletek száma alapján két arányainak különbségét hajt végre. Több lehetséges esetben ez a szolgáltatás neve is lehet a alkalmazásból a film összehasonlító, a felhasználó jelzi, hogy a filmek egyik van valójában "tetszett" gyakrabban más, mint a film minősítések alapján.

>Ez a webszolgáltatás sikerült által igénybe vehető felhasználók – esetleg keresztül mobilalkalmazásban, egy webhelyen keresztül, vagy akár a helyi számítógépre, például. De a webszolgáltatás célja is, hogyan Azure gépi tanulási webszolgáltatásokhoz R meg felvételével létrehozásához használható példaként szolgálnak. R néhány kódsorokat és Azure gépi tanulási Studio gomb gombra kattint egy kísérlet R kóddal létrehozható és közzétett webes szolgáltatásként. A webszolgáltatás majd a Microsoft Azure piactéren közzétett és felhasználó és eszköz elfogyasztott világszerte a szerző a webszolgáltatás infrastruktúra beállítás nélkül.


##<a name="consumption-of-web-service"></a>Webszolgáltatás felhasználása

A szolgáltatás fogadja a 4 argumentumokat, és nem egy hipotézis tesztelje az arányok.

A beviteli argumentumok:

* Successes1 - 1 mintában sikeres események száma.
* Successes2 - 2 mintában sikeres események száma.
* Total1 - 1 minta mérete.
* Total2 - 2 minta mérete.

A szolgáltatás a kimenet eredménye a feltevést együtt chi-square statisztikai, df, p – érték tesztelése és az 1/2 és a konfidencia-intervallum átállítása korlátok minta arányok szerint.

>Ez a szolgáltatás, az Azure piactéren elérhető is egy OData-szolgáltatás; Ezek a előfordulhat, hogy keresztül POST vagy GET módszerek neve. 

Az automatikus módon szolgáltatás használata más több módja létezik (például alkalmazás van [Itt](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C# kód web service fogyasztási kezdési:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
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

Azure gépi tanulási, belül egy új üres kísérlet készült két [R parancsfájl végrehajtása] [ execute-r-script] modulokat. Az adatok sémát első modulban közben a második modul használja belüli R prop.test parancs végrehajtása a t-próba 2 arányainak. 


###<a name="experiment-flow"></a>Kísérlet folyamat:

![Kísérlet továbbításához][2]


####<a name="module-1"></a>A modul 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>A modul 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Korlátozások 

Ez a 2 arányainak a különbség a vizsgálat rendkívül egyszerű példa. Ahogy a fenti példa kódból is láthatja, nem hiba kifogására hajtanak végre, és a szolgáltatás feltételezi, hogy az összes változó figyelembevételével folyamatos.

##<a name="faq"></a>GYAKORI KÉRDÉSEK
A webszolgáltatás vagy a közzététel az Azure Piactérhez felhasználási a gyakori kérdések című [alábbi](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
