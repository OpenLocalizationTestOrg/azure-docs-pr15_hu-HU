<properties
    pageTitle="Hozzáférés-vezérlés (Java) használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként fejlesztése, és a hozzáférés-vezérlés használata Java Azure-ban."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Hogyan kívánja hitelesíteni a webes felhasználók Holdas használata az Access Azure vezérlő szolgáltatással

Ez az útmutató kell követnie az Azure Access vezérlő szolgáltatást (ACS) belül az Azure eszközkészlet Holdas használni. ACS a további tudnivalókért lásd: a [következő lépésekkel](#next_steps) szakaszban.

> [AZURE.NOTE]
> Az Azure az Access Services vezérlő szűrő közösségi technológia előnézetét. Előzetes verziójú szoftvereihez, mint azt nem hivatalos támogatott Microsoft.

## <a name="what-is-acs"></a>Mit nevezünk ACS?

A legtöbb fejlesztők nem identitás szakértői és általában nem szeretné, hogy mennyit idő fejlesztéséhez hitelesítési és engedélyezési mechanizmusok a saját alkalmazások és szolgáltatások. ACS egyszerűvé telepíteniük kell a webalkalmazások és a szolgáltatások elérése anélkül, hogy a kód tényező összetett hitelesítési logika a felhasználók Azure szolgáltatásainak.

Az alábbi funkciók állnak rendelkezésre a ACS:

-   A Windows identitás Foundation (WIF) integrációja.
-   Népszerű webes Identitásszolgáltatók (IP-címei), beleértve a Windows Live ID azonosítója, a Google, a Yahoo!-ból és a Facebook támogatása.
-   Támogatás az Active Directory összevonási szolgáltatások (AD FS) 2.0-s verziója.
-   Az adatok protokollhoz (OData-)-alapú alkalmazáskezelési szolgáltatás, amely ACS beállítások programozott hozzáférést biztosít.
-   A felügyeleti portál, amely lehetővé teszi a ACS beállításai rendszergazdai hozzáféréssel.

ACS kapcsolatos további tudnivalókért olvassa el az [Access vezérlő szolgáltatás 2.0-s][]című témakört.

## <a name="concepts"></a>Fogalmak

Azure ACS az identitás - egységes megközelítése hitelesítési mechanizmusok futó helyszíni alkalmazások vagy a felhőben létrehozása jogcímalapú alapelvei épül. Jogcímalapú identitás közös biztosítja az alkalmazások és szolgáltatások szerezheti be az identitás információt szükségük van, azok más szervezetekben lévő és az interneten, szervezeten belüli felhasználók számára.

Ebből az útmutatóból a tevékenység befejezéséhez ismernie kell a következő fogalmak:

**Ügyfél** – Ez az útmutató útmutató környezetében az a böngészőben, próbálnak hozzáférni a webalkalmazás.

**Relying fél (RP) alkalmazást** – egy RP alkalmazás áll egy webhely vagy a szolgáltatás, amely egy külső hitelesítésszolgáltató hitelesítés outsources. Az identitás egyéb szakzsargon azt mondja ki, hogy a RP megbízik-e, hogy a szervezet. Ez az útmutató ismerteti, hogyan konfigurálása adatvédelmi ACS az alkalmazást.

**Jogkivonat** - jogkivonat gyűjteménye biztonsági adatok sikeres hitelesítési felhasználó esetén általában kibocsátott. Egy sor olyan *jogcímeken*, a hitelesített felhasználó tulajdonságainak tartalmaz. Igényt megfelelhet a felhasználó nevét, a szerepkör a felhasználói azonosítót tartozik, a felhasználó kor, és így tovább. Jogkivonat általában a digitális aláírás, ami azt jelenti, akkor is mindig kell kifejezéskészletébe vissza a kibocsátó, és a tartalmát nem hamisították. Egy felhasználó hozzáfér RP alkalmazás által a RP alkalmazás megbízhatónak egy szervezet által kibocsátott érvényes token bemutatót tart.

**Identitás szolgáltató IP-** – egy IP egy szervezet hitelesíti felhasználói identitások és biztonsági tokenek problémák. A tényleges munkát tokenek kibocsátó történik, ha olyan biztonsági jogkivonat szolgáltatás (STS) nevű speciális szolgáltatás. Tipikus IP-címei többek között a Windows Live ID Azonosítóját, Facebook, üzleti felhasználó tárhelyek (például az Active Directory) és így tovább.
Megbízhatósági IP-címre ACS van beállítva, amikor a rendszer fogadja el, és ellenőrzése, hogy IP által kibocsátott tokenek. ACS megbízhat több IP-címei egyszerre, ami azt jelenti, hogy, hogy amikor az alkalmazás megbízhatónak ACS azonnali ajánlhat az alkalmazás az összes az IP-címei minden hitelesített felhasználó számára, hogy megbízik-e a ACS az Ön nevében.

**Összevonás szolgáltató (FP)** - IP-címei közvetlen ismerő felhasználók hitelesítő őket a hitelesítő adatokkal és követelések mi tudják, hogy róluk kapcsolatos hiba. Egy összevonási szolgáltató (FP) a szervezet más típusú: helyett hitelesítő közvetlenül a felhasználók, akkor működik-e egy közvetítő és kereskedők hitelesítési között egy RP és egy vagy több IP-címei. IP-címei és a képkocka hiba biztonsági tokenek, így azok is használni a biztonsági jogkivonat Services (STS). ACS egy FP.

Egyszerű követelések átalakítása szabályok formában **ACS szabály motor** – a bejövő token származhat megbízható IP-címei a tokenek szeretett volna a RP által igénybe vehető való átalakításához használt logikát van szerkezetbe. ACS egy szabály motor, amely bármely átalakítása logika a RP megadott alkalmazásának gondoskodik szolgáltatásait.

**ACS Namespace** - névteret a legfelső szintű partíciót ACS használt rendezése a beállításokat. Névtér IP-címei megbízik, a szolgáló kívánt RP alkalmazásokat listáját tartalmazza, az, hogy a szabály várt szabályok feldolgozása a bejövő tokenek motor, és így tovább. A névtér ACS végrehajtani a függvény első az alkalmazás és a fejlesztő által használt különféle végpontok közzététele.

Az alábbi ábrán látható, hogyan ACS hitelesítési működik-e a webalkalmazás:

![ACS munkafolyamat-diagram][acs_flow]

1.  Az ügyfél (ebben az esetben a böngészőben) lap kér a RP.
2.  A kérés még nem hitelesített, a RP irányítja át a felhasználót a szervezet megbízhatónak, mivel ez ACS. Az ACS a felhasználó-e RP megadott IP-címei választható eltéréseit. Egy felhasználó kijelöli a megfelelő IP.
3.  Az ügyfél megnyitja azt az IP-hitelesítés lapot, és a felhasználótól való bejelentkezéshez.
4.  Az ügyfél hitelesítése (például a hitelesítő adatok megadott identitásnak) után a IP a biztonsági jogkivonat bocsát ki.
5.  A biztonsági jogkivonat kibocsátása, miután ACS átirányítja az ügyfelet a IP, és az ügyfél küld a biztonsági jogkivonat ACS az IP-által kibocsátott.
6.  ACS ellenőrzi a biztonsági jogkivonat IP, az identitás a ACS szabályok motor be a token követelések számítja ki a kimenet identitás követelések és egy új biztonsági jogkivonat, amely tartalmazza a következő kimeneti követelések problémák bemenetben által kibocsátott.
7.  ACS átirányítja az ügyfelet a RP. Az ügyfél az új biztonsági jogkivonat, a RP ACS által kibocsátott küld. A RP ellenőrzi az aláírást, kattintson a biztonsági jogkivonat ACS által kibocsátott, ez a token követelések ellenőrzi, és adja vissza a lap, hogy eredetileg a kért.

## <a name="prerequisites"></a>Előfeltételek

Ebből az útmutatóból a feladatokat végrehajtani, a következőkre lesz szüksége:

- Egy Java Developer Kit (JDK), v 1,6 vagy újabb verziója.
- IDE Holdas Java EE fejlesztőknek Indigo vagy újabb verziója. Ez a <http://www.eclipse.org/downloads/>lehet letölteni. 
- Java-alapú webkiszolgálóra vagy alkalmazáskiszolgáló, például Apache Tomcat, GlassFish, JBoss Application Server vagy rakodóhely megoszlása.
- az Azure előfizetéssel, amely a <http://www.microsoft.com/windowsazure/offers/>szerezhetők be.
- Az Azure-Holdas, az eszközkészlet áprilisi 2014-es kiadás vagy újabb verziójában. További tudnivalókért olvassa el a [Holdas az Azure eszközkészlete telepítése](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx)című témakört.
- Az alkalmazás használatához X.509 tanúsítvány. A nyilvános tanúsítvány (.cer) és a személyes információk cseréje a tanúsítvány szüksége lesz (. PFX) formátumot. (A tanúsítvány létrehozásával kapcsolatos beállítások ismerteti később ebben az oktatóanyagban).
- Az Azure ismerős a irányító és a [egy helló világ alkalmazás létrehozása az Azure Holdas a](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx)tárgyalt telepítési technikákkal számítja ki.

## <a name="create-an-acs-namespace"></a>Hozzon létre egy ACS Namespace

Access-vezérlő szolgáltatást (ACS) Azure használatának megkezdéséhez, létre kell hoznia egy ACS névtér. A névtér megcímezheti az alkalmazáson belül ACS erőforrásainak egyedi hatókör biztosít.

1. Jelentkezzen be az [Azure Kezelőportálja segítségével][].
2. Kattintson **az Active Directory**. 
3. Hozzon létre egy új hozzáférés-vezérlés névtér, kattintson az **Új**, **Alkalmazás szolgáltatások**elemre, kattintson a **Hozzáférés-vezérlés**, és kattintson a **Gyors létrehozása**. 
4. Adjon meg egy nevet a névtér. Azure ellenőrzi, hogy a név egyedi.
5. Jelölje ki a névtér régióját. Használja a régió, az alkalmazás telepítését, amelyben a legjobb teljesítmény elérése érdekében.
6. Ha egynél több előfizetése van, jelölje ki azt az előfizetést, a ACS névtér használni kívánt.
7. Kattintson a **létrehozása**gombra.

Azure hoz létre, és aktiválja a névtér. Várja meg, amíg az új névtér állapota **aktív** a továbblépés előtt. 

## <a name="add-identity-providers"></a>Identitásszolgáltatók hozzáadása

Ebben a feladatban adja hozzá a RP alkalmazással hitelesítéshez használt IP-címei. Bemutató célokra ebben a feladatban bemutatja, hogyan adja hozzá a Windows Live mint IP-címre, de az IP-címei az adatkezelési portálon ACS felsorolt bármelyikét használhatja.


1.  Az [Azure Kezelőportálja][]kattintson **Az Active Directory**, jelöljön ki egy hozzáférés-vezérlés névteret, és kattintson a **kezelés**gombra. Ekkor megnyílik a ACS Kezelőportálja segítségével.
2.  Kattintson a bal oldali navigációs ablakban a ACS felügyeleti portál **Identitásszolgáltatók**.
3.  Windows Live ID alapértelmezés szerint engedélyezve van, és nem lehet törölni. Ebben az oktatóanyagban alkalmazásában csak a Windows Live ID Azonosítóra használják. Ez a képernyő azonban, ahol hozzáadhat más IP-címei, kattintson a **Hozzáadás** gombra.

Windows Live ID engedélyezve van, a ACS névtér IP-címre. Ezután adja meg a Java webalkalmazás (a későbbiekben létrehozott) megegyezik egy RP.

## <a name="add-a-relying-party-application"></a>Megbízó fél alkalmazás hozzáadása

Ebben a feladatban felismerje a Java webalkalmazás érvényes RP alkalmazásként ACS konfigurálása.

1.  Kattintson a ACS Kezelőportálja **Relying alkalmazásait**.
2.  A **Használna alkalmazásait** lapon kattintson a **Hozzáadás**gombra.
3.  A **Külső használna alkalmazás hozzáadása** lapon tegye a következőket:
    1.  A **név**mezőbe írja be a RP nevét. Ebben az oktatóanyagban alkalmazásában írja be az **Azure Web App alkalmazásban**.
    2.  **Mód**, jelölje be a **Adja meg a beállításait manuálisan**.
    3.  A **tartomány**mezőbe írja be a biztonsági jogkivonat ACS által kibocsátott vonatkozik. Ebben a feladatban írja be a **8080 /**.
        ![Külső tartománynév használata az számítási irányító használna][relying_party_realm_emulator]
    4.  **Visszatérési URL-CÍMÉT,** írja be az URL-címet, amelyre ACS a biztonsági jogkivonat adja eredményül. Ebben a feladatban írja be a **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![Relying fél visszatérési URL-cím a számítási irányító való használatra][relying_party_return_url_emulator]
    5.  Fogadja el az alapértelmezett értékeket a többi a mezőket.

4.  Kattintson a **Mentés**gombra.

Sikeresen beállította a Java webalkalmazás az Azure számítási irányító futtatáskor (a 8080 /) egy RP a ACS névtér lesz. Ezután hozzon létre a szabályok feldolgozása a RP követelések használó ACS.

## <a name="create-rules"></a>Szabályok létrehozása

Ebben a feladatban határozzák meg, hogy hogyan jogcímeken át az IP-címei a a RP meghajtó szabályokat. Ez az útmutató érdekében azt egyszerűen beállítja a ACS másolása a bemeneti állítást típusok és értékek közvetlenül a kimeneti jogkivonat, szűrés vagy őket módosítása nélkül.

1.  ACS Kezelőportálja fő lapon kattintson a **szabályok csoportjai**.
2.  A **Szabály csoportok** lapon kattintson a **Szabály csoport alapértelmezett Azure-webalkalmazások számára**.
3.  A **Szabály csoport szerkesztése** lapon kattintson a **Létrehozás**gombra.
4.  A a **szabályok létrehozása: Azure webalkalmazás szabály csoport alapértelmezett** lapon győződjön meg róla, a Windows Live ID be van jelölve, és ezután kattintson a **Létrehozás**gombra.    
5.  A **Szabály csoport szerkesztése** lapon kattintson a **Mentés**gombra.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Töltse fel a ACS névtér tanúsítvány

Ebben a feladatban feltöltése egy. Jelentkezzen be a token kérelmek a ACS névtér által létrehozott használt PFX tanúsítvány.

1.  ACS Kezelőportálja fő lapon kattintson a **tanúsítványok és kulcsok**.
2.  A **tanúsítványok és kulcsok** lapon kattintson a **Hozzáadás** **Jogkivonat-aláíró**felett.
3.  A **hozzáadása jogkivonat-aláíró tanúsítvány és a kulcs** lapon:
    1. **A használt** csoportban kattintson a **Használna fél alkalmazást** , és válassza az **Azure Web App** (amely a korábban legyen a megbízó fél alkalmazás neve).
    2. A **típus** csoportban jelölje be a **X.509-es tanúsítvány**.
    3. **Tanúsítvány** csoportban kattintson a Tallózás gombra, és keresse meg a használni kívánt x.509-es tanúsítvány fájlt. Ez lesz egy. PFX fájlt. Jelölje ki a fájlt, kattintson a **Megnyitás**gombra, és írja be a tanúsítvány jelszót a **jelszó** mezőben. Megjegyzés: a tesztelésre, előfordulhat, hogy használható automatikus-signed-tanúsítvány. Önaláírt tanúsítvány létrehozása, használja az **Új** gombra a a **ACS szűrő** párbeszédpanel (későbbi ismertetett), vagy a **encutil.exe** segédprogram az Azure Starter Kit a [project-webhely][] Java.
    4. Győződjön meg arról, hogy **Ellenőrizze az elsődleges** be van jelölve. **Hozzáadása jogkivonat-aláíró tanúsítvány vagy billentyű** -lapjának a következőhöz hasonlóan kell kinéznie.
        ![Jogkivonat-aláíró tanúsítvány felvétele][add_token_signing_cert]
    5. Mentse a beállításokat, és zárja be a **hozzáadása jogkivonat-aláíró tanúsítvány és a kulcs** lapot a **Mentés** gombra.

Ezután tekintse át az információkat az alkalmazás integrálását lapon, és másolja a vágólapra az, hogy meg kell ACS használni a Java webalkalmazás konfigurálása URI.

## <a name="review-the-application-integration-page"></a>Az alkalmazás integrálását áttekintése

Minden információt és találhat a kódot a Java webalkalmazás (az RP alkalmazás) a ACS Kezelőportálja alkalmazás integrálását lapján ACS végezhető beállításához szükséges. A szövetséges hitelesítési Java webalkalmazásban konfigurálásakor szüksége lesz az ezt az információt.

1.  Az adatkezelési portálon ACS kattintson az **alkalmazás integrálását**.  
2.  Az **Alkalmazás integrálását** lapon kattintson a **Bejelentkezési oldal**.
3.  A **Bejelentkezési oldal integrációs** lapon kattintson az **Azure Web App alkalmazásban**.

Az a **bejelentkezési oldal integráció: Azure Web App** lapon, az URL-cím szerepel **beállítás 1: ACS által üzemeltetett bejelentkezési lapra mutató hivatkozás** a Java webalkalmazás fogja használni. Ha az Azure Access vezérlő szolgáltatások szűrő tár hozzáadása a Java-alkalmazások szüksége lesz az ezt az értéket.

## <a name="create-a-java-web-application"></a>Java webalkalmazás létrehozása
1. Belül Holdas a menüben a kattintson a **fájl**, kattintson az **Új**gombra, és válassza a **Dinamikus a Project Web**. (Ha nem látható a **fájl**és az **Új**parancsra kattintást követően a rendelkezésre álló projektként felsorolt **Dinamikus webes projekt** végezze el a következőket: kattintson a **fájl**, kattintson az **Új**, kattintson a **Projekt**, bontsa ki a **webes**, kattintson a **Dinamikus webhely projekt**, kattintson a **Tovább**gombra.) Ebben az oktatóanyagban céljából adjon nevet a projekt **MyACSHelloWorld**. (Biztosítására használja ezt a nevet, további lépéseket, ebben az oktatóanyagban várt HÁBORÚ fájljait MyACSHelloWorld neve). A képernyő jelenik meg az alábbihoz hasonló:

    ![ACS exampple Helló, világ projekt létrehozása][create_acs_hello_world]

    Kattintson a **Befejezés gombra**.
2. Belül Holdas a Project Explorer nézetben bontsa ki a **MyACSHelloWorld**. Kattintson a jobb gombbal a **Web tartalma**, kattintson az **Új**gombra, és válassza a **Fájl JSP**.
3. Az **Új JSP fájlt** párbeszédpanelen nevezze el a fájlt **index.jsp**. Tartsa MyACSHelloWorld/Web tartalma, mint a szülőmappát, ahogy a következő:

    ![ACS például JSP fájl][add_jsp_file_acs]

    Kattintson a **Tovább**gombra.

4. A **JSP sablon kiválasztása** párbeszédpanelen jelölje ki az **Új JSP fájl (html)** , és kattintson a **Befejezés gombra**.
5. A szöveg hozzáadása a index.jsp fájl megnyitásakor a Holdas **helló ACS, világ!** a meglévő belül `<body>` elemet. A frissített `<body>` tartalom meg kell jelennie a következőket:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Mentse a index.jsp.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>Az alkalmazás a ACS szűrő tár hozzáadása

1. A Projektböngésző Holdas meg kattintson a jobb gombbal a **MyACSHelloWorld** **Összeállítása**kattintson, kattintson a **Összeállítása elérési konfigurálása**.
2. **Java-elérési út összeállítása** párbeszédpanelen kattintson a **tárak** fülre.
3. Kattintson a **tár hozzáadása**gombra.
4. Kattintson az **Azure Access vezérlő szolgáltatások szűrés (MS megnyitott technikai)** , és kattintson a **Tovább gombra**. Az **Azure Access vezérlő szolgáltatások szűrés** párbeszédpanel jelenik meg.  (Előfordulhat, hogy a **hely** mezőben attól függően, amelyen telepítette Holdas, egy másik elérési utat és a verziószám szoftverfrissítések függően eltérő lehet.)

    ![ACS szűrő tár hozzáadása][add_acs_filter_lib]

5. Az adatkezelési portál **Bejelentkezési oldal integrációs** lapjának megnyitása a böngészőben másolja a vágólapra az URL-cím szerepel a **beállítás 1: ACS által üzemeltetett bejelentkezési lapra mutató hivatkozás** mezőben, és illessze be a **ACS hitelesítési végpont** mező Holdas párbeszédpanel.
6. Az adatkezelési portál **Használna fél alkalmazás Szerkesztés** lapjának megnyitott böngészőprogramot használ, másolja a vágólapra az URL-cím szerepel a **tartomány** mezőben, és illessze be a **Külső tartománynév használna** mező Holdas párbeszédpanel.
7. A Holdas párbeszédpanel **Biztonság** szakaszában belül, ha egy meglévő tanúsítvány használni kívánt, kattintson a **Tallózás gombra**, nyissa meg azt a tanúsítványt szeretne használni, jelölje ki azt, és kattintson a **Megnyitás**gombra. Vagy, ha szeretne új tanúsítvány létrehozása, kattintson az **Új** a **Új tanúsítvány** párbeszédpanel megjelenítéséhez, majd adja meg a jelszót, .cer fájl nevét és az új tanúsítvány .pfx fájl nevét.
8. Jelölje be a **beágyazási a tanúsítvány a HÁBORÚ fájlban**. A tanúsítvány beágyazás ily módon tartalmazza azt a üzembe anélkül, hogy manuálisan adhatja hozzá elemeként. (Ha inkább kell tárolni a tanúsítvány módjáról a HÁBORÚ fájlból, sikerült a tanúsítvány felvétele szerepkör-összetevő és törölje a jelet **a HÁBORÚ fájlban a tanúsítvány beágyazási**.)
9. [Választható] Megőrzése **csak HTTPS-kapcsolatokat** ellenőrizni. Ha ezt a beállítást, kell elérni az alkalmazás, a HTTPS protokollt használ. Ha nem szeretné HTTPS-kapcsolatokat megkövetelése, törölje ezt a beállítást.
10. A számítási irányító a telepítéshez **Azure ACS szűrési** beállítások az alábbihoz hasonlóan fog megjelenni.

    ![A számítási irányító történő telepítés Azure ACS szűrőbeállítások][add_acs_filter_lib_emulator]

11. Kattintson a **Befejezés gombra**.
12. Kattintson az **Igen** amikor az egy párbeszédpanel, amely arról tájékoztat, hogy egy web.xml fájl jön létre.
13. Kattintson **az OK gombra** kattintva zárja be a **Java-elérési út összeállítása** párbeszédpanelen.

## <a name="deploy-to-the-compute-emulator"></a>A számítási irányító telepítése

1. A Project Explorer Holdas meg kattintson a jobb gombbal a **MyACSHelloWorld** **Azure**kattintson, és válassza az **Azure-csomagja**.
2. A **projekt nevét**írja be a **MyAzureACSProject** , és kattintson a **Tovább**gombra.
3. Jelölje ki a JDK és alkalmazáskiszolgáló. (Ezeket a lépéseket tartoznak részletesen a [egy helló világ alkalmazás létrehozása az Azure Holdas az](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) oktatóprogram során).
4. Kattintson a **Befejezés gombra**.
5. Kattintson az **Azure irányító a Futtatás** gombra.
6. Után a Java webalkalmazás a számítási irányító elindul, zárja be a böngésző összes előfordulását (hogy az minden olyan aktuális böngésző-munkamenetet ne zavarja meg a ACS login próba).
7. Az alkalmazás futtatásához <> 8080/MyACSHelloWorld/megnyitása a böngészőben ( <vagy/https://localhost:8080/MyACSHelloWorld vagy> Ha **csak HTTPS-kapcsolatokat**be van jelölve). Arra kéri, a Windows Live ID-bejelentkezés, majd meg kell tenni a megbízó fél alkalmazáshoz megadott visszatérési URL-cím.
99.  Ha befejezte a kérelem megtekintése, kattintson az **Azure irányító alaphelyzetbe állítása** gombra.

## <a name="deploy-to-azure"></a>Azure telepítése

Azure telepítéséhez kell a megbízó fél terület módosítása és a visszaküldési URL-cím a ACS névtér.

1. Belül a Azure Kezelőportálja **Használna fél alkalmazás szerkesztése** lapon módosítsa a **tartomány** a telepített webhely URL-címe lesz. **Példa** cserélje le a telepítéshez megadott DNS-nevét.

    ![Külső tartomány használata az gyártási használna][relying_party_realm_production]

2. Módosítsa a **Visszatérési URL-CÍMÉT** az alkalmazás az URL-címe lesz. **Példa** cserélje le a telepítéshez megadott DNS-nevét.

    ![Használna fél visszatérési URL-cím a termelési való használatra][relying_party_return_url_production]

3. Kattintson a **Mentés** , mentse a frissített válasz fél tartománynevet, és visszatér az URL-cím módosítása gombra.
4. Tartsa a böngészőben nyissa meg a **Bejelentkezési oldal integráció** lap, hamarosan másolja azt kell.
5. A Projektböngésző Holdas meg kattintson a jobb gombbal a **MyACSHelloWorld** **Összeállítása**kattintson, kattintson a **Összeállítása elérési konfigurálása**.
6. A **tárak** lapján kattintson az **Azure Access vezérlő szolgáltatások szűrőt**, és kattintson a **Szerkesztés**.
7. Az adatkezelési portál **Bejelentkezési oldal integrációs** lapjának megnyitása a böngészőben másolja a vágólapra az URL-cím szerepel a **beállítás 1: ACS által üzemeltetett bejelentkezési lapra mutató hivatkozás** mezőben, és illessze be a **ACS hitelesítési végpont** mező Holdas párbeszédpanel.
8. Az adatkezelési portál **Használna fél alkalmazás Szerkesztés** lapjának megnyitott böngészőprogramot használ, másolja a vágólapra az URL-cím szerepel a **tartomány** mezőben, és illessze be a **Külső tartománynév használna** mező Holdas párbeszédpanel.
9. A Holdas párbeszédpanel **Biztonság** szakaszában belül, ha egy meglévő tanúsítvány használni kívánt, kattintson a **Tallózás gombra**, nyissa meg azt a tanúsítványt szeretne használni, jelölje ki azt, és kattintson a **Megnyitás**gombra. Vagy, ha szeretne új tanúsítvány létrehozása, kattintson az **Új** a **Új tanúsítvány** párbeszédpanel megjelenítéséhez, majd adja meg a jelszót, .cer fájl nevét és az új tanúsítvány .pfx fájl nevét.
10. Megőrzése **beágyazása a HÁBORÚ fájlban a tanúsítvány** be van jelölve, feltéve, hogy a tanúsítvány beágyazása a HÁBORÚ fájlt szeretne.
11. [Választható] Megőrzése **csak HTTPS-kapcsolatokat** ellenőrizni. Ha ezt a beállítást, kell elérni az alkalmazás, a HTTPS protokollt használ. Ha nem szeretné HTTPS-kapcsolatokat megkövetelése, törölje ezt a beállítást.
12. Az Azure telepítéshez Azure ACS szűrési beállítások az alábbihoz hasonlóan fog megjelenni.

    ![Gyártási telepítés Azure ACS szűrőbeállítások][add_acs_filter_lib_production]

13. Kattintson a **Befejezés gombra** a **Szerkesztés** párbeszédpanel bezárásához.
14. Kattintson az **OK gombra** a **MyACSHelloWorld tulajdonságai** párbeszédpanel bezárásához.
15. Holdas kattintson a **Közzététel Azure felhőbe** gombra. Válaszoljon a rendszer kéri, a [egy helló világ alkalmazás létrehozása az Azure Holdas a](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) témakör **az Azure alkalmazás telepítéséhez** részében elvégzettként hasonló. 

A webes alkalmazás telepítését követően zárjon be minden megnyitott böngésző-munkamenetet, a webes alkalmazás futtatásához és, arra kéri, hogy jelentkezzen be Windows Live ID hitelesítő adatait, utána a megbízó fél alkalmazás visszatérési URL-címre küldött.

Amikor befejezte a ACS Helló, világ alkalmazásban, ne felejtse el törölni a példányban (elolvashatja, hogy miként törlése [egy helló világ alkalmazás létrehozása az Azure Holdas a](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) témakörnek a telepítés).


## <a name="next_steps"></a>Következő lépések

A vizsgálat a a biztonsági állítás Markup Language (SAML) az alkalmazás ACS által visszaadott tájékozódhat [a Azure Access vezérlő szolgáltatás által visszaadott SAML megtekintése][]. További feltárása a ACS a használható funkciók körét, és kísérletezzen bonyolultabb esetek, lásd: [Access-vezérlő szolgáltatást 2.0][].

Ebben a példában is a **beágyazás a tanúsítvány a HÁBORÚ fájl** lehetőséget. Ez a beállítás egyszerűvé a tanúsítvány telepítése. Ha inkább meg szeretné őrizni a tanúsítvány külön a HÁBORÚ fájlból, az alábbi módszerrel is használhatja:

1. Belül az **Azure Access vezérlő szolgáltatások szűrés** párbeszédpanel **Biztonság** csoportban írja be a **${Boríték. JAVA_HOME}/MyCert.cer** , és törölje a jelet **a HÁBORÚ fájlban a tanúsítvány beágyazási**. (Ha állítsa át mycert.cer a tanúsítvány fájl neve eltér.) Kattintson a **Befejezés gombra** a párbeszédpanel bezárásához.
2. A tanúsítvány másolja a környezetben elemeként: A Holdas Projektböngésző bontsa ki a **MyAzureACSProject**, kattintson a jobb gombbal a **WorkerRole1**, kattintson a **Tulajdonságok**parancsra, bontsa ki az **Azure szerepkör**és **-összetevők**gombra.
3. Kattintson a **hozzáadása**gombra.
4. Belül a **Összetevő hozzáadása** párbeszédpanelen:
    1. Az **Importálás** csoportban:
        1. Nyissa meg a használni kívánt tanúsítványt használja a **fájl** gombot. 
        2. **A módszer**a **Másolás**gombot.
    2. **Név szerint**kattintson a szövegmezőbe, és elfogadhatja az alapértelmezett nevet.
    3. A **központi telepítés** csoportban:
        1. **A módszer**a **Másolás**gombot.
        2. A **címtárhoz**írja be a **JAVA_HOME %**.
    4. A **Összetevő hozzáadása** párbeszédpanelen az alábbihoz hasonlóan kell kinéznie.

        ![Tanúsítvány-összetevő hozzáadása][add_cert_component]

    5. Kattintson az **OK gombra**.

Ezen a ponton a tanúsítvány felveszik a környezetben. Megjegyzendő, hogy függetlenül-e a tanúsítvány beágyazása a HÁBORÚ fájlt, vagy elemeként vegye fel a üzembe, kell a tanúsítvány feltöltése a névtér [feltöltése egy tanúsítványt az ACS névtér][] szakaszban leírt módon.

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Töltse fel a ACS névtér tanúsítvány]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[a Project-webhely]: http://wastarterkit4java.codeplex.com/releases/view/61026
[A vezérlő Azure szolgáltatás által visszaadott SAML megtekintése]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Access 2.0-s szolgáltatás]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Azure Kezelőportálja segítségével]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 
