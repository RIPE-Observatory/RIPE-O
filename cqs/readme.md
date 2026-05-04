# Competency Questions

The list of RIPE-O competency questions. Each question is answered by following the classes and properties used by the ontology to represent a research integrity assessment: the work under assessment, the evidence considered, the assessment questions, the hypotheses answering those questions, and the agents involved in producing or reviewing the assessment.

## CQ1. What research work has been assessed?

**Key concepts:** `ripe:ResearchIntegrityAssessment`, `ripe:Work`, `ripe:PublicationDetails`, `ripe:assesses`, `prov:wasMemberOf`, `prism:doi`, `dcterms:title`

RIPE-O represents an assessment as `ripe:ResearchIntegrityAssessment` and links it to the assessed work with `ripe:assesses`. Publication metadata is represented separately as `ripe:PublicationDetails`, which is part of the same assessment through `prov:wasMemberOf`.

```sparql
PREFIX ripe:     <https://w3id.org/ripe/ripe-o#>
PREFIX prov:     <http://www.w3.org/ns/prov#>
PREFIX prism:    <http://prismstandard.org/namespaces/basic/3.0/>
PREFIX dcterms:  <http://purl.org/dc/terms/>

SELECT DISTINCT ?work ?doi ?title ?publicationDate ?journal WHERE {
  ?assessment a ripe:ResearchIntegrityAssessment ;
              ripe:assesses ?work .
  ?publication a ripe:PublicationDetails ;
               prov:wasMemberOf ?assessment ;
               dcterms:title ?title .
  OPTIONAL { ?work prism:doi ?doi }
  OPTIONAL { ?publication prism:publicationDate ?publicationDate }
  OPTIONAL { ?publication prism:publicationName ?journal }
}
ORDER BY DESC(BOUND(?doi)) ?doi ?title ?work
LIMIT 3
```

| work | doi | title | publicationDate | journal |
| --- | --- | --- | --- | --- |
| https://w3id.org/ripe/ripe-kg/work/10.1001%2Fjama.2024.23898 | 10.1001/jama.2024.23898 | Intravenous Lidocaine for Gut Function Recovery in Colonic Surgery | 2024-11-27 | JAMA |
| https://w3id.org/ripe/ripe-kg/work/10.1001%2Fjamanetworkopen.2018.5630 | 10.1001/jamanetworkopen.2018.5630 | Prevalence and Severity of Food Allergies Among US Adults | 2019-01-04 | JAMA Network Open |
| https://w3id.org/ripe/ripe-kg/work/10.1001%2Fjamanetworkopen.2019.14393 | 10.1001/jamanetworkopen.2019.14393 | Effect of Escalating Financial Incentive Rewards on Maintenance of Weight Loss | 2025 | JAMA Network Open |

The answer is the `ripe:Work` IRI. DOI, title, publication date, and journal are returned from the publication metadata attached to the same assessment.

## CQ2. What assessments, when, and by whom have been performed for this work?

**Key concepts:** `ripe:ResearchIntegrityAssessment`, `tido:Evaluation`, `ripe:Work`, `ripe:assesses`, `tido:contributesTo`, `prov:wasAssociatedWith`, `prov:startedAtTime`, `prov:endedAtTime`

The assessed work is selected by DOI. RIPE-O then reaches each assessment through `ripe:assesses`, and the TIDO evaluation activity through `tido:contributesTo`. The temporal and agent provenance of the evaluation is recorded with PROV properties.

```sparql
PREFIX ripe:   <https://w3id.org/ripe/ripe-o#>
PREFIX tido:   <https://w3id.org/tido#>
PREFIX prov:   <http://www.w3.org/ns/prov#>
PREFIX prism:  <http://prismstandard.org/namespaces/basic/3.0/>

SELECT DISTINCT ?assessment ?started ?ended ?agent WHERE {
  ?work prism:doi "10.1016/j.jad.2017.12.049" .
  ?assessment a ripe:ResearchIntegrityAssessment ;
              ripe:assesses ?work .
  ?evaluation a tido:Evaluation ;
              tido:contributesTo ?assessment ;
              prov:startedAtTime ?started ;
              prov:endedAtTime ?ended ;
              prov:wasAssociatedWith ?agent .
}
ORDER BY ?started ?assessment ?agent
LIMIT 3
```

