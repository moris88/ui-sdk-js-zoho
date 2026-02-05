---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: /assets/images/background.png
# some information about your slides (markdown enabled)
title: Novità SDK JS per Zoho CRM
class: text-center
author: Maurizio Tolomeo
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# duration of the presentation
duration: 30min
---

# Novità SDK JS per Zoho CRM

Presentazione per team dev

Guida alle Funzionalità Client-Side (ZDK.Client)

La UI Diventa più facile e veloce

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Cosa Scopriremo Oggi

- 1 - Interazione con l'Utente

Scopri come raccogliere input, mostrare avvisi e conferme
con getInput, showAlert, showConﬁrmation e showMessage.

- 2 - Gestione dello Stato

Impara a gestire operazioni lunghe e l'esperienza di
caricamento con showLoader e hideLoader.

- 3 - Integrazione Avanzata

Esplora le potenzialità di comunicazione bidirezionale con
openPopup e sendResponse per widget personalizzati.

- 4 - Le Zoho Request Client

Analizzeremo le ZRC per effettuare chiamate REST dentro i
widget (una sorta di invoke 2.0).

<!--
- Oggi vedremo quattro macro aree: come raccogliere input dall’utente, gestire lo stato con loader, integrare flussi avanzati tra widget diversi e, infine, le Zoho Request Client per fare chiamate API in modo più veloce (detta da zoho).  
- Questi strumenti ci permettono di scrivere meno codice e rendono le nostre applicazioni più reattive e sicure
-->

---

# Concetti Fondamentali

Namespace e Sintassi

Tutte le funzioni sono accessibili tramite l'oggetto
ZDK.Client:

```javascript
ZDK.Client.showAlert(...)showConfirmation.
ZDK.Client.getInput(...)Soluzione: Chiama sempre hideLoader() prima di
ZDK.Client.openPopup(...)mostrare una ﬁnestra di dialogo.
```

Supporto Markdown
Formattazione ricca per comunicare in modo efﬁcace:

```markdown
**testo** → testo (Grassetto)
_testo_ → testo (Corsivo)
<a href="..."> → link cliccabili
```

⚠ ATTENZIONE

Un loader attivo (showLoader) blocca l'apertura di
altri popup come showAlert, getInput, e showConfirmation.

<!--
- Tutte le funzioni che vedremo oggi sono accessibili tramite ZDK.Client. Questo ci permette di avere un punto centrale per gestire alert, input, loader e popup.
- Importante le funzioni supportano la formattazione Markdown.
- ⚠ Unico punto di attenzione: se showLoader è attivo, altri popup come showAlert o getInput non possono aprirsi. Però lo vedremo più avanti nelle slide.
-->

---

# getInput(): Raccolta Dati Flessibile

La funzione getInput() permette di raccogliere dati dall'utente attraverso ﬁnestre di input personalizzate, supportando
molteplici tipi di campo per adattarsi a ogni esigenza.

## Tipi di Campo

Text, number, textarea, picklist e multiselect per qualsiasi scenario.

## Sintassi Intuitiva

getInput(options, heading, accept_message, reject_message)

## Configurazione Avanzata

Valori predeﬁniti, liste personalizzate e validazione integrata.

<!--
- La prima funzione che andiamo a vedere è la getInput() che ci permette di creare form dinamici direttamente nel widget, senza doverli costruire manualmente nel DOM.
- Abbiamo i testi, numeri, textarea, picklist e multiselect… praticamente tutti gli scenari comuni.
- Chiamare getInput(options, heading, acceptMessage, rejectMessage) è sufficiente per raccogliere dati strutturati.
-->

---
layout: two-cols
---

