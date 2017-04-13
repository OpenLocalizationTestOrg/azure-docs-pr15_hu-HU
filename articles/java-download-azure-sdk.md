<properties 
    pageTitle="Töltse le a Java Azure SDK" 
    description="Megtudhatja, hogy miként töltheti le a Java Azure SDK maven tesztelése projektek és telepítésével kapcsolatos alapvető lépések előírt az Azure Tookit Holdas a minta kóddal." 
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

# <a name="download-the-azure-sdk-for-java"></a>Töltse le a Java Azure SDK

Ez a cikk útmutatók letöltése és telepítése az Azure-tárak Java tartalmazza.

**Megjegyzés:** Az Azure-tárak Java kerülnek terjesztésre csoportban a [Apache licenc, 2.0-s verziójú][license].

## <a name="azure-libraries-for-java---manual-download"></a>Java - kézi letöltés Azure tárak

Az Azure-tárak Java manuális telepítéséhez kattintson a <http://go.microsoft.com/fwlink/?LinkId=690320> összes a tárak és az összes függőségek tartalmazó ZIP-fájl letöltése gombra.

Miután letöltötte a zip-fájl a számítógépen, tartalmát kiolvasó, és a üveg fájlok hozzáadása a projekthez használatával az alábbi lehetőségek közül:

* A üveg-fájlok importálása a Holdas Java projektjét.

* Állítsa be a Java projekthez **Összeállítása elérési út** Holdas üveg fájlok elérési útjának feltüntetése az.

Holdas összeállítás elérési beállításával kapcsolatos részletes tudnivalókért témakörben [Java összeállítása elérési útját] a Holdas webhelyén.

**Megjegyzés:** Lásd az license.txt és ThirdPartyNotices.txt fájl belül a ZIP-licenc és egyéb adatok megadása.

## <a name="azure-libraries-for-java---building-with-maven"></a>Java - Azure tárak felépíteni a maven tesztelése

### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Lépés: 1 – állítsa be a projekt maven tesztelése-összeállítás

Az [Első lépések az adatkezelési tárak Azure Java] utasításait követve az Azure tárak használó Java Holdas maven tesztelése projektek létrehozása[ maven-getting-started] cikk. 

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Lépés: 2 – a maven tesztelése beállításainak konfigurálása és a kapcsolódó függőségek

Miután a projekt konfigurált maven tesztelése használandó fejlesztése, felveheti a a szintaxissal pom.xml fájlhoz kapcsolódó függőségek, mint az alábbi példa. Fontos tudni, hogy Ön nem adja hozzá minden függőség, amely szerepel az alábbi példában; csak kell az adott függőségek, amely előírja a projekt hozzáadása.

> [AZURE.NOTE] Minden egyes belül `<version>` elemet az alábbi példa az ebben a példában a "n.n.n" helyőrzők cseréje érvényes verziószáma, amely szerezhető be a [Tárak tárházából Azure maven tesztelése].

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Az Azure eszközkészlet Holdas telepítése

Ez a szakasz az Azure eszközkészlete Holdas; telepítésével kapcsolatos alapvető útmutatót tartalmaz részletes útmutatásért lásd [az Azure eszközkészlete Holdas telepítése].

### <a name="prerequisites"></a>Előfeltételek

1. Windows operting rendszerek szerepel a felsorolásban, [az az Azure eszközkészlete Holdas újdonságai] című témakörben.
1. Macintosh vagy Linux operting felsorolt rendszerek [az Azure eszközkészlete Holdas az újdonságai] című témakörben.
1. IDE Holdas Java EE fejlesztőknek Indigo vagy újabb verziója. Ez a <http://www.eclipse.org/downloads/>lehet letölteni.

### <a name="basic-installation-steps"></a>Telepítési alaplépései

1. Holdas, a **Súgó** menüben válassza az **Új szoftver telepítése**.
1. Adja meg a webhely <http://dl.microsoft.com/eclipse> helyét, és nyomja le az **ENTER billentyűt**.
1. Jelölje ki a cikkek telepítve van, és kattintson a **Befejezés gombra**.

Az Azure eszközkészlete Holdas az Azure SDK legújabb verzióját használja. A letölthető <http://go.microsoft.com/fwlink/?LinkID=252838>a webes Platform telepítő (WebPI) használatával. Jó helyen jár Ha nincs telepítve van, az első Azure környezetben project az Azure eszközkészlete Holdas létrehozásakor automatikusan telepíti az Azure SDK megfelelő verzióját.

## <a name="see-also"></a>Lásd még:

[Azure eszközkészlete Holdas]

[Az Azure eszközkészlet Holdas telepítése] 

[A Holdas Azure egy helló világ alkalmazás létrehozása]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure tárak tárházából maven tesztelése]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java összeállítás elérési út]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Az Azure eszközkészlete Holdas újdonságai]: http://go.microsoft.com/fwlink/?LinkId=690333
