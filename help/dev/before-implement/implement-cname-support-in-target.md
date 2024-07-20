---
keywords: client care, cname, programma di certificazione, nome canonico, cookie, certificato, amc, adobe managed certificate, digicert, convalida del controllo di dominio, dcv, client care2
description: Utilizzare [!UICONTROL Adobe Client Care] per implementare il supporto CNAME (Canonical Name) in [!DNL Adobe Target] per gestire i problemi di blocco degli annunci.
title: Come si utilizza CNAME in Target?
feature: Privacy & Security
exl-id: 5709df5b-6c21-4fea-b413-ca2e4912d6cb
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1164'
ht-degree: 1%

---

# CNAME e Target

Istruzioni per l&#39;utilizzo di [!DNL Adobe Client Care] per implementare il supporto CNAME (Canonical Name) in [!DNL Adobe Target]. Utilizza CNAME per gestire i problemi di blocco degli annunci o i criteri dei cookie relativi a ITP (Intelligent Tracking Prevention). Con CNAME, le chiamate vengono effettuate a un dominio di proprietà del cliente anziché a un dominio di proprietà di Adobe.

## Richiedere il supporto per i CNAME in Target

1. Determina l’elenco di nomi host necessari per il certificato SSL (consulta le domande frequenti di seguito).

1. Per ogni nome host, crea un record CNAME nel DNS che punti al nome host `clientcode.tt.omtrdc.net` [!DNL Target] regolare.

   Ad esempio, se il codice client è &quot;namecustomer&quot; e il nome host proposto è `target.example.com`, il record CNAME DNS sarà simile al seguente:

   ```
   target.example.com.  IN  CNAME  cnamecustomer.tt.omtrdc.net.
   ```

   >[!WARNING]
   >
   >L&#39;autorità di certificazione di Adobe, DigiCert, non può rilasciare un certificato fino al completamento di questo passaggio. Pertanto, Adobe non può soddisfare la richiesta di implementazione di un CNAME fino al completamento di questo passaggio.