<template v-slot:default>
<div class="px-6">
```javascript
/**
 * Note: The presence of an active loader restricts the availability of other pop-ups.
 * Open popup widget and await a response from the widget's $Client.close(). 
 * Moreover, custom data can be passed to widget's
 * 'PageLoad' event. Supported Widget SDK version >= 1.2.
 * @param config popup configuration
 *  - api_name: string : API name of the popup widget to be opened
 *  - type: string : Type of popup. (e.g., 'widget')
 *  - header?: string : Header text for the popup
 *  - close_icon?: boolean : Whether to show close icon
 *  - close_on_escape?: boolean : Whether to allow closing on escape key press
 *  - animation_type?: 1 | 2 | 3 | 4 | 5 | 6 : Animation type for popup appearance
 *      `1` - slides popup from top
 *      `2` - slides popup from right
 *      `3` - slides popup from left
 *      `4` - slides popup from bottom
 *      `5` - fades in and fades out the popup
 *      `6` - zoom in and zoom out the popup
 *  - bottom?: string : Bottom position of the popup
 *  - right?: string : Right position of the popup
 *  - height?: string : Height of the popup (e.g., '400px', '50%')
 *  - width?: string : Width of the popup (e.g., '600px', '80%')
 *  - top?: string : Top position of the popup
 *  - left?: string : Left position of the popup
 * @param data custom data to be passed to popup widget's 'PageLoad' event
 * @returns object | string | number | boolean : Response from widget's $Client.close()
 */
```
</div>
</template>
<template v-slot:right>
<div class="mt-8 px-6">
```javascript
ZDK.Client.getInput(
openPopup: (
  config: {
    api_name: string
    type: 'widget'
    header?: string
    close_icon?: boolean
    close_on_escape?: boolean
    animation_type?: 1 | 2 | 3 | 4 | 5 | 6
    height?: string
    width?: string
    top?: string
    left?: string
    bottom?: string
    right?: string
  },
  data: Record<string, any>
) => Promise<object | string | number | boolean>
```
</div>
</template>

<!--
- “Possiamo configurare animazioni, dimensioni, posizione e icona di chiusura dei popup.”  
- 💡 Mostra codice, non leggere i parametri uno per uno, solo far vedere  
- “Questo permette di aprire popup widget complessi, personalizzando completamente l’esperienza utente.”
-->

---

# Esempio Pratico di getInput()

<pre class="custom-language-javascript">
<code>
  ZDK.Client.getInput(
  [
    {
      type: 'text',
      label: 'Enter your name',
      default_value: 'placeholder',
    },
    {
      type: 'number',
      label: 'Enter your age',
      default_value: '1',
    },
    {
      type: 'textarea',
      label: 'Enter your text',
      default_value: 'lorem ipsum dolor sit amet ...',
    },
    {
      type: 'picklist',
      label: 'Enter your option',
      list_options: [
        { actual_value: 'option_1', display_value: 'Option 1' },
        { actual_value: 'option_2', display_value: 'Option 2' },
        { actual_value: 'option_3', display_value: 'Option 3' },
      ],
    },
    {
      type: 'multiselectpicklist',
      label: 'Enter your options',
      default_value: ['option_2', 'option_3'],
      list_options: [
        { actual_value: 'option_1', display_value: 'Option 1' },
        { actual_value: 'option_2', display_value: 'Option 2' },
        { actual_value: 'option_3', display_value: 'Option 3' },
      ],
    },
  ],
  'Please provide the following information:',
  'Submit',
  'Cancel'
  )
</code>
</pre>

<!--
Un esempio pratico di codice da utilizzare
-->

---

# Esempio UI di getInput()

<div class="w-full flex justify-center">
<img src="./assets/images/getinput.png" style="width: 400px; margin-top: 20px;" />
</div>

<!--
Output di come si vedrebbe su Zoho CRM
-->

---

# Comunicazione con l'Utente

Le funzioni che seguono sono progettate per migliorare l'interazione con l'utente, offrendo modi semplici e intuitivi per mostrare messaggi, richiedere conferme e gestire notifiche.

Le funzioni includono:

- showMessage()
- showConfirmation()
- showAlert()

<!--
Le prossime funzioni sono:
- showMessage
- showConfermation
- showAlert

queste funzioni aiutano a interagire facilmente con l'utente
-->

---
layout: two-cols
---

<template v-slot:default>
<div class="px-6">

- showMessage():

Scopo: Mostra messaggi informativi con un solo pulsante di conferma.

Casi d'uso: Notiﬁche all'utente, avvisi informativi, di successo, di warning o di errore.

Sintassi:

```javascript
/**
 * Show a toast message in Page with following markdown support.
 * Note: The presence of an active loader restricts the availability of other pop-ups.
 * Image Markdown not supported for showMessage.
 * Italics: _text_ Bold: *text* Underline: __text__
 * Strikeout: ~text~ Code: `text` Heading1: # text
 * Heading3: ### text Blockquote: !text
 * Hyperlink   - [click here](https://www.zoho.com)
 * @param message text to be displayed
 * @param options message options
 * @returns void
 */
```

