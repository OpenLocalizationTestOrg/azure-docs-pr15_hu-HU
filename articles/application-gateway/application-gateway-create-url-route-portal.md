<properties
   pageTitle="Egy alkalmazás átjáró elérési út alapuló szabály létrehozása a portál használatával |} Microsoft Azure"
   description="Megtudhatja, hogy miként alkalmazás átjáró elérési út alapuló szabály létrehozása a portál használatával"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Egy alkalmazás átjáró elérési út alapuló szabály létrehozása a portál használatával

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-url-route-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-url-route-arm-ps.md)

URL-CÍMÉT az elérési út-alapú útválasztás lehetővé teszi, hogy a HTTP-kérelem URL-címe alapján útvonalak társítani. Azt ellenőrzi, hogy be van állítva az URL-cím listák az alkalmazás átjáró háttéradatbázist erőforráskészlethez tartozik út, és küldje el a hálózati forgalmat a definiált háttéradatbázist kvótáját. A közös URL-alapú útválasztás használata másik háttéradatbázis kiszolgáló készletek a különböző tartalomtípusok egyenleg kérelem betöltéséhez.

Az alkalmazás átjáró útválasztás URL-alapú vezet be egy új szabálytípust. Alkalmazás átjáró rendelkezik kétféle szabályt: egyszerű és elérési útját szabályokat. Egyszerű szabálytípus közben elérési út szabályok ciklikus terjesztési mellett a háttéradatbázist készletek ciklikus szolgáltatást, akkor is a kérelem URL-címének elérési_út minta figyelembe veszi a kódmentes készlet kiválasztásakor.

## <a name="scenario"></a>Eset

A következő esetben keresztül elérési út alapuló szabály létrehozása egy meglévő alkalmazás átjáró kerül.
Az alkalmazási példát feltételezi, hogy Ön már lépések [Egy alkalmazás átjáró](application-gateway-create-gateway-portal.md)létrehozásához.

![URL-cím továbbítására][scenario]

## <a name="createrule"></a>Az elérési út alapuló szabály létrehozása

Elérési út alapuló szabály saját figyelő igényel, előtt ellenőrizze a szabály létrehozásának kell használni az elérhető figyelő van.

### <a name="step-1"></a>Lépés: 1

Keresse meg a http://portal.azure.com, és jelölje ki egy meglévő alkalmazás átjáró. Kattintson a **szabályok**

![Alkalmazás átjáró – áttekintés][1]

### <a name="step-2"></a>Lépés: 2

Kattintson az **elérési út-alapú** gombra, új elérési alapuló szabály hozzáadásához.

### <a name="step-3"></a>3 lépés

Az **elérési út alapuló szabály hozzáadása** lap két részből áll. Az első szakasz, ahol definiált a figyelő, a szabály, és az alapértelmezett elérési útja beállításokat nevét. Az alapbeállítások elérési utakat, az elérési út-alapú egyéni útvonal nem alá tartozó készültek. Az **elérési út alapuló szabály hozzáadása** lap, a második szakasz, itt adhatja meg az elérési út szabályok magukat.

**Alapvető beállításai**

- **Név** – Ez az a szabályt, amely a portál elérhető egy rövid nevet.
- **Figyelő** – Ez az a figyelő, amellyel a szabályt.
- **Alapértelmezett kódmentes alkalmazáskészlet** - ezt a beállítást a beállítás, amely meghatározza a háttéradatbázist az alapértelmezett szabály használandó
- **Alapértelmezett HTTP beállítások** – Ez a beállítás a beállítás, amely definiálja az alapértelmezett szabály használandó a HTTP-beállításait.

**Elérési út szabályok**

- **Név** – Ez az elérési út alapuló szabály egy rövid nevet.
- **Elérési út** – Ez a beállítás határozza meg, az elérési utat a szabály fognak kinézni a forgalom átirányítása
- **Kódmentes alkalmazáskészlet** - ezt a beállítást a beállítás, amely meghatározza a háttéradatbázist a szabály használandó
- **HTTP beállítás** – Ez a beállítás beállítás határozza meg a szabály használandó a HTTP-beállításait.

>[AZURE.IMPORTANT] Elérési út: A megfelelő elérési utat mintázatok listája. Minden egyes kell kezdődnie / és a csak egy "\*" engedélyezett végén. Példák /xyz, /xyz* vagy /xyz/*érvényes.  

![Elérési út alapuló szabály lap hozzáadása kitöltött adatokkal][2]

Elérési út alapuló szabály hozzáadása egy meglévő alkalmazás átjáró egy egyszerű folyamatábra a portálon keresztül. Elérési út alapuló szabály létrehozása után, egyszerűen hozzáadása további szabályok szerkeszthető. 

![További elérési út szabályok hozzáadása][3]

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként konfigurálható Azure alkalmazás átjáró, mert az SSL látható [SSL kiürítése konfigurálása](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png