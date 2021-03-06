Mapping Police Violence - Notes, Methodology, General Documentation
===================================================================

This work concerns a collaboration with BlackLivesMatter activist DeRay Mckesson seeking to evaluate if, and in what ways, mainstream press coverage of police killings has changed in the last few years. 

List of victims
---------------
There exist no 100% reliable lists of all victims of police violence, due to lack of transparency and poor documentation on the part of law enforcement agencies. This work relies on three databases which draw from news reporting, public record, and crowdsourced information in an attempt to comprehensively document victims of police violence. They are:
* [Mapping Police Violence](http://mappingpoliceviolence.org/) (MPV), with data [here](http://mappingpoliceviolence.org/aboutthedata/).
* [The Counted](http://www.theguardian.com/us-news/ng-interactive/2015/jun/01/the-counted-police-killings-us-database), by the Guardian, with data [here](http://www.theguardian.com/us-news/ng-interactive/2015/jun/01/about-the-counted).
* [Police Shootings](https://www.washingtonpost.com/graphics/national/police-shootings/), by the Washington Post, with data [here](https://github.com/washingtonpost/data-police-shootings).

Analysis of police violence in 2013 and 2014 relies solely on the list of [unarmed black victims](http://mappingpoliceviolence.org/unarmed2014/) provided by MPV. Analysis of police violence in 2015 relies on a combined list from all three sources. Analysis of police violence in 2016 relies only on WaPo and Guardian data. In sum we have included all victims killed between January 2013 and June 2016, inclusive.

* 2013 spreadsheet: https://docs.google.com/spreadsheets/d/1ArisyAjhUE1eeuA490-rPPI1nfft2cJIyDpaeOBqyj8/edit?usp=sharing
* 2014 spreadsheet: https://docs.google.com/spreadsheets/d/1699_rxlNIK3KSNzqpoczw0ehiwTp4IKEaEP_dfWo6vM/edit?usp=sharing
* 2015 spreadsheet: https://docs.google.com/spreadsheets/d/1HoG8jdioarEbxVI_IbuqRwQFCFqbUxzCHc6T2SymRUY/edit?usp=sharing
* 2016 spreadsheet: https://docs.google.com/spreadsheets/d/19wsyttAqa4jbPnqmxQWbu79rwzp3eq_EHbzzsRiomTU/edit?usp=sharing

Media sources
------------------------
We use the following sets of news sources provided in the Mediacloud database:

* [8875027: US Mainstream Media](https://sources.mediacloud.org/#media-tag/8875027/details)
* [2453107: US Regional Mainstream Media](https://sources.mediacloud.org/#media-tag/2453107/details)
* [129: US Wire Services](https://sources.mediacloud.org/#media-tag/129/details)
* [8878292: US Partisan Sources 2012 - Conservative](https://sources.mediacloud.org/#media-tag/8878292/details)
* [8878293: US Partisan Sources 2012 - Liberal](https://sources.mediacloud.org/#media-tag/8878293/details)
* [8878294: US Partisan Sources 2012 - Libertarian](https://sources.mediacloud.org/#media-tag/8878294/details)

Measurement of media coverage
-----------------------------
We initially avoid examining type or tone, and examine only quantity of media coverage about each victim. For each victim, we retrieve from MediaCloud:
* links to all US mainstream news stories in the timeframe (d-5, d+14), where d is the date of the incident--that is, stories in a timeframe from five days before to two weeks after.
* total number of news stories written about the victim in the specified timeframe
* total number of news stories written in total in the specified timeframe

This timeframe is chosen because anecdotally, news coverage of an event often ends after two or three days, and very rarely persists after two weeks. The two-week window should therefore provide a reasonable snapshot of primary coverage while excluding post-hoc secondary coverage, which might for instance be more concerned with follow-up events, investigations, trials, or activism around the victim's death. We also include the five days prior to capture coverage of the events leading up to the victim's death (for instance, if a victim was arrested and injured and then died in custody a few days later).

After we generate a combined query, create a controversy object, and generate a list of stories and bit.ly counts for each story, we sample some random names in the generated `mpv-controversy-stories.csv` to check for unrelated stories, and manually remove those unrelated stories from the controversy.

#### Multiple mentions
What happens if a story mentions two or more victims? `list-all-stories.py` counts a story twice if it mentions two victims, but only if the story falls within the specified timeframe around both victims' deaths. So if a story published today mentions person A who died two days ago, and person B who died last year, that story is only counted under person A. However, if a story published today mentions person A who died two days ago, and person C who died five days ago, the story is counted twice: once under person A and again under person C.

#### Normalization and deduplication
If we want to make statements about how the quantity of news coverage about police violence has changed over time, we need to normalize the article counts against the total number of articles published by our media sources in the timeframe we're concerned with. 

We also account for the fact that scraping news data is complicated and rarely results in perfect data. In particular, queries will often return the same story multiple times, resulting in overcounting. We do a simple URL check on each story about a victim to make sure that each unique story is only counted once. However, it's not practical to perform this check on every single story in the MediaCloud database, so this deduplication is not performed for the aggregate article counts used to normalize our data.

Misc. data details
------------------------------
#### Definition of "unarmed"
MPV codes victims as "unarmed" based on guidelines published [here](http://mappingpoliceviolence.org/aboutthedata/); most notably, they classify victims "holding a toy weapon" as unarmed. This is a discrepancy from both the Guardian and WaPo; the Guardian distinguishes between codings of "unarmed" and "non-lethal firearm", while WaPo distinguishes between "unarmed" and "toy weapon". The decision of whether victims holding toy weapons should be classified as "armed" or "unarmed" is both unobvious and political. Our list includes all black victims coded by our sources in the categories below, and we include the original codings in our spreadsheets.

We include all incidents coded as follows:
* "unarmed" or "unclear" by MPV 
* "unarmed", "unknown", "disputed", or "non-lethal firearm" by the Guardian
* "unarmed", "undetermined", or "toy weapon" by WaPo

#### Definition of race
We include only victims explicitly coded by the source as "Black".

#### Date of incident
MPV lists the date as "date of injury resulting in death" while WaPo and the Guardian only say they list the "date." This results in some discrepancies in the date of the "event." For one-day discrepancies, we use the earlier date. Freddie Gray and Natasha McKenna have larger discrepancies due to the particular circumstances of their deaths:
* Gray fell into a coma during his arrest on 4/12, and died a week later on 4/19
* McKenna had a heart attack after being tasered on 2/3, and was removed from life support on 2/8.
For Gray and McKenna, we use the dates of their deaths to timeframe the queries.

### Query adjustments
Specifying a MediaCloud query that retrieves all the reportage of a victim while excluding irrelevant stories is often more complicated than simply querying the victim's name; for instance, many victims share names with prominent public figures (like Jerry Brown, who shares a name with the California governor) or have names that are otherwise common (like Robert Baltimore). Each query was manually entered on [Dashboard](https://dashboard.mediameter.org/), and the sentences sampled in the "Mentions: Sentences Matching" box were manually checked to ensure that all the stories found are relevant. If a victim has multiple names, name variants, or name misspellings (for instance, "Asshams Manley" and "Asshams Pharoah Manley"), the query was adjusted to `name1 OR name2 OR name3 OR ...`. If irrelevant stories were found, the query was adjusted to exclude irrelevant stories; for instance, `Jerry Brown` was adjusted to `"Jerry Dwight Brown" OR ("jerry brown" AND (pasco OR zephyrhills OR fl OR florida))) AND -(gov* or CA or california or sacramento or democrat*)` to exclude references to the California governor.

#### Other metadata and notes
For all other data about each person/incident (age, gender, signs of mental illness, city/state, responsible law enforcement agency) we use data provided by MPV. If the incident is not listed in the MPV dataset, we use data provided by the Guardian. If the incident is not listed in the Guardian dataset either, we use data provided by WaPo. All information missing after this process is tabulated as NaN.

Population data for each city is manually retrieved from the US Census Bureau's [American FactFinder](http://factfinder.census.gov/faces/nav/jsf/pages/community_facts.xhtml) tool. If the listed city is unavailable, we use Wikipedia. If Wikipedia does not provide the information, we estimate population using the city's zip code(s).

MPV has one case listed under "Liberty City" (a district of Miami) and one case listed under "Vermont Square" (a district of Los Angeles) -- for these cases we use the city for our analysis, and not the specific district.