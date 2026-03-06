# Research Methods Context

## Overview

The Research Methods Context is the bounded context responsible for the design, execution, analysis, and dissemination of psychological research. It encompasses experimental design, participant management, Institutional Review Board (IRB) protocol management, data collection, statistical analysis, and publication workflows. This context applies the scientific method to advance psychological knowledge and validate clinical practices.

Research methods in psychology span a broad range of approaches from randomized controlled trials and single-case experimental designs to qualitative studies and mixed-methods research. The domain model must accommodate this methodological diversity while enforcing the ethical and procedural standards that govern human subjects research.

## Core Aggregate: Research Study

The Research Study aggregate is the primary unit of consistency. It is rooted in the Research Study entity and contains Study Design value objects, Participant Enrollment entities, Data Collection entities, and Analysis Plan entities.

The Research Study aggregate enforces invariants including: data collection cannot begin until IRB approval is granted; participant enrollment requires documented informed consent; data analysis must follow the pre-registered analysis plan unless deviations are justified and documented; and all study modifications must be submitted for IRB review before implementation.

## Key Entities

Research Study: The root entity representing a complete research project from conceptualization through publication. It carries the study identifier, principal investigator, co-investigators, IRB protocol number, study status, and timeline.

Participant: An entity representing an individual enrolled in a study. It carries a de-identified research code (not the individual's clinical identifier), enrollment date, consent status, group assignment (for experimental designs), and data collection status. The Participant entity is distinct from the Client entity in clinical contexts.

Dataset: An entity representing the collected data for a study. It includes variable definitions, data points, data quality indicators, and provenance information tracking how data was collected and processed.

Manuscript: An entity representing a written report of research findings intended for publication. It tracks the manuscript status through drafting, internal review, journal submission, peer review, revision, and publication.

## Key Value Objects

Study Design: The methodological framework for the study, including the design type (between-subjects, within-subjects, mixed, single-case), the independent and dependent variables, control conditions, and randomization procedures.

Effect Size: A quantitative measure of the magnitude of a finding, expressed as Cohen's d, eta-squared, odds ratio, or other appropriate metric, with associated confidence interval.

Statistical Test Result: The outcome of a statistical analysis, including the test statistic, degrees of freedom, p-value, and effect size. This value object is shared with the Outcomes Measurement Context through a shared kernel.

IRB Protocol: The ethics-board-approved research plan specifying study procedures, risk assessment, informed consent procedures, data security measures, and adverse event reporting procedures.

Power Analysis: A calculation determining the sample size needed to detect a specified effect size with a given level of statistical power, informing study design decisions.

Inclusion Criteria: The set of requirements that participants must meet to be eligible for study enrollment, defined by demographic, diagnostic, and other relevant characteristics.

## Research Design Types

The context supports multiple research design paradigms.

Randomized Controlled Trials (RCTs): The gold standard for establishing treatment efficacy. Participants are randomly assigned to treatment and control conditions, and outcomes are compared between groups. The domain model tracks randomization, blinding, and intention-to-treat analysis.

Single-Case Experimental Designs: Designs that demonstrate experimental control within individual participants through systematic introduction and withdrawal of interventions. The model supports A-B, A-B-A-B, multiple baseline, and alternating treatment designs.

Longitudinal Studies: Designs that track variables over extended time periods. The model manages repeated measurement schedules, attrition tracking, and growth curve analysis.

Qualitative Research: Studies using interviews, focus groups, or observational methods to explore psychological phenomena. The model supports coding schemes, thematic analysis, and audit trail documentation.

## Domain Events

IRBApprovalGranted: The Institutional Review Board has approved the study protocol, enabling participant recruitment.

ParticipantEnrolled: A participant has completed informed consent and been enrolled in the study.

ParticipantWithdrawn: A participant has withdrawn from the study, triggering data handling procedures.

DataCollectionPointCompleted: A scheduled data collection event has been completed for a participant.

StudyDataCollectionCompleted: All planned data collection for the study is finished.

AnalysisCompleted: Statistical analysis has been completed and results are available for interpretation.

ManuscriptSubmitted: A research manuscript has been submitted to a journal for peer review.

## Ethical Safeguards

The domain model enforces ethical requirements at multiple levels. The IRB Protocol value object captures the approved procedures and constraints. The Participant entity tracks consent status and ensures that no data is collected from participants who have not provided informed consent or who have withdrawn. The Research Study aggregate prevents data collection from proceeding without active IRB approval.

## Integration with Other Contexts

The Research Methods Context consumes assessment instruments and normative data from the Psychological Assessment Context and Cognitive Assessment Context when studies use standardized measures. It shares statistical analysis value objects with the Outcomes Measurement Context through a shared kernel. Research findings may inform treatment protocol updates in the Therapeutic Intervention Context.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Kazdin, A. E. (2017). Research Design in Clinical Psychology (5th ed.). Cambridge University Press.
- American Psychological Association. (2020). Publication Manual of the American Psychological Association (7th ed.).
