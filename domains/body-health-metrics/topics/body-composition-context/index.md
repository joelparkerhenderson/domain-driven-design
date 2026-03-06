# Body Composition Context

## Overview

The Body Composition Context analyzes and interprets raw biometric measurements to produce comprehensive body composition profiles. It transforms basic weight and measurement data into a detailed breakdown of fat mass, lean mass, bone mineral density, hydration levels, and metabolic rate estimates. This context applies established analytical methodologies from exercise physiology and clinical nutrition to derive composition insights that inform health scoring and progress tracking.

## Scope and Responsibilities

This context manages the following composition components:

- **Lean Mass**: Non-fat body tissue including skeletal muscle, organs, connective tissue, and body water
- **Fat Mass**: Total adipose tissue including subcutaneous, visceral, and intramuscular fat deposits
- **Bone Mineral Density (BMD)**: Mineral content per unit area of bone, indicating skeletal health
- **Hydration Level**: Total body water as a percentage of body weight, reflecting fluid balance
- **Basal Metabolic Rate (BMR)**: Estimated daily caloric expenditure at rest, derived from composition and demographic data

## Analysis Methodologies

The context supports multiple composition analysis methods, each with different input requirements and accuracy profiles:

- **Bioelectrical Impedance Analysis (BIA)**: Uses resistance and reactance measurements from impedance devices. Requires body weight, height, age, and sex. Moderate accuracy; widely accessible.
- **DEXA Scan Interpretation**: Processes dual-energy X-ray absorptiometry results providing direct measurement of bone, lean, and fat tissue. Highest accuracy for clinical applications.
- **Skinfold Caliper Estimation**: Applies regression equations to skinfold thickness measurements at standardized body sites. Requires trained administrator. Moderate accuracy.
- **Hydrostatic Weighing Derivation**: Interprets underwater weighing data using body density calculations. High accuracy but limited availability.

## Aggregate Design

### CompositionProfile (Aggregate Root)

The CompositionProfile aggregate represents the complete body composition analysis for an individual at a specific point in time.

**Components**:
- FatMass (value object): absolute weight and percentage of total body mass
- LeanMass (value object): absolute weight and percentage of total body mass
- BoneMineralDensity (value object): density value with T-score and Z-score
- HydrationLevel (value object): total body water percentage
- BasalMetabolicRate (value object): estimated daily caloric expenditure at rest
- AnalysisMethod (value object): the methodology used for this composition analysis
- SourceMeasurements: references to the biometric measurements used as inputs

**Invariants**:
- Fat mass percentage plus lean mass percentage must equal 100% within rounding tolerance
- Fat mass absolute value plus lean mass absolute value must equal total body weight within measurement precision
- Hydration level must fall between 35% and 75% of total body weight
- BMR must be positive and within physiologically plausible range (500-5000 kcal/day)
- A complete profile requires all five components; partial analyses are modeled separately

## Domain Events Published

- **CompositionProfileUpdated**: Raised when a new composition analysis is completed, carrying the full profile data for downstream consumption
- **MetabolicRateCalculated**: Raised when BMR is estimated, providing metabolic baseline data for health scoring
- **BoneDensityThresholdCrossed**: Raised when bone mineral density crosses clinical thresholds (osteopenia or osteoporosis T-score boundaries)
- **VisceralFatAlertRaised**: Raised when visceral fat measurements exceed health risk thresholds

## Domain Services

- **BodyCompositionAnalysisService**: Coordinates the analysis process, selecting appropriate equations for the chosen methodology and validating input completeness
- **BasalMetabolicRateService**: Applies Mifflin-St Jeor, Harris-Benedict, or Katch-McArdle equations based on available composition and demographic data
- **MetabolicAgeService**: Estimates biological metabolic age by comparing individual BMR to population age-group averages

## Integration Points

- **Upstream**: Consumes BiometricMeasurementRecorded events from Biometric Collection Context to obtain raw measurement inputs
- **Downstream**: Publishes CompositionProfileUpdated events consumed by Health Scoring Context and Progress Tracking Context

## Ubiquitous Language (Context-Specific)

- **Composition Profile**: The complete breakdown of body mass into its constituent tissue types at a point in time
- **Fat-Free Mass**: All body tissue that is not adipose, equivalent to lean mass in most analytical frameworks
- **Visceral Adiposity**: Fat stored within the abdominal cavity surrounding internal organs, distinct from subcutaneous fat
- **Skeletal Muscle Mass**: The portion of lean mass specifically attributable to voluntary skeletal muscle tissue
- **Metabolic Age**: An estimated biological age derived by comparing individual BMR to average BMR values across chronological age groups
- **Body Water Compartments**: The distribution of total body water between intracellular and extracellular spaces

## Clinical Significance

Body composition data carries significant clinical implications. A person with normal BMI may have elevated body fat percentage and reduced lean mass (sometimes called "normal weight obesity"), which carries metabolic risk that BMI alone does not detect. Bone mineral density measurements identify osteoporosis risk. Visceral fat levels predict cardiovascular and metabolic disease risk independent of total body fat. The Body Composition Context captures these clinically meaningful distinctions that simpler biometric measures miss.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Heymsfield, S.B. et al. *Human Body Composition*. 2nd ed. Human Kinetics, 2005.
- Mifflin, M.D. et al. "A new predictive equation for resting energy expenditure in healthy individuals." *American Journal of Clinical Nutrition*, 51(2), 1990.
