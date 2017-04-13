<properties
   pageTitle="Nyilvános elérésének ACS alkalmazás |} Microsoft Azure"
   description="Hogyan lehet az Azure tároló szolgáltatásainak elérésének engedélyezése."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, a tárolók, Micro-szolgáltatások, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="timlt"/>

# <a name="enable-public-access-to-an-azure-container-service-application"></a>Azure tároló szolgáltatásalkalmazás elérésének engedélyezése

A ACS [nyilvános ügynök készlet](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) bármelyik Adatközpont/OS tároló automatikusan megjelenő az internethez. Alapértelmezett, portok **80**, **443** **8080** nyílnak meg, és bármelyik (nyilvános) tároló azokat a portokat figyel érhetők el. Ez a cikk bemutatja, hogyan kattintva nyissa meg az alkalmazások számára a további portokat Azure tároló szolgáltatás.

## <a name="open-a-port-portal"></a>Nyissa meg a olyan portot (portal) 

Először is azt kell nyissa meg a szeretnénk port.

1. Jelentkezzen be a portálon.
2. Keresse meg az erőforráscsoport telepített az Azure tároló szolgáltatást.
3. Jelölje ki a ügynök terheléselosztó (amely nevű **XXXX-agent-lb-XXXX**hasonló).

    ![Azure tároló szolgáltatás terheléselosztó](media/container-service-dcos-agents/agent-load-balancer.png)

4. Kattintson a **ellenőrzi** , majd **adja hozzá**.

    ![Azure tároló szolgáltatás terheléselosztó ellenőrzi.](media/container-service-dcos-agents/add-probe.png)

5. Töltse ki a vizsgálati űrlapot, és kattintson az **OK gombra**.

  	| A mező | Leírás |
  	| ----- | ----------- |
  	| név  | A vizsgálati leíró jellegű neve. |
  	| Port  | A port, tesztelje a tároló. |
  	| Elérési út  | (Ha a HTTP üzemmódban) Relatív webhely elérési útja probe. Nem támogatott HTTPS. |
  	| Intervallum | Vizsgálati között eltelt idő mennyiségét a másodpercben megadott megpróbálja. |
  	| Sérült küszöbérték | Egymást követő vizsgálati száma megpróbálja, figyelembe véve a tároló sérült előtt. | 
    

6. Kattintson a ügynök terheléselosztó tulajdonságainak **terheléselosztó szabályok betöltése** , majd az **Add**.

    ![Azure tároló szolgáltatás betöltés terheléselosztó szabályok](media/container-service-dcos-agents/add-balancer-rule.png)

7. Töltse ki a betöltés terheléselosztó űrlapot, és kattintson az **OK gombra**.

  	| A mező | Leírás |
  	| ----- | ----------- |
  	| név  | A terheléselosztó leíró jellegű neve. |
  	| Port  | A nyilvános bejövő port. |
  	| Kódmentes port | A belső nyilvános portja a tároló szeretné a forgalmat. |
  	| Kódmentes készlet | A tárolók a készlet lesz-e terheléselosztó a cél. |
  	| Vizsgálati | A vizsgálati azt határozza meg, ha a célként **Kódmentes készlet** megfelelő. |
  	| Munkamenet megőrzése | Határozza meg, hogy egy ügyfél érkező forgalmat a munkamenet időtartama kezelésének módját.<br><br>**Nincs**: bármely tároló kezelhetik a azonos ügyfélprogramból egymást követő kérések.<br>**Ügyfél IP**: az azonos ügyfél IP egymást követő kérései ugyanabban a tárolóban kezeli.<br>**Ügyfél IP- és Protocol (protokoll)**: az ügyfél IP- és protocol-kombináció egymást követő kéréseket ugyanabban a tárolóban kezeli. |
  	| Üresjárati időkorlát | (Csak TCP) Perc alatt a TCP/HTTP ügyfél megtartása indításkor anélkül, hogy az *életben* üzeneteket. |

## <a name="add-a-security-rule-portal"></a>Biztonsági szabály (portal) hozzáadása

Következő lépésként el kell, amelyek a forgalom a megnyitott helyéről a tűzfalon keresztül biztonsági szabály hozzáadásához.

1. Jelentkezzen be a portálon.
2. Keresse meg az erőforráscsoport telepített az Azure tároló szolgáltatást.
3. Jelölje ki a **nyilvános** ügynök hálózati biztonsági csoportot (amely nevű **XXXX-agent-nyilvános-nsg-XXXX**hasonló).

    ![Azure tároló szolgáltatás hálózati biztonsági csoport](media/container-service-dcos-agents/agent-nsg.png)

4. Jelölje be a **Bejövő szabályok biztonsági** majd a **Hozzáadás gombra**.

    ![Azure tároló szolgáltatás hálózati biztonsági csoport szabályok](media/container-service-dcos-agents/add-firewall-rule.png)

5. Töltse ki a tűzfalat szabály lehetővé teszi a nyilvános port, és kattintson az **OK gombra**.

  	| A mező | Leírás |
  	| ----- | ----------- |
  	| név  | A tűzfal szabály leíró jellegű neve. |
  	| Prioritás | A szabály rangját prioritását. Minél kisebb számot annál nagyobb a prioritás. |
  	| Forrás | A bejövő IP-címtartományokat engedélyezett vagy megtagadja a Ez a szabály korlátozása. Segítségével **bármely** nem adja meg a korlátozás. |
  	| Szolgáltatás | Válasszon előre definiált szolgáltatások Ez a szabály szolgál. Más módon használhatja az **egyéni** létrehozásához a saját. |
  	| Protocol (protokoll) | Korlátozza a **TCP** - és **UDP-**alapú forgalmat. Segítségével **bármely** nem adja meg a korlátozás. |
  	| Porttartományt | **Egyéni** **szolgáltatás** esetén adja meg, hogy ez a szabály alkalmazásával miként változna porttartományt. Egyetlen port, például **80**és tartomány például **1024-1500**is használhatja. |
  	| Művelet | Engedélyezze, vagy megtagadja a feltételeknek megfelelő forgalmat. |

## <a name="next-steps"></a>Következő lépések

Tudjon meg többet a [nyilvános és titkos Adatközpont/OS ügynökök](container-service-dcos-agents.md)közötti különbség.

Olvassa el [az Adatközpont/OS tárolók kezelésével](container-service-mesos-marathon-ui.md)kapcsolatban további tudnivalókat.