| assessment | started | ended | agent |
| --- | --- | --- | --- |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA0C83AF9CE6274D42 | 2025-10-30T15:39:46.521925Z | 2025-10-30T15:58:44.744913Z | https://w3id.org/ripe/ripe-kg/automated-agent/inspect-ai/1.0.0 |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA0C83AF9CE6274D42 | 2025-10-30T15:39:46.521925Z | 2025-10-30T15:58:44.744913Z | https://w3id.org/ripe/ripe-kg/human-reviewer/RV003 |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA4BED4D9BD987577D | 2025-11-03T10:51:40.563277Z | 2025-11-03T12:56:13.393595Z | https://w3id.org/ripe/ripe-kg/automated-agent/inspect-ai/1.0.0 |

The result shows the assessment activity, its time interval, and each associated agent. A single assessment can have more than one associated agent because automated and human review can both contribute to the same assessment.

## CQ3. What published third-party evidence, including retraction notices, expressions of concern, corrections, or peer comments, has been used to assess the work?

**Key concepts:** `ripe:ResearchIntegrityAssessment`, `ripe:RetractionNotice`, `ripe:ExpressionOfConcern`, `ripe:CorrectionNotice`, `ripe:PeerComment`, `tido:Evaluation`, `prov:used`, `tido:contributesTo`

RIPE-O models publication notices and peer comments as forms of `ripe:IntegrityAssessmentEvidence`. The assessment identifies the work, while the evaluation records which evidence was used through `prov:used`.

```sparql
PREFIX ripe:   <https://w3id.org/ripe/ripe-o#>
PREFIX tido:   <https://w3id.org/tido#>
PREFIX prov:   <http://www.w3.org/ns/prov#>
PREFIX prism:  <http://prismstandard.org/namespaces/basic/3.0/>
PREFIX fabio:  <http://purl.org/spar/fabio/>

SELECT DISTINCT ?evidence ?type ?evidenceDoi ?date ?recordId ?url WHERE {
  ?work prism:doi "10.3109/14767058.2014.954241" .
  ?assessment ripe:assesses ?work .
  ?evaluation a tido:Evaluation ;
              tido:contributesTo ?assessment ;
              prov:used ?evidence .
  VALUES ?type {
    ripe:RetractionNotice
    ripe:ExpressionOfConcern
    ripe:CorrectionNotice
    ripe:PeerComment
  }
  ?evidence a ?type .
  OPTIONAL { ?evidence prism:doi ?evidenceDoi }
  OPTIONAL { ?evidence prism:publicationDate ?date }
  OPTIONAL { ?evidence ripe:retractionWatchRecordId ?recordId }
  OPTIONAL { ?evidence fabio:hasURL ?url }
}
ORDER BY ?type ?evidence
LIMIT 3
```

| evidence | type | evidenceDoi | date | recordId | url |
| --- | --- | --- | --- | --- | --- |
| https://w3id.org/ripe/ripe-kg/expression-of-concern/67049 | https://w3id.org/ripe/ripe-o#ExpressionOfConcern | 10.1080/14767058.2020.1842963 | 2021-01-27 | 67049 |  |
| https://w3id.org/ripe/ripe-kg/peer-comment/RIPEA534C4E03EC302AE0-1 | https://w3id.org/ripe/ripe-o#PeerComment |  |  |  | https://pubpeer.com/publications/69464483D9B5BD62C0DB53E3DA6E15 |
| https://w3id.org/ripe/ripe-kg/peer-comment/RIPEA534C4E03EC302AE0-2 | https://w3id.org/ripe/ripe-o#PeerComment |  |  |  | https://pubpeer.com/publications/69464483D9B5BD62C0DB53E3DA6E15 |

The answer returns the evidence resources and their RIPE-O evidence types. DOI, date, Retraction Watch identifier, and URL are optional descriptive properties of the evidence.

## CQ4. What authors of this assessed work have been associated with retraction notices for other works?

