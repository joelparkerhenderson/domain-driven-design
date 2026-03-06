# Neuromuscular Disorders Context - Neurology Domain

## Overview

The Neuromuscular Disorders Context is a supporting bounded context that addresses the diagnostic workup and management of disorders affecting peripheral nerves, neuromuscular junctions, and skeletal muscles. This context coordinates complex diagnostic pathways that integrate clinical findings, electrodiagnostic testing, laboratory analysis, and genetic testing to arrive at precise diagnoses and inform treatment decisions.

Neuromuscular disorders present unique modeling challenges because diagnosis often requires integrating data from multiple sources over time. A patient may progress through clinical evaluation, electrodiagnostic studies, laboratory tests, muscle biopsy, and genetic analysis before a definitive diagnosis is established.

## Scope and Responsibilities

### Myopathy Evaluation

**Inflammatory Myopathies**
- Dermatomyositis: characteristic rash (heliotrope, Gottron's papules), proximal weakness, elevated CK.
- Polymyositis: proximal weakness without rash, elevated CK, myopathic EMG.
- Inclusion body myositis (IBM): asymmetric weakness affecting finger flexors and knee extensors, slowly progressive, poor response to immunotherapy.
- Necrotizing autoimmune myopathy: severe proximal weakness, very high CK, anti-HMGCR or anti-SRP antibodies.
- Myositis-specific antibody panel interpretation.

**Metabolic Myopathies**
- Glycogen storage diseases: McArdle disease (myophosphorylase deficiency), Pompe disease (acid maltase deficiency).
- Mitochondrial myopathies: progressive external ophthalmoplegia (PEO), MELAS, MERRF.
- Lipid storage myopathies: carnitine deficiency, CPT II deficiency.

**Genetic Myopathies**
- Muscular dystrophies: Duchenne, Becker, facioscapulohumeral (FSHD), limb-girdle (LGMD), myotonic dystrophy types 1 and 2.
- Congenital myopathies: nemaline myopathy, centronuclear myopathy, central core disease.

### Neuropathy Classification and Workup

**By Distribution**
- Mononeuropathy: single nerve involvement (carpal tunnel, ulnar neuropathy, peroneal neuropathy).
- Mononeuropathy multiplex: multiple individual nerves involved (vasculitis, diabetes, sarcoidosis).
- Polyneuropathy: length-dependent symmetric involvement (diabetic, toxic, hereditary).
- Polyradiculoneuropathy: nerve root and peripheral nerve involvement (GBS, CIDP).

**By Pathology**
- Axonal: reduced amplitudes on NCS with preserved conduction velocities.
- Demyelinating: prolonged distal latencies, slowed conduction velocities, conduction block, temporal dispersion.
- Mixed: features of both axonal and demyelinating pathology.

**Specific Neuropathies**
- Guillain-Barre syndrome (GBS): acute inflammatory demyelinating polyradiculoneuropathy, treatment with IVIG or plasmapheresis.
- Chronic inflammatory demyelinating polyradiculoneuropathy (CIDP): chronic relapsing or progressive, treatment with IVIG, corticosteroids, or plasmapheresis.
- Hereditary neuropathies: Charcot-Marie-Tooth (CMT) disease types 1, 2, X-linked.
- Small fiber neuropathy: pain and autonomic dysfunction with normal standard NCS, diagnosed by skin biopsy (IENFD).

### Neuromuscular Junction Disorders

**Myasthenia Gravis (MG)**
- Antibody testing: anti-AChR, anti-MuSK, anti-LRP4.
- Edrophonium (Tensilon) test and ice pack test.
- Thymoma screening with chest CT.
- Treatment: pyridostigmine, immunosuppression (prednisone, azathioprine, mycophenolate), thymectomy, IVIG, plasmapheresis, complement inhibitors (eculizumab, ravulizumab), FcRn inhibitors (efgartigimod, rozanolixizumab).
- Myasthenic crisis management: respiratory monitoring, ICU care.

**Lambert-Eaton Myasthenic Syndrome (LEMS)**
- Anti-VGCC antibodies.
- Paraneoplastic workup (small cell lung cancer).
- NCS findings: low CMAP amplitudes with incremental response on rapid repetitive stimulation.
- Treatment: 3,4-diaminopyridine, immunosuppression.

### Electrodiagnostic Testing Coordination

This context coordinates electrodiagnostic studies with the Neuroimaging Context:
- Defining clinical questions that the EMG/NCS should answer.
- Selecting appropriate nerve and muscle study targets based on clinical presentation.
- Interpreting results in the context of the clinical phenotype.
- Requesting additional studies if initial results are inconclusive.

### Genetic Testing Workflow

- Panel selection based on clinical phenotype (muscular dystrophy panel, CMT panel, hereditary neuropathy panel).
- Whole exome sequencing for undiagnosed cases.
- Variant interpretation using ACMG guidelines (pathogenic, likely pathogenic, variant of uncertain significance, likely benign, benign).
- Genetic counseling coordination for confirmed genetic diagnoses.
- Family screening recommendations for hereditary conditions.

## Aggregate Design

**DiagnosticWorkup** is the primary aggregate root, coordinating the multi-step diagnostic process. It enforces the invariant that a diagnostic conclusion requires at least clinical findings and one confirmatory test result.

## Domain Events

- DiagnosticWorkupCompleted: triggers treatment planning.
- GeneticTestResultReceived: updates diagnostic assessment and triggers genetic counseling.
- ElectrodiagnosticStudyCompleted: provides key data for pattern classification.
- TreatmentResponseEvaluated: informs ongoing management decisions.

## Integration Points

- Receives electrodiagnostic results from Neuroimaging Context (customer-supplier).
- Receives genetic test results through laboratory system anti-corruption layer.
- Publishes diagnostic conclusions to Neurorehabilitation Context for therapy planning.
- References Clinical Assessment Context findings for clinical correlation.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Preston, David C., and Shapiro, Barbara E. *Electromyography and Neuromuscular Disorders*. 4th Edition. Elsevier, 2020.
- Gilhus, Nils Erik, et al. "Myasthenia Gravis." *Nature Reviews Disease Primers*, 2019.
- Barohn, Richard J., and Amato, Anthony A. "Pattern-Recognition Approach to Neuropathy and Neuronopathy." *Neurologic Clinics*, 2013.
