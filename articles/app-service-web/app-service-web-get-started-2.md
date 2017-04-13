<properties
    pageTitle="Funkciók felvétele a az első web App alkalmazásba"
    description="Izgalmas funkciók felvétele az első webalkalmazást néhány perc alatt."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Funkciók felvétele a az első web App alkalmazásba

[Az első öt perc az Azure web app Deploy](app-service-web-get-started.md), a minta webalkalmazást [Azure alkalmazás](../app-service/app-service-value-prop-what-is.md)szolgáltatás telepítését. Ebben a cikkben gyorsan néhány nagyszerű funkciókkal jegyezze fel a a telepített web App alkalmazásban. Néhány perc alatt lesz:

- a hivatkozási hitelesítés, a felhasználók számára
- az alkalmazás automatikus méretezése
- az alkalmazás teljesítményének értesítések fogadása

Melyik minta alkalmazást, az előző cikkben telepítette, függetlenül attól, kövesse az oktatóprogram során.

Ebben az oktatóanyagban három tevékenységek csak néhány példa a számos hasznos szolgáltatást, amikor a web app alkalmazás szolgáltatásban táblázatát. Érhetők el az **ingyenes** réteg szolgáltatásainak nagy része (amely a Mi az első webalkalmazást fut), és próbálja ki az magasabb árak rétegek igénylő a próbaverzió credits is használhatja. A többi biztosítani, hogy a a web app **ingyenes** réteg marad, kivéve ha Ön kifejezetten változik egy másik árak réteg.

