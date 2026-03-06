# Urology Standards

## Overview

Domain-specific standards inform the design of value objects, published languages, and business rules throughout the urology domain model. Urology is governed by a rich set of clinical standards from professional organizations, staging systems from oncologic bodies, and laboratory standards from diagnostic societies. These standards shape how the domain model represents clinical data and how business rules enforce guideline-concordant care.

## American Urological Association (AUA) Guidelines

The AUA publishes evidence-based clinical practice guidelines that directly inform the urology domain model's business rules. Key guidelines include:

### Prostate Cancer Early Detection

The AUA/SUO guideline on prostate cancer early detection defines PSA screening recommendations by age and risk group. The domain model implements these recommendations in the Clinical Assessment Context's screening protocol, defining when PSA testing is offered, how shared decision-making is documented, and when biopsy is recommended based on PSA level and risk factors.

### Surgical Management of Stones

The AUA/Endourology Society guideline on surgical management of stones defines the evidence-based criteria for ESWL, ureteroscopy, and PCNL based on stone size, location, and composition. The Stone Disease Context's ProcedureSelectionService implements these criteria directly, ensuring that treatment recommendations align with current evidence.

### Overactive Bladder

The AUA/SUFU guideline on overactive bladder defines the treatment escalation pathway from behavioral therapies through pharmacotherapy to neuromodulation and surgical intervention. The Incontinence Management Context's TreatmentEscalationService implements this guideline, enforcing the requirement for documented failure at each level before escalation.

## European Association of Urology (EAU) Guidelines

The EAU publishes comprehensive guidelines that complement AUA recommendations and provide European perspective on urological care. The domain model references EAU guidelines where they provide additional granularity, particularly in oncologic staging, active surveillance protocols, and stone disease management. Where AUA and EAU guidelines differ, the domain model supports configuration to follow institutional preference.

## AJCC TNM Staging System

The American Joint Committee on Cancer (AJCC) TNM staging system is the standard for classifying the anatomic extent of urological malignancies. The Oncologic Urology Context conforms to this standard without modification.

### Prostate Cancer Staging

The TNM staging for prostate cancer defines T stages from T1 (clinically inapparent) through T4 (invasion of adjacent structures). The domain model's TNMStage value object enforces valid T, N, and M combinations and derives the overall prognostic stage group according to the AJCC 8th edition rules, which incorporate PSA level and Grade Group.

### Bladder Cancer Staging

Bladder cancer TNM staging distinguishes Ta (non-invasive papillary), Tis (carcinoma in situ), T1 (lamina propria invasion), T2 (muscularis propria invasion), T3 (perivesical tissue invasion), and T4 (adjacent organ invasion). The domain model uses this staging to determine the NMIBC versus MIBC classification that drives fundamentally different treatment pathways.

### Kidney Cancer Staging

Kidney cancer staging follows size-based T classification: T1a (up to 4 cm), T1b (4-7 cm), T2a (7-10 cm), T2b (greater than 10 cm), T3 (renal vein or perinephric invasion), T4 (beyond Gerota fascia). The domain model uses this staging along with Fuhrman/WHO-ISUP nuclear grade for prognostic grouping.

## Gleason Grading and ISUP Grade Groups

The Gleason grading system for prostate cancer was updated by the 2014 International Society of Urological Pathology (ISUP) consensus conference. The GleasonScore value object in the Oncologic Context implements both the traditional Gleason sum (6-10) and the modern ISUP Grade Group classification (1-5). Grade Group 1 corresponds to Gleason 3+3=6, Grade Group 2 to Gleason 3+4=7, Grade Group 3 to Gleason 4+3=7, Grade Group 4 to Gleason 8, and Grade Group 5 to Gleason 9-10.

## WHO Semen Analysis Standards

The World Health Organization Laboratory Manual for the Examination and Processing of Human Semen (6th edition, 2021) defines reference values for semen analysis parameters. The Male Reproductive Health Context's SemenParameters value object uses WHO reference ranges: volume greater than 1.5 mL, concentration greater than 16 million/mL, total motility greater than 42%, progressive motility greater than 30%, and normal morphology greater than 4% by strict criteria.

## International Continence Society (ICS) Standards

The ICS standardization of terminology for lower urinary tract function defines the vocabulary used in the Clinical Assessment and Incontinence Management contexts. Terms such as "detrusor overactivity," "bladder compliance," "maximum cystometric capacity," and "Valsalva leak point pressure" are defined by ICS standards and used precisely within the domain model.

## Clavien-Dindo Classification

The Clavien-Dindo classification system standardizes the reporting of surgical complications. The Surgical Management Context uses this system to classify all post-operative complications: Grade I (any deviation without pharmacological or surgical intervention), Grade II (requiring pharmacological treatment), Grade III (requiring surgical, endoscopic, or radiological intervention), Grade IV (life-threatening), and Grade V (death). This standardization enables meaningful comparison of surgical outcomes across procedures and institutions.

## NCCN Guidelines

The National Comprehensive Cancer Network (NCCN) guidelines provide detailed treatment algorithms for urological cancers. The Oncologic Context implements NCCN risk groups for prostate cancer (very low, low, favorable intermediate, unfavorable intermediate, high, very high, metastatic) and references NCCN treatment pathways for bladder and kidney cancer management decisions.

## Laboratory Reference Standards

Clinical laboratory standards (CLIA, CAP) govern how urological laboratory tests are performed, reported, and interpreted. The domain model's anti-corruption layers translate laboratory results according to these standards, ensuring that reference ranges, measurement units, and quality indicators are consistently applied across all bounded contexts that consume laboratory data.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Urological Association. Clinical Practice Guidelines. https://www.auanet.org/guidelines
- European Association of Urology. EAU Guidelines. https://uroweb.org/guidelines
- Amin, M.B. et al. AJCC Cancer Staging Manual. 8th Edition. Springer, 2017.
- Epstein, J.I. et al. "The 2014 ISUP Gleason Grading Conference." American Journal of Surgical Pathology, 2016.
- World Health Organization. WHO Laboratory Manual for the Examination and Processing of Human Semen. 6th Edition, 2021.
