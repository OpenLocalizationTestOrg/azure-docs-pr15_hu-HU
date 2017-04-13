
<properties 
    pageTitle="Swashbuckle által generált API-definíciók testreszabása" 
    description="Megtudhatja, hogy miként szabhatja testre az API-alkalmazásokban Azure App szolgáltatásban Swashbuckle által létrehozott Swagger API-definíciók." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Swashbuckle által generált API-definíciók testreszabása 

## <a name="overview"></a>– Áttekintés

Ez a cikk megtudhatja, hogyan kezelheti a gyakori alkalmazási területek, előfordulhat, hogy amelyre megváltoztathatja az alapértelmezett működés Swashbuckle testre:

* Swashbuckle vezérlő módszerek túlterhelések ismétlődő művelet azonosítóját hoz létre.
* Swashbuckle azt feltételezi, hogy a módszer csak érvényes válasza HTTP 200 (OK) 
 
## <a name="customize-operation-identifier-generation"></a>A művelet azonosító generációs testreszabása

Swashbuckle Swagger művelet azonosítók vezérlő és módszer nevét összefűzésével hoz létre. A minta probléma hoz létre, amikor több túlterhelések módszer: Swashbuckle hoz létre ismétlődő művelet azonosítók, amely olyan, érvénytelen Swagger JSON.

Például a következő vezérlő kódot okoz Swashbuckle három Contact_Get művelet azonosítók létrehozásához.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Egyedi nevét, például az ebben a példában a következő módszerek értesítéssel manuálisan megoldhatja a problémát:

* Get
* GetById
* GetPage

Az alternatív is, hogy legyen automatikus generálása jelölőnégyzet a művelet egyedi azonosítókat Swashbuckle bővítése.

A következő lépések bemutatják, hogyan Swashbuckle testreszabása a Visual Studio API-alkalmazások előzetes projektsablon a projektben szereplő *SwaggerConfig.cs* -fájl használatával.  Ön mint API-alkalmazás telepítéshez konfigurálható webes API projekt is testre szabhatja Swashbuckle.

1. Hozzon létre egy egyéni `IOperationFilter` végrehajtása 

    A `IOperationFilter` felület bővítési pontként nyújt a Swashbuckle a felhasználók, akik szeretné szabni a Swagger metaadatok folyamat számos tulajdonságát. A következő kódot több mód módosítja a művelet generációs azonosító mutatja be. A kód paraméternevek hozzáfűzi a művelet azonosító neve.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. *App_Start\SwaggerConfig.cs* fájlban, hívja fel a `OperationFilter` mód használata az új Swashbuckle okozhat `IOperationFilter` végrehajtása.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    A *SwaggerConfig.cs* fájlt, amely a Swashbuckle NuGet csomag megszakad bővítési pontokat példák számos tárgyalt meg tartalmazza. A további megjegyzéseket nem jelennek meg az alábbi. 

    Ez a módosítás elvégzése után a `IOperationFilter` végrehajtása szolgál, és egyedi művelet azonosítók jöjjön okoz.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Válasz kódok eltérő 200 engedélyezése

Alapértelmezés szerint Swashbuckle feltételezi, hogy HTTP 200 (OK) választ a webes API a módszer *csak* jogos válaszát. Egyes esetekben érdemes más válasz kódok anélkül, hogy az ügyfél kivételt való visszatéréshez.  Például a következő webes API-kódot szeretne egy 200 vagy egy 404 érvényes válaszokat, fogadja el az ügyfél tetszőleges példa mutatja be.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

Ebben az esetben a Swagger Swashbuckle generáló alapértelmezés szerint csak egy jogos HTTP-állapotkódot HTTP 200 határozza meg.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Mivel a Visual Studio az ügyfél-kód létrehozása a Swagger API-definíciót használ, ügyfél kódot, amely nem egy HTTP 200 választ kivételt hatványra hoz létre. Az alábbi kód ügyfélről C# hoz létre a minta webes API módszer van.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

