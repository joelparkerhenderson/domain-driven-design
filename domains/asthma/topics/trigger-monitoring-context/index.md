# Trigger Monitoring Context - Asthma Domain

## Overview

The Trigger Monitoring Context is responsible for tracking, recording, and analyzing environmental, allergen, and occupational factors that provoke asthma symptoms and exacerbations. This bounded context owns the models for allergen sensitivity profiles, air quality monitoring, environmental factor tracking, occupational exposure assessment, and trigger-exacerbation correlation analysis. It is classified as a supporting subdomain.

Trigger identification and avoidance is a foundational element of asthma management. While pharmacotherapy treats symptoms and inflammation, trigger monitoring enables proactive prevention by alerting patients and clinicians to high-risk environmental conditions and by establishing causal relationships between specific exposures and symptom worsening.

## Key Concepts

**Allergen Tracking** maintains a patient-specific profile of allergen sensitivities confirmed through clinical testing (skin prick tests, specific IgE blood tests) or clinical history. Common indoor allergens include dust mites, mold spores, pet dander (cat, dog), and cockroach allergen. Common outdoor allergens include tree pollen, grass pollen, weed pollen (ragweed), and mold spores. The trigger profile records sensitivity level (mild, moderate, severe), confirmation method, and seasonal patterns for each identified allergen.

**Air Quality Monitoring** tracks outdoor air pollution levels that affect respiratory health. The Air Quality Index (AQI) is a standardized 0-500 scale used by the EPA and international agencies. Key pollutants affecting asthma include particulate matter (PM2.5, PM10), ozone (O3), nitrogen dioxide (NO2), and sulfur dioxide (SO2). AQI categories range from Good (0-50) through Hazardous (301-500). Patients with asthma are in the sensitive group, meaning health effects begin at the Moderate level (AQI 51-100).

**Environmental Factors** encompass non-allergen triggers including weather changes (cold air, humidity, barometric pressure shifts), tobacco smoke (secondhand and thirdhand), strong odors (perfumes, cleaning chemicals), viral respiratory infections, exercise in cold or dry air, gastroesophageal reflux (GERD), and emotional stress. These factors are tracked through patient-reported logs and environmental sensor data.

**Occupational Exposures** are workplace-related triggers that cause or worsen asthma. Occupational asthma is classified as sensitizer-induced (immunological, with a latency period) or irritant-induced (non-immunological, including reactive airways dysfunction syndrome). Common occupational sensitizers include isocyanates, flour dust, wood dust, laboratory animal proteins, latex, and glutaraldehyde. Assessment includes workplace exposure inventory, temporal relationship between work and symptoms, and peak flow monitoring during work periods versus off-work periods.

## Aggregates

### TriggerProfile

The patient-specific aggregate containing all identified allergen sensitivities, environmental factor sensitivities, and trigger categories. Enforces uniqueness of allergen entries and validates allergen codes against standardized taxonomies.

### EnvironmentalExposure

Records environmental conditions at a specific time and location relevant to a patient. Contains air quality readings, pollen counts, weather data, and exposure duration. Validates that readings fall within plausible ranges.

### OccupationalAssessment

Encapsulates a comprehensive workplace exposure evaluation including identified sensitizers, exposure patterns, protective measures, and clinical recommendations. Supports longitudinal tracking of occupational asthma across job changes.

## Entities

- **AllergenSensitivity:** A specific allergen within a trigger profile with sensitivity level, confirmation method, and date.
- **OccupationalAssessment:** A workplace evaluation with identified exposures, protective measures, and recommendations.
- **ExposureEvent:** A discrete exposure incident with trigger type, duration, intensity, and associated symptoms.

## Value Objects

