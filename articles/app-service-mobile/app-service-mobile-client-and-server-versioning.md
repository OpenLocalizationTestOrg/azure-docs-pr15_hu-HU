<properties
  pageTitle="Ügyfél- és a Mobile-alkalmazások és a Mobile Services SDK verziószámozás |} Azure alkalmazás szolgáltatás"
  description="Ügyfél SDK listáját, és a Mobile szolgáltatások és Azure Mobile-alkalmazások a kiszolgáló SDK verziókkal való kompatibilitás érdekében"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>A Mobile-alkalmazások és a Mobile szolgáltatások ügyfél- és verziószámozás

Az Azure Mobile Services legújabb verzióját az Azure alkalmazás szolgáltatás **Mobile-alkalmazások** funkciója.

A Mobile-alkalmazások ügyfél- és kiszolgálóoldali SDK eredetileg alapuló a mobil szolgáltatások, de ezek *nem* kompatibilis egymással.
Ez azt jelenti, hogy-kiszolgálóval SDK *Mobile-alkalmazások* és a *Mobile Services*hasonlóan *Mobile-alkalmazások* ügyfél SDK kell használnia. Ezt a szerződést szabja az ügyfél- és kiszolgálóoldali SDK, által használt speciális élőfej érték `ZUMO-API-VERSION`.

Megjegyzés: amikor a dokumentum egy *Mobil-szolgáltatások* kódmentes hivatkozik, azt nem feltétlenül szükséges Mobile szolgáltatások szerepeltethetők. Most egy mobilszolgáltatás kód módosítások nélkül szolgáltatás alkalmazás futtatásához áttelepítendő lehetséges, de a szolgáltatás szokott továbbra is használni *Mobile Services* SDK verziójában.

Többet szeretne tudni az alkalmazás szolgáltatás kód módosítások nélkül áttelepítése, hogy a cikke [áttelepítése Azure alkalmazás szolgáltatás Mobile szolgáltatás].

## <a name="header-specification"></a>Élőfej meghatározása

A kulcs `ZUMO-API-VERSION` a HTTP-fejléc vagy a lekérdezési karakterlánc adható meg. Az űrlap **x.y.z**a verzió karakterlánc érték.

Példa:

Https://service.azurewebsites.net/tables/TodoItem beszerzése

FEJLÉCEK: ZUMO API-VERZIÓJÚ: 2.0.0

BEJEGYZÉS https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Leiratkozás verzió ellenőrzése

Választhatja ki verzió ellenőrzése értéke **Igaz,** az alkalmazás **MS_SkipVersionCheck**beállítás megadásával. Adja meg, a Web.config vagy az Azure portál beállításai szakaszában.

> [AZURE.NOTE] Számos működését változásokról Mobile szolgáltatások és a Mobile-alkalmazások, különösen a kapcsolat nélküli szinkronizálás, a hitelesítés és a leküldéses értesítések területek között. Meg kell csak lemondása verzió ellenőrzése annak érdekében, hogy a viselkedési módosítások nem oldaltörés az alkalmazás funkciók teljes ellenőrzése után.

## <a name="summary-of-compatibility-for-all-versions"></a>Az összes verzió kompatibilitási összefoglalása

A diagram az alábbi ügyfél- és kiszolgálóoldali diagramtípusokat kompatibilitása jeleníti meg. Egy kódmentes mobil **szolgáltatások** és a Mobile- **alkalmazások** alapján a kiszolgáló használt SDK kategóriába tartozik.

|                           | **Mobil szolgáltatások** NODE.js vagy a .NET rendszerhez | **Mobilalkalmazások** NODE.js vagy a .NET rendszerhez |
| ----------                | -----------------------             |   ----------------              |
| [Mobil szolgáltatások ügyfelek] | oké                                  | Hibaüzenet\*                         |
| [Ügyfelek Mobile-alkalmazások]     | Hibaüzenet\*                             | oké                              |

