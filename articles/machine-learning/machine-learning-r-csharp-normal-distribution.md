<properties 
    pageTitle="Normál eloszlás Web Service Suite |} Microsoft Azure" 
    description="Normál eloszlás Web Service Suite" 
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

#<a name="normal-distribution-suite"></a>Normál eloszlás programcsomagban



A normális eloszlás csomagja egy minta webszolgáltatásokhoz ([nyilvántartás-készítő alkalmazás]( https://datamarket.azure.com/dataset/aml_labs/ndg7), [Ki osztóérték Számológép]( https://datamarket.azure.com/dataset/aml_labs/ndq5) [Valószínűség Számológép]( https://datamarket.azure.com/dataset/aml_labs/ndp5)), az létrehozása és kezelése a normál terjesztését. A szolgáltatások engedélyezése bármelyik hossza, a megadott valószínűség quantiles kiszámítása, és az egy adott ki osztóérték eloszlás kiszámítása a normális eloszlás sorozatában generálni. Egyes szolgáltatások bocsát alapján a kijelölt szolgáltatás különböző kimeneti értékeket (lásd: a leírás alább). A normális eloszlás csomagja az R függvények qnorm rnorm és pnorm, amelyek szerepelnek R stat csomag alapul.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Ez a webszolgáltatás sikerült által igénybe vehető felhasználók – esetleg keresztül mobilalkalmazásban, egy webhelyen keresztül, vagy akár a helyi számítógépre, például. De a webszolgáltatás célja is, hogyan Azure gépi tanulási webszolgáltatásokhoz R meg felvételével létrehozásához használható példaként szolgálnak. R néhány kódsorokat és Azure gépi tanulási Studio gomb gombra kattint egy kísérlet R kóddal létrehozható és közzétett webes szolgáltatásként. A webszolgáltatás majd a Microsoft Azure piactéren közzétett és felhasználó és eszköz elfogyasztott világszerte a szerző a webszolgáltatás infrastruktúra beállítás nélkül.  
 

##<a name="consumption-of-web-service"></a>Webszolgáltatás felhasználása
A normális eloszlás csomagja tartalmazza a következő 3 szolgáltatásokat.

###<a name="normal-distribution-quantile-calculator"></a>Normális eloszlást ki osztóérték Számológép
Ez a szolgáltatás fogadja el a normális eloszlás 4 argumentumai, és ki a társított osztóérték számítja ki.

A beviteli argumentumok:

* p – a normális eloszlás esemény egyetlen valószínűség. 
* Közép - a normális eloszlás középérték.
* Ft - a normális eloszlás szórása. 
* Az oldal - a alsó részén az eloszlás L és a felső sarkában a terjesztési U.

A kimenet a szolgáltatás ki a számított osztóérték a megadott valószínűség társított.

###<a name="normal-distribution-probability-calculator"></a>Normál eloszlás valószínűségértékét Számológép
Ez a szolgáltatás fogadja el a normális eloszlás 4 argumentumai, és a kapcsolódó valószínűségét számítja ki.

A beviteli argumentumok:

* kérdések a normális eloszlás esemény-egyetlen ki azt osztóérték. 
* Közép - a normális eloszlás középérték.
* Ft - a normális eloszlás szórása. 
* Az oldal - a alsó részén az eloszlás L és a felső sarkában a terjesztési U.

A szolgáltatás a kimenet számított számítja ki az adott osztóérték társított.

###<a name="normal-distribution-generator"></a>Normál eloszlás nyilvántartás-készítő alkalmazás
Ez a szolgáltatás fogadja el a normális eloszlás 3 argumentumai, és hozza létre a véletlen számok, amelyek normális eloszlású sorozatát. Az alábbi argumentumokat a kérelem belül oda kell megadni:

* n - megfigyelések száma. 
* Közép - a normális eloszlás középérték.
* Ft - a normális eloszlás szórása. 

A szolgáltatás a kimenet egy olyan karakterlánc, hossz n a normális eloszlás középértéke és Ft argumentumai alapján.

>Ez a szolgáltatás, az Azure piactéren elérhető is egy OData-szolgáltatás; Ezek a előfordulhat, hogy keresztül POST vagy GET módszerek neve. 

Az automatikus módon szolgáltatás használata más több módja létezik (például alkalmazásokat tartalmaz az alábbi: [nyilvántartás-készítő alkalmazás](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), a [Valószínűség Számológép](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Ki osztóérték Számológép](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>C# kód web service fogyasztási kezdési:

###<a name="normal-distribution-quantile-calculator"></a>Normális eloszlást ki osztóérték Számológép
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Normál eloszlás valószínűségértékét Számológép
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Normál eloszlás nyilvántartás-készítő alkalmazás
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>Ez a webszolgáltatás készült Azure gépi tanulási használatával. Az ingyenes próbaverzióra, valamint bevezető szintű videoklipek kísérletek és a [közzétételi webszolgáltatások](machine-learning-publish-a-machine-learning-web-service.md)létrehozásával kapcsolatban olvassa el [azure.com/ml](http://azure.com/ml). 

Az alábbi van a kísérlet a webes szolgáltatás és példa-kódot a kísérlet moduljainak minden egyes létrehozott látható.

###<a name="normal-distribution-quantile-calculator"></a>Normális eloszlást ki osztóérték Számológép

Kísérlet folyamat:

![Kísérlet továbbításához][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Normál eloszlás valószínűségértékét Számológép
Kísérlet folyamat:

![Kísérlet továbbításához][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Normál eloszlás nyilvántartás-készítő alkalmazás
Kísérlet folyamat:

![Kísérlet továbbításához][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Korlátozások 

Ezek billentyűzetre a normális eloszlás rendkívül egyszerű példa. A fenti példa kódot is látható, mint kis kifogására hiba áll rendelkezésre.

##<a name="faq"></a>GYAKORI KÉRDÉSEK
A webszolgáltatás vagy a közzététel az Azure Piactérhez felhasználási a gyakori kérdések című [alábbi](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
