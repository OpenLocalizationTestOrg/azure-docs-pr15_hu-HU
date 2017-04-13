<properties 
    pageTitle="Lineáris regressziós multivariate |} Microsoft Azure" 
    description="Lineáris regressziós multivariate" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Lineáris regressziós multivariate   
 

 
Tegyük fel, egy adatkészlet van, és be szeretné gyorsan előrejelzésére egy függő változó (y) minden egyes felhasználónak (i), a független változó alapján. Lineáris regressziós használt-e az előrejelzések népszerű statisztikai módszer. Itt a függő változó y folyamatos érték-nek tekinti.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Egyszerű példa lehet, ahol a munkatársa partnere kapcsolatba próbál találnia a magasság (x) alapján (y) Egyéni vonalvastagság. Speciális példa lehet, ahol a munkatársa további információt az egyes (például a vastagság, gender, fajta) van, és próbálja meg az egyes vastagságának előrejelzésére. A [webszolgáltatás]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) az lefedje a lineáris regressziós modell az adatokat, és a becsült értéke (y) exportálja az adatokat a megfigyelések minden egyes.

>Ez a webszolgáltatás sikerült által igénybe vehető felhasználók – esetleg keresztül mobilalkalmazásban, egy webhelyen keresztül, vagy akár a helyi számítógépre, például. De a webszolgáltatás célja is, hogyan Azure gépi tanulási webszolgáltatásokhoz R meg felvételével létrehozásához használható példaként szolgálnak. R néhány kódsorokat és Azure gépi tanulási Studio gomb gombra kattint egy kísérlet R kóddal létrehozható és közzétett webes szolgáltatásként. A webszolgáltatás majd a Microsoft Azure piactéren közzétett és felhasználó és eszköz elfogyasztott világszerte a szerző a webszolgáltatás infrastruktúra beállítás nélkül.  

##<a name="consumption-of-web-service"></a>Webszolgáltatás felhasználása  
Ez a szolgáltatás lehetővé teszi a becsült értékek alapján a független változó összes megfigyelések függő változó. A webszolgáltatás vár a végfelhasználó adatok beviteléhez karakterláncként, ahol a sorok vannak elválasztva (,), és oszlopok egymástól pontosvesszővel (;) elválasztva egymástól. A webszolgáltatás 1 sor vár egyszerre, és várja meg az első oszlopot a függő változó. Egy példa adatkészlet ábrája ilyen:

![Mintaadatok][1]

Egy függő változó nélkül észrevételek kell szövegbeviteli "Név", az y. A fenti adathalmazban lennének a következő karakterlánc bemeneti adatok: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1-es hiányzik; 3; 4". A kimenet az előre jelzett érték a független változó alapján sorok mindegyikéhez. 

>Ez a szolgáltatás, az Azure piactéren elérhető is egy OData-szolgáltatás; Ezek a előfordulhat, hogy keresztül POST vagy GET módszerek neve. 

Az automatikus módon szolgáltatás használata más több módja létezik (például alkalmazás van [Itt](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C# kód web service fogyasztási kezdési:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


Azure gépi tanulási, belül egy új üres kísérlet készült és két [R parancsfájl végrehajtása] [ execute-r-script] modulok lekért alakzatot a munkaterületet. Ez a webszolgáltatás Azure gépi tanulási kísérlet fut, az alapul szolgáló R parancsfájl. Ez a kísérlet 2 részből: sémadefiníciója, és képzési modell + pontozási. Az első modult a bemeneti adatkészlet, ahol az első változó a függő változó és a hátralévő változók független várható szerkezetét adja meg. A második modul illeszkedik a bemeneti adatok egy lineáris regressziós általános modellt.  
  
![Kísérlet továbbításához][3]

####<a name="module-1"></a>A modul 1:
 
####<a name="schema-definition"></a>Sémadefiníciója  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>A modul 2:
####<a name="lm-modeling"></a>LM modellezési   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Korlátozások
Ez a rendkívül egyszerű példa: egy lineáris regressziós több webszolgáltatás. Ahogy a fenti példa kódból is láthatja, nem hiba kifogására hajtanak végre, és a szolgáltatás azt feltételezi, hogy mindent egy folyamatos változó (ahol nem kockák szolgáltatások engedélyezett), a szolgáltatás csak bemenetben numerikus értékek létrehozása a webszolgáltatás idején. Is a szolgáltatás jelenleg képes korlátozott mennyiségű adat méretét, határidő kérés/válasz jellegét a webszolgáltatás hívása és arra, hogy a modell éppen alkalmas minden alkalommal a webszolgáltatás nevezik. 

##<a name="faq"></a>GYAKORI KÉRDÉSEK
A webszolgáltatás vagy a közzététel az Azure Piactérhez felhasználási a gyakori kérdések című [alábbi](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