</div>
</template>
<template v-slot:right>
<div class="mt-8 px-6">
```javascript
showMessage: (
  message: string,
  options?: {
    type?: 'success' | 'info' | 'warning' | 'error'
  }
) => void
```
</div>

</template>

<!--
- “showMessage serve per notifiche semplici, tipo toast, senza interrompere l’utente.”
-->

---

# Esempio UI di showMessage()

```javascript
ZDK.Client.showMessage("test description", { type: "info" });
ZDK.Client.showMessage("test description", { type: "success" });
ZDK.Client.showMessage("test description", { type: "warning" });
ZDK.Client.showMessage("test description", { type: "error" });
```

<div class="w-full flex flex-col items-center justify-center gap-2 mt-8">
<img src="./assets/images/info.png" class="max-w-[180px]" />
<img src="./assets/images/success.png" class="max-w-[180px]" />
<img src="./assets/images/warning.png" class="max-w-[180px]" />
<img src="./assets/images/error.png" class="max-w-[180px]" />
</div>

<!--
il parametro type identifica il tipo di alert
-->

---

- showConfirmation():

Scopo: Richiede conferma dall'utente con opzioni di accettazione o rifiuto.

Casi d'uso: Conferme per azioni critiche, decisioni importanti.

Sintassi:

```javascript
/**
 * Note: The presence of an active loader restricts the availability of other pop-ups.
 * Show confirmation box with markdown support and accept/reject message.
 * @param message text to be displayed
 * @param accept_message accept button message
 * @param reject_message reject button message
 * @returns Boolean : Confirmation response
 */
showConfirmation: (
  message: string,
  accept_message?: string,
  reject_message?: string
) => Promise<boolean>
```

<!--
- “showConfirmation serve per confermare azioni critiche. Dove serve una conferma da parte dell'utente. Restituisce un boolean, utile per flussi condizionali.”
-->

---

# Esempio UI di showConfirmation()

```javascript
ZDK.Client.showConfirmation("test description", "accept", "reject");
```

<div class="w-full flex justify-center mt-8">
<img src="./assets/images/confirm.png" class="max-w-[480px]" />
</div>

<!--
come si vedrebbe sul CRM
-->

---

- showAlert():

Scopo: Mostra messaggi all'utente che devono essere confermati con un click.

Casi d'uso: Notiﬁche che richiedono attenzione senza opzioni multiple.

Sintassi:

```javascript
/**
 * Note: The presence of an active loader restricts the availability of other pop-ups.
 * Show Alert message with markdown support.
 * @param message primary text to be displayed
 * @param heading heading to be displayed
 * @param accept_message accept button message
 * @returns boolean
 */
showAlert: (
  message: string,
  heading?: string,
  accept_message?: string
) => Promise<boolean>
```

<!--
- “showAlert serve per semplici messaggi che richiedono attenzione immediata, un solo pulsante per chiudere.”
-->

---

# Esempio UI di showAlert()

```javascript
ZDK.Client.showAlert("test desciption", "info header", "button close");
```

<div class="w-full flex justify-center mt-8">
<img src="./assets/images/alert.png" class="max-w-[480px]" />
</div>

<!--
come si vedrebbe sul CRM, unica differenza da quello precedente la presenza di un solo pulsante.
-->

---

# Gestione dello Stato con il Loader

### Le funzioni showLoader() e hideLoader()

Gestisci l'esperienza utente durante operazioni che richiedono del tempo, come chiamate API esterne o
elaborazioni complesse.

Best Practice: Chiama sempre hideLoader() sia in caso di successo che di errore per evitare che l'interfaccia
rimanga bloccata.

Possibili Tipi di Loader:

- spinner: Copre l'intera pagina, e compare un pop-up con un cerchio che gira.
- vertical-bar: Copre l'intera pagina, e compare un pop-up con le barre verticali che si muovono.
- standard: Il classico loader delle Custom Function da pulsante.

<!--
- Quando facciamo operazioni lunghe e complesse o fetch di dati, il loader è fondamentale. 

Ricordate sempre di chiamare hideLoader() alla fine o in caso di errore, altrimenti la UI rimane bloccata.

- Possiamo scegliere tra spinner, vertical-bar e standard a seconda del contesto.
-->

---

# Esempio Pratico di showLoader() e hideLoader()

