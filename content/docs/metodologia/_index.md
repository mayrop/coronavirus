---
author: "Mayra Valdes"
linktitle: Metodología
title: Metodología
weight: 2
bookFlatSection: true
---

# Metodología

## Introducción
Diariamente, a las 19:00 horas, se emite una [Conferencia de Prensa](https://coronavirus.gob.mx/noticias/), donde se presenta el informe diario sobre la situación del Coronavirus COVID-19 en México. Dicha conferencia es encabezada comúnmente por [Hugo López-Gatell](https://twitter.com/HLGatell), Subsecretario de Prevención y Promoción de la Salud. 

Mientras se lleva a cabo la conferencia, la [Secretaría de Salud de México](https://www.gob.mx/salud), a través de la Dirección General de Epidemiología,  publica documentos actualizados [aquí](https://www.gob.mx/salud/documentos/coronavirus-covid-19-comunicado-tecnico-diario-238449). Dichos documentos tienen tres formatos diferentes: un comunicado técnico diario ([ejemplo](https://www.gob.mx/cms/uploads/attachment/file/546100/Comunicado_Tecnico_Diario_COVID-19_2020.04.09.pdf)), una tabla de casos sospechosos al día de corte ([ejemplo](https://datos.covid19in.mx/tablas-diarias/sospechosos/202004/20200409.pdf)) y una tabla de casos positivos al día de corte ([ejemplo](https://datos.covid19in.mx/tablas-diarias/positivos/202004/20200409.pdf)) en formato PDF. 

Si bien existe un historial de los comunicados técnicos diarios [aquí](https://www.gob.mx/salud/documentos/informacion-internacional-y-nacional-sobre-nuevo-coronavirus-2019-ncov), las tablas de casos positivos y sospechosos desaparecen por alguna razón 🤷 del sitio oficial después de unos días. Remover los documentos del sitio oficial no es el único problema para analizar los datos, publicarlos en formato PDF 
hace el proceso muy difícil. 

La información de los casos negativos así como de las defunciones solo se publica como un acumulado total diario en los documentos oficiales. Sin embargo, existe un [mapa](https://ncov.sinave.gob.mx/mapa.aspx), también publicado por la Secretaría de Salud de México donde se puede visualizar (con colores) la situación de cada estado.

## Recopilación de los Datos
A partir del [28 de Marzo](https://github.com/mayrop/covid19in-mx/commit/d472d10cc7a7fad9b11099af8d5ee4f7dc07037c) empecé este proyecto de extracción y limpieza de datos. De igual manera, a partir del día 4 de Abril se obtiene de manera automática los datos oficiales de los casos negativos y defunciones por estado a partir del [mapa](https://ncov.sinave.gob.mx/mapa.aspx). 

### Primer Reto
Como ya se mencionó, el primer reto encontrado fue que, por algún motivo, algunos de los documentos PDF originales son removidos del sitio de la [Secretaría de Salud de México](https://www.gob.mx/salud/documentos/coronavirus-covid-19-comunicado-tecnico-diario-238449). Se consiguió obtener los documentos faltantes (del ~15 de Marzo al 28 Marzo) a través de fuentes no oficiales (que inteligentemente archivaron los documentos oficiales):
* [Serendipia](https://serendipia.digital/2020/03/datos-abiertos-sobre-casos-de-coronavirus-covid-19-en-mexico/). Una gran fuente que publica ambas fuentes, en formato abierto, y en formato original de PDF.
* [github.com/guzmart/covid19_mex](https://github.com/guzmart/covid19_mex). Un gran trabajo por parte de [@guzmart_](https://twitter.com/guzmart_).
* [github.com/carranco-sga/Mexico-COVID-19](https://github.com/carranco-sga/Mexico-COVID-19). Otro gran ejemplo de un gran trabajo por parte de [Gabriel Carranco-Sapiéns](https://github.com/carranco-sga).

#### Documentos Adicionales
* **8 de Marzo**: Tabla de casos sospechosos. El documento fue encontrado en formato original PDF [aquí](https://slp.gob.mx/SSALUD/Documentos%20compartidos/Coronavirus/marzo/Tabla_casos_sospechosos_COVID-19_2020.03.08.pdf). Fue encontrado a través de exhaustas búsquedas en Google hasta encontrar [una interfaz](https://slp.gob.mx/SSALUD/Documentos%20compartidos/Forms/AllItems.aspx?RootFolder=%2FSSALUD%2FDocumentos%20compartidos%2FCoronavirus&FolderCTID=0x0120002C4A6E2BDD73D34899963849CA684C1C&View=%7BFA81CA67%2D551E%2D4BDD%2D9C03%2DCA3F799D0382%7D) del gobierno que probablemente no debería estar pública.
* **13 de Marzo**: Tabla de casos positivos. El documento fue encontrado en formato PNG [aquí](https://www.scribd.com/document/452680821/Tabla-casos-positivos-resultado-InDRE-2020-03-13).

## Transformación de los Datos
El primer reto para poder obtener los datos para posteriormente analizarlos, es que los mismos son publicados en formato PDF. Eso hace imposible analizar dichos datos sin primero transformar los archivos a un formato amigable (CSV, XLSX, API).

Para empezar, los documentos PDF publicados son muy pesados (tardan mucho en cargar), así que se decidió optimizar los archivos usando una [herramienta en línea](https://smallpdf.com/compress-pdf). Actualmente se esta [probando](https://github.com/mayrop/datos-covid19in-mx/blob/parse/scripts/processing/run.sh#L57) una solución automática para la [compresión de los PDF](https://stackoverflow.com/questions/16530510/pdf-compression-like-smallpdf-com-programmatically-in-c-sharp). Los archivos que se pueden encontrar en este sitio son al menos **50%** mas ligeros que los originalmente publicados. 

{{< hint info >}}
**Nota:** Optimizar los archivos no altera el contenido del mismo, simplemente reduce el tamaño del mismo, lo cual ayuda al usuario final.
{{< /hint >}}

Existen diferentes formas de poder transformar archivos PDF a CSV, por ejemplo herramientas [en línea](https://convertio.co/pdf-csv/). [Gabriel Carranco-Sapiéns](https://github.com/carranco-sga) utiliza [una elegante solución](https://github.com/carranco-sga/Mexico-COVID-19/blob/master/Scraping/pdf_scraping.jl#L7) con el lenguaje de programación [Julia](https://julialang.org/). 

El proceso que se utilizó para este proyecto fue la de primero transformar el PDF a HTML utilizando [pdf2htmlEX](https://github.com/pdf2htmlEX/pdf2htmlEX), para posteriormente parsear dicho HTML a formato CSV. No bastó con obtener la información y pasarla a CSV, sino primero normalizar de los datos. Para obtener más información acerca de la normalización de los datos, haz [click aquí](/docs/datos/tablas-casos/normalizacion/).


## Código Fuente
El código fuente este proceso para puede ser encontrado [aquí](https://github.com/mayrop/datos-covid19in-mx/blob/master/scripts/processing/process.sh).

