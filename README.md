# Calcolo Matrici e Distanze e Durate Comuni-Emilia-Romagna

---

## 💻 Descrizione Tecnica e Flusso del Codice

Lo script automatizza il calcolo della **Matrice delle Distanze Stradali** tra le località dell'Emilia-Romagna in due fasi distinte.

### FASE 1: Estrazione e Preparazione dei Dati

Questa fase è dedicata all'acquisizione delle coordinate geografiche di partenza (i centroidi dei comuni) necessarie per il calcolo del routing.

* **Sorgente Dati:** Il codice recupera l'elenco delle località dall'**API Open Data di Emilia Romagna Turismo (ERT)**, utilizzando l'endpoint `[/opendata/v1/localities](https://emiliaromagnaturismo.it/opendata/v1/localities)`.
* **Elaborazione:** Viene effettuata una richiesta paginata all'API. I dati vengono estratti e puliti in un **DataFrame Pandas** (`df_comuni`), registrando per ogni località il nome (`title`), la provincia, il codice ISTAT e le coordinate (`lat`, `lng`).
* **Pulizia Cruciale:** Vengono rimossi i **duplicati** basati sulle coordinate per garantire che ogni punto geografico sia unico. Infine, le coordinate sono convertite nel formato **[Longitudine, Latitudine]** (`ors_coordinates`), il formato standard richiesto dal servizio di routing.

### FASE 2: Calcolo della Matrice 

---

## 🔑 Prerequisiti: Chiave API e Limiti di Quota

Per eseguire con successo la FASE 2 del calcolo, è fondamentale comprendere e rispettare i limiti del servizio di routing gratuito.

### 1. Come Ottenere la Chiave API di OpenRouteService

Per utilizzare l'API di ORS, è necessario un token di accesso personale:

* **Registrazione:** Vai al sito web ufficiale di **[OpenRouteService](https://openrouteservice.org/)**.
* **Iscrizione:** Registrati per un account gratuito e ottieni la tua **chiave API (API Key)** univoca.

Una volta ottenuta, dovrai inserirla nello script Python in sostituzione del placeholder `'YOUR TOKEN ORS'`.

### 2. Limiti della Quota Gratuita (Cruciale)

Il calcolo della matrice è intensivo e consuma la maggior parte della quota giornaliera gratuita di ORS:

| Parametro | Dettagli |
| :--- | :--- |
| **Limite Giornaliero** | La quota massima giornaliera è di **500 richieste** (chiamate API). |
| **Consumo dello Script** | Poiché lo script invia circa 1 richiesta per ogni comune di origine (circa 404 comuni), l'esecuzione **consuma quasi tutta la quota giornaliera**. |

**Importante:** Dopo un'esecuzione completa, sarà necessario **attendere 24 ore** per il reset della quota API prima di poter eseguire nuovamente l'intero script con lo stesso token.
