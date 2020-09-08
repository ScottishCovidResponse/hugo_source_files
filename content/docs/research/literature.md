---
title: Sytematic Literature Review
weight: 1
---

## SCRC Procedure for Systematised Literature Review (V1.2 19/6/20)

**The purpose of this procedure is to outline an approach to systematised literature searches to obtain parameters and distributions for SCRC models.**

SCRC lacks capacity to carry out comprehensive reviews ourselves; therefore we seek to find the most salient readily available data, focussing on other people’s reviews, papers cited in these, and the papers underpinning other major modelling exercises.  

The systematised procedure to identify and process these is as follows:

1. Based on the needs of the models, encourage focus on a series of specific questions (eg what is the incubation period?)  We believe this is best practice, as advised by the experience of the Irish team.  In the first instance, this should be informed by the specifications from each model stored in MS Teams /General/Modelling/model-parameters and the unified compartment approach based on the Irish work.  Questions are most likely to be specified by the Epidemiology team.

2. Inclusion and exclusion criteria for studies are as follows:  
    1. Quality of research design: official government data; formal randomised study; census of sub-population (eg contact tracing); observational study are optimal.  Convenience samples are least acceptable.
    2. Dates study was conducted (if relevant)
    3. Geographic region (if relevant)
    4. Social/Legal conditions (eg during what type of societal intervention) (if relevant)
    5. Where these can be identified in advance, special cases of the parameter class should be specified (eg incubation period for asymptomatic as opposed to symptomatic individuals)
    6. These criteria should be adapted as required for different questions.

3. Search the following resources:
    1. LitCovid, Cochrane COVID-19 and Edinburgh University Repository of Systematic Reviews databases, looking for well conducted reviews.
    2. McMaster; this provides pointers to different key resources for COVID-19 evidence, including appraised studies and reviews. This list of resources will be quite comprehensive.
    3. Collations of ‘grey’ literature: MedRxiv (note MedRxiv materials are pre-prints, and hence have not been peer reviewed; they cover all articles and hence are not necessarily reviews) https://connect.medrxiv.org/relate/content/181 .  In addition we can access the SRUC collated list of reports and papers.  
    4. Prospero is a database of protocols of registered systematic reviews. Prospero can be used to see if a review is underway and when it is likely to give results.  This is particularly important if information is otherwise lacking, but also has a more general utility.
    5. Relevant reports/publications on the ‘approved list’ of equivalent mathematical models (Currently the Irish group, Imperial, LSTMH, SCoVMod.  Likely to be extended to all documents in DDI Project Repo B).
    6. Primary papers and reports identified or used in a secondary review or model paper identified under criteria 3.1-3.5 above, where the secondary document does not itself produce a relevant data product different to that in the primary document.
    7. Estimates extracted from secondary or primary literature reviewed as part of a pathway arising from criterion 3.5 should be flagged as not having been independently identified.  A flag to this effect is included in the Extraction sheet.
    8. To elaborate on criterion 3.6: a (secondary) paper reporting a meta-analysis generates a new product which should be reported in preference to any of the primary estimates used in the analysis.  A peer-reviewed paper which assesses the validity of estimates from the grey literature is generating new products with superior provenance to the originals, since the former have been peer reviewed.  Hence the products from the secondary paper should be reported in preference.  However, a paper which merely lists parameter estimates from other papers, although useful as a platform to identify potentially relevant primary estimates, should not be used as the basis of a data extraction.

4. Data extraction sheet for papers to map to requirements in data registry; including fields for quality of review/study, the suitability of the estimation methods for parameter of interest, and the generalizability/applicability of the products. 
    1. Quality of Estimate: Score as Good/Potentially Good, but not Peer-Reviewed/Adequate/Poor, taking account of assessed quality of review/study and assessed suitability of the estimation methods for parameter of interest.  Only documents which have been subject to peer review can be scored as Good.  
    2. Generalizability/applicability of products: Score as Universal/Partial/Low
        1. Universal means that the quantity is applicable in all contexts.  For example, an estimate of incubation period in adults might be designated as universal since it is believed relevant to all such individuals.
        2. Partial means that the quantity is applicable in some modelling contexts, but not in others.  For example, an estimate of infection rate in London might be designated as partial since it is might be generalised as relevant to urban areas but not to rural, or not generalizable at all, in which case it would still be relevant to any model which modelled London separately.  Operationally, this should serve as a warning to the modellers to carefully consider the context in which the estimate is used.  It’s applicability is limited in some way, and so consideration should be given to its validity in any specific context.
        3. Low means that the applicability to any likely SCRC activity is consistently weak. For example, estimates of mortality derived from Wuhan in the earliest days of the outbreak are arguably irrelevant now.
    3. Ways in which the estimate fails to generalise should be recorded as important aspects of the narrative attached to the estimate.
    4. These are assessed properties of the estimate.   ‘Fitness for Purpose’ is a derived property which seeks, on the basis of these assessed properties, as they apply to all SCRC models, to assign a status to the estimate, flagging priorities for further research.