- **AirQualityReading:** AQI value, pollutant breakdown, category, geographic location, timestamp.
- **PollenReading:** Pollen type, count (grains per cubic meter), severity level, geographic location, date.
- **TriggerCategory:** Classification of trigger type (allergen, irritant, weather, occupational, exercise, infection, emotional).
- **SensitivityLevel:** Degree of sensitivity (mild, moderate, severe) for a specific trigger.
- **GeographicLocation:** Latitude, longitude, and descriptive location for environmental readings.
- **ExposureDuration:** Start time, end time, and calculated duration of an exposure event.

## Domain Events Published

- **HighRiskExposureDetected:** Published when environmental conditions exceed risk thresholds for a patient based on their trigger profile. Consumed by Action Planning Context for precautionary measures.
- **TriggerProfileUpdated:** Published when allergen sensitivities are added, removed, or modified. Consumed by Action Planning and Treatment Management for recommendation updates.
- **OccupationalExposureIdentified:** Published when a new occupational trigger is confirmed. Consumed by Treatment Management for occupational asthma management planning.

## Domain Events Consumed

- **ExacerbationRecorded (from Outcomes Tracking):** Used to correlate exacerbation events with environmental conditions at the time of symptom onset.
- **AssessmentFinalized (from Clinical Assessment):** Allergy test results within assessments may update the trigger profile.

## Domain Services

- **TriggerCorrelationService:** Analyzes temporal correlations between recorded trigger exposures and exacerbation events. Calculates correlation strength and identifies the most impactful triggers for each patient.
- **ExposureRiskAssessmentService:** Evaluates current environmental conditions against a patient's trigger profile to determine real-time risk level and generate alerts.

## Integration Points

- **ACL to Air Quality APIs:** Translates responses from EPA AirNow, BreezoMeter, IQAir, and other air quality data providers into standardized AirQualityReading value objects.
- **ACL to Pollen Data Services:** Translates pollen count data from providers like Pollen.com, AAAAI NAB, and regional pollen networks into standardized PollenReading value objects.
- **ACL to Weather Services:** Translates weather data relevant to asthma triggers (temperature, humidity, barometric pressure, wind speed) from meteorological services.
- **Upstream to Action Planning:** Environmental alerts inform precautionary action recommendations. Customer-supplier relationship.
- **Published Language to Outcomes Tracking:** Exposure data published in standardized format for trigger-exacerbation correlation analysis.

## Monitoring Patterns

**Continuous Environmental Monitoring.** Air quality and pollen data are updated at regular intervals (hourly for AQI, daily for pollen counts). The context maintains current readings for patient-relevant geographic locations and publishes alerts when thresholds are exceeded.

**Patient-Reported Trigger Logging.** Patients log trigger exposure events through self-reporting mechanisms. The context captures exposure type, duration, intensity, and any immediate symptoms to build a comprehensive exposure history.

**Occupational Monitoring Protocols.** Serial peak flow measurements during work and away-from-work periods establish occupational exposure patterns. The context supports structured occupational asthma investigation protocols including workplace visit assessments.

**Seasonal Pattern Analysis.** The context tracks trigger exposure patterns across seasons, identifying predictable high-risk periods (spring pollen, fall ragweed, winter cold air, summer ozone) for proactive management planning.

## Business Rules

1. Trigger profile allergen entries must reference validated allergen codes from standardized taxonomies.
2. Air quality alerts are generated when AQI exceeds 100 (Unhealthy for Sensitive Groups) for patients with identified air quality sensitivity.
3. Pollen alerts are generated based on patient-specific allergen sensitivities, not generic pollen counts.
4. Occupational exposure assessments require documentation of temporal relationship between work exposure and symptom patterns.
5. Trigger-exacerbation correlations require a minimum number of data points before being classified as confirmed associations.
6. Environmental data from external sources must be validated for completeness and plausibility before incorporation.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023. Chapter on risk factors and trigger management.
- Tarlo et al. Diagnosis and Management of Work-Related Asthma. Chest, 2008.
- EPA AirNow. Air Quality Index Basics. https://www.airnow.gov/aqi/aqi-basics/