>[AZURE.NOTE] A web app Azure CLI a létrehozott **ingyenes** réteg, amely csak lehetővé teszi, hogy egy megosztott virtuális-példányt tartson a kiszolgálóerőforrás-kvótájának fut. További tájékoztatást a **szabad** réteg az első című cikkben találhat [Alkalmazás szolgáltatás](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>A felhasználók hitelesítő

Most vegyük megtudhatja, hogy milyen könnyen hitelesítési hozzáadása az app ( [Alkalmazás szolgáltatás hitelesítési/engedélyt](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)a További olvasnivaló).

1. Az alkalmazás portál lap, amely csak a megnyitott, kattintson a **Beállítások** > **hitelesítési / engedélyezési**.  
    ![Hitelesítő - beállítások lap](./media/app-service-web-get-started/aad-login-settings.png)

2. Kattintson **a** hitelesítés bekapcsolásához.  

4. **Hitelesítésszolgáltatók**az **Azure Active Directory**gombja  
    ![Hitelesítő - jelölje ki az Azure Active Directory](./media/app-service-web-get-started/aad-login-config.png)

5. Az **Azure Active Directory-beállítások** lap kattintson a **gyors**gombra, majd kattintson az **OK gombra**. Az alapértelmezett beállítások hozzon létre egy új alapértelmezett címtárában Azure AD-alkalmazást.  
 ![Hitelesítő - express konfiguráció](./media/app-service-web-get-started/aad-login-express.png)

6. Kattintson a **Mentés**gombra.  
    ![Hitelesítő - konfiguráció mentése](./media/app-service-web-get-started/aad-login-save.png)

    Miután a változtatások sikeres, megjelenik az értesítési harang, kapcsolja be a zöld, valamint egy rövid üzenetet.

7. Kattintson az alkalmazás portál lap, a hivatkozás **URL-cím** (vagy **Keresse meg** a menüsávon). A hivatkozás nem HTTP-cím.  
    ![Hitelesítő - tallózással keresse meg azt az URL-címe](./media/app-service-web-get-started/aad-login-browse-click.png)  
    De egy új lapon nyílik meg az alkalmazást, az URL-cím átirányítások többször mezőbe, és a fejeződik be az alkalmazás, a HTTPS-címre. Mi jelenik meg az Azure-előfizetése van, hogy Ön már bejelentkezve, és esetén automatikusan hitelesíteni a alkalmazásban.  
    ![Hitelesítő - e jelentkezve](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Így most nyisson meg egy nem hitelesített munkamenetet egy másik böngészőben, ha egy bejelentkezési képernyője láthatja, nyissa meg az azonos URL-címet.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Ha soha nem megtette az Azure Active Directory címtárral semmit, az alapértelmezett könyvtár előfordulhat, hogy nincs bármely Azure Active Directory-felhasználók. Ebben az esetben valószínűleg az ott egyetlen fiók az Azure előfizetés a Microsoft-fiókjával. Ezért voltak automatikusan bejelentkezve az ugyanabban a böngészőben korábbi alkalmazásban.
   Hogy egy Microsoft-fiók használatával jelentkezzen be a bejelentkezési lapon is.

Gratulálunk, akik a minden forgalom a web App hitelesítést.

A láthatta a **hitelesítési / engedélyezési** végezhető sokkal több, mint például a lap:

- Közösségi bejelentkezési engedélyezése
- Több bejelentkezési beállítások engedélyezése
- Személyek először nyissa meg az alkalmazást az alapértelmezett működés módosítása

Alkalmazás szolgáltatással néhány gyakori hitelesítés bekapcsolása billentyűs megoldást van szüksége, így nem kell adnia a hitelesítési logika saját maga.
További tudnivalókért lásd: [Alkalmazás szolgáltatás hitelesítési/engedélyt](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Az alkalmazás automatikusan alapján igény szerinti méretezése

A következő, nézzük Automatikus méretezéssel az alkalmazást, hogy akkor fog automatikusan korrigálhatja kapacitása ahhoz, hogy a felhasználó igény szerint ( [skála be az alkalmazás Azure-ban](web-sites-scale.md) és a [példányok száma méretezni, automatikusan vagy manuálisan](../monitoring-and-diagnostics/insights-how-to-scale.md)További olvasnivaló) válaszolni.

A web app röviden kétféleképpen méretezni:

- [Méretezni](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): további Processzor, memóriát, lemezterületet és további szolgáltatásokat, például a dedikált VMs, az egyéni tartományok és a tanúsítványok, a helyek, autoscaling és az egyéb átmeneti tárolására. Az alkalmazás szolgáltatás terv tartozik, az alkalmazás a árak réteg módosításával méretezni.
- [Ki méretarány](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): az alkalmazás futó példányok virtuális számának növelése.
Méretezheti akár 50 példányaiban, attól függően, hogy a árak réteg.

További ado, nélkül vegyük beállítása autoscaling.

1. Először nézzük szerkezetének kialakítása ahhoz, hogy autoscaling. Az alkalmazás portál lap, kattintson a **Beállítások** > **Skála felfelé (alkalmazás szolgáltatás megtervezése)**.  
    ![Fel - beállítások lap méretezése](./media/app-service-web-get-started/scale-up-settings.png)

2. Görgessen le, és válassza a **Normál S1** réteg, a legalacsonyabb réteg, amely támogatja a autoscaling (bekarikázott képernyőképen), majd kattintson a **Jelölje ki**.  
    ![Méretezni,-szint kiválasztása](./media/app-service-web-get-started/scale-up-select.png)

    Ezzel elkészült méretezési be.

    >[AZURE.IMPORTANT] A réteg az ingyenes próbaverzió credits kiadásokból. Használati fizetési fiókkal rendelkezik, ha azt, a fiókjához költségeket vonz.

3. Ezután autoscaling vegyük konfigurálása. Az alkalmazás portál lap, kattintson a **Beállítások** > **Skála ki (alkalmazás szolgáltatás megtervezése)**.  
    ![Méretezés ki - beállítások lap](./media/app-service-web-get-started/scale-out-settings.png)

4. **A skála** átállítása **Processzor százalékos**. A csúszkák a legördülő menü alatt árnyalatait. Adjon egy **1** és **2** és egy **tartományt a célhely** **40** és a **80**között **példányok** között. Írja be a mezőbe, vagy a csúszka mozgatásával, tegye meg.  
 ![Méretezése - autoscaling konfigurálása](./media/app-service-web-get-started/scale-out-configure.png)

    Ebben a konfigurációban alapján, az alkalmazás automatikusan méretezze át amikor processzort feletti 80 %-át és méretezze át a 40 % alatti Processzor kihasználtsági esetén.

5. A menüsávon kattintson a **Mentés** gombra.

Gratulálunk, az alkalmazás autoscaling.

A **Méretarány-beállításai** lap végezhető sokkal több, mint például a láthatta:

- Adott számú példányok manuális méretezése
- Más teljesítménymutatók, például a memória százalék vagy lemez várólista méretarányát
- Méretezési beállításainak testreszabása teljesítmény szabály elindításakor
- Automatikus méretezéssel időközönként
- Egy jövőbeli esemény autoscaling értesítés beállítása

További információt a méretezés fel az alkalmazást olvassa el a [méretezni az Azure-alkalmazás mentése](../app-service-web/web-sites-scale.md)című témakört. Méretezés kifelé kapcsolatos további tudnivalókért olvassa el a [kézzel és automatikusan átméretezheti a példányok száma](../monitoring-and-diagnostics/insights-how-to-scale.md)című témakört.

## <a name="receive-alerts-for-your-app"></a>Az alkalmazás riasztást megkapnak

Most, hogy az alkalmazás autoscaling, mi történik, ha a maximális példányok száma (2) eléri pedig Processzor fölé a kívánt kihasználtság (80 %)?
Értesítés (a [fogadás értesítések](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)További olvasnivaló) tájékoztat ebben az esetben, további méretezhet fel-, illetve arról az alkalmazást, például beállíthatja. Vegyük pillanatok alatt beállíthatnak ebben az esetben értesítés.

1. Kattintson az alkalmazás portál lap, **eszközök** > **értesítések**.  
    ![Figyelmeztetések - beállítások lap](./media/app-service-web-get-started/alert-settings.png)

2. Kattintson a **Hozzáadás értesítés**. Az **erőforrások** mezőben válassza az erőforrás, amely **(serverfarms)**végződik. Ez az alkalmazás szolgáltatáscsomagja.  
    ![Értesítések – az alkalmazás szolgáltatáscsomagja értesítés hozzáadása](./media/app-service-web-get-started/alert-add.png)

3. Adja meg a **nevét** , mint `CPU Maxed`, **metrikus** **Processzor százalékos**, valamint **küszöbérték** , `90`, majd jelölje be a **tulajdonosok e-mailek, a munkatársak, és a olvasók**, és kattintson **az OK**gombra.   
 ![Figyelmeztetések - értesítés beállítása](./media/app-service-web-get-started/alert-configure.png)

    Azure befejeződése után az értesítés létrehozása, megjelenik a **riasztások** lap.  
    ![Figyelmeztetések - kész megtekintése](./media/app-service-web-get-started/alert-done.png)

Gratulálunk, értesítések, amelyről most.

A riasztási beállítás Processzor kihasználtsági öt percenként ellenőrzi. Ha a szám kerül 90 %-nál, kap e-mailben riasztás, és bárki, aki engedéllyel rendelkezik. Bárki, aki jogosult-e a riasztást megkapnak megjelenítéséhez térjen vissza az alkalmazás a portál lap, és kattintson a **kezelés** gombra.  
![Lásd: a milyen esetekben az értesítéseket](./media/app-service-web-get-started/alert-rbac.png)

Győződjön meg arról, hogy **előfizetése rendszergazdáknak** már a **tulajdonos** , az alkalmazás is megtalálja. Ezt a csoportot tartalmaz, ha a fiók rendszergazda az Azure előfizetése (például a próba-előfizetés). Azure szerepköralapú hozzáférés-vezérlés a további tudnivalókért olvassa el a [Azure Role-Based hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)című témakört.

> [AZURE.NOTE] A figyelmeztetési szabályok Azure fiókfunkció. További információért [fogadási riasztási értesítés](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)jelenik meg.

## <a name="next-steps"></a>Következő lépések

Az értesítés beállítása útközben láthatta az **eszközök** lap eszközök széles körű. Ebben az esetben elháríthatja a problémák, teljesítményének figyelése, biztonsági tesztelése, erőforrások kezelése, együttműködhet a virtuális konzol és hasznos-bővítmények. Meghívása azt, hogy ezek közül az eszközök fel az egyszerű, mégis hatékony eszközöket az ujját tippek a mindegyikre gombra.

Megtudhatja, hogy miként további lehetőségek a telepített alkalmazással. Az alábbiakban csak részleges listája:

- [Vásárol, és állítsa be a saját tartománynév](custom-dns-web-site-buydomains-web-app.md) - helyett a web App-vonzó tartomány vásárlása a *. azurewebsites.net tartományt. Vagy olyan tartomány használata, akkor már van.
- [Átmeneti tárolására környezetekben beállítása](web-sites-staged-publishing.md) - átmeneti tárolásra szolgáló URL-címre, éles helyezés előtt telepítse az alkalmazást. Frissítse az élő webalkalmazást biztonságos. Állítson be egy összetett DevOps megoldás több telepítési helyek.
- A [folyamatos telepítési beállítása](app-service-continuous-deployment.md) - alkalmazások telepítésének integráció az adatforrás-vezérlő rendszerbe. Telepítse az Azure a minden jóváhagyás.
- [Az access a helyszíni erőforrások](web-sites-hybrid-connection-get-started.md) - az Access egy meglévő helyszíni adatbázis vagy CRM rendszerrel.
- Háttér beállítása, [készítsen biztonsági másolatot az alkalmazás](web-sites-backup.md) - be, és visszaállíthatja a webalkalmazásban. Felkészülés a váratlan hibák és helyreállítása őket.
- [A diagnosztikai naplók engedélyezése](web-sites-enable-diagnostic-log.md) – olvasni az IIS naplók Azure vagy alkalmazás nyomkövetések. Olvasás adatfolyam, töltse le őket, vagy port őket az [Alkalmazás az összefüggéseket](../application-insights/app-insights-overview.md) a bekapcsolása billentyűs elemzéshez.
- [Az alkalmazás biztonsági az ellenőrzési](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
beolvasása a webalkalmazás [Tinfoil biztonsági](https://www.tinfoilsecurity.com/)által nyújtott szolgáltatás használatával modern kockázatok ellen.
- [Futtatása a háttérben feladatok](../azure-functions/functions-overview.md) - Futtatás feladatok adatkezelési jelentéskészítési stb.
- [Ismerje meg az App szolgáltatás működése](../app-service/app-service-how-works-readme.md)