**Key concepts:** `ripe:Work`, `ripe:Author`, `ripe:RetractionNotice`, `dcterms:creator`, `ripe:concerns`, `cito:retracts`, `foaf:name`

Authors are connected to works with `dcterms:creator`. Retraction notices are represented as `ripe:RetractionNotice`, linked to the concerned author with `ripe:concerns` and to the retracted work with `cito:retracts`.

```sparql
PREFIX ripe:     <https://w3id.org/ripe/ripe-o#>
PREFIX prism:    <http://prismstandard.org/namespaces/basic/3.0/>
PREFIX dcterms:  <http://purl.org/dc/terms/>
PREFIX foaf:     <http://xmlns.com/foaf/0.1/>
PREFIX cito:     <http://purl.org/spar/cito/>

SELECT DISTINCT ?authorName (COUNT(DISTINCT ?otherWork) AS ?retractedWorks) WHERE {
  ?work prism:doi "10.1016/j.jad.2017.12.049" ;
        dcterms:creator ?author .
  ?author foaf:name ?authorName .
  ?notice a ripe:RetractionNotice ;
          ripe:concerns ?author ;
          cito:retracts ?otherWork .
  FILTER(?otherWork != ?work)
}
GROUP BY ?authorName
ORDER BY DESC(?retractedWorks) ?authorName
LIMIT 3
```

| authorName | retractedWorks |
| --- | --- |
| Zatollah Asemi | 21 |
| Mehri Jamilian | 18 |
| Esmat Aghadavod | 13 |

The answer identifies authors of the selected assessed work who are also associated with retraction notices concerning other works. The count is over distinct retracted works.

## CQ5. Is the assessed work retrospectively registered and what evidence has been assessed to support this?

**Key concepts:** `ripe:RegistryEvidence`, `ripe:StudyDesignEvidence`, `prov:wasMemberOf`, `ripe:isProspective`, `ripe:registrationAssessmentRationale`, `ripe:recruitmentStartDate`, `ripe:studyEndDate`

Registration evidence and study-design evidence are represented as separate evidence resources in the same assessment. `ripe:RegistryEvidence` records the registration identifier, registry, date, and prospective judgement; `ripe:StudyDesignEvidence` records the recruitment period used to support that judgement.

```sparql
PREFIX ripe:   <https://w3id.org/ripe/ripe-o#>
PREFIX prov:   <http://www.w3.org/ns/prov#>
PREFIX prism:  <http://prismstandard.org/namespaces/basic/3.0/>

SELECT DISTINCT ?assessment ?trialId ?registryName ?registrationDate ?isProspective ?rationale ?recruitmentStartDate ?studyEndDate WHERE {
  ?work prism:doi "10.1016/j.jacl.2015.12.017" .
  ?assessment ripe:assesses ?work .
  ?registry a ripe:RegistryEvidence ;
            prov:wasMemberOf ?assessment ;
            ripe:registrationId ?trialId ;
            ripe:registryName ?registryName ;
            ripe:registrationDate ?registrationDate ;
            ripe:isProspective ?isProspective ;
            ripe:registrationAssessmentRationale ?rationale .
  ?studyDesign a ripe:StudyDesignEvidence ;
               prov:wasMemberOf ?assessment ;
               ripe:recruitmentStartDate ?recruitmentStartDate .
  OPTIONAL { ?studyDesign ripe:studyEndDate ?studyEndDate }
}
ORDER BY ?assessment
```

| assessment | trialId | registryName | registrationDate | isProspective | rationale | recruitmentStartDate | studyEndDate |
| --- | --- | --- | --- | --- | --- | --- | --- |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA233158DEF9C3F087 | IRCT201506085623N43 | IRCT | 2015-06-23 | false | Comparison performed at month level. Registration: 2015-06, LLM Recruitment Start: 2015-05. Deemed retrospective at month level. | 05-2015 | 07-2015 |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEAF7E1928294A86E83 | IRCT201506085623N43 | IRCT | 2015-06-23 | false | Comparison performed at month level. Registration: 2015-06, LLM Recruitment Start: 2015-05. Deemed retrospective at month level. | 05-2015 | 07-2015 |

