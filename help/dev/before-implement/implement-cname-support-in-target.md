---
keywords: client care;cname;programma di certificato;nome canonico;cookie;certificato;amc;adobe managed certificate;digicert;convalida del controllo di dominio;dcv
description: Lavora con  [!DNL Adobe] l'Assistenza clienti per implementare il supporto CNAME (Canonical Name) in [!DNL Adobe Target] per gestire i problemi di blocco degli annunci.
title: Come si utilizza CNAME in Target?
feature: Privacy & Security
role: Developer
exl-id: bf533771-6d46-48ba-964c-3ad9ce9f7352
TQID: https://experienceleague.adobe.com/gTS60hypD2WGc2fJh-sUkq2-pkzt2KnM4CzSQ050L40
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1255
ht-degree: 1%

---

# CNAME e [!DNL Target]

Istruzioni per l&#39;utilizzo di [!DNL Adobe] Client Care per implementare il supporto CNAME (Canonical Name) in [!DNL Adobe Target]. Utilizza CNAME per gestire i problemi di blocco degli annunci o i criteri dei cookie relativi a ITP (Intelligent Tracking Prevention). Con CNAME, le chiamate vengono effettuate a un dominio di proprietà del cliente anziché a un dominio di proprietà di [!DNL Adobe].

## Richiedi supporto CNAME in [!DNL Target]

