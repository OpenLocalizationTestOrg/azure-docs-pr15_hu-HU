<properties
   pageTitle="Adjon meg egy alkalmazás átjárót, az SSL kiürítése a portál használatával |} Microsoft Azure"
   description="Ez az oldal ismerteti a képernyőn megjelenő utasításokat követve hozzon létre egy alkalmazás átjáró SSL kiürítése a portál használatával"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Adjon meg egy alkalmazás átjárót, az SSL kiürítése a portál használatával

> [AZURE.SELECTOR]
-[Azure portál](application-gateway-ssl-portal.md)
-[Azure erőforrás-kezelő PowerShell](application-gateway-ssl-arm.md)
-[Azure klasszikus PowerShell](application-gateway-ssl.md)

Azure alkalmazás átjáró beállítható úgy, hogy megszünteti a költséges SSL visszafejtés tevékenységek fordulhat elő, a webes farm elkerülése érdekében az átjáró Secure Sockets Layer (SSL) munkamenetet. Az SSL kiürítése is egyszerűbbé teszi az előtér-kiszolgáló beállítása és kezelése a webalkalmazás.

## <a name="scenario"></a>Eset

A következő esetben végig konfigurálása a egy meglévő alkalmazás átjáró kiürítése SSL. Az alkalmazási példát feltételezi, hogy Ön már lépések [Egy alkalmazás átjáró](application-gateway-create-gateway-portal.md)létrehozásához.

## <a name="before-you-begin"></a>Első lépések

Az alkalmazás átjáró SSL kiürítése konfigurálásához tanúsítvány szükség. A tanúsítvány alkalmazás átjáró betöltött, és a használt titkosítása és visszafejteni az SSL keresztül küldött forgalmat. A tanúsítvány kell lennie a személyes információkat Exchange (pfx) formátumban. Ez a fájlformátum lehetővé teszi, hogy a titkos kulcs exportálni, amely szerint az alkalmazás átjáró titkosításához és forgalom visszafejtése végrehajtásához szükséges.

## <a name="add-an-https-listener"></a>Egy HTTPS-figyelő hozzáadása

A HTTPS-figyelő forgalmat a konfigurációjának keres, és segítséget nyújt a kódmentes készletek irányítja a forgalmat.

### <a name="step-1"></a>Lépés: 1

Nyissa meg az Azure portált, és jelölje ki egy meglévő alkalmazás átjáró

### <a name="step-2"></a>Lépés: 2

Kattintson a hallgatók, és kattintson a Hozzáadás gombbal figyelő.

![alkalmazás átjáró áttekintése lap][1]

### <a name="step-3"></a>3 lépés

Adja meg a szükséges információkat a figyelő és a feltöltés a .pfx tanúsítvány, ha a teljes kattintson az OK gombra.

**Név** – Ez az érték egy rövid nevet, a figyelő.

**Beállítása Frontend IP** - ezt az értéket a frontend IP-konfigurációja, amellyel a figyelő.

**Frontend port (neve/Port)** – az alkalmazás átjáró és a használt tényleges port eleje használt portszámként egy rövid nevet.

**Protocol (protokoll)** – határozza meg, ha a https- vagy http szolgál az előtér váltani.

**Tanúsítvány (nevét és jelszóval)** – Ha SSL kiürítése használják, .pfx tanúsítvány szükség a ezt a beállítást és egy rövid nevet, és a jelszó szükséges.

![figyelő lap hozzáadása][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Hozzon létre egy szabályt, és azt a figyelő társítani

A figyelő létrehozva. Pedig kezelheti a forgalmat, a figyelő a szabály létrehozása.

### <a name="step-1"></a>Lépés: 1

Kattintson a **szabályok** az alkalmazás átjáró, és kattintson a Hozzáadás gombra.

![alkalmazás átjáró szabályok lap][3]

### <a name="step-2"></a>Lépés: 2

Az **egyszerű szabály hozzáadása** lap írja be a könnyen megjegyezhető nevet a szabálynak, és válassza a figyelő az előző lépésben létrehozott. Kattintson az **OK gombra** , és válassza ki a megfelelő kódmentes készlet és a http beállítás

![HTTPS beállításai ablakban][4]

A beállításokat ekkor menti az alkalmazás átjáró. A Mentés feldolgozni vonatkozó beállítások eltarthat egy ideig, mielőtt azok megtekintése a portálon vagy PowerShell keresztül érhető el. Miután menti az alkalmazás átjáró kezeli a titkosítási és forgalom visszafejtése. Az alkalmazás átjáró és a kódmentes webkiszolgálón közötti minden forgalom kezelje a rendszer HTTP-n keresztül. Bármely kommunikáció az ügyfél által kezdeményezett HTTPS Ha visszatér az ügyfél titkosított.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként állítható be egy egyéni állapot vizsgálati az Azure alkalmazás átjáró, olvassa el a [egy egyéni állapot vizsgálati létrehozása](application-gateway-create-gateway-portal.md)című témakört.

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png