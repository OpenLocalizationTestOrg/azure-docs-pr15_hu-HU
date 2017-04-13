<properties
   pageTitle="Hitelesítése és a Power BI beágyazott engedélyezése"
   description="Hitelesítése és a Power BI beágyazott engedélyezése"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Hitelesítése és a Power BI beágyazott engedélyezése

A Power BI beágyazott szolgáltatást használja **kulcsok** és az **Alkalmazás tokenek** hitelesítési és engedélyezési explicit végfelhasználói hitelesítési helyett. Az alkalmazás a modellben kezeli a hitelesítési és engedélyezési a végfelhasználók számára. Ha szükséges, az alkalmazás hoz létre, és az alkalmazás tokenek, amely arra utasítja jeleníti meg a kért jelentés a szolgáltatás elküldi. A tervezés nincs szükség a felhasználói hitelesítés és a hitelesítés, Azure Active Directory használható az alkalmazás, de még mindig nem tud.

## <a name="two-ways-to-authenticate"></a>Kétféleképpen hitelesítést végezni

**Billentyű** - kulcsok is használhatja az összes Power BI beágyazott REST API-hívások. A billentyűk kattintson **az összes beállítások ikonra** , majd a **hívóbetűk**az **Azure portálon** található. A kulcs mindig kezelje, mintha egy jelszót. Billentyűk, hogy minden REST API-t egy adott munkaterület webhelycsoport telefonál engedélyekkel rendelkezni.

A többi hívás termékkulccsal, a következő engedélyezési élőfej hozzáadása:            

    Authorization: AppKey {your key}

**Alkalmazás jogkivonat** - alkalmazás tokenek használnak az összes beillesztése kérések. Éppen azokat, amelyeket ügyféloldali, futtatható, azokat egyetlen jelentés korlátozva van, és a lejárat időpontja értékre.

Alkalmazás tokenek egy JWT (JSON webes jogkivonat), amelyek közül a kulcsok aláírtak.

Az alkalmazás jogkivonat a következő jogcímalapú tartalmazhatják:

| Igénylése      | Leírás        |
|--------------|------------|
| **ver**      | Az alkalmazás jogkivonathoz verziója. 0.2.0 az aktuális verziója.       |
| **és**      | A címzetthez a jogkivonat. A Power BI beágyazott használatra: "https://analysis.windows.net/powerbi/api".  |
| **iss**      |  Egy karakterlánc, amely az alkalmazás a token kiadó.    |
| **típus**     | A létrehozott alkalmazás jogkivonat típusát. Aktuális az egyetlen támogatott típus jelű **beágyazása**   |
| **Windows azonnali csatlakozás**      | Munkaterület a webhelycsoport neve a token küldése történik.  |
| **WID**      | Munkaterület-Azonosítóját a token küldése történik.  |
| **volna**      | Jelentés Azonosítót a token küldése történik.     |
| **felhasználónév** (nem kötelező) |  RLS használata esetén ez egy olyan karakterlánc, amely segíthet azonosítani a felhasználót, amikor RLS szabályok alkalmazására. |
| **szerepkörök** (nem kötelező)   |   Jelölje ki a sorban a biztonsági szint szabályok alkalmazásakor a szerepkörök tartalmazó karakterlánc. Ha egynél több szerepkör átadása, át kell őket karakterlánc tömbként.    |
| **Exp** (nem kötelező)    |   Azt jelzi, hogy az idő, amelyben a token lejár. Ezek kell lennie át a Unix időbélyegeket.   |
| **NBF** (nem kötelező)    |   Azt jelzi, hogy a indításakor, amelyben a token alatt érvényes. Ezek kell lennie át a Unix időbélyegeket.   |

A minta alkalmazás jogkivonat fog kinézni:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Ha dekódolva, így jelenik meg:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Íme, a munkafolyamat működése

1. Másolja az API-kulcsok az alkalmazás. A billentyűk elérheti az **Azure-portálon**.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Jogkivonat igényt asserts és az elévülési időt.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Jogkivonat-API hívóbetűk kap aláírva.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Felhasználói kérések a jelentés megtekintéséhez.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Jogkivonat-API hívóbetűk az érvényességét.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  A Power BI beágyazott küld egy jelentés felhasználói.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Miután a **Power BI beágyazott** elküld egy jelentést a felhasználónak, a felhasználó megtekintheti a jelentést az egyéni alkalmazásban. Például az [értékesítési adatok PBIX elemzése minta](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix)importált, ha a minta web app módon néz ki:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Lásd még:
- [Ismerkedés a Microsoft Power BI beágyazott minta](power-bi-embedded-get-started-sample.md)
- [Microsoft Power BI beágyazott tipikus esetei](power-bi-embedded-scenarios.md)
- [Ismerkedés a Microsoft Power BI beágyazott](power-bi-embedded-get-started.md)