1. Determina l’elenco di nomi host necessari per il certificato SSL (consulta le domande frequenti di seguito).
1. [Compila questo modulo](/help/dev/implement/assets/FPC_Request_Form.xlsx) e includilo quando [apri un  [!DNL Adobe] ticket di assistenza clienti per richiedere il supporto CNAME](https://experienceleague.adobe.com/it/docs/target/using/cmp-resources-and-contact-information#reference_ACA3391A00EF467B87930A450050077C):

   * Codice client [!DNL Adobe Target]:
   * Nomi host certificati SSL (esempio: `target.example.com target.example.org`):
   * L&#39;acquirente del certificato SSL ([!DNL Adobe] è vivamente consigliato, vedere Domande frequenti): Adobe/cliente
   * Se il cliente acquista il certificato, noto anche come &quot;Bring Your Own Certificate&quot; (BYOC), compila questi ulteriori dettagli:

      * Organizzazione del certificato (esempio: Società di esempio S.p.a.):
      * Unità organizzativa del certificato (facoltativo, esempio: Marketing):
      * Paese del certificato (ad esempio, Stati Uniti):
      * Stato/regione del certificato (esempio: California):
      * Città certificato (esempio: San Jose):

1. Per ogni richiesta di nome host, Adobe creerà l&#39;implementazione e restituirà un nome di record CNAME da creare, che conterrà una stringa casuale suffissata da `tt.omtrdc.net`

   Ad esempio, se hai effettuato una richiesta per `target.example.com`, ti restituiremo un CNAME sotto forma di `abcdefgh.tt.omtrdc.net`. Il record CNAME DNS deve essere simile al seguente:

   ```
   target.example.com.  IN  CNAME  abcdefgh.tt.omtrdc.net.
   ```

   >[!IMPORTANT]
   >
   >L&#39;autorità di certificazione di [!DNL Adobe], DigiCert, non può rilasciare un certificato fino al completamento di questo passaggio. Pertanto, [!DNL Adobe] non può soddisfare la richiesta per un&#39;implementazione CNAME fino al completamento di questo passaggio.

1. Se [!DNL Adobe] sta acquistando il certificato, [!DNL Adobe] collabora con DigiCert per acquistare e distribuire il certificato sui server di produzione di [!DNL Adobe].

   Se il cliente acquista il certificato (BYOC), l&#39;assistenza clienti [!DNL Adobe] ti invia la richiesta di firma del certificato (CSR, Certificate Signing Request). Utilizza la CSR quando acquisti il certificato tramite l’autorità di certificazione scelta. Dopo il rilascio del certificato, inviare una copia del certificato ed eventuali certificati intermedi all&#39;assistenza clienti [!DNL Adobe] per la distribuzione.

   [!DNL Adobe] L&#39;Assistenza clienti ti avvisa quando l&#39;implementazione è pronta.

1. Aggiorna la `serverDomain` ([documentazione](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverDomain)) al nuovo nome host CNAME e imposta `overrideMboxEdgeServer` su `false` ([documentazione](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#overridemboxedgeserver)) nella configurazione at.js.

## Domande frequenti

Le informazioni seguenti rispondono alle domande frequenti sulla richiesta e l&#39;implementazione del supporto CNAME in [!DNL Target]:

### Posso fornire un certificato personalizzato (portare il certificato o il BYOC)?

Puoi fornire un certificato personalizzato. Tuttavia, [!DNL Adobe] consiglia vivamente di non seguire questa procedura. La gestione del ciclo di vita del certificato SSL è più semplice sia per [!DNL Adobe] che per te, se [!DNL Adobe] acquista e controlla il certificato. La durata dei certificati SSL sarà solo più breve nel tempo (consulta la sezione successiva sulla durata del certificato). Pertanto, l&#39;Assistenza clienti [!DNL Adobe] deve contattarti ogni volta per ottenere un nuovo certificato in modo tempestivo. Questo si rivelerà un problema se la durata del certificato verrà ridotta a soli 47 giorni. L&#39;implementazione di [!DNL Target] è a rischio alla scadenza del certificato perché i browser rifiutano le connessioni.

>[!IMPORTANT]
>
>Se richiedi un&#39;implementazione di CNAME con certificato [!DNL Target], dovrai fornire certificati rinnovati all&#39;Assistenza clienti [!DNL Adobe] ogni volta che scadrà. Consentendo che il certificato CNAME scada prima che [!DNL Adobe] possa distribuire un certificato rinnovato, si verifica un&#39;interruzione per l&#39;implementazione specifica di [!DNL Target].

### Quanto tempo manca alla scadenza del mio nuovo certificato SSL?

La durata di tutti i certificati verrà ridotta nell’ambito di un’importante iniziativa delle autorità di certificazione. Per DigiCert, il provider di certificati di [!DNL Adobe], verrà applicata la seguente pianificazione:

Fino al 15 marzo 2026, la durata massima di un certificato TLS è di 398 giorni.
A partire dal 15 marzo 2026, la durata massima di un certificato TLS sarà di 200 giorni.
A partire dal 15 marzo 2027, la durata massima di un certificato TLS sarà di 100 giorni.
A partire dal 15 marzo 2029, la durata massima di un certificato TLS sarà di 47 giorni.
Per ulteriori informazioni, vedere l&#39;articolo di [DigiCert sulla riduzione della durata dei certificati](https://www.digicert.com/blog/tls-certificate-lifetimes-will-officially-reduce-to-47-days)

### Quali nomi host scegliere? Quanti nomi host scegliere per dominio?

[!DNL Target] implementazioni CNAME richiedono un solo nome host per dominio sul certificato SSL e nel DNS del cliente. [!DNL Adobe] consiglia un nome host per dominio. Alcuni clienti richiedono più nomi host per dominio per i propri scopi (ad esempio, test nella gestione temporanea), che è supportato.

La maggior parte dei clienti sceglie un nome host come `target.example.com`. [!DNL Adobe] consiglia di seguire questa procedura, ma la scelta è in definitiva tua. Non richiedere un nome host di un record DNS esistente. Questa operazione causa un conflitto e ritarda il tempo di risoluzione della richiesta CNAME di [!DNL Target].

### Ho già un&#39;implementazione CNAME per [!DNL Adobe Analytics], posso usare lo stesso certificato o nome host?

No, [!DNL Target] richiede un nome host e un certificato separati.

### La mia implementazione corrente di [!DNL Target] è interessata da ITP 2.x?

La versione 2.3 di Apple Intelligent Tracking Prevention (ITP) ha introdotto la funzione di mitigazione del cloaking CNAME, che è in grado di rilevare [!DNL Adobe Target] implementazioni CNAME e riduce la scadenza del cookie a sette giorni. Attualmente [!DNL Target] non dispone di alcuna soluzione alternativa per la mitigazione del cloaking CNAME di ITP. Per ulteriori informazioni su ITP, vedere [Apple Intelligent Tracking Prevention (ITP) 2.x](/help/dev/before-implement/privacy/apple-itp-2x.md).

### Che tipo di interruzioni del servizio posso aspettarmi quando viene implementata la mia implementazione CNAME?

Non si verifica alcuna interruzione del servizio quando il certificato viene distribuito (inclusi i rinnovi del certificato).

Tuttavia, dopo aver modificato il nome host nel codice di implementazione [!DNL Target] (`serverDomain` in at.js) nel nuovo nome host CNAME (`target.example.com`), i browser Web considerano i visitatori di ritorno come nuovi visitatori. I dati di profilo dei visitatori di ritorno andranno persi perché il cookie precedente non è accessibile con il vecchio nome host (`clientcode.tt.omtrdc.net`). Il cookie precedente non è accessibile a causa di modelli di sicurezza del browser. Questa interruzione si verifica solo al cut-over iniziale al nuovo CNAME. I rinnovi del certificato non hanno lo stesso effetto perché il nome host non cambia.

### Quale tipo di chiave e algoritmo di firma del certificato sono utilizzati per l’implementazione del CNAME?

Per impostazione predefinita, tutti i certificati sono RSA SHA-256 e le chiavi sono RSA 2048 bit. Le dimensioni delle chiavi superiori a 2048 bit devono essere richieste esplicitamente tramite l’Assistenza clienti.

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

1. Incolla questo comando (sostituendo `target.example.com` con il tuo nome host):

   ```
   adobeTargetCnameValidation target.example.com
   ```

   Se l’implementazione è pronta, viene visualizzato un output simile al seguente. La parte importante è che tutte le righe dello stato di convalida mostrano `✅` anziché `🚫`. Ogni partizione CNAME edge [!DNL Target] deve mostrare `CN=target.example.com`, che corrisponde al nome host primario nel certificato richiesto (in questo output non vengono stampati nomi host SAN aggiuntivi nel certificato).

```
$ adobeTargetCnameValidation target.example.com

==========================================================

Adobe Target CNAME implementation validation for hostname target.example.com:
✅ target.example.com passes DNS CNAME validation
✅ target.example.com passes TLS and HTTP response validation for region IRL1
✅ target.example.com passes TLS and HTTP response validation for region IND1
✅ target.example.com passes TLS and HTTP response validation for region SIN
✅ target.example.com passes TLS and HTTP response validation for region OR
✅ target.example.com passes TLS and HTTP response validation for region SYD
✅ target.example.com passes TLS and HTTP response validation for region VA
✅ target.example.com passes TLS and HTTP response validation for region TYO
✅ target.example.com passes shard validation for the following 7 edge shards:

===== ✅ target.example.com [edge shard: IRL1-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: IND1-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: SIN-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: OR-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: SYD-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: VA-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: TYO-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

==========================================================  

For additional TLS/SSL validation, see SSL Shopper:

    🔎  https://www.sslshopper.com/ssl-checker.html#hostname=target.example.com  

To check DNS propagation around the world, see whatsmydns.net:

    🔎  DNS A records:     https://whatsmydns.net/#A/target.example.com
    🔎  DNS CNAME record:  https://whatsmydns.net/#CNAME/target.example.com 
```

>[!NOTE]
>
>Se il comando di convalida non riesce durante la convalida DNS ma sono già state apportate le modifiche DNS necessarie, potrebbe essere necessario attendere la propagazione completa degli aggiornamenti DNS. Ai record DNS è associato un valore [TTL (time-to-live)](https://en.wikipedia.org/wiki/Time_to_live#DNS_records) che determina la scadenza della cache per le risposte DNS di tali record. Di conseguenza, potrebbe essere necessario attendere almeno tanto quanto i TTL. Puoi utilizzare il comando `dig target.example.com` o [la Casella degli strumenti di G Suite](https://toolbox.googleapps.com/apps/dig/#CNAME) per cercare i TTL specifici. Per verificare la propagazione del DNS in tutto il mondo, vedere [whatsmydns.net](https://whatsmydns.net/#CNAME).

### Come si utilizza un collegamento di rinuncia con CNAME

Se utilizzi CNAME, il collegamento di rinuncia deve contenere il parametro &quot;client=`clientcode`, ad esempio:
`https://my.cname.domain/optout?client=clientcode`.

Sostituisci `clientcode` con il tuo codice client, quindi aggiungi il testo o l&#39;immagine da collegare all&#39;[URL di rinuncia](/help/dev/before-implement/privacy/privacy.md).

## Limitazioni note

* La modalità di controllo qualità non è definitiva se si dispone di CNAME e at.js 1.x, in quanto si basa su un cookie di terze parti. La soluzione consiste nell’aggiungere i parametri di anteprima a ogni URL a cui accedi. La modalità di controllo qualità è persistente quando si dispone di CNAME e at.js 2.x.
* Quando si utilizza CNAME, è più probabile che la dimensione dell&#39;intestazione del cookie per le chiamate [!DNL Target] aumenti. [!DNL Adobe] consiglia di mantenere le dimensioni del cookie inferiori a 8 KB.
