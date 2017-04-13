Most, hogy hozzáadott egy eseményindító, az időt szeretne elhelyezni, amely az eseményindító által generált adatokat tartalmazó érdekes eredményeket. Kövesse ezeket a lépéseket a **SharePoint Online - fájl létrehozása** művelet hozzáadása. Ez a művelet fog fájl létrehozása a SharePoint Online minden alkalommal, amikor az új elem eseményindító akkor indul el. 

Állítsa be az Ez a művelet, meg kell a következő adatokat. Ezek az információk, megfigyelheti, hogy az egyes az új fájl tulajdonságainak bemeneteként az eseményindító által generált könnyen használható adatok:

|Fájl tulajdonság létrehozása|Leírás|
|---|---|
|Webhely URL-címe|Ez a, amelyhez az új fájl létrehozása a SharePoint Online-webhely URL-CÍMÉT. Jelölje ki a listában, bemutatják a webhelyet.|
|Mappa elérési útja|Ez az a mappa (a webhely URL-CÍMÉT az előző lépésben kiválasztott) Ha az új fájlt helyez el. Tallózással keresse meg és jelölje ki a mappát.|
|Fájlnév|Az éppen létrehozott fájl nevét.|
|Fájl tartalma|A tartalom, a fájl írt.|

1. Válassza az **+ új lépést** hozzáadni a műveletet.  
![A SharePoint online művelet kép 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Jelölje ki az **művelet hozzáadása** hivatkozásra. A megnyílik a Keresés mezőbe, ahol kereshet minden olyan művelet, szeretné venni. Ebben a példában a SharePoint-műveletek érdeklődésre számot tartó vannak.    
![A SharePoint online művelet kép 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- Írja be a keresett SharePoint kapcsolódó műveletek a *sharepoint* .
- Jelölje be **A SharePoint Online - fájl létrehozása** a végrehajtandó műveletet.   **Megjegyzés**: az logika alkalmazást a SharePoint fiók eléréséhez, ha nem hozott létre kapcsolatot a SharePoint Online korábban engedélyezése kéri.    
![A SharePoint online művelet kép 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- Ekkor megnyílik a **Létrehozás** vezérlőt.   
![A SharePoint online művelet kép 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Jelölje ki a **Webhely URL-CÍMÉT** , és tallózással keresse meg a webhelyet, ahol meg szeretné a fájlt.     
![A SharePoint online művelet kép 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Jelölje ki a **mappa elérési útja** , és tallózással keresse meg a mappát, ahová az új fájl kerül.  
![A SharePoint online művelet kép 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Jelölje be a **fájl nevét** vezérlőelemre, és adja meg a létrehozni kívánt fájl nevét. Ebben az esetben a fájl nevét vagy közvetlenül írja, vagy az imént létrehozott eseményindító az a tulajdonságokat bármelyike használható. Ehhez tulajdonságok kijelölése a **kimeneti értékeket az új elem létrehozásakor**listáját. A lista, a csak a megjelenítés, a **fájl nevét** vezérlőelemre kijelölése után. Az ebben a walkthough lehet választotta azonosító (az új listaelem-azonosító) a **SharePoint Online - fájl létrehozása** művelet által létrehozott fájl nevét.    
![A SharePoint online művelet kép 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Jelölje ki a **fájl tartalmát** vezérlőt, és adja meg a tartalmat, azt a fájlt, létrejön írt. Fájl tartalmát figyelje meg, hogy az imént létrehozott eseményindító az a tulajdonságokat bármelyikét használhatja. Egyszerűen jelölje ki a Tulajdonságok mutatják be, a listából. A **fájl tartalmát** szöveget írhatnak azt is megteheti, közvetlenül a vezérlőt. Ebben a példában e kijelölt egyes tulajdonságok és szóközt és az egyes tulajdonság közötti elválasztójel vett fel.        
![A SharePoint online művelet kép 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- A munkafolyamat a módosítások mentése  
- Gratulálunk, most már az alkalmazás a teljes funkcionalitású logika, induljanak új elem hozzáadásakor a SharePoint Online listáiba. Az alkalmazás automatikusan létrehoz egy fájlt, az új listaelem a tulajdonságok egy része.  Most már tesztelheti, a SharePoint-lista az új elem létrehozásával. 