The answer shows that the registration evidence is retrospective for the selected work. The rationale records the comparison between registration timing and recruitment timing.

## CQ6. What are the automated and human-reviewed outcomes for each integrity question associated with this assessed work?

**Key concepts:** `ripe:IntegrityAssessmentQuestion`, `ripe:IntegrityAssessmentHypothesis`, `ripe:AutomatedAgent`, `ripe:HumanReviewer`, `tido:answers`, `ripe:resultOutcome`, `ripe:rationale`, `prov:wasAttributedTo`

Each integrity question is a `ripe:IntegrityAssessmentQuestion`. Answers are represented as `ripe:IntegrityAssessmentHypothesis` instances connected to the question with `tido:answers`. The same question can have an automated hypothesis and a human-reviewed hypothesis, distinguished by `prov:wasAttributedTo`.

```sparql
PREFIX ripe:  <https://w3id.org/ripe/ripe-o#>
PREFIX tido:  <https://w3id.org/tido#>
PREFIX prov:  <http://www.w3.org/ns/prov#>
PREFIX prism: <http://prismstandard.org/namespaces/basic/3.0/>
PREFIX rdfs:  <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?assessment ?question ?automatedOutcome ?humanOutcome ?humanRationale WHERE {
  ?work prism:doi "10.1001/jamanetworkopen.2019.14393" .
  ?assessment ripe:assesses ?work .
  ?questionNode a ripe:IntegrityAssessmentQuestion ;
                rdfs:label ?question .
  ?automated a ripe:IntegrityAssessmentHypothesis ;
             tido:answers ?questionNode ;
             ripe:resultOutcome ?automatedOutcome ;
             prov:wasMemberOf ?assessment ;
             prov:wasAttributedTo ?automatedAgent .
  ?automatedAgent a ripe:AutomatedAgent .
  ?reviewed a ripe:IntegrityAssessmentHypothesis ;
            tido:answers ?questionNode ;
            ripe:resultOutcome ?humanOutcome ;
            prov:wasMemberOf ?assessment ;
            prov:wasAttributedTo ?reviewer .
  ?reviewer a ripe:HumanReviewer .
  OPTIONAL { ?reviewed ripe:rationale ?humanRationale }
}
ORDER BY ?assessment ?question
LIMIT 3
```

| assessment | question | automatedOutcome | humanOutcome | humanRationale |
| --- | --- | --- | --- | --- |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEAF6F62EBFADD42DFB | 1.1. Does the study have an associated retraction? | no | no |  |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEAF6F62EBFADD42DFB | 1.2. Does the study have an associated expression of concern or other relevant post-publication notice? | no | no |  |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEAF6F62EBFADD42DFB | 1.3. Do other studies by the research team highlight causes for concern? | yes | no | Although a correction has been flagged, this relates to the failure to acknowledge a particular individual and is not related to the integrity of the study results. |

The answer starts from the selected work and compares the automated and human-reviewed hypotheses for the integrity questions associated with its assessment. Human-reviewed hypotheses may also carry a `ripe:rationale`.

## CQ7. For which integrity questions did the human reviewer disagree with the automated outcome, and why?

**Key concepts:** `ripe:IntegrityAssessmentQuestion`, `ripe:IntegrityAssessmentHypothesis`, `ripe:AutomatedAgent`, `ripe:HumanReviewer`, `ripe:rationale`, `ripe:resultOutcome`

A disagreement is represented by two `ripe:IntegrityAssessmentHypothesis` instances in the same assessment, answering the same `ripe:IntegrityAssessmentQuestion`, with different `ripe:resultOutcome` values. The human-reviewed hypothesis records the reviewer rationale.

