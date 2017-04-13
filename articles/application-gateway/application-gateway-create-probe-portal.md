<properties
   pageTitle="Hozzon létre egy egyéni vizsgálati alkalmazás átjáró a portál használatával |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy egyéni vizsgálati alkalmazás átjáró, a portál használatával"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Hozzon létre egy egyéni vizsgálati alkalmazás átjáró a portál használatával

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-probe-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-probe-ps.md)
- [Azure klasszikus PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Eset

A következő esetben egy egyéni állapot vizsgálati létrehozása egy meglévő alkalmazás átjáró keresztül kerül.
Az alkalmazási példát feltételezi, hogy Ön már lépések [Egy alkalmazás átjáró](application-gateway-create-gateway-portal.md)létrehozásához.

## <a name="createprobe"></a>A vizsgálati létrehozása

A portálon keresztül lépéssel szondákat vannak beállítva. Első lépésként a vizsgálati létrehozásához, ezután a vizsgálati hozzáadása az alkalmazás átjáró kódmentes http beállításait.

### <a name="step-1"></a>Lépés: 1

Keresse meg a http://portal.azure.com, és jelölje ki egy meglévő alkalmazás átjáró.

![Alkalmazás átjáró – áttekintés][1]

### <a name="step-2"></a>Lépés: 2

Kattintson a **ellenőrzi** , és kattintson a **Hozzáadás** gombra, egy új vizsgálati hozzáadni.

![Adja hozzá a vizsgálati lap kitöltött adatokkal][2]

### <a name="step-3"></a>3. lépés

Töltse ki a vizsgálati szükséges adatait, és teljes kattintson **az OK gombra**.

- **Név** – Ez az a vizsgálati a portál elérhető egy rövid nevet.
- **A host** - a szolgáltató neve, amellyel a vizsgálati. Alkalmazandó csak akkor, ha több helyet be van állítva a alkalmazás átjáró, egyéb esetben a "127.0.0.1" használja. Ez különbözik virtuális host Name mezőbe.
- **Elérési út** – a teljes URL-címét az egyéni vizsgálati maradéka. Érvényes elérési utat kezdődik "/"
- **Időköze (másodperc)** - gyakoriságának a vizsgálati állapot ellenőrzése, hogy fut-e. Az alsó beállítása nem ajánlott 30 másodpercnél.
- **Időtúllépése (másodperc)** – a vizsgálati Várjon, mielőtt időtúllépés idő mennyiségét. Az időkorlát kell lennie elég magas, hogy a http-hívás tehetők annak érdekében, hogy a kódmentes állapota lapján érhető el.
- **Sérült küszöb** - figyelembe veendő sérült sikertelen kísérletek száma. A 0 azt jelenti, hogy ha egy állapotának ellenőrzése sikertelen a háttéradatbázist határozzák meg sérült azonnal küszöbértéknél.

> [AZURE.IMPORTANT] az állomásnév nincs a kiszolgáló nevét. Ez a az application server rendszeren futó virtuális állomás nevét. A vizsgálati a szolgáltatás elküldi a http://(host name):(port from httpsetting)/urlPath

![vizsgálati beállításai][3]

## <a name="add-probe-to-the-gateway"></a>Az átjáró vizsgálati hozzáadása

Most, hogy a vizsgálati létrehozását követően pedig vegye fel az átjárót. Az alkalmazás átjáró kódmentes http beállításai a vizsgálati beállításainak megadása

### <a name="step-1"></a>Lépés: 1

Az alkalmazás átjáró a **HTTP-beállítások** parancsra, és kattintson a jelenlegi kódmentes http beállításai ablakban jelenítse meg a konfigurációs lap gombra.

![HTTPS beállításai ablakban][4]

### <a name="step-2"></a>Lépés: 2

A a **appGatewayBackEndHttp** a beállítások lap kattintson a **használata egyéni vizsgálati** , és válassza a vizsgálati [létrehozása a vizsgálati](#createprobe) szakaszban létrehozott.
Amikor elkészült, kattintson **az OK gombra** , és a beállítások alkalmazása.

![appgatewaybackend a beállítások lap][5]

Az alapértelmezett vizsgálati ellenőrzi a webalkalmazás az alapértelmezett hozzáférési. Most, hogy egy egyéni vizsgálati már elkészült, az alkalmazás átjáró használata a Lync-állapota a a kijelölt kódmentes definiált egyéni mozgásvonal A megadott feltétel alapján, az alkalmazás átjáró ellenőrzi a vizsgálati a megadott fájlt. Ha a hívást fogadó: Port / elérési út nem ad vissza egy Http 200 OK állapot válasz, a kiszolgáló ki a Forgatás venni, a sérült küszöbértékét elérése után. A sérült példányon megállapítani, hogy mikor válik megfelelő újra számlálása továbbra is. A példány vissza felvételét követően megfelelő kiszolgáló kvótáját forgalom kezdődik, lépés rá, és példány számlálása továbbra is a szokásos módon felhasználó megadott időközönként.


## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként konfigurálható Azure alkalmazás átjáró, mert az SSL lásd: [SSL kiürítése konfigurálása](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png