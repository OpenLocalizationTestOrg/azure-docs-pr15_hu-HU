<properties
    pageTitle="Útmutató az Azure alkalmazás szolgáltatás Web Apps alkalmazásokban a saját tartománynév vásárlásához"
    description="További információ az Azure-App szolgáltatásban webalkalmazást egyéni tartománynév vásárlásához."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Vásárol, és állítsa be a saját tartománynév Azure App szolgáltatásban

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Egy webalkalmazás létrehozásakor Azure rendel a azurewebsites.net altartomány. Például a webes alkalmazás **contoso**neve, ha az URL-címet is **contoso.azurewebsites.net**. Azure is rendel egy virtuális IP-címet.

Egy gyártási webalkalmazás valószínűleg kívánt felhasználók számára megjeleníteni az egyéni tartománynevet. Ez a cikk ismerteti, hogyan vásárlása és az egyéni tartomány beállítása az [Alkalmazás szolgáltatás Web Apps alkalmazásokkal](http://go.microsoft.com/fwlink/?LinkId=529714). 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>– Áttekintés

Ha nem rendelkezik a web app tartománynév, könnyen vásárolhat egyet [Azure](https://portal.azure.com/)-portálon. A vásárlás során megadhatja, hogy a www-t és a legfelső szintű tartomány DNS-rekordokat kell megfeleltetni automatikusan a web App alkalmazásban. Azure portál belül a tartományával közvetlenül is kezelheti.


Kövesse az alábbi lépéseket a tartománynevek vásárlása és hozzárendelése a web App alkalmazásban.

1. A böngészőben nyissa meg az [Azure-portálon](https://portal.azure.com/).

2. A **Web Apps alkalmazások** lapon kattintson a nevére a web App alkalmazásban, válassza a **Beállítások**, és válassza az **egyéni tartományok**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. Kattintson az **egyéni tartományok** lap a **tartomány vásárlása**.

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. A **Tartományok megvásárlása** lap a szövegdoboz használatával írja be a tartománynevet szeretne vásárolni, és le az ENTER billentyűt. A javasolt elérhető tartományok csak a szövegmező alatt jelenik meg. Jelölje ki milyen tartományt szeretne vásárolni. Megadhatja, hogy egyszerre vásárlásához több tartományt. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Kattintson a **Kapcsolattartási adatok** , és töltse ki a tartományt partneradatok űrlap.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] Nagyon fontos, hogy adja meg az összes szükséges mezőt a lehető, különösen az e-mail címet minél nagyobb pontossággal. Abban az esetben a tartomány "Adatvédelem" nélkül vásárolt, akkor a rendszer kérheti ellenőrizze a levelezés, a tartomány aktív válik. Bizonyos esetekben a kapcsolattartási adatok helytelen adatokat kell vásárolnia tartományok hiba eredményezi. 

6. Most lehetősége van arra,

    a) "az automatikus megújítást" a tartomány minden évben
    
    b) funkcióról az "Adatvédelem" amely szerepel a vételár ingyen (kivéve a TLD ki adatait a beállításjegyzék nem támogatja az adatvédelmi. For example:. co.in,. co.uk stb.)  
    
    c) "alapértelmezett állomásnevekké hozzárendelése" a www-t, és a legfelső szintű tartomány az aktuális Web App alkalmazásban. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] C beállítás konfigurálja a DNS kötések és a hostname (állomásnév) kötések automatikusan az Ön számára.  Ezzel a módszerrel a Web App érhetők el egyéni tartományt használni, amint a vételi befejeződött (DNS propagálása késések néhány esetekben baring). Abban az esetben, a Web App Azure forgalom Manager mögött van, nem láthatja hozzá szeretné rendelni a gyökértartomány, beállítást, A rekordok nem működnek a forgalom kezelő. Mindig hozzárendelheti a tartományok/alterület-domains vásárolt – egy webalkalmazást egy másik Web App és fordítva. Lásd: további információt a 8 lépést. 
    
