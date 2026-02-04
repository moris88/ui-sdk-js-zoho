# Presentazione con Slidev

Questo repository contiene il codice sorgente per una presentazione web creata con [Slidev](https://sli.dev/).

## A cosa serve

Questo progetto è un esempio di come creare presentazioni interattive e basate sul web utilizzando tecnologie come Vue.js e Markdown. Le slide sono scritte in un file `slides.md` e possono includere componenti Vue interattivi.

## Come si usa

Per utilizzare questo repository, è necessario avere installato [Node.js](https://nodejs.org/) e [pnpm](https://pnpm.io/).

1. **Clonare il repository:**

    ```bash
    git clone <URL_DEL_REPOSITORY>
    cd <NOME_DELLA_CARTELLA>
    ```

2. **Installare le dipendenze:**

    ```bash
    pnpm install
    ```

3. **Avviare la presentazione in modalità sviluppo:**
    Questo comando avvierà un server locale e aprirà la presentazione nel browser. Le modifiche al file `slides.md` verranno aggiornate automaticamente.

    ```bash
    pnpm dev
    ```

4. **Creare una build di produzione:**
    Questo comando genererà i file statici della presentazione nella cartella `dist/`.

    ```bash
    pnpm build
    ```

5. **Esportare le slide in PDF:**
    Questo comando creerà una versione PDF della presentazione.

    ```bash
    pnpm export
    ```

## Visualizza le Slide

È disponibile una versione PDF delle slide esportate in questo repository.

- [Visualizza le slide (PDF)](./slides-export.pdf)
