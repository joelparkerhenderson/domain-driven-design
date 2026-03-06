Create Domain-Driven Design (DDD) for hospital management creates, modular, and scalable system by modeling software directly after complex healthcare workflows (patients, doctors, appointments, treatments) using a shared "ubiquitous language" between developers and clinicians. It breaks the system into bounded contexts like EMR, scheduling, and billing to manage complexity. 

TODO:

- Create CLAUDE.md fille, plan.md file, tasks.md file.

- Create ubiquious language. 

- Each topic in its own directory with index.md file: ./topics/{topic}/index.md

- Create comprensive documentation.

Do not create images, diagrams, web application, front-end, back-end, etc.

Key Components & Bounded Contexts

- Patient Context: Manages registration, demographic data, and medical history.

- Appointment/Scheduling Context: Handles doctor schedules, patient appointments, and resource allocation.

- Clinical/EMR Context: Manages diagnostics, laboratory tests, imaging, and treatment plans.

- Billing/Administrative Context: Manages invoices, insurance claims, and patient payments.

- Emergency Services Context: Dedicated workflows for urgent, high-priority patient care. 

DDD Technical Implementation:

- Aggregates: Group related entities (e.g., Patient, MedicalRecord, and Allergies are treated as a single unit for data consistency).

- Domain Events: Used to trigger actions across contexts (e.g., AppointmentScheduled triggers NurseAssigned or RoomAllocated).

- Ubiquitous Language: Standardized terms used by developers and medical staff, such as "inpatient," "outpatient," "triage," or "clinical pathway".

- Repositories: Encapsulate database access to store and retrieve aggregate roots like Patient or Appointment. 

Benefits in Healthcare

- Improved Patient Outcomes: Systems better reflect real-world care, reducing errors.

- Handling Complexity: Modular design allows for manageable, independent development of complex modules.

- Scalability & Maintenance: Easier to update specific domains (e.g., billing) without affecting others (e.g., patient records). 

Strategic Design Approach

- Identify Domain Experts: Work with doctors, nurses, and administrators to define workflows.

- Define Context Map: Map how different contexts (e.g., Emergency and Billing) interact.

- Model Core Domain: Prioritize the most critical, complex parts of the system. 
