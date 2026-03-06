# Context Map: Attention-Deficit Domain

The context map provides a visual and conceptual representation of the relationships and interactions between the six bounded contexts in the ADHD management domain. It documents how each context communicates with others, which contexts are upstream or downstream, and what integration patterns govern their interactions.

## Purpose

ADHD management inherently requires coordination across clinical, educational, therapeutic, and pharmacological systems. The context map makes these integration relationships explicit, preventing hidden dependencies and ensuring that each team understands how their bounded context relates to others. Without a context map, the relationships between assessment, treatment, monitoring, accommodation, medication, and outcomes tracking remain implicit and fragile.

## Context Relationships

### Clinical Assessment Context (Upstream)

The Clinical Assessment Context serves as the primary upstream context, supplying diagnostic data to multiple downstream consumers:

- **Clinical Assessment -> Treatment Management**: Customer-Supplier relationship. Assessment results, including DSM-5 diagnostic codes, ADHD presentation type, comorbidity findings, and severity ratings, flow downstream to inform treatment planning. The Treatment Management Context is the customer requesting specific diagnostic data formats.
- **Clinical Assessment -> Medication Management**: Customer-Supplier relationship. Diagnostic confirmation and clinical recommendations flow to the medication context to initiate or adjust pharmacological treatment.
- **Clinical Assessment -> Educational Accommodation**: Published Language. Assessment reports are translated into educational terminology through a published language based on eligibility determination criteria.

### Treatment Management Context (Midstream)

The Treatment Management Context both consumes upstream data and produces data for downstream contexts:

- **Treatment Management -> Behavioral Monitoring**: Partnership. Treatment goals and behavioral targets defined in the treatment plan drive what the Behavioral Monitoring Context tracks. Both contexts collaborate on defining measurable behavioral objectives.
- **Treatment Management -> Outcomes Tracking**: Customer-Supplier. Treatment interventions and their timelines flow downstream so the Outcomes Tracking Context can correlate treatment changes with outcome measurements.

### Behavioral Monitoring Context (Supporting)

- **Behavioral Monitoring -> Treatment Management**: Conformist. Monitoring data conforms to the behavioral target definitions established by the Treatment Management Context.
- **Behavioral Monitoring -> Outcomes Tracking**: Published Language. Standardized symptom severity scores and behavioral frequency data are published in a shared measurement format.

### Educational Accommodation Context (Downstream with ACL)

- **Educational Accommodation <- Clinical Assessment**: Anti-Corruption Layer. Clinical terminology (DSM-5 codes, symptom severity levels) is translated into educational terminology (eligibility categories, functional limitations, accommodation needs) through an explicit translation layer.
- **Educational Accommodation <- Treatment Management**: Separate Ways with periodic sync. Educational accommodations operate largely independently of clinical treatment, with periodic alignment during IEP/504 review meetings.

### Medication Management Context (Downstream)

- **Medication Management <- Clinical Assessment**: Customer-Supplier. Medication decisions depend on diagnostic data from the assessment context.
- **Medication Management -> Behavioral Monitoring**: Open Host Service. Medication change events are published so the Behavioral Monitoring Context can correlate medication adjustments with behavioral changes.
- **Medication Management -> Outcomes Tracking**: Published Language. Medication trial data, including dosage changes and side effect reports, flows to outcomes tracking.

### Outcomes Tracking Context (Downstream Aggregator)

- **Outcomes Tracking <- All Contexts**: The Outcomes Tracking Context acts as an aggregating downstream consumer, receiving data from all other contexts through published language interfaces. It does not modify upstream data but correlates information across contexts for longitudinal analysis.

## Integration Patterns Summary

| Upstream | Downstream | Pattern |
|---|---|---|
| Clinical Assessment | Treatment Management | Customer-Supplier |
| Clinical Assessment | Medication Management | Customer-Supplier |
| Clinical Assessment | Educational Accommodation | Anti-Corruption Layer |
| Treatment Management | Behavioral Monitoring | Partnership |
| Treatment Management | Outcomes Tracking | Customer-Supplier |
| Behavioral Monitoring | Outcomes Tracking | Published Language |
| Medication Management | Behavioral Monitoring | Open Host Service |
| Medication Management | Outcomes Tracking | Published Language |

## Evolution Considerations

The context map evolves as ADHD management practices change. New bounded contexts may emerge (for example, a Telehealth Context or a Family Support Context), and existing relationships may shift as integration standards mature or regulatory requirements change.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
