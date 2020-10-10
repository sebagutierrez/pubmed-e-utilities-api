# PubMed API

## Entrez Programming Utilities

Las E-Utilities componen una API pública del NCBI que nos permite hacer diversas operaciones como: obtener PubMed ids bajo términos de búsqueda, fetchear
la metadata más importante de un PubMed id específico, obtener data relevante sobre algún artículo específico, etc.

### Importante

En cada llamada a la API, el NCBI incentiva el uso de los parámetros ``tool`` e ``email``, donde en el primero se debe especificar el nombre (sin whitespaces) de la aplicación que está 
realizando la llamada, mientras que en el segundo se debe especificar el email del usuario realizando la llamada.

**Ejemplo**

```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id=32945846&retmode=json&tool=i2-covid-site&email=sebastian@hicapps.cl
```

## ESearch

La utilidad ESearch nos permite obtener los PubMed ids bajo términos de búsqueda.

### Endpoint
``` 
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi
```

### Parámeteros requeridos

``db``

La base de datos donde se buscará el término. Por ejemplo: ``pubmed``, ``protein``, ``nuccore`` (por defecto viene pubmed). 
[Ver todas las bases de datos.](https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi)

``term``

El término por el cuál se buscará. Por ejemplo ``covid``, ``washington+university``, ``school+of+medicine``. 
Importante: utilizar ``+`` en lugar de whitespaces.

### Parámeteros opcionales

``retmax``

El número de PubMed ids que deseas conseguir.

``retmode``

El formato en que vendrá la response de la API. Por ejemplo: ``json`` o ``xml``.

### Ejemplo de request

Si deseo buscar las PubMed ids del término ``school of medicine``, obtenerla en formato JSON y conseguir sólo 10 resultados, la llamada se vería de esta manera:

```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&retmode=json&retmax=10&term=school+of+medicine
```

### ¿Qué data retornaría esta llamada?

```
{
  "header": {
    "type": "esearch",
    "version": "0.3"
  },
  "esearchresult": {
    "count": "2392535",
    "retmax": "10",
    "retstart": "0",

    // las ids que estábamos buscando
    "idlist": [ 
      "33036065",
      "33036063",
      "33036062",
      "33036061",
      "33036049",
      "33036036",
      "33036029",
      "33036025",
      "33036005",
      "33035998"
    ],
    ...
    ...
  }
}
```

## ESummary

La utilidad ESummary nos permite obtener la metadata de los artículos a través de los PubMed ids obtenidos previamente con la utilidad ESearch.

### Endpoint
```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi
```

### Parámeteros requeridos

``id``

Lista de PubMed ids por los que se desea obtener la metadata. Si se desea buscar más de un id, se pueden separar por ``,``.

### Ejemplo de request

Si deseo obtener la metadata de las 10 PubMed ids que obtuve anteriormente bajo el término ``school of medicine``, la llamada se vería de la siguiente manera:

```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&retmode=json&id=33036065,33036063,33036062,33036061,33036049,33036036,33036029,33036025,33036005,33035998
```

### ¿Qué data retornaría esta llamada?

```
{
  "header": {
    "type": "esummary",
    "version": "0.3"
  },
  "result": {
    "33035998": {
      "uid": "33035998",
      "pubdate": "2020 Oct 9",
      "epubdate": "2020 Oct 9",
      "source": "J Neurosurg",
      "authors": [...],
      "lastauthor": "Ramirez-Zamora A",
      "title": "Long-term clinical outcomes of bilateral GPi deep brain stimulation in advanced Parkinson's disease: 5 years and beyond.",
      "sorttitle": "long term clinical outcomes of bilateral gpi deep brain stimulation in advanced parkinson s disease 5 years and beyond",
      "volume": "",
      "issue": "",
      "pages": "1-10",
      "lang": [...],
      "nlmuniqueid": "0253357",
      "issn": "0022-3085",
      "essn": "1933-0693",
      "pubtype": [...],
      "recordstatus": "PubMed - as supplied by publisher",
      "pubstatus": "10",
      "articleids": [...],
      "history": [...],
      "references": [],
      "attributes": [...],
      "pmcrefcount": "",
      "fulljournalname": "Journal of neurosurgery",
      "elocationid": "doi: 10.3171/2020.6.JNS20617",
      "doctype": "citation",
      "srccontriblist": [],
      "booktitle": "",
      "medium": "",
      "edition": "",
      "publisherlocation": "",
      "publishername": "",
      "srcdate": "",
      "reportnumber": "",
      "availablefromurl": "",
      "locationlabel": "",
      "doccontriblist": [],
      "docdate": "",
      "bookname": "",
      "chapter": "",
      "sortpubdate": "2020/10/09 00:00",
      "sortfirstauthor": "Tsuboi T",
      "vernaculartitle": ""
    },
    "33036005": { ... },
    "33036025": { ... },
    "33036029": { ... },
    "33036036": { ... },
    "33036049": { ... },
    "33036061": { ... },
    "33036062": { ... },
    "33036063": { ... },
    "33036065": { ... },
    "uids": [...]
  }
}
```

