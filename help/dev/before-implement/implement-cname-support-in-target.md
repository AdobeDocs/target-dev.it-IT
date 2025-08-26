---
keywords: client care, cname, programma di certificazione, nome canonico, cookie, certificato, amc, adobe managed certificate, digicert, convalida del controllo di dominio, dcv, client care2
description: Utilizzare [!UICONTROL Adobe Client Care] per implementare il supporto CNAME (Canonical Name) in [!DNL Adobe Target] per gestire i problemi di blocco degli annunci.
title: Come si utilizza CNAME in Target?
feature: Privacy & Security
exl-id: 5709df5b-6c21-4fea-b413-ca2e4912d6cb
source-git-commit: 71a8a2d9d324cd31452a4400d76052432efbfdd4
workflow-type: tm+mt
source-wordcount: '1169'
ht-degree: 1%

---

# CNAME e Target

Istruzioni per l&#39;utilizzo di [!DNL Adobe Client Care] per implementare il supporto CNAME (Canonical Name) in [!DNL Adobe Target]. Utilizza CNAME per gestire i problemi di blocco degli annunci o i criteri dei cookie relativi a ITP (Intelligent Tracking Prevention). Con CNAME, le chiamate vengono effettuate a un dominio di proprietà del cliente anziché di Adobe.

## Richiedere il supporto per i CNAME in Target

1. Determina l’elenco di nomi host necessari per il certificato SSL (consulta le domande frequenti di seguito).

1. Per ogni nome host, crea un record CNAME nel DNS che punti al nome host [!DNL Target] `clientcode.tt.omtrdc.net` regolare.

   Ad esempio, se il codice client è &quot;namecustomer&quot; e il nome host proposto è `target.example.com`, il record CNAME DNS sarà simile al seguente:

   ```
   target.example.com.  IN  CNAME  cnamecustomer.tt.omtrdc.net.
   ```

   >[!WARNING]
   >
   >DigiCert, l’autorità di certificazione di Adobe, non può rilasciare un certificato fino al completamento di questo passaggio. Pertanto, Adobe non può soddisfare la richiesta di implementazione di un CNAME fino al completamento di questo passaggio.

