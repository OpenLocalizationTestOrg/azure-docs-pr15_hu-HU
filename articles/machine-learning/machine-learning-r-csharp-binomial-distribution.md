<properties 
    pageTitle="Binomiális eloszlás csomagja |} Microsoft Azure" 
    description="Binomiális eloszlás programcsomagban" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 


#<a name="binomial-distribution-suite"></a>Binomiális eloszlás programcsomagban




A binomiális eloszlás csomagja egy minta webszolgáltatásokhoz ([Binomiális nyilvántartás-készítő alkalmazás](https://datamarket.azure.com/dataset/aml_labs/bdg5), a [Valószínűség Számológép]( https://datamarket.azure.com/dataset/aml_labs/bdp4), [Ki osztóérték Számológép]( https://datamarket.azure.com/dataset/aml_labs/bdq5)), az létrehozása és kezelése binomiális terjesztését. A szolgáltatások engedélyezése a diszkrét binomiális eloszlás sorozatában bármilyen hosszúságú quantiles kiszámítása létrehozása "Házon kívül" megadott valószínűség és eloszlás kiszámítása az egy adott ki osztóérték. Egyes szolgáltatások bocsát alapján a kijelölt szolgáltatás különböző kimeneti értékeket (lásd: a leírás alább). A binomiális eloszlás csomagja az R függvények qbinom rbinom és pbinom, amelyek szerepelnek az R stat csomag alapul. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Ezek a szolgáltatások webes sikerült felhasználandó felhasználók – esetleg közvetlenül a piactér keresztül mobilalkalmazásban, egy webhelyen keresztül, vagy akár a helyi számítógépre, például. De a webszolgáltatás célja is, hogyan Azure gépi tanulási webszolgáltatásokhoz R meg felvételével létrehozásához használható példaként szolgálnak. R néhány kódsorokat és Azure gépi tanulási Studio gomb gombra kattint egy kísérlet R kóddal létrehozható és közzétett webes szolgáltatásként. A webszolgáltatás majd lehet a Microsoft Azure piactéren közzétett és világszerte felhasználó és eszköz felhasznált – nincs infrastruktúra beállítása a webes szolgáltatás a szerző nem szükséges.

##<a name="consumption-of-web-service"></a>Webszolgáltatás felhasználása
A binomiális eloszlás csomagja tartalmazza a következő 3 szolgáltatások.

###<a name="binomial-distribution-quantile-calculator"></a>Binomiális eloszlás ki osztóérték Számológép
Ez a szolgáltatás fogadja el a normális eloszlás 4 argumentumai, és ki a társított osztóérték számítja ki.
A beviteli argumentumok:

- p – a több kísérletek egyetlen összesített valószínűség.  
- méret - kísérletek száma.
- valószínűség – próbaverzió a siker valószínűsége.
- Az oldal - L az alsó részén az eloszlás, a felső sarkában a terjesztési U. 

A kimenet a szolgáltatás ki a számított osztóérték a megadott valószínűség társított.

###<a name="binomial-distribution-probability-calculator"></a>Binomiális eloszlás valószínűségértékét Számológép
Ez a szolgáltatás fogadja el a binomiális eloszlás alapján 4 argumentumai és kapcsolódó valószínűségét számítja ki.
A beviteli argumentumok:

- kérdések-egyetlen ki osztóérték binomiális eloszlás az esemény. 
- méret - kísérletek száma.
- valószínűség – próbaverzió a siker valószínűsége.
- az oldal - L az alsó részén az eloszlás, a felső sarkában a terjesztési vagy az E, amely megegyezik sikeres egyetlen számú U.

A szolgáltatás a kimenet számított számítja ki az adott osztóérték társított.

###<a name="binomial-distribution-generator"></a>Binomiális eloszlás nyilvántartás-készítő alkalmazás
Ez a szolgáltatás fogadja el a binomiális eloszlás 3 argumentumokat, és hozza létre a véletlen számok binomially elosztott sorozatát. Az alábbi argumentumokat a kérelem belül oda kell megadni:

- n - megfigyelések száma. 
- méret - kísérletek száma.
- valószínűség – a siker valószínűsége.

A szolgáltatás a kimenet egy olyan karakterlánc, hossz n a binomiális eloszlás alapján a méret és a valószínűség argumentum.

>Ez a szolgáltatás, az Azure piactéren elérhető is egy OData-szolgáltatás; Ezek a előfordulhat, hogy keresztül POST vagy GET módszerek neve. 

Az automatikus módon szolgáltatás használata más több módja létezik (például alkalmazásokat tartalmaz az alábbi: [nyilvántartás-készítő alkalmazás](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), a [Valószínűség Számológép](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Ki osztóérték Számológép](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C# kód web service fogyasztási kezdési:

###<a name="binomial-distribution-quantile-calculator"></a>Binomiális eloszlás ki osztóérték Számológép
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Binomiális eloszlás valószínűségértékét Számológép
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Binomiális eloszlás nyilvántartás-készítő alkalmazás
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>Binomiális eloszlás ki osztóérték Számológép

![Munkaterület létrehozása][4]

####<a name="module-1"></a>A modul 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>A modul 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Binomiális eloszlás valószínűségértékét Számológép

![Munkaterület létrehozása][5]

####<a name="module-1"></a>A modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>A modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Binomiális eloszlás nyilvántartás-készítő alkalmazás

![Munkaterület létrehozása][6]

####<a name="module-1"></a>A modul 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>A modul 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Korlátozások 
Ezek billentyűzetre a binomiális eloszlás rendkívül egyszerű példa. A fenti példa kódot is látható, mint kis kifogására hiba áll rendelkezésre.

##<a name="faq"></a>GYAKORI KÉRDÉSEK
A webszolgáltatás vagy a közzététel az Azure Piactérhez felhasználási a gyakori kérdések című [alábbi](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
