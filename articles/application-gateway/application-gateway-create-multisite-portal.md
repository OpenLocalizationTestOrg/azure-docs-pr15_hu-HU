<properties
   pageTitle="Adjon meg egy meglévő alkalmazás átjárót több webhelyen az Azure-portálon elhelyezésére |} Microsoft Azure"
   description="Ez az oldal a képernyőn megjelenő utasításokat követve adjon meg egy meglévő Azure alkalmazás átjárót Azure Portal azonos átjáró több webalkalmazások elhelyezésére ismerteti."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Adjon meg egy meglévő alkalmazás átjárót több webalkalmazások tárolására

> [AZURE.SELECTOR]
- [Azure portál](application-gateway-create-multisite-portal.md)
- [Azure erőforrás-kezelő PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Több webhely-üzemeltető lehetővé teszi a azonos alkalmazás átjáró egynél több webes alkalmazás telepítéséhez. Határozza meg, mely figyelő szeretné fogadni forgalmat a bejövő HTTP-kérés host fejlécének jelenlétét támaszkodik. A figyelő majd a szabályok definícióban az átjáró konfigurált arra utasítja megfelelő kódmentes alkalmazáskészlet-alapú forgalmat. Az SSL engedélyezve van a webalkalmazások alkalmazás átjáró választhatja ki a megfelelő figyelő a webes forgalmat a kiszolgáló neve megjelölése (SNI) bővítmény támaszkodik. A közös elhelyezésére több webhely használata másik háttéradatbázis kiszolgáló készletek különböző webes tartományok egyenleg kérelem betöltéséhez. Ugyanígy több altartományokat azonos legfelső szintű tartomány is szerepeltethetők az alkalmazás az átjárón.

## <a name="scenario"></a>Eset

A következő példában alkalmazás átjáró van szolgáló contoso.com és fabrikam.com irányítja a két háttér-kiszolgálón készletek: contoso kiszolgáló készlet és fabrikam kiszolgáló készlet. A telepítő hasonló host altartományokat, mint app.contoso.com és blog.contoso.com használhatóak.

![többhelyszínű eset][multisite]

## <a name="before-you-begin"></a>Első lépések

Ebben az esetben több webhelyen támogatási ad egy meglévő alkalmazás átjáró. Ebben az esetben befejezéséhez forgatókönyv egy meglévő alkalmazás átjáró kell lennie a rendelkezésre álló szolgáltatáshoz való konfigurálása. Látogasson el [a portál használatával egy alkalmazás átjáró létrehozása](./application-gateway-create-gateway-portal.md) című témakörben olvashat a portálon egyszerű alkalmazás átjáró létrehozása.

## <a name="requirements"></a>Követelmények

- **Háttéradatbázist kiszolgáló készlet:** A háttér-kiszolgálók IP-címek listája. Az IP-címek szerepel a listában vagy tartozzanak a virtuális alhálózathoz, vagy egy nyilvános IP virtuális kell lennie. Teljesen minősített tartománynév is használható.
- **Háttéradatbázist készlet kiszolgálóbeállítások:** Minden készlet beállításokat, például a port, protokoll és cookie-alapú affinitás tartalmaz. Ezeket a beállításokat is tartozik egy csoportba, és alkalmazott összes kiszolgálón a készlet belül.
- **Előtér-port:** Ez a portja a nyilvános olyan portot alkalmazás átjáró megnyitott. Forgalmat a port találatok, és kattintson az egyik a háttéradatbázist kiszolgálók átirányítását.
- **Figyelő:** A figyelő előtér-port, protokoll van (Http vagy Https, ezek az értékek-és nagybetűk), és az SSL-tanúsítvány neve (ha az SSL beállítása kiürítése). Az átjárók több webhelyen engedélyezett alkalmazás, a host name és SNI jelölők is kerülnek.
- **Szabály:** A szabály figyelő, a háttéradatbázist kiszolgáló készlet köti és határozza meg, mely a forgalmat szeretné irányítani, amikor találatok száma a egy adott figyelő háttéradatbázist kiszolgáló készlet.
- **Tanúsítványok:** Minden egyes figyelő egyedi tanúsítványt igényel, ebben a példában a 2 hallgatók több webhely létrehozásakor. Két .pfx tanúsítvány és a jelszavának létre kívánja hozni.

## <a name="create-an-application-gateway"></a>Az alkalmazás átjáró létrehozása

Az alábbiakban az alkalmazás átjáró frissítése szükséges lépéseket:

1. Hozzon létre háttéradatbázist készletek mindkét webhelyen használható.
2. Minden webhely alkalmazás átjáró támogatni fogja létrehozása új figyelő.
3. Feleltesse meg a megfelelő háttéradatbázist az egyes figyelő szabályok létrehozásához.

## <a name="create-back-end-pools-for-each-site"></a>Minden webhely háttéradatbázist készletek létrehozása

Minden webhely háttéradatbázist erőforráskészlethez tartozik, hogy az alkalmazás átjáró fog támogatásra van szükség, ebben az esetben 2 készül, egy, a contoso11.com és egy fabrikam11.com.

### <a name="step-1"></a>Lépés: 1

Nyissa meg egy meglévő alkalmazás átjáró az Azure-portálon (https://portal.azure.com). Jelölje ki a **Kódmentes készletek** , és kattintson a **Hozzáadás** gombra.

![kódmentes készletek hozzáadása][7]

### <a name="step-2"></a>Lépés: 2

Töltse ki a háttéradatbázist készlet **pool1**, az IP-címek vagy a teljes tartományneve hozzáadása a háttéradatbázist kiszolgálók adatait, majd kattintson az **OK gombra**

![kódmentes alkalmazáskészlet pool1 beállításai][8]

### <a name="step-3"></a>3 lépés

A a kódmentes-készletek lap kattintson a **Hozzáadás** gombra egy további háttéradatbázist készlet **pool2**, a háttéradatbázist kiszolgálók hozzá az IP-címek vagy a teljes TARTOMÁNYNEVE, és kattintson az **OK gombra**

![kódmentes alkalmazáskészlet pool2 beállításai][9]

### <a name="create-listeners-for-each-back-end"></a>Az egyes háttéradatbázist hallgatók létrehozása

Alkalmazás átjáró HTTP 1.1 host fejlécek tárolni a azonos nyilvános IP-cím és a port egynél több webhely támaszkodik. Az egyszerű figyelő a portálon létrehozva nem tartalmaz a tulajdonság értékét.

### <a name="step-1"></a>Lépés: 1

Kattintson a **hallgatók** alkalmazás meglévő átjáró, majd a **webhely több elem** hozzáadása a első figyelő parancsra.

![hallgatók áttekintése lap][1]

### <a name="step-2"></a>Lépés: 2

Töltse ki az információkat a értesülnie ebben a példában az SSL lemondási be van állítva, és hozzon létre egy új frontend portot. Töltse fel a .pfx tanúsítvány SSL lemondási használható. A csak ez a lap és a normál, egyszerű figyelő lap összehasonlítása a különbség a hostname (állomásnév).

![figyelő tulajdonságai lap][2]

### <a name="step-3"></a>3 lépés

Kattintson a **több webhelyen** , és hozzon létre egy másik figyelő, az előző lépésben a második webhely leírt módon. Győződjön meg arról, hogy egy másik tanúsítványt használni a második figyelő. A csak ez a lap és a normál, egyszerű figyelő lap összehasonlítása a különbség a hostname (állomásnév). Töltse ki a figyelő adatait, és kattintson az **OK gombra**.

![figyelő tulajdonságai lap][3]

> [AZURE.NOTE] A hallgatók az Azure-portálon alkalmazás átjáró létrehozása feladata időigényes, eltarthat egy kis időt, ebben az esetben a két hallgatók létrehozásához. Amikor töltse ki a hallgatók megjelenítése a portálon az alábbi képen látható módon.

![figyelő – áttekintés][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Szabályok hallgatók megfeleltetése kódmentes készletek létrehozása

### <a name="step-1"></a>Lépés: 1

Nyissa meg egy meglévő alkalmazás átjáró az Azure-portálon (https://portal.azure.com). Válassza a **szabályok** , és válassza a meglévő alapértelmezett szabály **Szabály1** , és kattintson a **Szerkesztés**gombra.

### <a name="step-2"></a>Lépés: 2

Töltse ki a szabályok lap, az alábbi képen látható módon. Az első figyelő és az első készlet közül választhat, és kattintson a **Mentés** , amikor a teljes.

![meglévő szabály szerkesztése][6]

### <a name="step-3"></a>3 lépés

Kattintson az **egyszerű szabály** a 2nd szabály létrehozásához. A második figyelő és a második kódmentes gyűjtő űrlap kitöltése és mentéséhez kattintson **az OK** gombra.

![egyszerű szabály lap hozzáadása][10]

Ez akkor egészíti ki egy meglévő alkalmazás átjáró beállítása több webhelyen támogatásával az Azure portálon keresztül.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként-webhelyek [alkalmazás](application-gateway-webapplicationfirewall-overview.md) átjáróval – webes alkalmazás tűzfal védelme

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png