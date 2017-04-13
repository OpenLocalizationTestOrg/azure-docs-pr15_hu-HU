<properties
   pageTitle="Útmutató a adatok szolgáltatás létrehozása a piactér |} Microsoft Azure"
   description="Részletes útmutatást, hogy miként hozhat létre, tanúsítása és az adatok szolgáltatás üzembe vásárolja meg a Microsoft Azure piactéren."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Egy meglévő webszolgáltatás megfeleltetése OData CSDL keresztül

>[AZURE.IMPORTANT] **Adott időben azt már nem bevezetési minden új adatszolgáltatás közzétevők. Új dataservices nem első jóváhagyva listaelem.** Ha a szoftver üzleti alkalmazások AppSource a közzétenni kívánt talál további információt [Itt](https://appsource.microsoft.com/partners). Ha egy IaaS alkalmazások vagy Fejlesztőeszközök szolgáltatás szeretné közzé a Microsoft Azure piactéren, hogy több információt találhat [az alábbi](https://azure.microsoft.com/marketplace/programs/certified/).

Ez a cikk áttekintést nyújt a CSDL használatát egy meglévő szolgáltatást hozzárendelése egy OData-kompatibilis szolgáltatás. Megtudhatja, hogyan szolgáltatás hívás keresztül az ügyféltől a bemeneti kérelem átalakítások a hozzárendelés dokumentum (CSDL) létrehozása és az ügyfél keresztül kompatibilis OData-adatcsatorna visszatérhet a kimenet (adatok). A Microsoft Azure piactéren elérhető szolgáltatások a végfelhasználók számára az OData-protokoll használatával teszi lehetővé. Tartalom szolgáltatók (adatok tulajdonosai) által elérhető szolgáltatások elérhető különböző űrlapok, például a többi, SOAP stb.

## <a name="what-is-a-csdl-and-its-structure"></a>Mi az a CSDL és felépítésétől?
Egy CSDL (elvi séma Definition Language) webszolgáltatás, vagy az adatbázis-szolgáltatás, XML-szóhasználatára az Azure Piactérhez közös hasonlítanak a leíró definiáló specifikáció.

Egyszerű áttekintése a **kérése folyamat:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

**Adatfolyam** irányának Ellentétesre változtatásához van:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Ábra 1** ábrázolja, hogy hogyan ügyfél volna adatokat olvas be (szolgáltatás) tartalma szolgáltatóról a Microsoft Azure piactéren keresztül.  A CSDL használja, a hozzárendelés/átalakítása összetevő kezelni a kérést, és az adatok adják át a tartalomszolgáltató-ra és az ügyfélnek között.

*Ábra 1: Részletes illusztrálja kérő ügyfélprogramból tartalomszolgáltató keresztül Azure Marketplace szolgáltatásból*

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Atom Pub, és az OData protokollt, amelyen az Azure piactéren elérhető bővítmények összeállítása a Atom háttérként, tájékoztassa a Véleményezés: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Hivatkozás feletti kivonat:     *"(a továbbiakban OData) megnyitása jegyzőkönyv célja adja meg a többi alapú protokoll CRUD stílusú műveletek (létrehozás, Olvasás, frissítés és törlés) ellen erőforrások data services közzétéve. A "adatok" szolgáltatása zárólap, ha egy vagy több "gyűjtemények" egyes bejegyzésekkel nulla vagy több"", amely áll adatát beírt nevű – érték párokká. OData van a Microsoft által közzétett alapján (a szervezet előrelépés az strukturált adatokat szabványokat) OASIS szabványügyi, hogy bárki szeretne nyújthat kiszolgálók, az ügyfelek vagy jogdíjak és korlátozás nélkül eszközök."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Három kritikus darab, amely szerint a CSDL kell megadni a következők:

- **Végpont** : a szolgáltatás szolgáltató a webes címet (URI) a szolgáltatás
- Bemenetként átadja a szolgáltató átadott **adatok paramétereket** paramétereket meghatározását elküldése a tartalomszolgáltató szolgáltatás le az adattípus.
- **Séma** a kérő szolgáltatás visszaküldött az adatok a tartalomszolgáltató szolgáltatás, beleértve a tároló, webhelycsoportok és táblázatok, változók és oszlopok és adattípusok kézbesíti a sémában.

A következő ábrán a folyamat, ahol az ügyfél belép meg az eredmények adatokat vissza az OData-utasítás (a tartalomszolgáltató webszolgáltatás hívása) a áttekintése.

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>A lépéseket:

1. Ügyfél küld szolgáltatás hívás útján teljes az XML az Azure Piactérhez definiált beviteli paraméterrel
2. Ellenőrizze a szolgáltatás hívás CSDL szolgál.
    - A formázott szolgáltatás hívja fel a rendszer elküldi a tartalom szolgáltatók szolgáltatás által az Azure piactérről
3. A webszolgáltatás végrehajt, majd a művelet a HTTP-művelet preformok (azaz első) van adja vissza az adatokat a kért adatokat (ha van ilyen) esetén teszi XML formátumban, az ügyfél számára láthatóvá Azure piactérről a CSDL meghatározott leképezés használatával.
4. Az ügyfél küldi el az adatok (ha van ilyen) XML- vagy JSON formátumban

## <a name="definitions"></a>Definíciók

### <a name="odata-atom-pub"></a>Atom-alapú OData pub

Állítsa a ATOM pub, ahol minden bejegyzésben egy sor olyan eredménye, bővítménye. A tartalom részét a bejegyzés azokat az értékeket, a sor – fő érték párokká, hogy van továbbfejlesztett. További információt itt talál: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL - elvi séma Definition Language

Lehetővé teszi, hogy (SPROCs) funkciók definiálása és szervezetek, amelyek adatbázis keresztül elérhető. Itt található további információ: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Kattintson az **egyéb verzióival** tartalmazó legördülő listára, és jelölje ki az verzióját, ha nem látja a cikk.

### <a name="edm---entry-data-model"></a>EDM - bejegyzés adatmodell
- Áttekintés: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Kép: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Adattípusok: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

Biztonsági másolat meg az eredmények adatok származó, ahol a az ügyfél belép az OData-utasítás (a tartalomszolgáltató webszolgáltatás hívása) flow részletes balról jobbra, a következő látható:

  ![rajz](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>CSDL alapjai

Egy CSDL (elvi séma Definition Language) webszolgáltatás, vagy az adatbázis-szolgáltatás, XML-szóhasználatára az Azure Piactérhez közös hasonlítanak a leíró definiáló specifikáció. CSDL ismerteti a kritikus darab, amely **lehetővé teszi az adatok átadása az adatforrásból az Azure Piactérhez.** A fő darab vannak az alábbiakban ismertetjük:

- A kapcsolat adatainak leíró összes nyilvánosan elérhető függvények (FunctionImport csomópont)
- Az összes üzenet requests(input) és az üzenet responses(outputs) (EntityContainer/EntitySet/EntityType csomópontok) adattípus
- Kötelező legyen átviteli protokoll adatainak használt (fejléc csomópont)
- Cím információk megkeresése a megadott szolgáltatás (BaseUri elemet meg attribútum)

Rövid a CSDL felel meg a szolgáltatás lekérdező és a hozzáférés-szolgáltató között platform - és nyelvi független szerződést. A CSDL, egy ügyfél segítségével keresse meg a szolgáltatás-adatbázis webszolgáltatás és meghívni a nyilvánosan elérhető funkciókat.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Egy CSDL vonatkozó egy adatbázisba vagy gyűjteménye
**Az CSDL beállítások**

CSDL webszolgáltatás leíró XML nyelvhelyesség-ellenőrzés. A meghatározás magát meg van osztva 4 fő elemek: EntitySet, FunctionImport; Névtér, és EntityType.

Hogy könnyebben megérthető, lehetővé teszi, hogy a hardverabsztrakciós egy táblázatot egy CSDL vonatkoznak.

Ne feledje, hogy;

  CSDL felel meg a **szolgáltatás lekérdező** és a **szolgáltató**között platform - és nyelvi független szerződést. CSDL, egy **ügyfél** segítségével keresse meg a **szolgáltatás-adatbázis webszolgáltatás** és a nyilvánosan elérhető meghívni **függvények.**

Az adatok szolgáltatás egy CSDL négy részei is értelmezhető pedig egy adatbázis, táblázat, oszlop vagy tár eljárás.

Kapcsolódó adatok szolgáltatás az alábbiak szerint:

- EntityContainer ~ = adatbázis
- EntitySet ~ = táblázat
- EntityType ~ = oszlopok
- FunctionImport ~ = a tárolt eljárás

**Engedélyezett HTTP-műveletek**
- GET – a KCS2 (a webhelycsoport adja eredményül) értékeket adja eredményül.
- BEJEGYZÉS – használt adatait és az eredményül adott értékek választható át a KCS2 (létrehozása új bejegyzés a gyűjteményben, a visszatérési azonosító/URI)
- Törlés – a KCS2 (törlése a webhelycsoport) adatok törlése
- HELYEZI el – be egy szabványú adatbázisból származó adatok frissítése (a webhelycsoport cseréje vagy hozzon létre egyet)

## <a name="metadatamapping-document"></a>Metaadat-hozzárendelés dokumentum

A metaadat-hozzárendelés dokumentum, hogy akkor is elérhető az OData-webszolgáltatásnak az Azure piactéren elérhető rendszer feleltetni egy tartalomszolgáltató meglévő webszolgáltatásokhoz használják. CSDL alapul, és eszközöket a többi igényeihez igazodik CSDL néhány bővítmények alapú webszolgáltatásokhoz Azure piactéren elérhető keresztül elérhetővé tett. A bővítmények [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) névtér találhatók.

Példa a CSDL követi: (Másolás és beillesztés az alábbi példában CSDL egy XML-szerkesztő és módosítása a szolgáltatás megfelelően.  Illessze be CSDL leképezése DataService lapján a szolgáltatás létrehozása az [Azure piactéren elérhető közzétételi portál](https://publish.windowsazure.com)).

**Kifejezések:** A [Közzétételi portál](https://publish.windowsazure.com) felhasználói felület (PPUI) feltételekre vonatkozó CSDL adatokra.
- A PPUI "cím" vonatkozik MyWebOffer felajánlása
- A PPUI az értéket vonatkozik, a [Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) felhasználói felület **Publisher megjelenítendő név**
- Egy webhelyre vagy Data Service (a PPUI csomagra) kapcsolódik az API

**Hierarchia:** 
 (tartalomszolgáltató) vállalata esetében, amelyek Plan(s) tulajdonában van, azaz service(s), mely Sornyit felfelé az API-val.

### <a name="webservice-csdl-example"></a>Webszolgáltatás CSDL példa

Csatlakozik a szolgáltatás ki van a webes alkalmazás végpont (például a C# alkalmazás)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] [Egy meglévő webszolgáltatás CSDLs keresztül OData-leképezés](marketplace-publishing-data-service-creation-odata-mapping-examples.md) példákat további CSDL webszolgáltatás példákat megtekintése

###<a name="dataservice-csdl-example"></a>Példa DataService CSDL

Egy szolgáltatás, akkor közzéteszi adatbázis táblák és nézetek zárólap az alábbi példában látható adatok Alap két API-khoz API CSDL (használható nézetek, hanem táblázatok) alapú csatlakozik.

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Lásd még:
- Ha érdeklik a tanulási az adott csomópontok és a paraméterek, ebből a cikkből [Adatok szolgáltatás OData-leképezés csomópontok](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és használatieset-környezetben használja.
- Ha érdeklik a példák megtekintésével, olvassa el a jelen cikk lásd: minta kód és értelmezhető a mezőkódok szintaxisa és a helyi [Adatok szolgáltatás OData-leképezés példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) .
- Szeretne visszatérni az Azure piactéren elérhető adatok szolgáltatás közzététele előírt elérési útját, olvassa el az [Adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md)Ez a cikk.
