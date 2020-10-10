# PubMed API

### Entrez Programming Utilities

Las E-Utilities componen una API pública del NCBI que nos permite hacer diversas operaciones como: obtener PubMed ids bajo términos de búsqueda, fetchear
la metadata más importante de un PubMed id específico, etc.

#### Importante

En cada llamada a la API, el NCBI incentiva el uso de los parámetros ``tool`` e ``email``, donde en el primero se debe especificar el nombre (sin whitespaces) de la aplicación que está 
realizando la llamada, mientras que en el segundo se debe especificar el email del usuario realizando la llamada.

**Ejemplo**

``https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id=32945846&retmode=json&tool=i2-covid-site&email=sebastian@hicapps.cl``

___

### ESearch

La utilidad ESearch nos permite obtener los PubMed ids bajo términos de búsqueda.

**Endpoint**
``` 
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi
```

**Parámeteros requeridos**

``db``

La base de datos donde se buscará el término. Por ejemplo: ``pubmed``, ``protein``, ``nuccore`` (por defecto viene pubmed). 
[Ver todas las bases de datos.](https://eutils.ncbi.nlm.nih.gov/entrez/eutils/einfo.fcgi)

``term``

El término por el cuál se buscará. Por ejemplo ``covid``, ``washington+university``, ``school+of+medicine``. 
Importante: utilizar ``+`` en lugar de whitespaces.

**Parámeteros opcionales**

``retmax``

El número de PubMed ids que deseas conseguir.

``retmode``

El formato en que vendrá la response de la API. Por ejemplo: ``json`` o ``xml``.

**Ejemplo de request**

Si deseo buscar las PubMed ids del término ``school of medicine``, obtenerla en formato JSON y conseguir sólo 10 resultados, la llamada se vería de esta manera:

``https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&retmode=json&retmax=10&term=school+of+medicine``

**¿Qué data traería esta llamada?**

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

___

### ESummary

La utilidad ESummary nos permite obtener la metadata de los artículos a través de los PubMed ids obtenidos previamente con la utilidad ESearch.

**Endpoint**
```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi
```

**Parámeteros requeridos**

``id``

Lista de PubMed ids por los que se desea obtener la metadata. Si se desea buscar más de un id, se pueden separar por ``,``.

**Ejemplo de request**

Si deseo obtener la metadata de las 10 PubMed ids que obtuve anteriormente bajo el término ``school of medicine``, la llamada se vería de la siguiente manera:

``https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&retmode=json&id=33036065,33036063,33036062,33036061,33036049,33036036,33036029,33036025,33036005,33035998``

**¿Qué data traería esta llamada?**

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

___

### Referencias

[PubMed APIs](https://www.ncbi.nlm.nih.gov/home/develop/api/)
[Introduction to the E-Utilities](https://www.ncbi.nlm.nih.gov/books/NBK25501/)
[The E-utilities In-Depth: Parameters, Syntax and More](https://www.ncbi.nlm.nih.gov/books/NBK25499/)