5. Flag parameters with derived assessment of ‘Fitness for Purpose’.  We propose 4 ordered categories.  The default values are:
    1. If the parameter estimate is not subject to any serious concerns and as such is unlikely to be improved upon, (Quality=Good; Applicability=Universal), it should be given a Green rating.  The existence of such an estimate means that this parameter class would not be a priority for further research.
    2. If a parameter estimate has mild quality concerns recorded against it, (Quality=Adequate.OR.Potentially Good; Applicability=Universal), it should be given an Amber rating.  If a better estimate emerges, it should be evaluated and potentially adopted for use, but this parameter class will not be a priority for further research.  If (Applicability=Partial)&(Quality>Poor), it should be given an Amber rating.  The assumption is that it will not be a priority for further research per se, although a modeller who assesses it as not applicable to their model might chose to request research on a similar, but more tightly defined new parameter product.
    3. If a parameter estimate has applicability concerns recorded, (Applicability=Low), it should be given a Red rating.   The desirability of finding a better estimate should be flagged as a task to literature reviewers to check for up-to-date reviews/further studies.  Ditto if Quality = Poor.
    4. If a parameter estimate has concerns recorded against it, which are so severe that it should not be used in preference to other available more highly rated data products, or that it has been superseded in some way, it should be given a Black rating, and this status propagated to all dependent products.

6. Source document should be uploaded to the SCRC MS Teams Literature Review Channel. **Folder structure TBC**.  Files should be named consistently.  File names should be short but descriptive (Preferably <30 characters).  Do not use special characters or spaces in a file name: use capitals and underscores instead of periods or spaces or slashes. Dates are unlikely to be useful, but if used, should be in ISO 8601 date format: YYYYMMDD.  

Format should be:  

`[Reviewer Surname]_[First Author Surname]_[Journal Name Abbreviation or Source Abbreviation]_[Final block of DOI if applicable]`

<table> 
    <tr>
        <thead>  
            <th>Fitness for Purpose</th> 
            <th scope="col" colspan="3">Applicability</th> 
        </thead>
    </tr> 
    <tr> 
        <thead>  
            <th>Quality</th> 
            <th>Universal</th> 
            <th>Partial</th> 
            <th>Low</th> 
        </thead>
    </tr>  
    <tr> 
        <td>Good</td> 
        <td style="background-color:#31a354"></td> 
        <td style="background-color:#fec44f"></td> 
        <td style="background-color:#f03b20"></td> 
    </tr> 
    <tr> 
        <td>Potentially Good</td> 
        <td style="background-color:#fec44f"></td> 
        <td style="background-color:#fec44f"></td> 
        <td style="background-color:#f03b20"></td> 
    </tr> 
    <tr> 
        <td>Adequate</td> 
        <td style="background-color:#fec44f"></td> 
        <td style="background-color:#fec44f"></td> 
        <td style="background-color:#f03b20"></td> 
    </tr> 
    <tr> 
        <td>Poor</td> 
        <td style="background-color:#f03b20"></td> 
        <td style="background-color:#f03b20"></td> 
        <td style="background-color:#f03b20"></td> 
    </tr> 
</table>

**Annex. Links to Databases:** There is likely to be a significant overlap in all these resources.

[LitCovid](https://www.ncbi.nlm.nih.gov/research/coronavirus/faq) - data from PubMed, updated daily. No pre-prints.

[McMaster](https://plus.mcmaster.ca/COVID-19/About ). This is an alerting service which curates and assesses publications about Covid, indexed in MEDLINE, that meet their defined scientific criteria. They  

[Cochrane Study Register](https://www.cochrane.org/coronavirus-covid-19-cochrane-resources-and-news). This covers ClinicalTrials.gov, WHO International Trials Registry Platform (ICTRP) and PubMed.

[Edinburgh Uni hosted Repository of Systematic Reviews](https://www.ed.ac.uk/usher/uncover/register-of-reviews)

[Prospero](https://www.crd.york.ac.uk/prospero/)

[Clinical trials and Pubmed](https://community.cochrane.org/about-covid-19-study-register) updated daily. ICTRP weekly.

Annex: Draft Data extraction sheet (https://forms.gle/E4YgYS9Zvckmc9R58)

| Field |  |
| Short Descriptor of Information Extracted |  |
| Detailed Description of Information Extracted |  |
| Point estimate (if available) |   |
| Uncertainty/variability metrics  (Best of: None/Interval/Distribution specification: distribution and parameters or Empirical distribution) |   |
| Title of Paper/Report |   |
| Authors |   |
| DOI |   |
| Uploaded to MSTeams? |   |
| Identification Route (eg LitCOVID; MedRxiv) |   |
| Potential Lack of Independence in Parameter Estimate |   |
| Status (Peer reviewed/Official Report/Preprint/Other) |   |
| Intrinsic Quality of Estimate |   |
| Comments on Intrinsic Quality |   |
| Tag this Document for Further, Future Review? |   |
| Applicability (Generalizability) |   |
| Narrative: Lack of Applicability |   |
| Fitness for Purpose |   |
| Primary Data Availability |   |
| Primary Data Location |   |
| Reviewer Name |   |
| Date Logged |   |