## EFetch

La utilidad EFetch nos permite obtener información más relevante de uno o más PubMed ids en específico (obtenidos previamente con ESearch), como por ejemplo: Abstract, Publisher, etc.

### Endpoint
``` 
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi
```

### Parámeteros requeridos

``id``

Al igual que en la utilidad ESummary, podemos entregar una lista de PubMed ids para obtener sus datos relevantes. Recordar separar con una ``,`` cada PubMed id, en caso de que se busque más de uno.

### Parámeteros opcionales

``retmode``

El formato en que vendrá la response de la API. Por ejemplo: ``json`` o ``xml``.

``rettype``

Es el tipo de respuesta. Pueden ser ``medline``, ``abstract`` o ``uilist``.

### Ejemplo de request

Si deseo buscar la data relevante del primer PubMed id que nos retornó la busqueda de ``school of medicine``, la llamada se vería de esta manera:

```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=pubmed&rettype=medline&id=33036065
```

**Nota**:
Recomiendo el uso de ``rettype=medline`` para este caso. Entrega la data de una manera más ordenada y fácil para encontrar el campo que se necesite.

### ¿Qué data retornaría esta llamada?

```
PMID- 33036065
OWN - NLM
STAT- Publisher
LR  - 20201009
IS  - 1097-0142 (Electronic)
IS  - 0008-543X (Linking)
DP  - 2020 Oct 9
TI  - Outcomes of Black men with prostate cancer treated with radiation therapy in the 
      Veterans Health Administration.
LID - 10.1002/cncr.33224 [doi]
AB  - BACKGROUND: Population-based studies demonstrate that Black men in the United
      States have an increased risk of death from prostate cancer. Determinants of
      racial disparities are multifactorial, including socioeconomic and biologic
      factors. METHODS: The authors conducted a pooled analysis of patients derived
      from 152 centers within the Veterans Health Administration. The cohort included
      men who had nonmetastatic prostate diagnosed between 2001 and 2015 and received
      definitive radiation therapy. The primary endpoint was prostate cancer-specific
      mortality (PCSM). Secondary endpoints included all-cause mortality (ACM) and the 
      time from a prostate-specific antigen level >/=4 ng/mL to biopsy and radiation
      therapy. A Cox regression model was performed to adjust for differences between
      clinical parameters. RESULTS: Among the 31,131 patients included in the cohort,
      9584 (30.8%) were Black. The 10-year cumulative incidence of death from prostate 
      cancer was lower in Black men compared with White men (4.0% vs 4.8%; P = .004).
      In a competing risk model, Black race was associated with a decreased risk of
      PCSM (subdistribution hazard ratio, 0.79; 95% CI, 0.69-0.92; P = .002).
      Similarly, the 10-year cumulative incidence of death from any cause was lower in 
      Black men (27.6% vs 31.8%; P < .001). In multivariable analysis, Black men had a 
      10% decreased risk of ACM (hazard ratio, 0.90; 95% CI, 0.85-0.95; P < .001).
      CONCLUSIONS: The current results indicate relatively lower PCSM and ACM among
      Black men who were included in a large Veterans Health Administration cohort and 
      received radiation therapy as primary treatment for nonmetastatic prostate
      cancer. There is an ongoing need to continue to understand and mitigate the
      factors associated with disparities in health care outcomes.
CI  - (c) 2020 American Cancer Society.
...
...
...
...
```
## Referencias

[PubMed APIs](https://www.ncbi.nlm.nih.gov/home/develop/api/)  
[Introduction to the E-Utilities](https://www.ncbi.nlm.nih.gov/books/NBK25501/)  
[The E-utilities In-Depth: Parameters, Syntax and More](https://www.ncbi.nlm.nih.gov/books/NBK25499/)  