<properties
   pageTitle="Projektek frissítéséről az Azure-eszközök a legújabb verzióra |} Microsoft Azure"
   description="Megtudhatja, hogy miként Azure projektté a Visual Studio frissítsen az aktuális verziója az Azure eszközök"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Projektek frissítéséről az Azure-eszközök a legújabb verzióra for Visual Studio

## <a name="overview"></a>– Áttekintés

Miután telepítette az Azure eszközök (vagy egy újabb 1,6-nél korábbi verzióból) a jelenlegi verziójában, egy Azure eszközökkel készített projektek engedje fel az 1,6 előtt (November 2011) automatikusan frissíteni fogják, amint az Ön megnyitotta őket. Ha ezek az eszközök 1,6 (November 2011) kiadását használatával létrehozott projektek, és továbbra is telepítve kiadás, nyissa meg az adott projekteket régebbi verziójában, és című dönt, hogy frissítse őket.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Hogyan változik a projekt, verzióra történő frissítés után

Ha automatikusan frissíti a projekt, illetve megadhatja, hogy azt frissíteni szeretné, a projekt bizonyos szerelvények aktuális verzióival folytatott munka módosul, és néhány tulajdonság szintén módosul, ez a szakasz ismerteti. Ha a projekt többi a változtatásokat az eszközök újabb verziójával kompatibilis igényel, ezek a módosítások manuálisan kell végeznie.

- A fájlt a webes szerepkörök és az app.config fájl dolgozói szerepkörök frissülnek Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll újabb verziója hivatkozni szeretne.

- A Microsoft.WindowsAzure.StorageClient.dll Microsoft.WindowsAzure.Diagnostics.dll és Microsoft.WindowsAzure.ServiceRuntime.dll szerelvények újabb verziói frissülnek.

- Egy külön fájlba, a bővítmény .azurePubXml **Közzététel** alkönyvtárban az Azure projektfájlba (.ccproj) tárolt közzététel profilok kerülnek.

- Az új és megváltozott funkciókat támogatja a közzététel profilban lévő egyes tulajdonságai frissülnek. **AllowUpgrade** helyettesíti be **DeploymentReplacementMethod** , mert egy telepített felhőszolgáltatásba egyidejű vagy fokozatosan frissítheti.

- A tulajdonság **UseIISExpressByDefault** hozzáadott és hamis beállítva, hogy az érintett webkiszolgálóra, a hibakereséshez használt IIS Express automatikus nem módosítása az Internet Information Services (IIS). Az alapértelmezett webkiszolgáló projektekhez, az eszközök újabb verzióiban létrehozott IIS Express.

- Ha egy vagy több a projekt szerepkörök Azure gyorsítótárazás üzemelteti, néhány tulajdonság a szolgáltatás beállítása (.cscfg-fájl) és a szolgáltatás definition (.csdef-fájl) módosul, projekt frissítésekor. Ha a projekt az Azure gyorsítótárazás NuGet-csomagot használ, a projekt frissítése a csomag a legújabb verzióra. Érdemes nyissa meg a fájlt, és ellenőrizze, hogy az ügyfél beállításának maradt megfelelően a frissítési folyamat során. Ha a NuGet csomag használata nélkül hozzáadott Azure gyorsítótár-ügyfél összeállítások hivatkozik, ezek az összeállítások nem frissülnek az; Ezeket a hivatkozásokat, új verzióira manuálisan kell frissítenie.

>[AZURE.IMPORTANT] F # projektekhez kézzel kell frissítenie Azure összeállítások mutató hivatkozásokat, hogy azok hivatkozást e összeállítások újabb verziója.

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>A jelenlegi kiadás Azure projektben frissítéséről

1. Az Azure eszközök aktuális verziójának telepítése a Visual Studio a frissített projekt használni kívánt telepítés be, és nyissa meg a frissíteni kívánt projektet. Ha a projekt egy Azure eszközzel létrehozott 1,6 előtt engedje fel (November 2011), a project automatikusan frissíti a legújabb verzióra. Ha a projekt létrehozásának a November 2011 és engedje fel, és kiadás még telepítve van, a projekt nyílik meg, hogy a verzióban.

1. A megoldás Intézőben és a projekt csomópontját helyi menüjének megnyitása, válassza a **Tulajdonságok parancsot**, és válassza az **alkalmazás** lapon, a megjelenő párbeszédpanel.

    Az **alkalmazás** lapon látható az eszközök verziója, amely a projekt van társítva. Ha az aktuális verziójának Azure eszközök jelenik meg, a projekt már frissült. Ha telepítette az eszközök újabb verziója, mint a lapon láthatók, megjelenik egy **frissítés** gomb.

1. Válassza az eszközök aktuális verziójának frissítése a projekt a **frissítés** gombra.

1. Hozza létre a projekt, és kattintson a cím a hibák API módosításainak eredményeként létrejövő. Az új verzió a kód módosításával kapcsolatos további tudnivalókért olvassa el a az adott API dokumentációját.
