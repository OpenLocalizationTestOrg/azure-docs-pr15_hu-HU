<properties 
    pageTitle="Ismerje meg, hogyan AS2 megállapodást a nagyvállalati integrációs csomag létrehozása" 
    description="Ismerje meg, a nagyvállalati integrációs csomag létrehozása AS2 megállapodást |} Microsoft Azure alkalmazás szolgáltatás" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>AS2 vállalati integrációja

## <a name="create-an-as2-agreement"></a>Hozzon létre egy AS2 szerződés
A vállalati szolgáltatások használatához az összefüggés-alkalmazásokban, először létre kell hoznia a rendelkezést. 

### <a name="heres-what-you-need-before-you-get-started"></a>Mire van szükség megkezdése előtt
- Az Azure-előfizetése definiált [integrációs fiók](./app-service-logic-enterprise-integration-accounts.md)  
- Integráció fiókban már definiált legalább két [partnerek](./app-service-logic-enterprise-integration-partners.md)  

>[AZURE.NOTE]Olyan megállapodást létrehozásakor a szerződés fájl tartalmának egyeznie kell a szerződés típusát.    


Miután, ha már [létrehozott integrációs fiók](./app-service-logic-enterprise-integration-accounts.md) , és [megjelenik a partnerek](./app-service-logic-enterprise-integration-partners.md), ezeket a lépéseket követve hozhat létre olyan megállapodást:  

### <a name="from-the-azure-portal-home-page"></a>Az Azure portál Kezdőlap lapján

