# Cardiac Analysis Context

The Cardiac Analysis Context is the bounded context responsible for processing normalized cardiac data to extract clinically meaningful insights. It performs rhythm classification, heart rate variability computation, blood pressure trend analysis, QRS morphology assessment, and anomaly detection. This context embodies the core clinical intelligence of the heart health metrics system.

## Overview

The Cardiac Analysis Context receives standardized measurement data from the Data Collection Context and applies signal processing algorithms, pattern recognition, and clinical classification logic to produce derived cardiac findings. It is classified as a core subdomain because the quality and sophistication of its analysis algorithms directly determine the clinical value of the entire cardiac monitoring system.

This context maintains its own domain model optimized for signal analysis and clinical classification. Its ubiquitous language includes terms from computational cardiology, signal processing, and clinical electrophysiology. Domain experts for this context include computational cardiologists, cardiac electrophysiologists, and biomedical signal processing engineers.

## Key Concepts

**Rhythm Analysis.** The context analyzes ECG tracings to classify cardiac rhythms. It identifies normal sinus rhythm, atrial fibrillation, atrial flutter, supraventricular tachycardia, ventricular tachycardia, and other rhythm types. Analysis considers rate, regularity, P wave morphology, QRS morphology, and P-QRS relationship.

**Heart Rate Variability (HRV) Computation.** The context computes HRV metrics from R-R interval series extracted from ECG or PPG data. It calculates time-domain statistics (SDNN, RMSSD, pNN50), frequency-domain measures (VLF, LF, HF power spectral components), and nonlinear measures (sample entropy, Poincare plot indices). HRV analysis follows the standards defined by the European Society of Cardiology Task Force (1996).

**Blood Pressure Trend Analysis.** The context analyzes sequences of blood pressure readings to identify patterns: sustained hypertension, white coat hypertension, masked hypertension, nocturnal dipping or non-dipping, and morning surge. It applies ACC/AHA staging criteria and produces classification results.

**QRS Morphology Assessment.** The context evaluates QRS complex shape, duration, and axis to detect conduction abnormalities such as left or right bundle branch block, fascicular blocks, and pre-excitation patterns. Morphology assessment supports rhythm classification and identifies structural conduction disease.

**Anomaly Detection.** The context identifies measurements that deviate significantly from a patient's established baseline or from population norms. Anomalies include sudden heart rate changes, arrhythmia onset, blood pressure spikes, QT prolongation, and ST segment changes. Detected anomalies generate domain events for downstream contexts.

**ECG Interpretation Pipeline.** The context implements a multi-stage processing pipeline: signal preprocessing (filtering, baseline correction), feature extraction (wave detection, interval measurement), pattern classification (rhythm typing, morphology categorization), and clinical finding generation.

## Domain Examples

A 24-hour Holter monitor recording enters the Cardiac Analysis Context after normalization by the Data Collection Context. The rhythm analysis pipeline segments the recording into episodes, identifying 20 hours of normal sinus rhythm with an average rate of 68 bpm, three episodes of paroxysmal atrial fibrillation totaling 3.5 hours, and 30 minutes of sinus bradycardia during deep sleep. Each finding is represented as a Rhythm Classification value object with onset time, duration, type, and confidence score.

A patient's home blood pressure series of 28 readings over 7 days enters the BP Trend Analysis pipeline. The analysis identifies average morning BP of 148/92 mmHg and average evening BP of 132/82 mmHg, classifying the patient as having Stage 2 hypertension with a morning surge pattern and adequate nocturnal dipping.

## Relationships to Other Topics

- [Data Collection Context](../data-collection-context/index.md) - Upstream provider of normalized measurement data.
- [Risk Assessment Context](../risk-assessment-context/index.md) - Downstream consumer of analysis results for risk scoring.
- [Monitoring & Alerts Context](../monitoring-alerts-context/index.md) - Downstream consumer of anomaly findings.
- [Domain Services](../domain-services/index.md) - Rhythm Classification and HRV Computation are key services.
- [Value Objects](../value-objects/index.md) - Produces QRS Complex, HRV Metrics, and Rhythm Classification value objects.
- [Subdomains](../subdomains/index.md) - Classified as a core subdomain.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021.
- Task Force of the European Society of Cardiology. "Heart Rate Variability: Standards of Measurement, Physiological Interpretation, and Clinical Use." *Circulation*, 93(5), 1043-1065, 1996.
- Goldberger, A.L. et al. "PhysioBank, PhysioToolkit, and PhysioNet: Components of a New Research Resource." *Circulation*, 101(23), e215-e220, 2000.
- Clifford, G.D. et al. *Advanced Methods and Tools for ECG Data Analysis*. Artech House, 2006.
