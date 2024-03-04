# Webové služby STAG

Tato část se věnuje tomu, jak lze data ze STAGu načíst.

- [Odkaz na WS](https://ws.ujep.cz/ws/web)
- [Dokumentace](https://is-stag.zcu.cz/napoveda/web-services/ws_ws.html)

## Instalace balíčků

Stažení nástrojů pro posílání http requestů a případně zpracování dat.

=== "pip"

    ```bash
    pip install requests pandas
    ```
    Pandas slouží jako nástroj pro práci s daty. Umožňuje načítat data z různých zdrojů, jako jsou například CSV nebo JSON. Dále umožňuje manipulaci s daty, jako je filtrování, agregace, nebo spojování.

    - [Dokumentace](https://pandas.pydata.org/docs/)

    Requests je knihovna pro posílání http requestů.

    - [Dokumentace](https://docs.python-requests.org/en/master/)

=== "Poetry"

    ```bash
    poetry add requests pandas
    ```
    Pandas slouží jako nástroj pro práci s daty. Umožňuje načítat data z různých zdrojů, jako jsou například CSV nebo JSON. Dále umožňuje manipulaci s daty, jako je filtrování, agregace, nebo spojování.

    - [Dokumentace](https://pandas.pydata.org/docs/)

    Requests je knihovna pro posílání http requestů.

    - [Dokumentace](https://docs.python-requests.org/en/master/)

=== "R"

    ```r
    install.packages(c("httr", "readr"))
    ```
    Jazyk R nepotřebuje žádný další balíček pro práci s daty.

    Knihovna `httr` slouží k posílání http requestů.

    - [Dokumentace](https://cran.r-project.org/web/packages/httr/index.html)

    Readr slouží k načítání dat z různých zdrojů, jako jsou například CSV nebo JSON.

    - [Dokumentace](https://readr.tidyverse.org/)

Více o http requestech [zde](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods).

## Request na webové služby

### Request bez přihlášení

Template pro request na webové služby.

=== "Python"

    ```python
    import requests
    import pandas as pd
    from io import StringIO

    url = "https://ws.ujep.cz/ws/services/rest2/test/endpoint"
    params = {
        "outputFormat": "CSV",
        "outputFormatEncoding": "utf-8",
    }

    data = requests.get(
        url,
        params=params,
    )

    df = pd.read_csv(StringIO(data.text), sep=";")
    ```
=== "R"

    ```r
    library(httr)
    library(readr)

    url <- "https://ws.ujep.cz/ws/services/rest2/test/endpoint"
    params <- list(
        outputFormat = "CSV",
        outputFormatEncoding = "utf-8"
    )

    data <- GET(
        url,
        query = params
    )

    df <- read_csv2(rawToChar(data$content))
    ```

K odeslání requestu je potřeba znát **url** služby a **url parametry** (také jako **query parametry**, dále jen **parametry**), které služba vyžaduje.

#### Request `/predmety/getPredmetInfo`

Na ukázku jeden konkrétní příklad. [`/predmety/getPredmetInfo`](https://ws.ujep.cz/ws/form/predmety/getPredmetInfo)

Výše uvedený odkaz otevře rozhraní služby `/predmety/getPredmetInfo`. K vyplnění je zde několik **parametrů**. Červenou hvězdičkou jsou označeny povinné parametry. Bez těchto **parametrů** nelze request odeslat (resp. server vrátí status kód `500 - Internal Server Error`).

Služba s vyplněnými **parametry** vypadá následovně:

![ws example](assets/ws/ws-example.png){ align=left }

Po odeslání requestu se zobrazí výsledek volání webové služby. Kromě konkrétního souboru ke stažení nás hlavně zajímá **Adresa**. Z té vyčteme **url** a názvy jednotlivých **parametrů**.

![ws output example](assets/ws/ws-output-example.png){ align=left }

??? note "Url a parametry"
    Parametry a jejch hodnoty se v adrese nacházejí za otazníkem.
    V našem případě vypadá takto:

    `?katedra=KMA&zkratka=MA2&rok=2023&lang=cs&outputFormat=CSV&outputFormatEncoding=utf-8`.

    Jednotlivé parametry a jejich hodnoty jsou ve formátu `parametr=hodnota`. Mezi jednotlivými parametry je oddělovač `&`. Naše parametry v tomto případě jsou:

    <center>
    | Parametr | Hodnota |
    | :-------- | :------- |
    | katedra  | KMA     |
    | zkratka  | MA2     |
    | rok      | 2023    |
    | lang     | cs      |
    | outputFormat | CSV |
    </center>

Po doplnění správné **url** a **parametrů** by request na tuto službu vypadal následovně:

=== "Python"

    ```python
    import requests
    import pandas as pd
    from io import StringIO

    url = "https://ws.ujep.cz/ws/services/rest2/predmety/getPredmetInfo"

    params = {
        "katedra": "KMA",
        "zkratka": "MA2",
        "rok": "2023",
        "lang": "cs",
        "outputFormat": "CSV",
        "outputFormatEncoding": "utf-8"
    }

    data = requests.get(
        url,
        params=params,
    )

    df = pd.read_csv(StringIO(data.text), sep=";")
    ```
=== "R"

    ```r
    library(httr)
    library(readr)

    url <- "https://ws.ujep.cz/ws/services/rest2/predmety/getPredmetInfo"

    params <- list(
        katedra = "KMA",
        zkratka = "MA2",
        rok = "2023",
        lang = "cs",
        outputFormat = "CSV",
        outputFormatEncoding = "utf-8"
    )

    data <- GET(
        url,
        query = params
    )

    df <- read_csv2(rawToChar(data$content))
    ```

???+ warning "Encoding"
    Soubory ve formátu **CSV** webové služby defaultně vrací v kódování **windows-1250**. Pro správné načtení dat je potřeba přidat parametr `outputFormatEncoding=utf-8`.
    [Dokumentace](https://is-stag.zcu.cz/napoveda/web-services/ws_formaty.html)

### Request s přihlášením

![ws ticket](assets/ws/ws-ticket.png){ align=left }

=== "Python"

    ```python
    import requests
    import pandas as pd
    from io import StringIO

    url = "https://ws.ujep.cz/ws/services/rest2/kvalifikacniprace/getKvalifikacniPraceAuth"

    ticket = "your_ticket"

    params = {
        "katedra": "KI",
        "outputFormat": "CSV",
        "outputFormatEncoding": "utf-8"
    }

    data = requests.get(url, params=params, cookies={"WSCOOKIE": ticket})

    df = pd.read_csv(StringIO(data.text), sep=";")
    ```

=== "R"

    ```r
    library(httr)
    library(readr)

    url <- "https://ws.ujep.cz/ws/services/rest2/kvalifikacniprace/getKvalifikacniPraceAuth"

    ticket <- "your_ticket"

    params <- list(
        "katedra" = "KI",
        "outputFormat" = "CSV",
        "outputFormatEncoding" = "utf-8"
    )

    data <- GET(
        url,
        query = params,
        add_headers("Cookie" = paste("WSCOOKIE", ticket, sep = "="))
    )

    df <- read_csv2(rawToChar(data$content))
    ```
