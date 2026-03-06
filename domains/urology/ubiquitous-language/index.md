# Urology Domain - Ubiquitous Language

Aggregate Root: The primary entity within an aggregate that serves as the entry point for all operations and enforces consistency rules across the cluster of related domain objects.

Anti-Corruption Layer: A translation boundary that protects a bounded context from the concepts and models of external systems such as EHR, pathology, or radiology platforms.

Bounded Context: A logical boundary within which a specific domain model and its terminology are consistent, such as the Clinical Assessment Context or the Oncologic Urology Context.

Clinical Assessment: The systematic process of evaluating a patient's urological condition through history, physical examination, symptom scoring, urodynamics, cystoscopy, and imaging.

Context Map: A visual and conceptual representation of the relationships and integration points between the six urology bounded contexts.

Cystoscopy: An endoscopic procedure in which a cystoscope is inserted through the urethra to visualize the bladder and urethra for diagnostic or therapeutic purposes.

Domain Event: A significant occurrence within the urology domain that captures something that happened, such as a biopsy result received, a surgery completed, or a stone analysis returned.

Erectile Dysfunction: The persistent inability to achieve or maintain an erection sufficient for satisfactory sexual performance, evaluated within the Male Reproductive Health Context.

ESWL (Extracorporeal Shock Wave Lithotripsy): A non-invasive procedure that uses shock waves to fragment kidney or ureteral stones into smaller pieces that can pass naturally.

Gleason Score: A grading system for prostate cancer based on the microscopic appearance of tumor tissue, combining primary and secondary pattern grades to produce a score from 6 to 10.

IPSS (International Prostate Symptom Score): A validated eight-question questionnaire used to quantify lower urinary tract symptoms and assess their impact on quality of life.

Metabolic Workup: A comprehensive evaluation of blood and urine chemistry to identify metabolic risk factors for recurrent kidney stone formation.

Nephrolithiasis: The presence of calculi (stones) within the kidney, evaluated and managed within the Stone Disease Context.

Oncologic Staging: The process of determining the extent and spread of a urological malignancy using the TNM classification system, which assesses tumor size, node involvement, and metastasis.

PCNL (Percutaneous Nephrolithotomy): A minimally invasive surgical procedure for removing large or complex kidney stones through a small incision in the back.

Pelvic Floor Therapy: A rehabilitative approach using targeted exercises, biofeedback, and behavioral techniques to strengthen pelvic floor muscles and improve continence.

Prostate-Specific Antigen (PSA): A serine protease produced by prostate epithelial cells, measured in serum as a biomarker for prostate cancer screening, diagnosis, and surveillance.

Radical Prostatectomy: The surgical removal of the entire prostate gland and seminal vesicles, performed robotically or via open approach, as definitive treatment for localized prostate cancer.

Renal Mass: An abnormal growth in the kidney identified on imaging, requiring characterization through biopsy or imaging features to determine malignant versus benign status.

Robotic Surgery: Surgical procedures performed using robotic-assisted platforms (such as the da Vinci system) that provide enhanced visualization, dexterity, and precision for complex urological operations.

Sacral Neuromodulation: A therapy that delivers mild electrical impulses to the sacral nerves to modulate bladder and bowel function in patients with refractory urge incontinence or urinary retention.

Semen Analysis: A laboratory evaluation of ejaculate parameters including volume, concentration, motility, and morphology, serving as the foundational test in male infertility evaluation.

Stone Composition Analysis: Laboratory determination of the mineral composition of a passed or surgically retrieved urinary stone, guiding targeted metabolic prevention strategies.

Stress Urinary Incontinence: The involuntary leakage of urine during activities that increase abdominal pressure such as coughing, sneezing, or exercise, caused by urethral sphincter weakness.

Testosterone Deficiency: A clinical syndrome characterized by low serum testosterone levels combined with symptoms such as fatigue, decreased libido, and reduced muscle mass.

TURBT (Transurethral Resection of Bladder Tumor): An endoscopic surgical procedure to diagnose, stage, and treat bladder tumors by resecting them through the urethra.

Ureteroscopy: An endoscopic procedure in which a thin scope is passed through the urethra, bladder, and into the ureter to diagnose or treat ureteral and renal pathology including stone fragmentation.

Urge Incontinence: The involuntary loss of urine associated with a sudden, compelling desire to void, often caused by detrusor muscle overactivity.

Urodynamic Study: A set of diagnostic tests that evaluate how well the bladder, sphincters, and urethra store and release urine, measuring pressures and flow rates during filling and voiding.

Value Object: An immutable object defined by its attributes rather than identity, used in the urology domain to represent measurements such as PSA levels, Gleason scores, stone dimensions, and symptom scores.