1. [Compila questo modulo](assets/FPC_Request_Form.xlsx) e includilo quando [apri un ticket dell&#39;Assistenza clienti di Adobe per richiedere il supporto CNAME](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?lang=it&#reference_ACA3391A00EF467B87930A450050077C):

   * Codice client [!DNL Adobe Target]:
   * Nomi host certificati SSL (esempio: `target.example.com target.example.org`):
   * Acquirente del certificato SSL (Adobe è vivamente consigliato, consulta Domande frequenti): Adobe/cliente
   * Se il cliente acquista il certificato, noto anche come &quot;Bring Your Own Certificate&quot; (BYOC), compila questi ulteriori dettagli:
      * Organizzazione del certificato (esempio: Società di esempio S.p.a.):
      * Unità organizzativa del certificato (facoltativo, esempio: Marketing):
      * Paese del certificato (ad esempio, Stati Uniti):
      * Stato/regione del certificato (esempio: California):
      * Città certificato (esempio: San Jose):

1. Se Adobe acquista il certificato, Adobe collabora con DigiCert per acquistare e distribuire il certificato sui server di produzione di Adobe.

   Se il cliente acquista il certificato (BYOC), l’Assistenza clienti di Adobe ti invia la richiesta di firma del certificato (CSR, Certificate Signing Request). Utilizza la CSR quando acquisti il certificato tramite l’autorità di certificazione scelta. Dopo il rilascio del certificato, invia una copia del certificato e di tutti i certificati intermedi all’Assistenza clienti di Adobe per la distribuzione.

   L’Assistenza clienti di Adobe ti avvisa quando l’implementazione è pronta.

1. Aggiorna la `serverDomain` [documentazione](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverdomain) al nuovo nome host CNAME e imposta `overrideMboxEdgeServer` su `false` [documentazione](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#overridemboxedgeserver) nella configurazione at.js.

## Domande frequenti

Le seguenti informazioni rispondono alle domande frequenti sulla richiesta e l’implementazione del supporto CNAME in Target:

### Posso fornire un certificato personalizzato (portare il certificato o il BYOC)?

Puoi fornire un certificato personalizzato. Tuttavia, Adobe sconsiglia questa procedura. La gestione del ciclo di vita del certificato SSL è più semplice sia per Adobe che per te se Adobe acquista e controlla il certificato. I certificati SSL devono essere rinnovati ogni anno. Pertanto, l’Assistenza clienti di Adobe deve contattarti ogni anno per ottenere un nuovo certificato in modo tempestivo. Alcuni clienti possono avere difficoltà a produrre un certificato rinnovato in modo tempestivo. L&#39;implementazione di [!DNL Target] è a rischio alla scadenza del certificato perché i browser rifiutano le connessioni.

>[!WARNING]
>
>Se richiedi un&#39;implementazione [!DNL Target] di CNAME con certificato personale, sei responsabile della fornitura di certificati rinnovati all&#39;Assistenza clienti di Adobe ogni anno. Consentendo la scadenza del certificato CNAME prima che Adobe possa distribuire un certificato rinnovato, si verifica un&#39;interruzione per l&#39;implementazione specifica di [!DNL Target].

### Quanto tempo manca alla scadenza del mio nuovo certificato SSL?

Tutti i certificati acquistati da Adobe sono validi per un anno. Per ulteriori informazioni, consulta l&#39;articolo di [DigiCert sui certificati a 1 anno](https://www.digicert.com/blog/position-on-1-year-certificates).

### Quali nomi host scegliere? Quanti nomi host scegliere per dominio?

Le implementazioni CNAME di Target richiedono un solo nome host per dominio sul certificato SSL e nel DNS del cliente. Adobe consiglia un nome host per dominio. Alcuni clienti richiedono più nomi host per dominio per i propri scopi (ad esempio, test nella gestione temporanea), che è supportato.

La maggior parte dei clienti sceglie un nome host come `target.example.com`. Adobe consiglia di seguire questa procedura, ma in ultima analisi la scelta è tua. Non richiedere un nome host di un record DNS esistente. Questa operazione causa un conflitto e ritarda il tempo di risoluzione della richiesta CNAME di [!DNL Target].

### Ho già un’implementazione CNAME per Adobe Analytics, posso usare lo stesso certificato o nome host?

No, [!DNL Target] richiede un nome host e un certificato separati.

### La mia implementazione corrente di [!DNL Target] è interessata da ITP 2.x?

La versione 2.3 di Apple Intelligent Tracking Prevention (ITP) ha introdotto la funzione di mitigazione del cloaking CNAME, che è in grado di rilevare [!DNL Target] implementazioni CNAME e riduce la scadenza del cookie a sette giorni. Attualmente [!DNL Target] non dispone di alcuna soluzione alternativa per la mitigazione del cloaking CNAME di ITP. Per ulteriori informazioni su ITP, vedere [Apple Intelligent Tracking Prevention (ITP) 2.x](../before-implement/privacy/apple-itp-2x.md).

### Che tipo di interruzioni del servizio posso aspettarmi quando viene implementata la mia implementazione CNAME?

Non si verifica alcuna interruzione del servizio quando il certificato viene distribuito (inclusi i rinnovi del certificato).

Tuttavia, dopo aver modificato il nome host nel codice di implementazione [!DNL Target] (`serverDomain` in at.js) nel nuovo nome host CNAME (`target.example.com`), i browser Web considerano i visitatori di ritorno come nuovi visitatori. I dati di profilo dei visitatori di ritorno andranno persi perché il cookie precedente non è accessibile con il vecchio nome host (`clientcode.tt.omtrdc.net`). Il cookie precedente non è accessibile a causa di modelli di sicurezza del browser. Questa interruzione si verifica solo al cut-over iniziale al nuovo CNAME. I rinnovi del certificato non hanno lo stesso effetto perché il nome host non cambia.

### Quale tipo di chiave e algoritmo di firma del certificato sono utilizzati per l’implementazione del CNAME?

Per impostazione predefinita, tutti i certificati sono RSA SHA-256 e le chiavi sono RSA 2048 bit. Le dimensioni delle chiavi superiori a 2048 bit devono essere richieste esplicitamente tramite [!UICONTROL Customer Care].

### Come posso verificare che la mia implementazione CNAME sia pronta per il traffico?

Utilizza il seguente set di comandi (nel terminale della riga di comando macOS o Linux, utilizzando bash e curl >=7.49):

1. Copiare e incollare la funzione bash nel terminale o incollarla nel file dello script di avvio bash (in genere `~/.bash_profile` o `~/.bashrc`) in modo che la funzione sia disponibile nelle sessioni terminale:

   +++ Vedi i dettagli

   ```bash {line-numbers="true"}
    function adobeTargetCnameValidation {
   
     local hostname="$1"
   
     if [ -z "$hostname" ]; then
       echo "ERROR: no hostname specified"
       return 1
     fi
   
     local service="Adobe Target CNAME implementation"
     local edges="41 42 44 45 46 47 48"
     local edgeDomain="tt.omtrdc.net"
     local edgeFormat="mboxedge%d%s.$edgeDomain"
     local poolDomain="pool.data.adobedc.net"
     local shards=5
     local shardsFoundCount=0
     local shardsFound=""
     local shardsFoundOutput=""
     local curlRegex="subject:.*CN=|expire date:|issuer:"
     local curlValidation="SSL certificate verify ok"
     local curlResponseValidation='"OK"'
     local curlEndpoint="/uptime?mboxClient=uptime3"
     local url="https://$hostname$curlEndpoint"
     local sslShopperUrl="https://www.sslshopper.com/ssl-checker.html#hostname=$hostname"
     local success="✅"
     local failure="🚫"
     local info="🔎"
     local rule="="
     local horizontalRule="$(seq ${COLUMNS:-30} | xargs printf "$rule%.0s")"
     local miniRule="$(seq 5 | xargs printf "$rule%.0s")"
     local curlVersion="$(curl --version | head -1 | cut -d' ' -f2)"
     local curlVersionRequired=7.49
     local edgeCount="$(wc -w <<< "$edges" | tr -d ' ')"
     local cnameExists=""
     local endToEndTestSucceeded=""
   
     for region in IRL1 IND1 SIN OR SYD VA TYO; do
       local currShard="${region}-${poolDomain}"
       local curlResult="$(curl -vsm20 --connect-to "$hostname:443:$currShard:443" "$url" 2>&1)"
   
       if grep -q "$curlValidation" <<< "$curlResult"; then
         shardsFound+=" $currShard"
   
         if grep -q "$curlResponseValidation" <<< "$curlResult"; then
           shardsFoundCount=$((shardsFoundCount+1))
           shardsFoundOutput+="\n\n$miniRule $success $hostname [edge shard: $currShard] $miniRule\n"
         else
           shardsFoundOutput+="\n\n$miniRule $failure $hostname [edge shard: $currShard] $miniRule\n"
         fi
   
         shardsFoundOutput+="$(grep -E "$curlRegex" <<< "$curlResult" | sort)"
   
         if ! grep -q "$curlResponseValidation" <<< "$curlResult"; then
           shardsFoundOutput+="\nERROR: unexpected HTTP response from this shard using $url"
         fi
       fi
     done
   
     echo
     echo "$horizontalRule"
     echo
     echo "$service validation for hostname $hostname:"
   
     local dnsOutput="$(dig -t CNAME +short "$hostname" 2>&1)"
     if grep -qFi ".$edgeDomain" <<< "$dnsOutput"; then
       echo "$success $hostname passes DNS CNAME validation"
       cnameExists=true
     else
       echo -n "$failure $hostname FAILED DNS CNAME validation -- "
       if [ -n "$dnsOutput" ]; then
         echo -e "$dnsOutput is not in the subdomain $edgeDomain"
       else
         echo "required DNS CNAME record pointing to <target-client-code>.$edgeDomain not found"
       fi
     fi
   
     for region in IRL1 IND1 SIN OR SYD VA TYO; do
       local curlResult="$(curl -vsm20 --connect-to "$hostname:443:${region}-pool.data.adobedc.net:443" "https://$hostname$curlEndpoint" 2>&1)"
   
       if grep -q "$curlValidation" <<< "$curlResult"; then
         if grep -q "$curlResponseValidation" <<< "$curlResult"; then
           echo -en "$success $hostname passes TLS and HTTP response validation for region $region"
           if [ -n "$cnameExists" ]; then
             echo
           else
             echo " -- the DNS CNAME is not pointing to the correct subdomain for ${service}s with Adobe-managed certificates" \
               "(bring-your-own-certificate implementations don't have this requirement), but this test passes as configured"
           fi
           endToEndTestSucceeded=true
         else
           echo -n "$failure $hostname FAILED HTTP response validation for region $region --" \
             "unexpected response from $url -- "
           if [ -n "$cnameExists" ]; then
             echo "DNS is NOT pointing to the correct shard, notify Adobe Client Care"
           else
             echo "the required DNS CNAME record is missing, see above"
           fi
         fi
       else
         echo -n "$failure $hostname FAILED TLS validation for region $region -- "
         if [ -n "$cnameExists" ]; then
           echo "DNS is likely NOT pointing to the correct shard or there's a validation issue with the certificate or" \
             "protocols, see curl output below and optionally SSL Shopper ($sslShopperUrl):"
           echo ""
           echo "$horizontalRule"
           echo "$curlResult" | sed 's/^/    /g'
           echo "$horizontalRule"
           echo ""
         else
           echo "the required DNS CNAME record is missing, see above"
         fi
       fi
     done
   
     if [ "$shardsFoundCount" -ge "$edgeCount" ]; then
       echo -n "$success $hostname passes shard validation for the following $shardsFoundCount edge shards:"
       echo -e "$shardsFoundOutput"
       echo
   
       if [ -n "$cnameExists" ] && [ -n "$endToEndTestSucceeded" ]; then
         echo "$horizontalRule"
         echo ""
         echo "  For additional TLS/SSL validation, see SSL Shopper:"
         echo ""
         echo "    $info  $sslShopperUrl"
         echo ""
         echo "  To check DNS propagation around the world, see whatsmydns.net:"
         echo ""
         echo "    $info  DNS A records:     https://whatsmydns.net/#A/$hostname"
         echo "    $info  DNS CNAME record:  https://whatsmydns.net/#CNAME/$hostname"
       fi
     else
       echo -n "$failure $hostname FAILED shard validation -- shards found: $shardsFoundCount," \
         "expected: $edgeCount"
       echo ""
     fi
   
     echo
     echo "$horizontalRule"
     echo
   }
   ```

   +++

1. Incolla questo comando (sostituendo `target.example.com` con il tuo nome host):

   `adobeTargetCnameValidation target.example.com`

   Se l’implementazione è pronta, viene visualizzato un output simile al seguente. La parte importante è che tutte le righe dello stato di convalida mostrano `✅` anziché `🚫`. Ogni partizione CNAME edge di Target deve mostrare `CN=target.example.com`, che corrisponde al nome host primario nel certificato richiesto (in questo output non vengono stampati nomi host SAN aggiuntivi nel certificato).

   +++ Vedi i dettagli

   ```bash {line-numbers="true"}
    $ adobeTargetCnameValidation 
    target.example.com==========================================================Adobe Target CNAME implementation validation for hostname target.example.com:
    ✅ target.example.com passes DNS CNAME validation
    ✅ target.example.com passes TLS and HTTP response validation for region IRL1
    ✅ target.example.com passes TLS and HTTP response validation for region IND1
    ✅ target.example.com passes TLS and HTTP response validation for region SIN
    ✅ target.example.com passes TLS and HTTP response validation for region OR
    ✅ target.example.com passes TLS and HTTP response validation for region SYD
    ✅ target.example.com passes TLS and HTTP response validation for region VA
    ✅ target.example.com passes TLS and HTTP response validation for region TYO
    ✅ target.example.com passes shard validation for the following 7 edge shards:===== ✅ target.example.com [edge shard: IRL1-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: IND1-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: SIN-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: OR-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: SYD-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: VA-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: TYO-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com==========================================================  For additional TLS/SSL validation, see SSL Shopper:    🔎  https://www.sslshopper.com/ssl-checker.html#hostname=target.example.com  To check DNS propagation around the world, see whatsmydns.net:    🔎  DNS A records:     https://whatsmydns.net/#A/target.example.com
        🔎  DNS CNAME record:  https://whatsmydns.net/#CNAME/target.example.com 
   ```

   +++

>[!NOTE]
>
>Se il comando di convalida non riesce durante la convalida DNS ma sono già state apportate le modifiche DNS necessarie, potrebbe essere necessario attendere la propagazione completa degli aggiornamenti DNS. Ai record DNS è associato un valore [TTL (time-to-live)](https://en.wikipedia.org/wiki/Time_to_live#DNS_records) che determina la scadenza della cache per le risposte DNS di tali record. Di conseguenza, potrebbe essere necessario attendere almeno tanto quanto i TTL. Puoi utilizzare il comando `dig target.example.com` o [la Casella degli strumenti di G Suite](https://toolbox.googleapps.com/apps/dig/#CNAME) per cercare i TTL specifici. Per verificare la propagazione del DNS in tutto il mondo, vedere [whatsmydns.net](https://whatsmydns.net/#CNAME).

### Come si utilizza un collegamento di rinuncia con CNAME

Se utilizzi CNAME, il collegamento di rinuncia deve contenere il parametro &quot;client=`clientcode`, ad esempio:
`https://my.cname.domain/optout?client=clientcode`.

Sostituisci `clientcode` con il tuo codice client, quindi aggiungi il testo o l&#39;immagine da collegare all&#39;[URL di rinuncia](privacy/privacy.md).

## Limitazioni note

* La modalità di controllo qualità non è definitiva se si dispone di CNAME e at.js 1.x, in quanto si basa su un cookie di terze parti. La soluzione consiste nell’aggiungere i parametri di anteprima a ogni URL a cui accedi. La modalità di controllo qualità è persistente quando si dispone di CNAME e at.js 2.x.
* Quando si utilizza CNAME, è più probabile che la dimensione dell&#39;intestazione del cookie per le chiamate [!DNL Target] aumenti. Adobe consiglia di mantenere la dimensione del cookie al di sotto di 8 KB.
