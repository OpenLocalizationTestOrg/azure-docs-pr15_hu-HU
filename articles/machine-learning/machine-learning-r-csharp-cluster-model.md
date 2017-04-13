<properties 
    pageTitle="A modell fürt |} Microsoft Azure" 
    description="Modell" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Modell    

Hogyan azt előrejelzésére hitelképesség cardholders jelenségeket csoportok hitelkártya kibocsátó ingyenesen kikapcsolása kockázat csökkentése érdekében? Hogyan azt álló csoportokat definiálhat személy jellemzők az alkalmazottak munkahelyi a teljesítmény javítása érdekében? Hogyan lehet orvosok sorolják be betegek azok betegség jellemzői alapján csoportokba? Elvileg a fenti kérdések összes is megválaszolni fürt analysis keresztül.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Fürt analysis sorolja egy sor olyan észrevételek csoportokba két vagy több egymást kölcsönösen kizáró ismeretlen változót kombinációk alapján. A fürt analysis célja felfedezése a rendszert a észrevételek, általában a személyek és tulajdonságaik rendszerezésére csoportokba, ahol a csoportok tagjai megosztása tulajdonságok közös. Ez a [szolgáltatás](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) használja a K-eszközök módszertan, a gyakran használt fürtképző technika fürt tetszőleges adatok csoportokba. Ez a webszolgáltatás megnyitja az adatok és a k számát bemeneteként fürtök hoz létre, amelyek a k csoportjának minden észrevételek előrejelzések. 

>Ez a webszolgáltatás sikerült által igénybe vehető felhasználók – esetleg keresztül mobilalkalmazásban, egy webhelyen keresztül, vagy akár a helyi számítógépre, például. De a webszolgáltatás célja is, hogyan Azure gépi tanulási webszolgáltatásokhoz R meg felvételével létrehozásához használható példaként szolgálnak. R néhány kódsorokat és Azure gépi tanulási Studio gomb gombra kattint egy kísérlet R kóddal létrehozható és közzétett webes szolgáltatásként. A webszolgáltatás majd a Microsoft Azure piactéren közzétett és felhasználó és eszköz elfogyasztott világszerte a szerző a webszolgáltatás infrastruktúra beállítás nélkül.  

##<a name="consumption-of-web-service"></a>Webszolgáltatás felhasználása   
Ez a webszolgáltatás az adatok csoportosítja k csoportokat, és minden egyes sorára csoport-hozzárendelés exportálja. A webszolgáltatás vár a végfelhasználó adatok beviteléhez karakterláncként, ahol a sorok vannak elválasztva (,), és oszlopok egymástól pontosvesszővel (;) elválasztva egymástól. A webszolgáltatás 1 sor vár, egy időben. Egy példa adatkészlet ábrája ilyen:

![Mintaadatok][1]

Tegyük fel, hogy a felhasználó megy végbe, ezeket az adatokat külön 3 egymást kölcsönösen kizáró csoportokba. Az adatok, a fenti adathalmazban lennének az alábbi adatokat: érték = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". A kimenet az egyes sorok előre jelzett csoporttagságát.

>Ez a szolgáltatás, az Azure piactéren elérhető is egy OData-szolgáltatás; Ezek a előfordulhat, hogy keresztül POST vagy GET módszerek neve. 

Az automatikus módon szolgáltatás használata más több módja létezik (például alkalmazás van [Itt](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C# kód web service fogyasztási kezdési:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Belül Azure gépi tanulási, egy új üres kísérlet készült és két [R parancsfájl végrehajtása] [ execute-r-script] modulok lekért alakzatot a munkaterületet. Az adatok séma egy egyszerű [Végrehajtása R parancsfájl]készült[execute-r-script]. Ezután az adatok séma a fürt modell részen az [R parancsfájl végrehajtása]újra létre csatolt[execute-r-script]. A [Végrehajtás R parancsfájl] [ execute-r-script] a fürt modellt használja, a webszolgáltatás majd használja a "k-eszközöket" függvény, amely beépített be az [R parancsfájl végrehajtása] [ execute-r-script] az Azure gépi tanulási.    
   

     
![Kísérlet továbbításához][3]

####<a name="module-1"></a>A modul 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>A modul 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Korlátozások
A webes fürtszolgáltatásnak rendkívül egyszerű példa: az. Ahogy a fenti példa kódból is láthatja, nem hiba kifogására hajtanak végre, és a szolgáltatás azt feltételezi, hogy mindent egy folyamatos változó (ahol nem kockák szolgáltatások engedélyezett), a szolgáltatás csak bemenetben numerikus értékek létrehozása a webszolgáltatás idején. Is a szolgáltatás jelenleg képes korlátozott mennyiségű adat méretét, határidő kérés/válasz jellegét a webszolgáltatás hívása és arra, hogy a modell éppen alkalmas minden alkalommal a webszolgáltatás nevezik. 

##<a name="faq"></a>GYAKORI KÉRDÉSEK
A webszolgáltatás vagy a közzététel az Azure Piactérhez felhasználási a gyakori kérdések című [alábbi](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
