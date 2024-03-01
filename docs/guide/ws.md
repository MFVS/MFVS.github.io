# Webové služby STAG

Tato část se věnuje tomu, jak lze data ze STAGu načíst.

- [Odkaz na WS](https://ws.ujep.cz/ws/web)
- [Dokumentace](https://is-stag.zcu.cz/napoveda/web-services/ws_ws.html)

## Python

### Instalace balíčků

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

Více o http requestech [zde](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

### Requesty na webové služby

=== "Python"
    ```python
    import requests
    from io import StringIO
    import pandas as pd

    url = "https://ws.ujep.cz/ws/services/rest2/test/endpoint"
    params = {
        "lang": "cs",
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

#### Request `/getPredmetInfo`

<figure markdown="span">
  ![ws example](assets/ws/ws-example.png){ align=left }
  <figcaption>Náhled spouštění WS</figcaption>
</figure>

<figure markdown="span">
  ![ws output example](assets/ws/ws-output-example.png){ align=left }
  <figcaption>Náhled výstupu WS</figcaption>
</figure>

Request na tuto službu by vypadal následovně:

=== "Python"

    ```python
    import requests
    from io import StringIO
    import pandas as pd

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
