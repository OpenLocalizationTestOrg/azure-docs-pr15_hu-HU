<properties 
    pageTitle="Integráció a partnerek és a nagyvállalati integrációs csomag áttekintése |} Microsoft Azure alkalmazás szolgáltatás |} Microsoft Azure" 
    description="Ismerje meg, minden integrációs fiókokkal kapcsolatban, a nagyvállalati integrációs csomag és összefüggés-alkalmazások" 
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
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-integration-accounts"></a>Integráció fiókok áttekintése

## <a name="what-is-an-integration-account"></a>Mit nevezünk integrációs fiók?
Egy integrációs egy Azure fiók, amely lehetővé teszi a nagyvállalati integrációs alkalmazások kezeléséhez eltéréseket, beleértve a sémák, térképek, tanúsítványok, partnerek és rendelkezést. Bármely integration alkalmazás hoz létre kell ahhoz, hogy hozzáférjen a séma, a térkép vagy a tanúsítvány, például egy integrációs-fiókot használ.

## <a name="create-an-integration-account"></a>Integráció fiók létrehozása 
1. Válassza a **Tallózás gombra**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. **Integráció** a szűrés a Keresés mezőbe írja be, és válassza a **Fiókok integráció** az eredménylistából     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. A lap tetején a menüből válassza a *Hozzáadás* gombra.      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Írja be a **nevét**, jelölje ki az **előfizetés** szeretne használni, vagy hozzon létre egy új **erőforráscsoport** , vagy jelölje ki a meglévő erőforráscsoport, jelöljön ki egy **helyet** ahol integrációs fiókja fog tárolni, jelölje be az egy **réteg árak**, és válassza a **Létrehozás** gombra.   

  Ezen a ponton a kiválasztott hely a integrációs fiók kiépítve. Ez a 1 percen belül kell elvégezni.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Frissítse a lapot. Ekkor megjelenik a integrációs fiókja szerepel a listában. Gratulálok!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Hogyan integrációs ügyfél csatolása egy logikai alkalmazás
Ahhoz, hogy a összefüggés-alkalmazásokat, térképek, sémák, rendelkezést és más eltérések integrációs fiókjában található eléréséhez először a integrációs fiók hozzá kell rendelnie az logika alkalmazást.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Az alábbiakban a lépéseket követve integrációs ügyfél csatolása egy logikai alkalmazás 

#### <a name="prerequisites"></a>Előfeltételek
- Integráció fiók
- Egy logikai alkalmazás

>[AZURE.NOTE]Biztosíthatja, hogy a integráció és a logika alkalmazás az **Azure máshol** első lépések

1. A logikai alkalmazás a menüből válassza a **Beállítások** hivatkozása  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Válassza ki a **Integrációs fiók** elemet a beállítások lap  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Jelölje ki a kívánt a logika alkalmazást **Jelöljön ki egy integrációs fiókot** a legördülő lista csatolása integrációs fiókot  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Mentse a munkáját  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Ekkor megjelenik egy értesítés, amely azt jelzi, hogy a integrációs fiókja van csatolva az összefüggés-alkalmazást, és, hogy minden eltérések integrációs fiókban most érhetők el a logika alkalmazásba.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Most, hogy fiókja integráció a logika alkalmazás van csatolva, akkor is, nyissa meg a logika alkalmazását, és segítségével B2B összekötők, mint például az XML-adatok érvényesítése, strukturálatlan fájlhoz dekódolását és kódolását vagy átalakítás alkalmazások B2B funkcióival.  
    
## <a name="how-to-delete-an-integration-account"></a>Hogyan integrációs fiók törlése?
1. Válassza a **Tallózás gombra**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integráció** a szűrés a Keresés mezőbe írja be, és válassza a **Fiókok integráció** az eredménylistából     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Jelölje ki a törölni kívánt **integrációs fiók**  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Jelölje ki a hivatkozás **törlése** , amely a menüben található   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Erősítse meg    

## <a name="how-to-move-an-integration-account"></a>Hogyan lehet áthelyezése integrációs fiók?
Integráció fiók egyszerűen áthelyezése új előfizetést, és új erőforráscsoport. Ha módosítani szeretné a integrációs fiók áthelyezése, tegye a következőket:

>[AZURE.IMPORTANT] Meg kell frissíteni az új erőforrás lehetővé tevő dokumentumazonosítók használatához, integráció fiók áthelyezése után az összes parancsfájl.

1. Válassza a **Tallózás gombra**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integráció** a szűrés a Keresés mezőbe írja be, és válassza a **Fiókok integráció** az eredménylistából     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Jelölje ki a törölni kívánt **integrációs fiók**  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Jelölje ki az **áthelyezni** hivatkozást, amely a menüben található   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Erősítse meg    

## <a name="next-steps"></a>Következő lépések
- [További tudnivalók a nagyvállalati integrációs csomag] (./app-service-logic-enterprise-integration-overview.md "Tudnivalók a nagyvállalati integrációs csomag")  
- [További tudnivalók a rendelkezést] (./app-service-logic-enterprise-integration-agreements.md "Megtudhatja, hogy nagyvállalati integrációs rendelkezést")  


 