7. Kattintson a **Jelölje ki** a **Tartományok megvásárlása** lap, majd a vásárlás információkat találhat **Beszerzési visszaigazolások** lap. Fogadja el a jogi szerződést, és kattintson a **vásárol**, ha a sorrend nyújtanak, és figyelheti a vásárlási bejelentkezett az **értesítési**. Tartomány vásárlása is néhány percet igénybe vehet. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Ha sikeresen rendelt egy tartományt, kezeli a tartomány, és hozzárendelése a web App alkalmazásban. Kattintson a **(...)** a tartomány jobb oldalán. Ezután **Beszerzés visszavonása** vagy is **Manage domain**. Kattintson a **Manage domain**gombra, majd azt kell kötni az **altartomány** a web App alkalmazásban, a **Manage domain** lap. Ha szeretné kötni **altartományt** különböző webalkalmazást végezze el ezt a lépést a a megfelelő Web App környezetén belül. További lehetőségek fölé, kattintva hozzárendelése a tartomány forgalom manager végpontra (Ha a Web App TM mögött van) egyszerűen parancsot választva forgalom menedzser nevezze el a legördülő menüből. Ezzel az eljárással tartomány és altartomány automatikusan osztják mögött forgalom Manager végpontra webalkalmazásokra. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] "Lemondhatja vásárlás" teljes visszatérítésre 5 napon belül. Lehetővé teszi a 5 nap után nem tudja "Mégse vásárlás", helyette megjelenik a "delete"műveletet a tartományhoz egy lehetőség. Törli a tartomány eredményezi az annak engedélyezése a visszatérítés nélkül az előfizetésből, és a rendelkezésre álló tartomány válik. 

Konfigurációs befejeződése után az egyéni tartomány neve megjelenik a webalkalmazás **Hostname (állomásnév) kötések** szakaszában.

Ezen a ponton látnia kell a böngészőben adja meg az egyéni tartománynevet, és látható, hogy sikeresen szükséges időt, a web App alkalmazásban.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Mi történik az egyéni tartomány vásárolta

Az Azure előfizetés vásárolta az **egyéni tartományok és az SSL** lap az egyéni tartomány területhez tartozik. Azure erőforrásként ez egyéni tartománya külön és független először vásárolta a tartományt a alkalmazás szolgáltatás alkalmazásból. Ez azt jelenti, hogy:

- Az Azure portál belül vásárolta az egyéni tartomány egynél több alkalmazás szolgáltatási alkalmazást, és nem csak az alkalmazás az egyéni tartomány első vásárolt is használhatja. 
- Nyissa meg az **egyéni tartományok és az SSL** lap a *minden* alkalmazás szolgáltatás alkalmazás az adott előfizetés vásárolta az Azure előfizetés egyéni tartományok is kezelheti.
- Rendelhet bármely alkalmazás szolgáltatás alkalmazásra az azonos Azure előfizetésből altartományt egyéni tartományban.
- Ha úgy dönt, hogy egy App szolgáltatás alkalmazás törlése, megadhatja, hogy ne törölje az egyéni tartomány van kötve Ha meg szeretné tartani használja az egyéb alkalmazások.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Ha nem látja az egyéni tartomány vásárolta

Ha vásároltak belül az **egyéni tartományok és az SSL** -lap az egyéni tartományt, de nem látható az egyéni tartományt a **tartományok felügyelt**, ellenőrizze az alábbiakat:

- Az egyéni tartomány létrehozási nem befejezése után. Jelölje be a haladás tetején látható az Azure-portálra értesítési harang.
- Az egyéni tartomány létrehozási valamiért nem sikerült. Jelölje be a haladás tetején látható az Azure-portálra értesítési harang.
- Az egyéni tartomány sikeres volt, de a lap nem frissíthetők még. Próbálja ki az **egyéni tartományok és az SSL** lap ismételt megnyitásával.
- Előfordulhat, hogy törli az egyéni tartomány bizonyos pontján. Jelölje be a naplókat a **Beállítások**gombra kattintva > **Audit naplók** , az alkalmazás fő lap. 
- Az **egyéni tartományok és az SSL** a lap azt szeretné megtudni az alkalmazás, amely egy másik Azure-előfizetést jön létre tartozhat. Váltás másik előfizetést egy másik alkalmazásban, és jelölje be a saját **egyéni tartományok és az SSL** lap.  
  A portálon belül, nem látható, vagy egy másik Azure előfizetést, mint az alkalmazást a létrehozott egyéni tartományok kezelése elemre. Jó helyen jár, ha **Speciális kezelése** a tartomány **Manage domain** lap kattint, átirányítjuk a tartományszolgáltatót webhelyre, ahol is   [kézi](web-sites-custom-domain-name.md) beállításához az egyéni tartomány, például egy tetszőleges külső egyéni tartomány 
   -alkalmazások készült különböző Azure előfizetést. 