\*Ez a **MS_SkipVersionCheck**megadásával szabályozható.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobil szolgáltatások ügyfél- és kiszolgálóoldali

Az ügyfél SDK az alábbi táblázatban olyan **Mobile Services**szolgáltatással kompatibilis.

Megjegyzés: a mobil ügyfélprogram SDK *nem* küldése élőfej érték `ZUMO-API-VERSION`. Ha a szolgáltatás érkezik, a fejléc vagy a lekérdezési karakterlánc, visszatér hibát jelez, hacsak ki a fentebb ismertetett kifejezetten inaktív.

### <a name="MobileServicesClients"></a>Mobil *szolgáltatások* ügyfél SDK

| Ügyfél-platform                   | Verzió                                                                   | Verzió fejléc értéke |
| -------------------               | ------------------------                                                  | -------------------  |
| Felügyelt ügyfél (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | a #hiányzik                  |
| iOS                               | [2.2.2.](http://aka.ms/gc6fex)                                             | a #hiányzik                  |
| Android-alapú                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | a #hiányzik                  |
| A HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | a #hiányzik     |

### <a name="mobile-services-server-sdks"></a>*Mobil kiszolgálójának SDK*

| Kiszolgálói platform  | Verzió                                                                                                        | Elfogadott verzió élőfej |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [WindowsAzure.MobileServices.Backend.* verzió 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Nincs verzió fejléc** |
| NODE.js          | (hamarosan)                        | **Nincs verzió fejléc** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Mobil szolgáltatások háttérkiszolgálókon viselkedése

| ZUMO-API-VERZIÓ | MS_SkipVersionCheck értéke | Válasz |
| ---------------- | ---------------------------- | -------- |
| Nincs megadva    | Bármely                          | 200 - OK |
| Bármilyen érték        | Igaz                         | 200 - OK |
| Bármilyen érték        | Hamis vagy nincs megadva          | 400 – Hibás kérés |

## <a name="2.0.0"></a>Azure Mobile-alkalmazások ügyfél- és kiszolgálóoldali

### <a name="MobileAppsClients"></a>*Alkalmazások* mobilügyfél SDK

Verzió ellenőrzése jelent meg az ügyfél SDK csomagjában talál a következő verzióival **Azure Mobile**-appokról indítása:

| Ügyfél-platform                   | Verzió                   | Verzió fejléc értéke |
| -------------------               | ------------------------  | -----------------    |
| Felügyelt ügyfél (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android-alapú                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobil SDK *Apps* -kiszolgáló

A következő kiszolgálói SDK verziók szerepel a verzió ellenőrzése:

| Kiszolgálói platform  | SDK                                                                                                        | Elfogadott verzió élőfej |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| NODE.js          | [Azure-mobile-alkalmazások)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Háttérkiszolgálókon Mobile-alkalmazások működése

| ZUMO-API-VERZIÓ | MS_SkipVersionCheck értéke | Válasz |
| ---------------- | ---------------------------- | -------- |
| x.y.z vagy a Null    | Igaz                         | 200 - OK |
| NULL             | Hamis vagy nincs megadva          | 400 – Hibás kérés |
| 1.x.y            | Hamis vagy nincs megadva          | 400 – Hibás kérés |
| 2.0.0-2.x.y      | Hamis vagy nincs megadva          | 200 - OK |
| 3.0.0-3.x.y      | Hamis vagy nincs megadva          | 400 – Hibás kérés |


## <a name="next-steps"></a>Következő lépések

- [Egy mobilszolgáltatás áttelepítése az Azure alkalmazás szolgáltatás]


[Mobil szolgáltatások ügyfelek]: #MobileServicesClients
[Ügyfelek Mobile-alkalmazások]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Egy mobilszolgáltatás áttelepítése az Azure alkalmazás szolgáltatás]: app-service-mobile-migrating-from-mobile-services.md

