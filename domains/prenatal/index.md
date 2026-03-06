# Prenatal Domain - Domain-Driven Design

## Introduction

The prenatal domain applies Domain-Driven Design to the field of prenatal care, which encompasses all clinical services provided to expectant mothers and their developing fetuses from confirmation of pregnancy through the onset of labor. Prenatal care is a longitudinal medical discipline organized around gestational age, where the timing of assessments, screenings, and interventions follows evidence-based schedules defined by organizations such as the American College of Obstetricians and Gynecologists (ACOG) and the World Health Organization (WHO). The domain must model the interplay between maternal health and fetal development as two interdependent but distinct clinical concerns.

The domain is organized into six bounded contexts that reflect the natural divisions within prenatal care delivery: maternal health monitoring, fetal development tracking, prenatal screening and diagnostics, birth planning, high-risk pregnancy management, and prenatal education and counseling. Each bounded context maintains its own internal consistency while communicating with other contexts through well-defined integration patterns and domain events, enabling a cohesive model that supports both routine and high-risk pregnancy pathways.

## Bounded Contexts

1. **Maternal Health Monitoring Context** - Manages the ongoing assessment of the expectant mother's health throughout pregnancy. This includes vital signs tracking, weight gain management, gestational diabetes screening, preeclampsia detection, and blood panel analysis. The maternal health record serves as the primary aggregate, accumulating clinical observations organized by gestational age and trimester.

2. **Fetal Development Tracking Context** - Tracks the growth and well-being of the developing fetus using ultrasound measurements, growth curves, biophysical profiles, and fetal movement counting. Gestational age estimation and fetal biometry are central value objects that inform clinical decision-making about growth adequacy and developmental milestones.

3. **Prenatal Screening and Diagnostics Context** - Manages the ordering, execution, and interpretation of prenatal tests including first-trimester screening, cell-free DNA testing, quad screening, amniocentesis, and chorionic villus sampling. This context encodes the clinical significance thresholds and risk calculation algorithms that determine whether results are normal, screen-positive, or diagnostic.

4. **Birth Planning Context** - Captures maternal preferences for labor and delivery, assesses delivery method readiness, and coordinates hospital pre-registration. Birth plan documents are modeled as value objects reflecting a point-in-time snapshot of preferences, while delivery readiness assessments track evolving clinical indicators.

5. **High-Risk Pregnancy Management Context** - Handles risk stratification, complication management, specialist referral coordination, and maternal-fetal medicine consultations. This context activates when domain events from other contexts indicate elevated risk factors, applying specialized protocols for conditions such as preeclampsia, placenta previa, or multiple gestation.

6. **Prenatal Education and Counseling Context** - Manages childbirth education programs, breastfeeding preparation, nutritional counseling, genetic counseling sessions, and mental health screening. This supporting context tracks patient engagement and education completion to inform care coordination.

## Strategic Design

- The prenatal domain is classified as a core subdomain where the pregnancy journey, organized by gestational age, serves as the central organizing concept for all clinical workflows and screening schedules.
- Context mapping uses a customer-supplier relationship between the Maternal Health Monitoring and High-Risk Pregnancy Management contexts, where abnormal findings in maternal assessments trigger referral workflows in the high-risk context.
- Anti-corruption layers translate data from external laboratory information systems and imaging platforms into the domain's ubiquitous language for screening results and fetal measurements.

## Tactical Design

- **Pregnancy Aggregate** - The root aggregate representing an individual pregnancy, maintaining the gestational timeline, associated care episodes, and the current risk classification.
- **Gestational Age Value Object** - An immutable calculation derived from the estimated date of delivery, representing weeks and days of pregnancy and governing the timing of all scheduled screenings and assessments.
- **Screening Result Value Object** - Encapsulates test outcomes with clinical significance thresholds (e.g., screen-positive, screen-negative, inconclusive) and associated risk ratios.
- **High-Risk Identified Domain Event** - Raised when a risk assessment crosses a predefined threshold, triggering referral to maternal-fetal medicine and activation of specialized monitoring protocols.
- **Fetal Biometry Entity** - A measured set of fetal dimensions (biparietal diameter, femur length, abdominal circumference) taken at a specific gestational age, tracked over time to assess growth trajectory.
- **Screening Schedule Domain Service** - Generates the personalized schedule of prenatal screenings and assessments based on gestational age, risk factors, and prior test results.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- American College of Obstetricians and Gynecologists (ACOG). *Guidelines for Perinatal Care*.
- Gabbe, S. G. et al. (2020). *Obstetrics: Normal and Problem Pregnancies*. Elsevier.
- National Institute for Health and Care Excellence (NICE). *Antenatal Care Guidelines*.