```sparql
PREFIX ripe:   <https://w3id.org/ripe/ripe-o#>
PREFIX tido:   <https://w3id.org/tido#>
PREFIX prov:   <http://www.w3.org/ns/prov#>
PREFIX prism:  <http://prismstandard.org/namespaces/basic/3.0/>
PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?assessment ?doi ?question ?automatedOutcome ?humanOutcome ?humanRationale WHERE {
  ?assessment ripe:assesses ?work .
  ?work prism:doi ?doi .
  ?questionNode a ripe:IntegrityAssessmentQuestion ;
                rdfs:label ?question .
  ?automated a ripe:IntegrityAssessmentHypothesis ;
             tido:answers ?questionNode ;
             ripe:resultOutcome ?automatedOutcome ;
             prov:wasMemberOf ?assessment ;
             prov:wasAttributedTo ?automatedAgent .
  ?automatedAgent a ripe:AutomatedAgent .
  ?reviewed a ripe:IntegrityAssessmentHypothesis ;
            tido:answers ?questionNode ;
            ripe:resultOutcome ?humanOutcome ;
            ripe:rationale ?humanRationale ;
            prov:wasMemberOf ?assessment ;
            prov:wasAttributedTo ?reviewer .
  ?reviewer a ripe:HumanReviewer .
  FILTER(?automatedOutcome != ?humanOutcome)
}
ORDER BY ?doi ?question ?assessment
LIMIT 3
```

| assessment | doi | question | automatedOutcome | humanOutcome | humanRationale |
| --- | --- | --- | --- | --- | --- |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEAF6F62EBFADD42DFB | 10.1001/jamanetworkopen.2019.14393 | 1.3. Do other studies by the research team highlight causes for concern? | yes | no | Although a correction has been flagged, this relates to the failure to acknowledge a particular individual and is not related to the integrity of the study results. |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA3A616563C128D80E | 10.1002/ptr.6406 | 1.3. Do other studies by the research team highlight causes for concern? | yes | unclear | One co-author has 2 retractions |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA2D3EB3B0F74C441B | 10.1007/s00394-018-1760-8 | 1.3. Do other studies by the research team highlight causes for concern? | yes | unclear | Correction made to another article due to errors in measuring 25OHD. |

The answer returns the assessment, the work DOI, the integrity question, both outcomes, and the human rationale explaining the change or qualification.

## CQ8. What is the human-validated overall integrity assessment for this work?

**Key concepts:** `ripe:OverallIntegrityAssessmentQuestion`, `ripe:IntegrityAssessmentHypothesis`, `ripe:HumanReviewer`, `tido:answers`, `ripe:resultOutcome`, `ripe:rationale`

RIPE-O treats the overall judgement as another hypothesis. It answers a `ripe:OverallIntegrityAssessmentQuestion` and is attributed to a `ripe:HumanReviewer`.

```sparql
PREFIX ripe:      <https://w3id.org/ripe/ripe-o#>
PREFIX tido:      <https://w3id.org/tido#>
PREFIX prov:      <http://www.w3.org/ns/prov#>
PREFIX prism:     <http://prismstandard.org/namespaces/basic/3.0/>

SELECT DISTINCT ?assessment ?overallOutcome ?overallRationale WHERE {
  ?work prism:doi "10.1111/bjdp.12503" .
  ?assessment ripe:assesses ?work .
  ?overallQuestion a ripe:OverallIntegrityAssessmentQuestion .
  ?overall a ripe:IntegrityAssessmentHypothesis ;
           tido:answers ?overallQuestion ;
           ripe:resultOutcome ?overallOutcome ;
           prov:wasMemberOf ?assessment ;
           prov:wasAttributedTo ?reviewer .
  ?reviewer a ripe:HumanReviewer .
  OPTIONAL { ?overall ripe:rationale ?overallRationale }
}
ORDER BY ?assessment
LIMIT 3
```

| assessment | overallOutcome | overallRationale |
| --- | --- | --- |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA0E41D3F40BC99EC2 | some-concerns | Absence of study registration. |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA2062B78F752BEFA6 | some-concerns | I would judge this as some concerns based on the lack of information about study registration |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA21337653A73A2388 | some-concerns | Not registered |

The answer returns the human-reviewed overall outcome for the selected work and the rationale where one was recorded.

## CQ9. Which authors are associated with works where integrity assessment identified concerns?

**Key concepts:** `ripe:OverallIntegrityAssessmentQuestion`, `ripe:IntegrityAssessmentHypothesis`, `ripe:HumanReviewer`, `ripe:Work`, `ripe:Author`, `dcterms:creator`, `foaf:name`, `ripe:resultOutcome`