```javascript
// Mostra il loader
ZDK.Client.showLoader({
  type: "spinner",
  message: "Recupero dei dati in corso...",
});

fetch("https://api.example.com/data")
  .then((response) => response.json())
  .then((data) => {
    // Chiamata API completata, nascondi il loader
    ZDK.Client.hideLoader();
    ZDK.Client.showMessage("Completata!", { type: "success" });
  })
  .catch((error) => {
    // In caso di errore, nascondi comunque il loader
    ZDK.Client.hideLoader();
    ZDK.Client.showMessage("Errore durante il recupero dei dati.", {
      type: "error",
    });
  });
```

<!--
Codice di esempio
-->

---

# Integrazione Avanzata con openPopup

Si possono creare flussi di comunicazione bidirezionale tra il widget e un secondo widget per offrire esperienze utente più ricche e interattive.

## Flusso di Comunicazione

1. Il widget A apre un popup (widget B) usando openPopup, passando dati personalizzati.
2. Il widget B riceve i dati, gli elabora e può inviare una risposta al widget A usando $Client.close().
3. Il widget A riceve la risposta e può aggiornare l'interfaccia o eseguire azioni basate su di essa.

<!--
Una funzione carina, che potrebbe aprire a scenari interessanti è la openPopup.

- Qui possiamo aprire un popup con un altro widget, passare dati, ricevere risposta e aggiornare la UI dinamicamente.
-->

---

Sintassi di openPopup e sendResponse

```javascript
/**
 * Note: The presence of an active loader restricts the availability of other pop-ups.
 * Open popup widget and await a response from the widget's $Client.close(). Moreover, custom data can be passed to widget's
 * 'PageLoad' event. Supported Widget SDK version >= 1.2.
 * @param config popup configuration
 *  - api_name: string : API name of the popup widget to be opened
 *  - type: string : Type of popup. (e.g., 'widget')
 *  - header?: string : Header text for the popup
 *  - close_icon?: boolean : Whether to show close icon
 *  - close_on_escape?: boolean : Whether to allow closing on escape key press
 *  - animation_type?: 1 | 2 | 3 | 4 | 5 | 6 : Animation type for popup appearance
 *      `1` - slides popup from top
 *      `2` - slides popup from right
 *      `3` - slides popup from left
 *      `4` - slides popup from bottom
 *      `5` - fades in and fades out the popup
 *      `6` - zoom in and zoom out the popup
 *  - bottom?: string : Bottom position of the popup
 *  - right?: string : Right position of the popup
 *  - height?: string : Height of the popup (e.g., '400px', '50%')
 *  - width?: string : Width of the popup (e.g., '600px', '80%')
 *  - top?: string : Top position of the popup
 *  - left?: string : Left position of the popup
 * @param data custom data to be passed to popup widget's 'PageLoad' event
 * @returns object | string | number | boolean : Response from widget's $Client.close()
 */
```

<!--
Codice esempio
-->

---

# Sintassi di openPopup

```javascript
openPopup: (
  config: {
    api_name: string
    type: 'widget'
    header?: string
    close_icon?: boolean
    close_on_escape?: boolean
    animation_type?: 1 | 2 | 3 | 4 | 5 | 6
    height?: string
    width?: string
    top?: string
    left?: string
    bottom?: string
    right?: string
  },
  data: Record<string, any>
) => Promise<object | string | number | boolean>
```

<!--
esempio codice
-->

---

# Esempio Pratico (Widget A)

```javascript
ZDK.Client.openPopup(
  {
    api_name: "Test_Widget_2",
    type: "widget",
    header: "Sono un secondo widget",
    close_icon: true, // mostra l'icona di chiusura
    close_on_escape: false, // non permette di chiudere con ESC
    animation_type: 4,
  },
  {
    param_1: "test_value",
    param_2: 12345,
  },
);
```

<!--
esempio di utilizzo per chiamare il secondo widget
-->

---

# Esempio Pratico (Widget B)

```javascript
// Nel widget B
ZOHO.embeddedApp.on(event, (data) => {
  // ...
  // data contiene i parametri passati da widget A (param_1, param_2 ...)
});
ZOHO.embeddedApp.init();
// ...
// Quando si vuole inviare una risposta a widget A (questo fa chiudere il popup)
$Client.close({
  response: {
    test_response_1: "Response inviata a crmpartnerslib!",
    test_response_2: "test di parametro!",
  },
});
```

