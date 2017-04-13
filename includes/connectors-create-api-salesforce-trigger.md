Ez a segédlet megtanulhatja a **Salesforce - objektum létrehozásakor** eseményindító használatáról a Salesforce új érdeklődő létrehozásakor logika alkalmazás munkafolyamat indítására.

>[AZURE.NOTE]Jelentkezzen be a Salesforce-fiókjába, ha még nem hozott létre Salesforce- *kapcsolat* fogja kérni.  

1. *Salesforce* a logika alkalmazások Tervező a Keresés mezőbe írja be, majd válassza ki a **Salesforce - objektum létrehozásakor** eseményindító.  
![Salesforce eseményindító kép 1](./media/connectors-create-api-salesforce/trigger-1.png)   
- Az **objektum létrehozásakor** vezérlőelem jelenik meg.  
![Salesforce eseményindító kép 2](./media/connectors-create-api-salesforce/trigger-2.png)   
- Válassza ki az **Objektum típusa** , majd jelölje ki az *érdeklődő* objektumok listája. Ebben a lépésben vannak jelezve, hogy hoz létre, amely a logika alkalmazás értesítést, valahányszor új érdeklődő Salesforce jön létre az eseményindító.   
![Salesforce eseményindító kép 3](./media/connectors-create-api-salesforce/trigger-3.png)   
- Ez azt. Az eseményindító létrehozott. Azonban kell legalább egy művelet létrehozása kezdeményezéséhez meg egy érvényes logika alkalmazást.    
![Salesforce eseményindító kép 4](./media/connectors-create-api-salesforce/trigger-4.png)   

Ezen a ponton a logika alkalmazás van beállítva, hogy a Futtatás eseményindítók és a munkafolyamat műveletei elindul, amikor új elemet hoz létre a Salesforce az eseményindító.  