1. Adobe [Compila questo modulo](assets/FPC_Request_Form.xlsx) e includilo quando [apri un ticket di assistenza clienti per richiedere il supporto CNAME](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?#reference_ACA3391A00EF467B87930A450050077C):

   * Codice client [!DNL Adobe Target]:
   * Nomi host certificati SSL (esempio: `target.example.com target.example.org`):
   * Acquirente del certificato SSL (l’Adobe è vivamente consigliato, consulta Domande frequenti): Adobe/cliente
   * Se il cliente acquista il certificato, noto anche come &quot;Bring Your Own Certificate&quot; (BYOC), compila questi ulteriori dettagli:
      * Organizzazione del certificato (esempio: Società di esempio S.p.a.):
      * Unità organizzativa del certificato (facoltativo, esempio: Marketing):
      * Paese del certificato (ad esempio, Stati Uniti):
      * Stato/regione del certificato (esempio: California):
      * Città certificato (esempio: San Jose):

1. Se Adobe acquista il certificato, Adobe collabora con DigiCert per acquistare e distribuire il certificato sui server di produzione di Adobe.

   Se il cliente acquista il certificato (BYOC), l’Assistenza clienti Adobe ti invia la richiesta di firma del certificato (CSR, Certificate Signing Request). Utilizza la CSR quando acquisti il certificato tramite l’autorità di certificazione scelta. Dopo il rilascio del certificato, invia una copia del certificato e di tutti i certificati intermedi ad Adobe Client Care per la distribuzione.

   Adobe L’Assistenza clienti ti avvisa quando l’implementazione è pronta.

1. Aggiorna la `serverDomain` [documentazione](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverdomain) al nuovo nome host CNAME e imposta `overrideMboxEdgeServer` su `false` [documentazione](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#overridemboxedgeserver) nella configurazione at.js.

## Domande frequenti

Le seguenti informazioni rispondono alle domande frequenti sulla richiesta e l’implementazione del supporto CNAME in Target:

### Posso fornire un certificato personalizzato (portare il certificato o il BYOC)?

Puoi fornire un certificato personalizzato. Tuttavia, l&#39;Adobe non consiglia questa procedura. La gestione del ciclo di vita del certificato SSL è più semplice sia per Adobe che per l’utente se Adobe acquista e controlla il certificato. I certificati SSL devono essere rinnovati ogni anno. Pertanto, l’Assistenza clienti Adobe deve contattarti ogni anno per ottenere un nuovo certificato in modo tempestivo. Alcuni clienti possono avere difficoltà a produrre un certificato rinnovato in modo tempestivo. L&#39;implementazione di [!DNL Target] è a rischio alla scadenza del certificato perché i browser rifiutano le connessioni.

>[!WARNING]
>
>Se richiedi un&#39;implementazione [!DNL Target] di CNAME con certificato personalizzato, sei responsabile della fornitura di certificati rinnovati all&#39;Assistenza clienti Adobe ogni anno. Se si lascia scadere il certificato CNAME prima che Adobe possa distribuire un certificato rinnovato, si verifica un&#39;interruzione per l&#39;implementazione specifica di [!DNL Target].

### Quanto tempo manca alla scadenza del mio nuovo certificato SSL?

Tutti i certificati acquistati dall&#39;Adobe sono validi per un anno. Per ulteriori informazioni, consulta l&#39;articolo di [DigiCert sui certificati a 1 anno](https://www.digicert.com/blog/position-on-1-year-certificates).

### Quali nomi host scegliere? Quanti nomi host scegliere per dominio?

Le implementazioni CNAME di Target richiedono un solo nome host per dominio sul certificato SSL e nel DNS del cliente. L’Adobe consiglia un nome host per dominio. Alcuni clienti richiedono più nomi host per dominio per i propri scopi (ad esempio, test nella gestione temporanea), che è supportato.

La maggior parte dei clienti sceglie un nome host come `target.example.com`. L&#39;Adobe consiglia di seguire questa procedura, ma la scelta è alla fine tua. Non richiedere un nome host di un record DNS esistente. Questa operazione causa un conflitto e ritarda il tempo di risoluzione della richiesta CNAME di [!DNL Target].

### Ho già un’implementazione CNAME per Adobe Analytics, posso usare lo stesso certificato o nome host?

No, [!DNL Target] richiede un nome host e un certificato separati.

### La mia implementazione corrente di [!DNL Target] è interessata da ITP 2.x?

La versione 2.3 di Apple Intelligent Tracking Prevention (ITP) ha introdotto la funzione di mitigazione del cloaking CNAME, che è in grado di rilevare [!DNL Target] implementazioni CNAME e riduce la scadenza del cookie a sette giorni. Attualmente [!DNL Target] non dispone di alcuna soluzione alternativa per la mitigazione del cloaking CNAME di ITP. Per ulteriori informazioni su ITP, vedere [Apple Intelligent Tracking Prevention (ITP) 2.x](../before-implement/privacy/apple-itp-2x.md).

### Che tipo di interruzioni del servizio posso aspettarmi quando viene implementata la mia implementazione CNAME?

Non si verifica alcuna interruzione del servizio quando il certificato viene distribuito (inclusi i rinnovi del certificato).

Tuttavia, dopo aver modificato il nome host nel codice di implementazione [!DNL Target] (`serverDomain` in at.js) nel nuovo nome host CNAME (`target.example.com`), i browser Web considerano i visitatori di ritorno come nuovi visitatori. I dati di profilo dei visitatori di ritorno andranno persi perché il cookie precedente non è accessibile con il vecchio nome host (`clientcode.tt.omtrdc.net`). Il cookie precedente non è accessibile a causa di modelli di sicurezza del browser. Questa interruzione si verifica solo al cut-over iniziale al nuovo CNAME. I rinnovi del certificato non hanno lo stesso effetto perché il nome host non cambia.

### Quale tipo di chiave e algoritmo di firma del certificato sono utilizzati per l’implementazione del CNAME?

Per impostazione predefinita, tutti i certificati sono RSA SHA-256 e le chiavi sono RSA 2048 bit. Le dimensioni delle chiavi superiori a 2048 bit non sono attualmente supportate.

### Come posso verificare che la mia implementazione CNAME sia pronta per il traffico?

Utilizza il seguente set di comandi (nel terminale della riga di comando macOS o Linux, utilizzando bash e curl >=7.49):

1. Copiare e incollare la funzione bash nel terminale o incollarla nel file dello script di avvio bash (in genere `~/.bash_profile` o `~/.bashrc`) in modo che la funzione sia disponibile nelle sessioni terminale:

   ```
   function adobeTargetCnameValidation {
     local hostname="$1"
     if [ -z "$hostname" ]; then
       echo "ERROR: no hostname specified"
       return 1
     fi
   
     local service="Adobe Target CNAME implementation"
     local edges="31 32 34 35 36 37 38"
     local edgeDomain="tt.omtrdc.net"
     local edgeFormat="mboxedge%d%s.$edgeDomain"
     local shardFormat="-alb%02d"
     local shards=5
     local shardsFoundCount=0
     local shardsFound
     local shardsFoundOutput
     local curlRegex="subject:.*CN=|expire date:|issuer:"
     local curlValidation="SSL certificate verify ok"
     local curlResponseValidation='"OK"'
     local curlEndpoint="/uptime?mboxClient=uptime3"
     local url="https://$hostname$curlEndpoint"
     local sslLabsUrl="https://ssllabs.com/ssltest/analyze.html?hideResults=on&latest&d=$hostname"
     local success="✅"
     local failure="🚫"
     local info="🔎"
     local rule="="
     local horizontalRule="$(seq ${COLUMNS:-30} | xargs printf "$rule%.0s")"
     local miniRule="$(seq 5 | xargs printf "$rule%.0s")"
     local curlVersion="$(curl --version | head -1 | cut -d' ' -f2 )"
     local curlVersionRequired=">=7.49"
     local edgeCount="$(wc -w <<< "$edges" | tr -d ' ')"
     local edge
     local shard
     local currEdgeShard
     local dnsOutput
     local cnameExists
     local endToEndTestSucceeded
     local curlResult
   
     for shard in $(seq $shards); do
       if [ "$shardsFoundCount" -eq 0 ]; then
         for edge in $edges; do
           if [ "$shard" -eq 1 ]; then
             currEdgeShard="$(printf "$edgeFormat" "$edge" "")"
           else
             currEdgeShard="$(
               printf "$edgeFormat" "$edge" "$(
                 printf -- "$shardFormat" "$shard"
               )"
             )"
           fi
           curlResult="$(curl -vsm20 --connect-to "$hostname:443:$currEdgeShard:443" "$url" 2>&1)"
           if grep -q "$curlValidation" <<< "$curlResult"; then
             shardsFound+=" $currEdgeShard"
             if grep -q "$curlResponseValidation" <<< "$curlResult"; then
               shardsFoundCount=$((shardsFoundCount+1))
               shardsFoundOutput+="\n\n$miniRule $success $hostname [edge shard: $currEdgeShard] $miniRule\n"
             else
               shardsFoundOutput+="\n\n$miniRule $failure $hostname [edge shard: $currEdgeShard] $miniRule\n"
             fi
             shardsFoundOutput+="$(grep -E "$curlRegex" <<< "$curlResult" | sort)"
             if ! grep -q "$curlResponseValidation" <<< "$curlResult"; then
               shardsFoundOutput+="\nERROR: unexpected HTTP response from this shard using $url"
             fi
           fi
         done
       fi
     done
   
     echo
     echo "$horizontalRule"
     echo
     echo "$service validation for hostname $hostname:"
     dnsOutput="$(dig -t CNAME +short "$hostname" 2>&1)"
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
   
     curlResult="$(curl -vsm20 "$url" 2>&1)"
     if grep -q "$curlValidation" <<< "$curlResult"; then
       if grep -q "$curlResponseValidation" <<< "$curlResult"; then
         echo -en "$success $hostname passes TLS and HTTP response validation"
         if [ -n "$cnameExists" ]; then
           echo
         else
           echo " -- the DNS CNAME is not pointing to the correct subdomain for ${service}s with Adobe-managed certificates" \
             "(bring-your-own-certificate implementations don't have this requirement), but this test passes as configured"
         fi
         endToEndTestSucceeded=true
       else
         echo -n "$failure $hostname FAILED HTTP response validation --" \
           "unexpected response from $url -- "
         if [ -n "$cnameExists" ]; then
           echo "DNS is NOT pointing to the correct shard, notify Adobe Client Care"
         else
           echo "the required DNS CNAME record is missing, see above"
         fi
       fi
     else
   
       echo -n "$failure $hostname FAILED TLS validation -- "
       if [ -n "$cnameExists" ]; then
         echo "DNS is likely NOT pointing to the correct shard or there's a validation issue with the certificate or" \
           "protocols, see curl output below and optionally SSL Labs ($sslLabsUrl):"
         echo ""
         echo "$horizontalRule"
         echo "$curlResult" | sed 's/^/    /g'
         echo "$horizontalRule"
         echo ""
       else
         echo "the required DNS CNAME record is missing, see above"
       fi
     fi
   
     if [ "$shardsFoundCount" -ge "$edgeCount" ]; then
       echo -n "$success $hostname passes shard validation for the following $shardsFoundCount edge shards:"
       echo -e "$shardsFoundOutput"
       echo
   
       if [ -n "$cnameExists" ] && [ -n "$endToEndTestSucceeded" ]; then
         echo "$horizontalRule"
         echo ""
         echo "  For additional TLS/SSL validation, including detailed browser/client support,"
         echo "  see SSL Labs (click the first IP address if prompted):"
         echo ""
         echo "    $info  $sslLabsUrl"
         echo ""
         echo "  To check DNS propagation around the world, see whatsmydns.net:"
         echo ""
         echo "    $info  DNS A records:     https://whatsmydns.net/#A/$hostname"
         echo "    $info  DNS CNAME record:  https://whatsmydns.net/#CNAME/$hostname"
       fi
     else
       echo -n "$failure $hostname FAILED shard validation -- shards found: $shardsFoundCount," \
         "expected: $edgeCount"
       if bc -l <<< "$(cut -d. -f1,2 <<< "$curlVersion") $curlVersionRequired" 2>/dev/null | grep -q 0; then
         echo -n " -- insufficient curl version installed: $curlVersion, but this script requires curl version" \
           "$curlVersionRequired because it uses the curl --connect-to flag to bypass DNS and directly test" \
           "each Adobe Target edge shards' SNI confirguation for $hostname"
       fi
       if [ -n "$shardsFoundOutput" ]; then
         echo -e ":\n$shardsFoundOutput"
       fi
       echo
     fi
     echo
     echo "$horizontalRule"
     echo
   }
   ```

1. Incolla questo comando (sostituendo `target.example.com` con il tuo nome host):

   ```
   adobeTargetCnameValidation target.example.com
   ```

   Se l’implementazione è pronta, viene visualizzato un output simile al seguente. La parte importante è che tutte le righe dello stato di convalida mostrano `✅` anziché `🚫`. Ogni partizione CNAME edge di Target deve mostrare `CN=target.example.com`, che corrisponde al nome host primario nel certificato richiesto (in questo output non vengono stampati nomi host SAN aggiuntivi nel certificato).

   ```
   $ adobeTargetCnameValidation target.example.com
   
   ==========================================================
   
   Adobe Target CNAME implementation validation for hostname target.example.com:
   ✅ target.example.com passes DNS CNAME validation
   ✅ target.example.com passes TLS and HTTP response validation
   ✅ target.example.com passes shard validation for the following 7 edge shards:
   
   ===== ✅ target.example.com [edge shard: mboxedge31-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge32-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge34-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge35-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge36-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge37-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ===== ✅ target.example.com [edge shard: mboxedge38-alb02.tt.omtrdc.net] =====
   *  expire date: Jul 22 23:59:59 2022 GMT
   *  issuer: C=US; O=DigiCert Inc; CN=DigiCert TLS RSA SHA256 2020 CA1
   *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com
   
   ==========================================================
   
     For additional TLS/SSL validation, including detailed browser/client support,
     see SSL Labs (click the first IP address if prompted):
   
       🔎  https://ssllabs.com/ssltest/analyze.html?hideResults=on&latest&d=target.example.com
   
     To check DNS propagation around the world, see whatsmydns.net:
   
       🔎  DNS A records:     https://whatsmydns.net/#A/target.example.com
       🔎  DNS CNAME record:  https://whatsmydns.net/#CNAME/target.example.com
   
   ==========================================================
   ```

>[!NOTE]
>
>Se il comando di convalida non riesce durante la convalida DNS ma sono già state apportate le modifiche DNS necessarie, potrebbe essere necessario attendere la propagazione completa degli aggiornamenti DNS. Ai record DNS è associato un valore [TTL (time-to-live)](https://en.wikipedia.org/wiki/Time_to_live#DNS_records) che determina la scadenza della cache per le risposte DNS di tali record. Di conseguenza, potrebbe essere necessario attendere almeno tanto quanto i TTL. Puoi utilizzare il comando `dig target.example.com` o [la Casella degli strumenti di G Suite](https://toolbox.googleapps.com/apps/dig/#CNAME) per cercare i TTL specifici. Per verificare la propagazione del DNS in tutto il mondo, vedere [whatsmydns.net](https://whatsmydns.net/#CNAME).

### Come si utilizza un collegamento di rinuncia con CNAME

Se utilizzi CNAME, il collegamento di rinuncia deve contenere il parametro &quot;client=`clientcode`, ad esempio:
`https://my.cname.domain/optout?client=clientcode`.

Sostituisci `clientcode` con il tuo codice client, quindi aggiungi il testo o l&#39;immagine da collegare all&#39;[URL di rinuncia](privacy/privacy.md).

## Limitazioni note

* La modalità di controllo qualità non è definitiva se si dispone di CNAME e at.js 1.x, in quanto si basa su un cookie di terze parti. La soluzione consiste nell’aggiungere i parametri di anteprima a ogni URL a cui accedi. La modalità di controllo qualità è persistente quando si dispone di CNAME e at.js 2.x.
* Quando si utilizza CNAME, è più probabile che la dimensione dell&#39;intestazione del cookie per le chiamate [!DNL Target] aumenti. L’Adobe consiglia di mantenere la dimensione del cookie inferiore a 8 KB.
