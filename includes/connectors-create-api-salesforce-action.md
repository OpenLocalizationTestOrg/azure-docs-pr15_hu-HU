Most, hogy hozzáadott egy feltétel, az időt szeretne elhelyezni, amely az eseményindító által generált adatokat tartalmazó érdekes eredményeket. Kövesse ezeket a lépéseket a **Salesforce - Get-objektum** művelet hozzáadása. Ez a művelet az adatokat fog kapni minden alkalommal, amikor egy új érdeklődő jön létre. A második műveletet, amelyekkel a Salesforce - Get-Objektumesemény használata az Office 365-összekötő mailt küldhet adatainak is hozzáadja.  

Állítsa be az Ez a művelet, meg kell a következő adatokat. Megfigyelheti, hogy az egyes az új fájl tulajdonságainak bemeneteként az eseményindító által generált könnyen használható adatokat:

|Fájl tulajdonság létrehozása|Leírás|
|---|---|
|Objektumtípus|Ez a érdeklik Salesforce-objektum. Példák érdeklődő, fiók stb.|
|Objektumazonosító|Az objektum azonosítóját ez jelenti.|


1. Jelölje ki a **művelet hozzáadása** hivatkozásra. A megnyílik a Keresés mezőbe, ahol kereshet minden olyan művelet, szeretné venni. Ebben a példában a Salesforce-műveletek az érdeklődésre számot tartó vannak.      
![Salesforce művelet kép 1](./media/connectors-create-api-salesforce/action-1.png)  
- Írja be a *salesforce* keresésére salesforce kapcsolódó műveletek.
- Jelölje be a végrehajtandó műveletet **Salesforce - Get-objektumot** .   **Megjegyzés**: kérni fogja a Salesforce-fiók eléréséhez, ha, még nem tette a korábban a logika alkalmazás engedélyezik.    
![Salesforce művelet kép 2](./media/connectors-create-api-salesforce/action-2.png)    
- A **Get-objektum** vezérlő nyílik meg.  
- Jelölje ki az *érdeklődő* az objektum típusa.
- Jelölje ki a **Objektumazonosító** vezérlőt.
- Jelölje ki a **…** műveletekhez bemeneteként használható tokenek listájának kibontásához.       
![Salesforce művelet kép 3](./media/connectors-create-api-salesforce/action-3.png)    
- Jelölje ki az **Érdeklődő azonosító** vezérlő nyitja meg.   
![Salesforce művelet kép 4](./media/connectors-create-api-salesforce/action-4.png)     
- Figyelje meg, hogy a érdeklődő azonosító jogkivonat ettől kezdve a Objektumazonosító vezérlő, jelezve, hogy a Get-objektum műveletet, amely megegyezik az érdeklődő azonosító, amelyen az logika alkalmazás Érdeklődő azonosítójú érdeklődő keresi.  
![Salesforce művelet kép 5](./media/connectors-create-api-salesforce/action-5.png)  
- Mentse a munkáját. Ennyi az egész, a Get-Objektumesemény hozzáadta a logika alkalmazásba. A Get-objektum vezérlő így néz ki:    
![Salesforce művelet kép 6](./media/connectors-create-api-salesforce/action-6.png)  

Most, hogy hozzáadott egy művelet első, azaz egy érdeklődő, érdemes szeretne elhelyezni, az újonnan létrehozott érdeklődő érdekes. Nagyvállalati érdemes terjesztési lista értesíteni kell, hogy új érdeklődő létrehoztak egy e-mailt küldi. Az Office 365-összekötő segítségével néhány fontos információt az e-mail küldése a Salesforce az új érdeklődő objektum.  

1. Jelölje ki a **művelet Hozzáadás** , majd adja meg az *e-mailben* a keresési vezérlő. A szűrők a műveletek azokat, amelyek küldhet és fogadhat e-mailben.  
- Jelölje ki az **Office 365 Outlook - küldjön e-mailt** listaelemet. Ha az Office 365-ös fiókját még eddig nem hozott létre *kapcsolat* , a rendszer kéri, írja be az Office 365 hitelesítő adatait szeretné létrehozni a dokumentumot a. Miután elkészült, a **Küldés e-mailben** vezérlő nyílik meg.        
![Salesforce művelet kép 7](./media/connectors-create-api-salesforce/action-7.png)  
- Írja be az e-mail címet, és szeretné, hogy **a vezérlő** e-mailt küldi.
-  A **Tárgy** vezérlő írja be a *létrehozott új érdeklődő* –, majd válassza a *vállalat* jogkivonat parancsot. Ekkor megjelenik a Salesforce létrehozott új érdeklődő a *vállalat* mezőt.  
-  Az új érdeklődő objektumot a tokenek közül válassza a **szervezet** vezérlő és is megadhat, függetlenül az e-mail törzsében megjeleníteni kívánt szöveget. Lássunk egy példát:  
![Salesforce művelet kép 8](./media/connectors-create-api-salesforce/action-8.png)   
- Mentse a munkafolyamatot.  

Ez azt. A logikai alkalmazás befejeződött.  

Most tesztelheti az összefüggés-alkalmazás: Salesforce, hozzon létre egy új érdeklődő, amely megfelel a létrehozott feltétel.  Ha teljesen követik a segédlet, csak hozzon létre érdeklődő *amazon.com* , az azt tartalmazó e-mail címre. Néhány másodperc után a logika alkalmazás megtörténjen, és az eredmény az alábbihoz hasonló ez:  
![Salesforce művelet kép 9](./media/connectors-create-api-salesforce/action-9.png)  

