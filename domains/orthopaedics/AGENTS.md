# Orthopaedics Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to orthopaedic systems, encompassing musculoskeletal assessment, fracture management, joint replacement surgery, sports medicine, spinal disorders, and orthopaedic rehabilitation. Orthopaedics is the medical specialty devoted to the diagnosis, treatment, and prevention of disorders of the bones, joints, ligaments, tendons, muscles, and nerves that comprise the musculoskeletal system.

## Bounded Contexts

1. Musculoskeletal Assessment Context - Clinical evaluation of musculoskeletal conditions including history taking, physical examination, diagnostic imaging interpretation (radiography, MRI, CT), laboratory studies, and differential diagnosis for bone, joint, and soft tissue disorders.
2. Fracture Management Context - Classification, reduction, fixation, and follow-up of bone fractures, including closed reduction and casting, open reduction and internal fixation (ORIF), external fixation, and fracture healing monitoring with complication surveillance.
3. Joint Replacement Context - End-to-end management of arthroplasty procedures including patient selection, pre-operative planning, implant selection, surgical execution, post-operative care, and long-term implant surveillance for hip, knee, shoulder, and other joint replacements.
4. Sports Medicine Context - Diagnosis and treatment of athletic injuries and activity-related musculoskeletal conditions, including ligament reconstruction, meniscal repair, tendon repair, arthroscopic surgery, return-to-sport protocols, and injury prevention programs.
5. Spinal Disorders Context - Evaluation and management of conditions affecting the cervical, thoracic, and lumbar spine, including degenerative disc disease, spinal stenosis, scoliosis, herniated disc, spinal fusion, and decompression procedures.
6. Orthopaedic Rehabilitation Context - Post-surgical and post-injury rehabilitation protocols, including range of motion restoration, strengthening programs, weight-bearing progression, functional milestone tracking, and return-to-activity criteria.

## Domain Principles

- Model using shared ubiquitous language between orthopaedic surgeons, sports medicine physicians, physiotherapists, orthotists, radiologists, and software developers.
- Group the system into bounded contexts reflecting the distinct clinical workflows of assessment, fracture care, arthroplasty, sports medicine, spinal surgery, and rehabilitation.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow clinical practice guidelines from the American Academy of Orthopaedic Surgeons (AAOS), British Orthopaedic Association (BOA), and relevant subspecialty societies.
- Maintain patient safety, surgical outcomes tracking, and evidence-based practice as cross-cutting concerns.

## Key Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- musculoskeletal-assessment-context
- fracture-management-context
- joint-replacement-context
- sports-medicine-context
- spinal-disorders-context
- orthopaedic-rehabilitation-context
- event-driven-architecture
- integration-patterns
- orthopaedic-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Canale, S. Terry and Beaty, James H. Campbell's Operative Orthopaedics. Elsevier, 2021.
- American Academy of Orthopaedic Surgeons. Clinical Practice Guidelines and Evidence-Based Medicine. AAOS, various years.
- Solomon, Louis, Warwick, David, and Nayagam, Selvadurai. Apley and Solomon's System of Orthopaedics and Trauma. CRC Press, 2018.
