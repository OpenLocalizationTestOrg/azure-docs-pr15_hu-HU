## <a name="load-balancer-differences"></a>Különbségek az terheléselosztó betöltése

Másik lehetőség terjesztése használata a Microsoft Azure hálózati forgalmának engedélyezésére. A beállításokkal másképp működnek egymástól, egy másik szolgáltatás beállítása és támogatja a különböző helyzetekkel ismerkedhet problémákat. Minden egyes tudják önmagában használható, és megjelenítheti őket.

- **Azure terheléselosztó** (réteg 4 a OSI hálózati hivatkozás egymást fedő) átviteli rétegben működik. Hálózaton szinten terjesztési forgalom az azonos Azure adatközpontban futó alkalmazás példányok keresztül biztosít.

- **Alkalmazás átjáró** működik az alkalmazási réteg (a OSI hálózati hivatkozás egymást fedő réteg 7). A Fordított sorrend-proxy szolgáltatásként, az ügyfél kapcsolat megszakítása és háttéradatbázist végpontokhoz kérelmeket továbbít működik.

- **Adatforgalom Manager** működik, a DNS-szintjén.  Irányítsa át a végfelhasználói forgalom globálisan elosztott végpontok DNS válaszok használ. Ügyfelek csatlakozhat ezeket a végpontok közvetlenül.

Az alábbi táblázat összefoglalja az egyes szolgáltatásokhoz által kínált szolgáltatások:

| Szolgáltatás | Azure terheléselosztó | Alkalmazás átjáró | Adatforgalom Manager |
|---|---|---|---|
|Technológia| Átviteli szint (réteg 4) | Alkalmazás szintjén (réteg 7) | DNS-szint |
| Támogatott alkalmazás protokollok | Bármely | HTTP- és HTTPS-forgalom |  Bármely (HTTP zárólap a szükséges végpont figyelése) |
| Végpontok | Azure VMs és Felhőbeli szolgáltatások szerepkör-példányok | Bármely Azure belső IP-cím vagy internetes nyilvános IP-cím | Azure VMs, a Cloud Services, az Azure Web Apps alkalmazások és a külső végpontok |
| Vnet támogatás | Mindkét internetes szemben és belső (Vnet) alkalmazásokhoz használható | Mindkét internetes szemben és belső (Vnet) alkalmazásokhoz használható |    Csak a internetes alkalmazásokat támogatja. |
Végpont figyelése | Támogatott szondákat keresztül | Támogatott szondákat keresztül | Támogatott a HTTP/HTTPS GET keresztül | 

Azure terheléselosztó és az alkalmazás átjáró útvonal hálózati forgalmat végpontokhoz, de más használatát forgatókönyvek mely forgalom kezelése rendelkeznek. Az alábbi táblázat segítséget nyújt a két terheléselosztókkal közti különbség:

| Típus | Azure terheléselosztó | Alkalmazás átjáró |
|---|---|---|
| Protokollok | A TCP/UDP | HTTP / HTTPS |
| IP-foglalás | Támogatott | Nem támogatott | 
| Terhelési terheléselosztó módja | 5 – tuple(source IP, source port, destination IP, destination port, protocol type) | Ciklikus<br>Továbbítás URL-címe alapján | 
| Terheléselosztási mód (forrás IP-cím / öntapadó munkamenetek) |  2 – sor bármelyik eleme (forrás IP-Címének és cél IP), a 3-sor bármelyik eleme (forrás IP-címe, cél IP és port). Méretezheti felfelé vagy lefelé a virtuális gépeken futó száma alapján | Cookie-alapú affinitás<br>Továbbítás URL-címe alapján |
| Állapot szondákat | Alapértelmezés: vizsgálati időtartam - 15 másodperc. Elforgatás ki vett: 2 folyamatos hibák. Ellenőrzi a felhasználó által definiált támogatja | Üresjárati vizsgálati időtartam 30 másodperc. Vett 5 élő forgalom egymást követő hibák vagy üresjárati módban egyetlen vizsgálati hiba után. Ellenőrzi a felhasználó által definiált támogatja | 
| Az SSL mert | Nem támogatott | Támogatott | 
  