XML-megjegyzések használata biztosít Swashbuckle kétféleképpen testreszabásának hoz létre, várható HTTP válasz kódok listáját, vagy a `SwaggerResponse` attribútum. Az attribútum könnyebben, de csak akkor érhető el a Swashbuckle 5.1.5 vagy újabb. Az API-alkalmazások előzetes verzió új project-sablon a Visual Studio 2013 tartalmaz Swashbuckle verzió 5.0.0, ezért ha a sablont használják, és nem szeretné frissíteni Swashbuckle, csak XML megjegyzések használata. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>XML-megjegyzések használata válasz várt kódok testreszabása

Ez a módszer segítségével adja meg a választ kódok 5.1.5 legkorábban Swashbuckle verziójától esetén.

1. Első lépésként vegyen fel XML-dokumentáció megjegyzések fölé szeretne megadni a HTTP-válasz kódjait módszereket. A minta webes API véve fent látható művelet és az XML-dokumentáció alkalmazása eredményezne a következő példa hasonló kódot. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Utasítások hozzáadása a *SwaggerConfig.cs* fájlban irányítsa át az XML-tartalom használata Swashbuckle dokumentáció fájlt.

    * Nyissa meg a *SwaggerConfig.cs* , és hozzon létre egy módszer a *SwaggerConfig* osztály adja meg a dokumentáció XML-fájl elérési útját. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Görgessen le a *SwaggerConfig.cs* fájlban a megjegyzéssel ellátva kivételének sort hasonlítanak a képernyőkép alatt látható. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Vegye ki a megjegyzésjeleket a sor ahhoz, hogy az XML-megjegyzések feldolgozása Swagger létrehozása során. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Az XML-dokumentáció fájl létrehozásához mappába érkeznek a projekt tulajdonságainak és engedélyezése az XML-dokumentáció fájlt, az alábbi képernyőképen látható módon. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

A fenti lépések végrehajtása után Swashbuckle által generált Swagger JSON tükrözni fogja a HTTP-válasz kódokat az XML-megjegyzések megadott. Az alábbi képernyőképe a új JSON tartalom mutatja be. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Visual Studio használatakor az ügyfél kód újbóli a REST API-C# kód nélkül lehetővé teszi a igénybe kód döntések null kapcsolattartó rekord visszaadását kezelése a kivételt fogadja el mind a HTTP az OK gombra, és nem található állapot kódok. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

Ez a bemutató kódját [a GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse)adattárban találhatók. A webes API-val és a project XML-dokumentáció megjegyzések jelölt használata egy New project ehhez az API létrehozott ügyfél tartalmazó. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Válasz várt kódokat a SwaggerResponse attribútummal testreszabása

A [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) attribútum Swashbuckle 5.1.5 és újabb verziói. Abban az esetben, ha a projekt korábbi verzióval rendelkezik, ez a szakasz elindítja az, hogy miként kell a Swashbuckle NuGet csomag, hogy az adott jellemző is használhatja.

1. A **Megoldás Explorer**kattintson a jobb gombbal a webes API-projektet, majd kattintson a **NuGet csomagok kezelése**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Kattintson a *frissítés* gombra a *Swashbuckle* NuGet csomag mellett. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. A webes API művelet módszerek, amelynek meg szeretné adja meg a HTTP-válasz érvényes kódok adja hozzá a *SwaggerResponse* tulajdonságait. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Adja hozzá a `using` utasítás az attribútum névtér:

        using Swashbuckle.Swagger.Annotations;
        
1. Keresse meg a projekt */swagger/docs/v1* URL-CÍMÉT, és a HTTP-válasz különböző kódokat a Swagger JSON látható lesz. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Ez a bemutató kódját [a GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes)adattárban találhatók. A webes API-val és a project *SwaggerResponse* attribútumot tartalmazó díszítve használata egy New project ehhez az API létrehozott ügyfél tartalmazó. 

## <a name="next-steps"></a>Következő lépések

Ez a cikk azt mutatja, hogyan szabhatja Swashbuckle hoz létre, a művelet azonosítókhoz és az érvényes válasz kódok. További tudnivalókért lásd: [a GitHub Swashbuckle](https://github.com/domaindrivendev/Swashbuckle).
 
