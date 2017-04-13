<properties
   pageTitle="Hitelesítő adatok tó tárolóval, az Active Directory segítségével |} Microsoft Azure"
   description="Megtudhatja, hogy miként hitelesítő adatok tó tárolóval, az Active Directory használata"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Azure Active Directory használata adatok tó áruházzal végfelhasználói hitelesítés

> [AZURE.SELECTOR]
- [Szolgáltatás hitelesítés](data-lake-store-authenticate-using-active-directory.md)
- [Végfelhasználói hitelesítés](data-lake-store-end-user-authenticate-using-active-directory.md)


Azure tó adattár Azure Active Directory authentication használja. Létrehozása az Azure tó adattár vagy Azure adatok tó Analytics használható kérelmet, előtt döntse el, először hogyan szeretné, hogy az alkalmazás az Azure Active Directory (Azure Active Directory) hitelesítést végezni. A két fő elérhető beállítások a következők:

* Végfelhasználói hitelesítés, és 
* Szolgáltatás hitelesítést. 

Az alkalmazás éppen egy OAuth 2.0-s token, minden kérelme Azure tó adattár vagy Azure adatok tó Analytics kap csatolt ellátni vonhat mindkét beállításokkal.

Ez a cikk megbeszélések arról, hogy miként létrehozása az Azure Active Directory webalkalmazás végfelhasználói hitelesítéshez. A szolgáltatás hitelesítést Azure AD-alkalmazás konfigurációja tanulmányozza [tó áruházzal szolgáltatás hitelesítés használatával az Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Előfeltételek

* Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
* Az előfizetés azonosítójával. Az Azure portálról meghallgathatja. Ha például érhető el a tó adattár fiók lap a.

    ![Előfizetés ID azonosító beszerzése](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Azure Active Directory tartománynevét. Az elemekre az egérrel az Azure portál jobb felső sarkában meghallgathatja. Az alábbi a képernyőképet a tartománynév **contoso.microsoft.com**, és a globálisan egyedi azonosítója zárójelek a bérlői azonosítóját. 

    ![AAD tartomány beszerzése](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Végfelhasználói hitelesítés

Az ajánlott megközelítés, ha azt szeretné, hogy a végfelhasználó jelentkezhet be az Azure AD-e az alkalmazást. Az alkalmazás hozzáférhet az azonos szintű hozzáféréssel rendelkezzenek, a végfelhasználói a naplózott Azure erőforrások lesz. A végfelhasználói kell adnia a hitelesítő adatok rendszeres annak érdekében, hogy az alkalmazás hozzáférést megőrzéséhez.

Jelentkezzen be a végfelhasználói problémákat eredménye, hogy egy hozzáférési jogkivonat, és a frissítési jogkivonat adta-e az alkalmazást. A hozzáférési jogkivonat kap csatolt tó adattár vagy adatok tó Analytics minden kérelme, és azt érvényes alapértelmezés szerint egy órával. A frissítés jogkivonat használható szerezzen be egy új jogkivonat, és ez akkor alapértelmezés szerint legfeljebb két hétig is érvényes, ha rendszeresen használja. A végfelhasználói napló két különböző módszerekkel is használhatja.

### <a name="using-the-oauth-20-pop-up"></a>OAuth 2.0-s előugró ablakok használatával

Az alkalmazás válthat megjelenik egy OAuth 2.0-s engedélyezési előugró ablak, a végfelhasználói adhat meg a hitelesítő adatait. Ez az előugró ablak az Azure Active Directory-kétfaktoros hitelesítés (2FA) eljárással is használható, ha a szükséges. 

>[AZURE.NOTE] Ez a módszer még nem támogatott a a Azure Active Directory Authentication Library (ADAL) Python vagy Java.

### <a name="directly-passing-in-user-credentials"></a>A felhasználói hitelesítő adatokat közvetlenül átadása

Az alkalmazás az Azure Active Directory közvetlenül megadhatja az felhasználói hitelesítő adatokat. Ez a módszer csak akkor működik, szervezeti azonosító felhasználói fiókokkal; még nem kompatibilis a személyes és a "live ID azonosító" felhasználói fiókok, köztük a befejezési @outlook.com vagy @live.com. Ezenkívül ezt a módszert, nem kompatibilis a felhasználói fiókok, hogy az Azure Active Directory-kétfaktoros hitelesítés (2FA).

### <a name="what-do-i-need-to-use-this-approach"></a>Mit kell ezt a módszert használja?

* Azure Active Directory-tartomány nevét. Ez már szerepel a előfeltétel olvashatók.

* Azure Active Directory **webalkalmazáshoz**

* Ügyfél-azonosító az Azure Active Directory webalkalmazáshoz

* Válasz URI az Azure Active Directory webalkalmazáshoz

* Engedélyek beállítása a meghatalmazott

Hozzon létre egy Azure AD-webalkalmazást, és konfigurálja úgy a fent felsorolt követelményeket útmutatásért lásd: az [Active Directory-alkalmazás létrehozása](#create-an-active-directory-application) az alábbi szakasz. 

## <a name="create-an-active-directory-application"></a>Az Active Directory-alkalmazás létrehozása

Ebben a részben azt tudnivalókat létrehozása és konfigurálása az Azure Active Directory webalkalmazás végfelhasználói hitelesítéshez Azure adatok tó áruházzal Azure Active Directory használatával.


### <a name="step-1-create-an-azure-active-directory-application"></a>Lépés: 1: Az Azure Active Directory-alkalmazás létrehozása

>[AZURE.NOTE] Az alábbi lépéseket az Azure-portálon használja. Az Azure Active Directory-alkalmazások [Azure PowerShell](../resource-group-authenticate-service-principal.md) alrendszerrel vagy [Azure CLI](../resource-group-authenticate-service-principal-cli.md)is létrehozhat.

1. Jelentkezzen be az Azure-fiókjába a [Klasszikus portálon](https://manage.windowsazure.com/)keresztül.

2. A bal oldali ablaktáblában válassza ki **Az Active Directory** .

     ![Jelölje ki az Active Directoryban](./media/data-lake-store-end-user-authenticate-using-active-directory/active-directory.png)
     
3. Jelölje ki az Active Directory hozhat létre az új alkalmazás használni kívánt. Ha egynél több Active Directory, általában szeretne létrehozni az alkalmazás a címtár-előfizetése helye. Az előfizetés az előfizetés ugyanabban a mappában alkalmazások csak biztosíthat erőforráshoz való hozzáférés.  

     ![Válassza a címtár](./media/data-lake-store-end-user-authenticate-using-active-directory/active-directory-details.png)
    
    
3. Az alkalmazások címtárában megtekintéséhez kattintson a **alkalmazások**elemre.

     ![alkalmazások megtekintése](./media/data-lake-store-end-user-authenticate-using-active-directory/view-applications.png)

4. Ha még nem létrehozott alkalmazás könyvtár előtt meg kell jelennie valami az alábbi képhez hasonló. **Alkalmazás hozzáadása** gombja

     ![alkalmazás hozzáadása](./media/data-lake-store-end-user-authenticate-using-active-directory/create-application.png)

     Vagy az alsó ablaktáblában kattintson a **Hozzáadás** gombra.

     ![hozzáadása](./media/data-lake-store-end-user-authenticate-using-active-directory/add-icon.png)

6. Nevezze el az alkalmazást, és válassza ki a létrehozni kívánt alkalmazást. Ebben az oktatóprogramban egy **WEBES API WEB APPLICATION Programming és/vagy** hozzon létre, és kattintson a Tovább gombra.

     ![név alkalmazás](./media/data-lake-store-end-user-authenticate-using-active-directory/tell-us-about-your-application.png)

7. Adja meg az alkalmazás tulajdonságait. **SIGN-ON URL-CÍMÉT**adja meg a URI fel egy olyan webhelyet, amely leírja az alkalmazás. A webhely megléte érvényesítése nem volt. Az **Alkalmazás azonosítója URI**adja meg a URI, amely azonosítja az alkalmazást.

     ![szolgáltatásalkalmazás tulajdonságainak](./media/data-lake-store-end-user-authenticate-using-active-directory/app-properties.png)

    Kattintson a jelölőnégyzet be van jelölve a varázsló, és az alkalmazás létrehozása.

### <a name="step-2-get-client-id-reply-uri-and-set-delegated-permissions"></a>Lépés: 2: Az ügyfél-azonosító beszerzése, a válasz URI és meghatalmazott engedélyeinek beállítása

1. Kattintson a **beállítás** lapon állítsa be a jelszót az alkalmazás.

     ![alkalmazások konfigurálása](./media/data-lake-store-end-user-authenticate-using-active-directory/application-configure.png)

2. Másolja az **ügyfél-azonosító**.
  
     ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/client-id.png)

3. **Egyszeri bejelentkezés** csoportjában a **Válasz URI**másolja.

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-get-reply-uri.png)

4. **Engedélyek egyéb alkalmazások**csoportban válassza az **alkalmazás hozzáadása**

    ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

5. Az **engedélyek más alkalmazásokba** varázslóban válassza az **Azure adatok tó** és a **Windows** **Azure szolgáltatás felügyeleti API**, és a pipajeles gombra.

6. Alapértelmezés szerint a **Meghatalmazott engedélyeit** az újonnan hozzáadott szolgáltatások értéke nulla. Kattintson a **Meghatalmazott engedélyeit** az Azure adatok tó és a Windows Azure alkalmazáskezelési szolgáltatás legördülő listában, és a rendelkezésre álló jelölőnégyzetek bejelölésével, hogy az értékek értéke 1. Az eredmény így néz ki.

     ![ügyfél-azonosító](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)

7. Kattintson a **Mentés**gombra.


## <a name="next-steps"></a>Következő lépések

Ez a cikk létrehozása az Azure Active Directory-webalkalmazást, és a ügyfélalkalmazásokban, hogy a szerző elkészíti a .NET SDK, Java SDK stb használata a szükséges információk összegyűjtése. Most is folytassa az alábbi cikkekben, amely a beszélgetés első hitelesítő adatok tó áruházzal, és kattintson a tár egyéb műveletek hajthatók végre az Azure Active Directory webes alkalmazás használatáról.

- [Első lépések az Azure tó adattár .NET SDK használatával](data-lake-store-get-started-net-sdk.md)
- [Első lépések az Azure tó adattár Java SDK használatával](data-lake-store-get-started-java-sdk.md)
