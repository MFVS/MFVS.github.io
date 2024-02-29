# Webové služby STAG

Tato část se věnuje tomu, jak data ze STAGu načíst strojově.

- [Odkaz na WS](https://ws.ujep.cz/ws/web)
- [Dokumentace](https://is-stag.zcu.cz/napoveda/web-services/ws_ws.html)

## Python

### Instalace balíčků

```bash
pip install requests pandas
```

#### Pandas

Pandas je knihovna pro práci s daty. Umožňuje načítat data z různých zdrojů, jako jsou CSV nebo JSON. Dále umožňuje manipulaci s daty, jako je filtrování, agregace, nebo spojování.

- [Dokumentace](https://pandas.pydata.org/docs/)

### Request na webové služby

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

    df <- read_csv(rawToChar(data$content))
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

    df <- read_csv(rawToChar(data$content))
    ```
