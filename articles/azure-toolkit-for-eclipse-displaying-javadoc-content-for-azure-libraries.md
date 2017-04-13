<properties
    pageTitle="A tárak Azure csomag Java Holdas Javadoc tartalom jelennek meg"
    description="Hogyan jelenjen meg a Javadoc tartalmat az Holdas az Azure-tárakat."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>A tárak Azure csomag Java Holdas Javadoc tartalom jelennek meg #

Az Azure-tárak Java Javadoc tartalmának a Javadoc tartalom Java Azure-tárakhoz való társításával megtekintheti a Holdas környezetén belül. Az alábbi lépésekkel megjelenítése Holdas belül ez a funkció használatához.

Ez az eljárás feltételezi, hogy már hozzáadta az Azure tárat Java az összeállítás elérési útját.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Az Azure-tárak Java Holdas Javadoc tartalmat jelenítsen meg ##

* Belül Holdas a Project Explorer a **Hivatkozott tárak** szakaszában a projekt megnyitása a helyi menü a Azure tár Java JAR. Ha például **a microsoft-windowsazure-api-0.1.0.jar** (a verziószám lehet különböző, melyik verziója telepítve van függ).
* Kattintson a **Tulajdonságok**gombra.
* A **Tulajdonságok** párbeszédpanel, a bal oldali ablaktáblában kattintson az **Javadoc helyre**gombra. A **Javadoc Helykeresés** párbeszédpanel jelenik meg.
* Megadhatja, hogy **Javadoc URL-címet**, vagy egy **archívumba Javadoc**.
    * Ha úgy dönt, hogy adja meg a **Javadoc URL-CÍMÉT**, használja az URL-címeit, például **http://dl.windowsazure.com/javadoc** vagy **http://dl.windowsazure.com/storage/javadoc**.
    * Ha úgy dönt, hogy használata **Javadoc archívumban**, megadhatja egy külső fájlt, vagy a munkaterületfájl.
    Adja meg a példányszámot, és Tallózás/érvényesítése szükség szerint. Az alábbi példa az Azure képtárak Java társít a megfelelő Javadoc JAR letöltött **c:\MyAzureJARs**mappába helyi meghajtóra.
    ![][ic553487]
* *Nem kötelező*: kattintson az **érvényesítés**gombra. Itt a potenciális problémák a Javadoc JAR is látható.
* Kattintson az **OK gombra**.

Társított a tár, miután Javadoc tartalma megjelenjen-e a Holdas IDE belül. Például ha `blob` típusú definiált `CloudBlockBlob` belül a kód, a következő képen beírásakor megjelenő tartalom Javadoc `blob.acquireLease` a kódot:

![][ic553488]

## <a name="see-also"></a>Lásd még: ##

[Azure eszközkészlete Holdas][]

[A Holdas Azure egy helló világ alkalmazás létrehozása][]

[Az Azure eszközkészlet Holdas telepítése][] 

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic553487]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: ./media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png