A DNS-zóna használható egy adott tartomány DNS-rekordjait üzemelteti. Indítsa el a tartomány szolgáltatójánál, hogy a DNS-zóna létrehozásához szükséges. Bármelyik létrehozott egy adott tartomány DNS-rekordot a tartományhoz tartozó DNS-zóna belül lesz. 

Például a "contoso.com" tartomány DNS-rekordjait, például "mail.contoso.com" (a levelezési kiszolgáló) és a "www.contoso.com" (a webhely) számú is tartalmaz. 


## <a name="names"></a>Tudnivalók a DNS-zóna nevek
 
- A zóna nevét az erőforráscsoport belül egyedinek kell lennie, és a zóna nem kell már létezik. Egyéb esetben a művelet sikertelen lesz.

- Lehet, hogy az azonos nevű zóna újra felhasználni egy másik erőforráscsoport vagy egy másik Azure-előfizetést. 

- Ha több zónák megosztása ugyanazt a nevet, egyes példányok osztják más néven kiszolgálói címeket, és csak egy példány a szülő tartományból adható át. További tudnivalókért olvassa el a [meghatalmazott Azure DNS a tartomány](../articles/dns/dns-domain-delegation.md)című témakört.