A work is treated as having identified concerns when a human-reviewed hypothesis answering the overall integrity question has outcome `some-concerns` or `serious-concerns`. Authors are then reached from the assessed work using `dcterms:creator`.

```sparql
PREFIX ripe:      <https://w3id.org/ripe/ripe-o#>
PREFIX tido:      <https://w3id.org/tido#>
PREFIX prov:      <http://www.w3.org/ns/prov#>
PREFIX dcterms:   <http://purl.org/dc/terms/>
PREFIX foaf:      <http://xmlns.com/foaf/0.1/>

SELECT DISTINCT ?authorName (COUNT(DISTINCT ?work) AS ?publicationCount) WHERE {
  ?overallQuestion a ripe:OverallIntegrityAssessmentQuestion .
  ?overall a ripe:IntegrityAssessmentHypothesis ;
           tido:answers ?overallQuestion ;
           ripe:resultOutcome ?outcome ;
           prov:wasAttributedTo ?reviewer ;
           prov:wasMemberOf ?assessment .
  VALUES ?outcome { "some-concerns" "serious-concerns" }
  ?reviewer a ripe:HumanReviewer .
  ?assessment ripe:assesses ?work .
  ?work dcterms:creator ?author .
  ?author foaf:name ?authorName .
}
GROUP BY ?authorName
ORDER BY DESC(?publicationCount) ?authorName
LIMIT 3
```

| authorName | publicationCount |
| --- | --- |
| Zatollah Asemi | 16 |
| Mehri Jamilian | 8 |
| Alberto Ferlin | 4 |

The answer groups authors by the number of distinct works for which human-reviewed overall assessments identified concerns.

## CQ10. Which works were assessed by this reviewer?

**Key concepts:** `ripe:HumanReviewer`, `tido:Evaluation`, `ripe:ResearchIntegrityAssessment`, `ripe:Work`, `prov:wasAssociatedWith`, `tido:contributesTo`, `ripe:assesses`, `prism:doi`, `dcterms:title`

RIPE-O uses PROV and TIDO to connect a reviewer to an evaluation, the evaluation to an assessment, and the assessment to the work. This path retrieves the works reviewed by a selected `ripe:HumanReviewer`.

```sparql
PREFIX ripe:     <https://w3id.org/ripe/ripe-o#>
PREFIX tido:     <https://w3id.org/tido#>
PREFIX prov:     <http://www.w3.org/ns/prov#>
PREFIX prism:    <http://prismstandard.org/namespaces/basic/3.0/>
PREFIX dcterms:  <http://purl.org/dc/terms/>

SELECT DISTINCT ?assessment ?doi ?title WHERE {
  <https://w3id.org/ripe/ripe-kg/human-reviewer/RV014> a ripe:HumanReviewer .
  ?evaluation a tido:Evaluation ;
              prov:wasAssociatedWith <https://w3id.org/ripe/ripe-kg/human-reviewer/RV014> ;
              tido:contributesTo ?assessment .
  ?assessment ripe:assesses ?work .
  OPTIONAL { ?work prism:doi ?doi }
  OPTIONAL { ?work dcterms:title ?title }
}
ORDER BY ?doi ?assessment
LIMIT 3
```

| assessment | doi | title |
| --- | --- | --- |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA48CA011D332751A7 | 10.1007/s12160-014-9665-0 | Mindfulness Meditation Alleviates Fibromyalgia Symptoms in Women: Results of a Randomized Clinical Trial |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEAF7E1928294A86E83 | 10.1016/j.jacl.2015.12.017 | A randomized-controlled clinical trial investigating the effect of omega-3 fatty acids and vitamin E co-supplementation on markers of insulin metabolism and lipid profiles in gestational diabetes |
| https://w3id.org/ripe/ripe-kg/research-integrity-assessment/RIPEA84BEB5719212517B | 10.1016/j.pnpbp.2018.02.007 | The effects of vitamin D and probiotic co-supplementation on mental health parameters and metabolic status in type 2 diabetic patients with coronary heart disease: A randomized, double-blind, placebo-controlled trial |

The answer returns the assessment and publication metadata for the work connected to the selected reviewer through the evaluation provenance path.