<!--
esempio di codice per rispondere al widget chiamante
-->

---

# Esempio UI di openPopup()

<div class="w-full flex justify-center mt-8">
<video src="./assets/video/openpopup.webm" controls>
</video>
</div>

<!--
piccolo widget di esempio tramite piccolo video dimostrativo
-->

---

# Le Zoho Request Client (ZRC)

Un SDK integrato in Zoho CRM che fornisce un modo uniﬁcato per effettuare chiamate REST su tutte le funzionalità developer-centric di Zoho CRM.

## Funzionalità Chiave:

- Richieste API CRM
- Richieste basate su Connessioni
- Richieste API Esterne

<!--
- “Le ZRC ovvero le Zoho Request Client servono per fare chiamate REST uniformi, veloci, senza preoccuparci della autenticazione interna.” 
- A quanto ho capito sono una evoluzione delle invoke di Zoho
-->

---

## Perché usare ZRC?

- Supporta qualsiasi versione delle API CRM
- Caricamento ﬁle tramite Connessione o API Esterne
- Sintassi consistente per le chiamate API
- Nessuna autenticazione necessaria per le chiamate API all'interno della
  stessa organizzazione
- Supporto Async/await per codice più pulito
- Istanze ZRC riutilizzabili
- Gestione automatica dei dati JSON
- Codiﬁca automatica dei parametri di query

<!--
Possiamo effettuare chiamate API interne o esterne, gestire le connessioni, istanze e hanno un codice più pulito.
-->

---

# Esempio Pratico di ZRC

## Esempio 1: Richiesta API CRM

```javascript
const users = await zrc.get('/crm/v8/users');

// Output:
{
  "status": 200,
  "data":
  {
    "users": [
      {
        "id": 3891457000000556001,
        "full_name": "Catherin",
        "email": "test@zoho.com",
        // ... altri campi utente
      }
      // ... altri utenti
    ]
  }
}
```

<!--
- In questo esempio con zrc.get('/crm/v8/users') otteniamo gli utenti.
-->

---

# Esempio Pratico di ZRC

## Esempio 2: Creazione di un record

```javascript
const deal = await zrc.post('/crm/v8/Deals', {
  data: [
    {
      Deal_Name: 'Nuova Opportunità',
      Stage: 'Qualification',
      Amount: 5000,
    }
  ]
});
// Output:
{
  "status": 201,
  "data":
  {
    "code": "SUCCESS",
    "message": "record added",
    "status": "success",
  }
}
```

<!--
- In questo esempio, con zrc.post() possiamo creare record rapidamente, con risposta standardizzata.
-->

---

# Esempio Pratico di ZRC

## Esempio 3: Istanze Riutilizzabili e Connessioni Esterne

```javascript
const sheetZrc = zrc.createInstance({
  baseUrl: "https://sheets.googleapis.com/v4",
  connection: "google_sheets",
});

const sheet_info = await sheetZrc.get("/spreadsheets/98711121211100");
const sheet_value1 = await sheetZrc.get(
  "/spreadsheets/98711121211102/values/A1",
);
const sheet_value2 = await sheetZrc.get(
  "/spreadsheets/98711121211102/values/A2",
);
```

<!--
Abbiamo anche la possibilità di usarle esternamente con le connessioni, creando istanze.
-->

---

# Conclusioni

- Le nuove funzionalità di ZDK.Client offrono strumenti potenti e veloci per l'interazione con l'utente e gestire lo stato dell'applicazione.
- La comunicazione bidirezionale tra widget consente esperienze utente più avanzate e personalizzate.
- Le Zoho Request Client (ZRC) semplificano le chiamate API.
- Questi strumenti permettono a chiunque, anche senza esperienza di frontend, di costruire piccoli widget dinamici e interattivi con il minimo sforzo.

## GRAZIE PER L'ATTENZIONE

## Domande?

<!--
- Concludendo SDK JS ora ci offre: raccolta input flessibile, messaggi/alert gestibili, loader, popup bidirezionali e le ZRC per le API.
- Tutto questo ci permette di scrivere meno codice, avere widget più reattivi e flussi utente avanzati.
- Con queste implementazioni, chiunque, non ha esperienza di frontend, può costruire piccoli widget con il minimo indispensabile
- Grazie per l’attenzione! Ho finito.
-->
