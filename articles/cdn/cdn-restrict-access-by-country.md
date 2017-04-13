<properties
    pageTitle="Korlátozhatja a hozzáférést a Azure CDN-tartalom országonkénti |} Microsoft Azure"
    description="Megtudhatja, hogy miként való hozzáférés korlátozása a Geo szűrés funkcióval Azure CDN-tartalmakat."
    services="cdn"
    documentationCenter=""
    authors="camsoper, rli"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="casoper"/>

#<a name="restrict-access-to-your-content-by-country---verizon"></a>A tartalom ország - Verizon való hozzáférés korlátozása

> [AZURE.SELECTOR]
- [Verizon](cdn-restrict-access-by-country.md)
- [Normál Akamai](cdn-restrict-access-by-country-akamai.md)

##<a name="overview"></a>– Áttekintés

Amikor egy felhasználó a tartalmat, alapértelmezés szerint, a program a tartalom függetlenül, hogy a felhasználó jelzik a kérelem benyújtása fel. Egyes esetekben az országonkénti a tartalmak elérésének korlátozására érdemes. Ebből a témakörből megtudhatja, annak érdekében, hogy állítsa be a szolgáltatás lehetővé teszi a **Geo szűrés** funkció használatáról vagy hozzáférésének letiltása az országonkénti.

> [AZURE.IMPORTANT] A Verizon és Akamai termékek funkcionalitással az azonos geo szűrés, de a felhasználói felület eltér. Ehhez a dokumentumhoz a felület **Azure CDN normál a Verizon** és **Azure CDN-támogatás az Verizon**ismerteti. Geo-szűrés **Azure CDN**standard Akamai az [országonkénti - Akamai a tartalomhoz való hozzáférés korlátozása](cdn-restrict-access-by-country-akamai.md)talál.

Szempontok konfigurálása a korlátozástípust vonatkozó információkért című [előtt megfontolandó kérdések](cdn-restrict-access-by-country.md#considerations) a témakör végén.  

>[AZURE.NOTE] Miután beállította a konfigurációt, az Azure CDN profil összes **Azure CDN Verizon a** végpontok érvényesek.

![Ország szűrése](./media/cdn-filtering/cdn-country-filtering.png)

##<a name="step-1-define-the-directory-path"></a>Lépés: 1: Határozza meg a könyvtár elérési útja

Ország szűrő konfigurálásakor meg kell adnia a relatív elérési útját, amelyre felhasználók biztosított vagy megtagadja a hozzáférést. Az összes fájlok szűrése ország alkalmazhat "/" vagy a kijelölt mappák útjainak megadásával.

Példa a címtár elérési út szűrő:

    /                                 
    /Photos/
    /Photos/Strasbourg

##<a name="step-2-define-the-action-block-or-allow"></a>Lépés: 2: A művelet meghatározása: letiltása vagy engedélyezése

**Tiltása:** Az adott országban felhasználóit nem férhet hozzá az elérési út rekurzív kért eszközökre. Ha nincs más országban szűrési lehetőségek van beállítva az adott helyet, majd a többi felhasználó lesz jogosult access.

**Engedélyezése:** Csak azok a felhasználók, a megadott országokból származó lesz jogosult az elérési út rekurzív kért eszközök a hozzáférést.

##<a name="step-3-define-the-countries"></a>3 lépés: Az országok meghatározása

Jelölje ki a használni kívánt letiltom vagy engedélyezem az elérési út országok. További tudnivalókért lásd: [Azure CDN Verizon ország rendelkező](https://msdn.microsoft.com/library/mt761717.aspx).

A szabály blokkolása /Photos/Strasbourgban/fog szűrheti például fájlok, többek között:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


##<a name="country-codes"></a>Ország kódok

A **Geo szűrés** szolgáltatás meghatározása, amelyhez egy kérelmet biztosított vagy letiltott a védett könyvtárában országok ország kódokat használja. Az ország kódokat az [Azure CDN Verizon ország rendelkező](https://msdn.microsoft.com/library/mt761717.aspx)találja. Ha "Az Európai Unió" (Európa) vagy "Alkalmazás" (ázsiai és csendes-óceáni), egy részét, amely bármely ország származik, ebben a régiók fog tiltott vagy engedélyezett IP-címet ad meg.


##<a id="considerations"></a>Megfontolandó szempontok

- Az ország szűrő beállítás érvénybe léptetéséhez módosítások 90 percig is eltarthat.
- Ez a funkció nem támogatja a helyettesítő karakterek (például "*").
- A szűrés relatív elérési útja társított konfigurációs ország lesz az elérési út rekurzív módon alkalmazza.
- Csak egy szabály alkalmazhatók ugyanazt a relatív az elérési utat (nem hozhatók létre több ország szűrő mutató azonos relatív elérési útja. Mappa azonban a több ország szűrő lehetnek. Az ország szűrő rekurzív jellegét miatt. Más szóval azt az almappát, egy korábban beállított mappát egy másik ország szűrő rendelhetők.
