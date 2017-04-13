<properties 
    pageTitle="Hibaelhárítás Analytics – az alkalmazás az összefüggéseket a hatékony keresés eszköz |} Microsoft Azure" 
    description="Alkalmazás háttérismeretek analytics problémákat? Innen kell kiindulni. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/11/2016" 
    ms.author="awills"/>


# <a name="troubleshoot-analytics-in-application-insights"></a>Az alkalmazás az összefüggéseket Analytics – problémamegoldás


[Alkalmazás háttérismeretek Analytics](app-insights-analytics.md)problémákat? Innen kell kiindulni. Analytics segédprogram a hatékony keresés a Visual Studio alkalmazásban az összefüggéseket.



## <a name="limits"></a>Korlátai

* Jelenleg a lekérdezés eredményét fognak korlátozódni csak a múlt adatokat hetente fölé.
* Hogy tesztelje a böngészők: a Chrome, a szegély és az Internet Explorer legújabb verziója.


## <a name="known-incompatible-browser-extensions"></a>Ismert kompatibilis böngészőbővítményeket

* Ghostery

A bővítmény letiltása vagy más böngészőt használ.


##<a name="e-a"></a>"Váratlan hiba"

![Váratlan hiba képernyő](./media/app-insights-analytics-troubleshooting/010.png)

Belső hiba történt a portál futtatókörnyezet – esetén nem kezelt kivétel.

* A böngésző gyorsítótárát jelenjenek meg. 

## <a name="e-b"></a>403... Próbálja meg ismételt betöltése

![403... Próbálja meg ismételt betöltése](./media/app-insights-analytics-troubleshooting/020.png)

Hitelesítés kapcsolatos hiba (hitelesítés vagy hozzáférési jogkivonat előállítása során). Előfordulhat, hogy a portálon nem tudja helyreállítani böngésző beállításainak módosítása nélkül.

* Ellenőrizze, hogy [engedélyezve van a külső cookie-k használata](#cookies) a böngészőben. 


## <a name="authentication"></a>403... biztonsági zóna ellenőrzése

![403.. .verify biztonsági zóna](./media/app-insights-analytics-troubleshooting/030.png)

Hitelesítés kapcsolatos hiba (hitelesítés vagy hozzáférési jogkivonat előállítása során). Előfordulhat, hogy a portálon nem tudja helyreállítani böngésző beállításainak módosítása nélkül.

1. Ellenőrizze, hogy [engedélyezve van a külső cookie-k használata](#cookies) a böngészőben. 

2. Használta egy felvétele a Kedvencek közé, a könyvjelző vagy a mentett hivatkozásra kattintva nyissa meg az Analytics-portálra? Vannak, hogy jelentkezett be eltérő, a hivatkozás mentésekor használt hitelesítő adatok?

2. Próbálja meg használni egy a-magánjellegű/incognito böngészőablakban (a minden ablak bezárása) után. Be kell adnia a hitelesítő adatokat. 

2. Nyisson meg egy másik (normál) böngészőablakot, és nyissa meg az [Azure](https://portal.azure.com). Jelentkezzen ki. Ezután nyissa meg a hivatkozást, és be kell jelentkeznie a helyes hitelesítő adatokat.

2. Széle és az Internet Explorer felhasználók is megnyithatja ezt a hibát, ha megbízható zóna beállítások használatának támogatása megszűnik.

    Győződjön meg róla [Analytics-portál](https://analytics.applicationinsights.io) és [Azure Active Directory-portálon](https://portal.azure.com) is ugyanabban a biztonsági zónában:

 * Az Internet Explorerben nyissa meg az **Internetbeállítások párbeszédpanel**, a **Biztonság**, a **Megbízható helyek**, a **webhelyek**:

    ![Internetbeállítások párbeszédpanel hely hozzáadása a megbízható helyek](./media/app-insights-analytics-troubleshooting/033.png)

    Webhelyek listájában, ha az alábbi URL-ek szerepelnek, győződjön meg arról, hogy a többiek számításba veszi is:

    https://Analytics.applicationinsights.IO<br/>
   https://Login.microsoftonline.com<br/>
   https://Login.Windows.NET


## <a name="e-d"></a>404... Az erőforrás nem található

![404... erőforrás nem található](./media/app-insights-analytics-troubleshooting/040.png)

Alkalmazás erőforrás alkalmazás az összefüggéseket a törölt, és nem érhető el többé. Ez akkor fordulhat elő, ha az URL-címet az elemzés lap mentette.


## <a name="e-e"></a>403... Nincs engedély

![403... nincs engedélyezve](./media/app-insights-analytics-troubleshooting/050.png)

Nincs engedélye az alkalmazás megnyitásához Analytics.

* Szerezte be a hivatkozást valaki más át? Kérje meg őket, hogy Ön a [olvasók vagy az erőforrás csoport munkatársak](app-insights-resources-roles-access-control.md).
* Történt-e menteni a hivatkozást, eltérő hitelesítő adatokkal? Nyissa meg az [Azure portál](https://portal.azure.com), jelentkezzen ki, és próbálja meg ezt a hivatkozást újra, adja meg a megfelelő hitelesítő adatokat.

## <a name="html-storage"></a>403... HTML5-ös tárhely

A portálon használja a HTML5-ös localStorage és sessionStorage.

* A Chrome: Beállítások, adatvédelmi, a tartalom beállításait.
* Az Internet Explorer: Internetbeállítások, Speciális lap biztonság, a DOM-tárolás engedélyezése


![403... megpróbálja a HTML5-ös tárolás engedélyezése](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404... Nem található előfizetés


![404... Nem található előfizetés](./media/app-insights-analytics-troubleshooting/070.png)

Az URL argumentum valamelyike érvénytelen. 

* Nyissa meg az app erőforrás [Alkalmazás háttérismeretek](https://portal.azure.com)portálon. Ezután hajtsa végre a Analytics gombra.

## <a name="e-h"></a>404... lap nem létezik

![404... Lap nem létezik](./media/app-insights-analytics-troubleshooting/080.png)

Az URL argumentum valamelyike érvénytelen.

* Nyissa meg az app erőforrás [Alkalmazás háttérismeretek](https://portal.azure.com)portálon. Ezután hajtsa végre a Analytics gombra.

## <a name="cookies"></a>Külső cookie-k engedélyezése

  [Hogyan lehet letiltani a harmadik féltől származó sütik](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers)megjelennek, de figyelje meg, hogy **engedélyeznie** kell őket.

## <a name="e-x"></a>Ha nem sikerül egy összes egyéb    

[Kapcsolatfelvétel](app-insights-get-dev-support.md).
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

