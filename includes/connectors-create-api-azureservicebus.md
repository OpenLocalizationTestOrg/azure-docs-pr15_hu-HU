Most, hogy hozzáadott egy eseményindító, az időt szeretne elhelyezni, amely az eseményindító által generált adatokat tartalmazó érdekes eredményeket. Hajtsa végre az alábbi lépéseket követve hozzáadhat egy a **SharePoint Online - fájl létrehozása** művelet. Ez a művelet fog fájl létrehozása a SharePoint Online minden alkalommal, amikor az új elem eseményindító akkor indul el. 

Állítsa be az Ez a művelet, meg kell a következő adatokat. Megfigyelheti, hogy az egyes az új fájl tulajdonságainak bemeneteként az eseményindító által generált könnyen használható adatokat:

|Fájl tulajdonság létrehozása|Leírás|
|---|---|
|Webhely URL-címe|Ez a, amelyhez az új fájl létrehozása a SharePoint Online-webhely URL-CÍMÉT. Jelölje ki a listában, bemutatják a webhelyet.|
|Mappa elérési útja|Ez az, hogy a mappa (a webhely URL-CÍMÉT), ahol az új fájlt helyez el. Tallózással keresse meg és jelölje ki a mappát.|
|Fájlnév|Az éppen létrehozott fájl nevét.|
|Fájl tartalma|A tartalom, a fájl írt.|

1. Válassza az **+ új lépést** hozzáadni a műveletet.  
![](./media/connectors-create-api-sharepointonline/action-1.png)  
- Jelölje ki a **művelet hozzáadása** hivatkozásra. A megnyílik a Keresés mezőbe, ahol kereshet minden olyan művelet, szeretné venni. Ebben a példában a SharePoint-műveletek érdeklődésre számot tartó vannak.    
![](./media/connectors-create-api-sharepointonline/action-2.png)    
- Írja be a keresett SharePoint kapcsolódó műveletek a *sharepoint* .
- Jelölje be **A SharePoint Online - fájl létrehozása** a végrehajtandó műveletet.   **Megjegyzés**: az logika alkalmazást a SharePoint-fiók eléréséhez, ha, még nem tette korábban engedélyezése kéri.    
![](./media/connectors-create-api-sharepointonline/action-3.png)    
- Ekkor megnyílik a **Létrehozás** vezérlőt.   
![](./media/connectors-create-api-sharepointonline/action-4.png)     
- Jelölje ki a **Webhely URL-CÍMÉT** , és tallózással keresse meg a webhelyet, ahol meg szeretné a fájlt.     
![](./media/connectors-create-api-sharepointonline/action-5.png)  
- Jelölje ki a **mappa elérési útja** , és tallózással keresse meg a mappát, ahová az új fájlt helyez el.  
![](./media/connectors-create-api-sharepointonline/action-6.png)  
- Válassza a **fájl nevét** vezérlőelemre, és adja meg a létrehozni kívánt fájl nevét. A fájl nevét használható a bármely tulajdonsága az eseményindító szerint szeretne, egyszerűen ki a listában jelölje ki a korábban létrehozott származó látható.     
![](./media/connectors-create-api-sharepointonline/action-7.png)  
- Jelölje ki a **fájl tartalmát** vezérlőt, és adja meg a tartalmat, azt a fájlt, létrejön írt. Fájl tartalmát figyelje meg, hogy az imént létrehozott eseményindító az a tulajdonságokat bármelyikét használhatja. Egyszerűen jelölje ki a Tulajdonságok mutatják be, a listából. A **fájl tartalmát** szöveget írhatnak azt is megteheti, közvetlenül a vezérlőt. Ebben a példában e kijelölt egyes tulajdonságok és szóközt és az egyes tulajdonság közötti elválasztójel vett fel.        
![](./media/connectors-create-api-sharepointonline/action-8.png)  
- A munkafolyamat a módosítások mentése  
