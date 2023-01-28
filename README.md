**Since the data and code analyzing the data in this repository are part of a
research project, I moved the material into the supplements for
[the paper](https://github.com/apparebit/penal-colony)**

# Analyzing NCMEC's CSAM Reports for 2021

This repository contains the data and Python code for analyzing the geographic
spread of child sexual abuse material (CSAM).


## Data Sources

I used the following data sources:

  * [Reports of
    CSAM](https://www.missingkids.org/content/dam/missingkids/pdfs/2021-reports-by-country.pdf)
    made to the National Center for Missing and Exploited Children (NCMEC),
    which serves a legally mandated clearinghouse in the United States;
  * [Per-country population size](https://ourworldindata.org/grapher/population)
    from Our World in Data;
  * [ISO Alpha-3 country codes](https://www.iso.org/obp/ui/#search) scraped from
    ISO's website;
  * [Continents and continental
    regions](https://github.com/lukes/ISO-3166-Countries-with-Regional-Codes).

Since NCMEC only releases a PDF with the per-country report counts, I used some
online service to extract the table as CSV and manually added ISO Alpha-3 codes.
I similarly enriched ISO's country code table with information about continents.


## Bugs in the Data

While the manual editing was tedious, it also helped me discover several bugs in
NCMEC's book-keeping:

  * The table includes data for the Netherlands Antilles (ANT) even though it
    was split into Bonaire, Saint Eustatius and Saba (BES), Curaçao (CUW), and
    Sint Maarten (SXM) in 2010, which also are represented.
  * The table also includes Bouvet Island, but that island just north of
    antarctica is a nature preserve. Though apparently, researchers may spend up
    to 4 months per year in the only human structure, a weather station.
  * Finally, the table also has two entries for French Guiana.

With 2 reports for the Netherlands Antilles, 1 report for Bouvet Island, and 4
reports for antarctica, omitting the three from evaluation is an easy and safe
decision. After all, I already had to discard 1,193,266 reports without a
country.


## Lies, Damned Lies, and Statistics

The analysis [in the notebook](csam-analysis.ipynb) illustrates how even basic
frequency counts end up telling completely different stories, depending on small
changes in the analysis.

  * Notably, when just counting **reports per country**, without normalization,
    the five countries with the most CSAM reports are India (16.7% of all
    reports), Philipines (11.3%), Pakistan (7.2%), Indonesia (6.6%), and
    Bangladesh (6.2%).
  * As expected, when **accumulating by continental region**, Southern Asia is
    worst (33.4%), followed by South-Eastern Asia (27.0), Western Asia (12%),
    and North Africa (9.2%).
  * Again as expected, when **accumulating by continent**, Asia is worst with
    73.2% of all reports.
  * When normalizing the report counts by population size, only one of the
    countries with the most reports appears amongst the 20 countries with the
    **most reports per capita**. First are the Cocos Islands with 0.28 reports
    per capita (RPC) followed by Libya (0.04), the United Arab Emirates (0.04),
    Iraq (0.03), and Philippines (0.03).

Alas, the Cocos Islands had 168 reports for a population of 596. Given the tiny
population size, it's probably best to drop the Cocos from any comparison.


## Show Me the Choropleth

I pushed hard to produce the same Choropleth with Matplotlib and also Plotly.
Visually, I find the latter more appealing because, somehow, Matplotlib's border
lines are too soft and hence seem out of focus. Also, Plotly's interactive
features just work out of the box. At the same time, the code for Matplotlib is
a tad shorter and definitely better organized, including more uniform. Geopandas
also integrates mapclassify's many classification schemes for making sure the
data maximizes the available color gradient. Having a menu of standard
classifications readily available makes it much easier to produce a reasonably
shaded choropleth with Matplotlib. Because of that, I recommend being extra
vigilant when faced with a Plotly choropleth. Its mapping of values onto the
color gradient probably isn't a good one.


## For Context

For context, well over 90% of the reports originate from Meta. That firm also
conducted a visual inspection of a small sample over a year ago and learned that
75% of the sample were not really CSAM because they either were memes that
happened to feature children or they were teenagers sexting.

---

© 2023 [Robert Grimm](https://apparebit.com).
[Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) license.