Miután bejelentkezett az [Azure portál](http://portal.azure.com "Azure portál")be:  
1. A bal oldalon a menüből válassza a **Tallózás gombra** .  

>[AZURE.TIP]Ha nem látja a **tallózással keresse meg** hivatkozást, előfordulhat, bontsa ki előbb a menüt. Ehhez a **menü megjelenítése** hivatkozásra kattint, amely a található összecsukott menü bal felső.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integráció* a szűrés a Keresés mezőbe írja be, és válassza a **Fiókok integráció** a találatok listájában.       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. A **Fiókok integráció** a lap fel a megnyíló jelölje ki a integrációs-fiókot, amelyben a szerződést hoz létre. Ha nem látja a bármilyen integrációt fiókok listák [Hozzon létre egy újat első](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Jelölje ki a **rendelkezést** csempére. Ha nem látja a rendelkezést csempére, először felvennie azt.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. A **Hozzáadás** gombra, amely megnyitja az rendelkezést lap kijelölése  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Írja be egy **nevet** a szerződés, majd válassza ki a **Fogadó Partner**, **Host identitás**, **Vendégként való bekapcsolódáshoz Partner**, **A vendégként való bekapcsolódáshoz identitás**a megnyíló rendelkezést lap.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Íme néhány dolog, akkor azt tapasztalhatja hasznos, amikor a szerződés beállításainak konfigurálása: 
  
|A tulajdonság|Leírás|
|----|----|
|A Host partnernek|Egy szerződés egy host és a vendégként való bekapcsolódáshoz partner van szüksége. A host partner jelöl, amely a szerződés konfigurálja a szervezet.|
|A Host azonosító|A host partner azonosítója. |
|A vendégként való bekapcsolódáshoz partnernek|Egy szerződés egy host és a vendégként való bekapcsolódáshoz partner van szüksége. A vendégként való bekapcsolódáshoz partner jelöl, amely a fogadó partner az üzletet a szervezet.|
|A vendégként való bekapcsolódáshoz azonosító|A vendégként való bekapcsolódáshoz partner azonosítója.|
|Fogadás beállításai|A tulajdonságok egy megállapodás érkező összes üzenetet alkalmazása|
|Küldés beállításai|A tulajdonságok egy szerződés küldött összes üzenet alkalmazása|  
Vegyük továbbra is:  
7. Kattintson a **Fogadási beállítások** konfigurálása hogyan ezt a szerződést keresztül a Beérkezett üzenetek kezelésének történik.  
 
 - Ha szükséges felülbírálhatja a bejövő üzenetek a tulajdonságok. Ehhez jelölje be az **üzenet tulajdonságai felülbírálása** jelölőnégyzetet.
  - Jelölje be az **üzenet kell bejelentkeznie** jelölőnégyzetet, ha szeretné, hogy csak az aláírandó összes bejövő üzenetek. Ha ezt a beállítást választja, akkor is kell jelölje ki a **tanúsítvány** , amellyel az aláírást az üzenetek ellenőrzése.
  - Tetszés szerint üzenetek titkosítása, valamint előírhatja. Ehhez jelölje be az **üzenet kell titkosítható** jelölőnégyzetet. Ezután jelölje ki a bejövő üzenetek dekódolását használt **tanúsítvány** kellene.
  - Kérheti lehet tömöríteni üzenetek is. Ehhez jelölje be az **üzenet célszerű lehet tömöríteni** jelölőnégyzetet.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Az alábbi táblázatban látható, ha szeretné, hogy megtudhatja, hogy milyen a fogadás kapcsolatos beállítások engedélyezése.  

|A tulajdonság|Leírás|
|----|----|
|Üzenet tulajdonságai felülbírálása|Akkor válassza ezt jelzi, hogy tulajdonságokat a Beérkezett üzenetek felülírható |
|Üzenet kell bejelentkeznie.|Ez csak a digitálisan aláírt üzenetet engedélyezése|
|Üzenet kell titkosítható.|Üzenetek titkosítása megkövetelésére engedélyezéséhez. Nem titkosított üzenetek elutasításra kerül.|
|Üzenet célszerű lehet tömöríteni.|Engedélyezze a határozhatják meg lehet tömöríteni üzeneteket. Az üzenetek nem tömöríti elutasításra kerül.|
|MDN szöveg|Ez az alapértelmezett MDN küldeni szeretné az üzenet feladójának|
|MDN küldése|Engedélyezze ezt engedélyezni MDNs küldeni.|
|Aláírt MDN küldése|Az aláírandó MDNs megkövetelésére engedélyezéséhez.|
|Mikrofon algoritmus||
|Aszinkron MDN küldése|Engedélyezése erre a megkövetelése aszinkron küldeni.|
|URL-CÍME|Ez az URL-CÍMÉT, amelyre üzeneteket küld.|
Most vegyük továbbra is:  
8. Jelölje ki **Küldési beállítások** konfigurálása a hogyan ezt a szerződést keresztül küldött üzenetek kezelésének vannak.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Az alábbi táblázatban látható, ha azt szeretné, ha többet szeretne tudni milyen a Küldés engedélyezése.  

|A tulajdonság|Leírás|
|----|----|
|Üzenet aláírása engedélyezése|Jelölje be a kell aláírnia a szerződést érkező minden üzenet engedélyezése jelölőnégyzetet.|
|Mikrofon algoritmus|Jelölje ki a algoritmus az üzenet aláírása használata|
|Tanúsítvány|Jelölje ki a tanúsítványt használni az üzenet aláírása|
|Üzenetek titkosítása engedélyezése|Jelölje be ezt a jelölőnégyzetet, a jelen szerződés érkező minden üzenet titkosítása.|
|Titkosítási algoritmust|Jelölje ki a titkosítási algoritmust az üzenetek titkosítása|
|HTTP-fejlécek unfold|Jelölje be ezt a jelölőnégyzetet a HTTP-tartalomtípus-fejléc unfold be az egysoros.|
|MDN kérése|Ezt a jelölőnégyzetet, a jelen szerződés küldött összes üzenet-MDN kérni engedélyezése|
|Aláírt MDN kérése|Engedélyezni kell, hogy a jelen szerződés küldött összes MDNs van-e jelentkezve|
|Aszinkron MDN kérése|Aszinkron MDN küldeni szeretné ezt a szerződést kérhet engedélyezése|
|URL-CÍME|Az URL-címet, amelyre MDNs küld|
|NRR engedélyezése|A jelölőnégyzet bejelölésével nyugtát nem hitelesítettek engedélyezése|
Azt majdnem elkészült!  
9. Válassza a fiók integrációs lap a **rendelkezést** csempét, és jelennek meg az újonnan hozzáadott megállapodás szerepel a listában.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)

