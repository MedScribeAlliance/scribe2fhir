---
title: "FHIR Resource Types Overview"
description: "Comprehensive guide to all FHIR resource types supported by Scribe2FHIR, with examples and implementation details."
---

# FHIR Resource Types

Scribe2FHIR supports a comprehensive set of FHIR R5 resource types to handle various aspects of healthcare data. This section provides detailed documentation for each supported resource type.

## Core Administrative Resources

### Patient
The foundational resource representing individuals receiving healthcare services.

- **Purpose**: Demographics, identifiers, and contact information
- **Key Fields**: Name, birth date, gender, identifiers, contact details
- **Relationships**: Referenced by most other resources

### Encounter
Represents interactions between patients and healthcare providers.

- **Purpose**: Hospital stays, outpatient visits, virtual consultations
- **Key Fields**: Type, period, status, location, participants
- **Relationships**: Links patients to clinical activities

### Organization
Healthcare organizations, departments, and facilities.

- **Purpose**: Hospitals, clinics, labs, insurance companies
- **Key Fields**: Name, type, contact information, hierarchy
- **Relationships**: Referenced in encounters, practitioners

### Practitioner
Healthcare professionals and service providers.

- **Purpose**: Doctors, nurses, therapists, support staff
- **Key Fields**: Name, qualifications, specialties, contact info
- **Relationships**: Referenced in encounters, observations, procedures

## Clinical Resources

### Condition
Medical problems, diagnoses, and health concerns.

- **Purpose**: Diseases, symptoms, injuries, disorders
- **Key Fields**: Code, severity, onset, resolution status
- **Coding Systems**: ICD-10, SNOMED CT, custom codes
- **Relationships**: Links to evidence, encounters, patients

### Observation
Clinical measurements, assessments, and findings.

#### Vital Signs
- Blood pressure, heart rate, temperature
- Weight, height, BMI calculations  
- Oxygen saturation, respiratory rate

#### Laboratory Results
- Blood tests, urine analysis, microbiology
- Chemistry panels, hematology counts
- Reference ranges and abnormal flags

#### Diagnostic Findings
- Physical examination results
- Imaging study interpretations
- Pathology reports

### Medication Resources

#### MedicationStatement
Historical and current medication usage.

- **Purpose**: What medications the patient is taking/has taken
- **Key Fields**: Medication, dosage, adherence, source
- **Use Cases**: Medication reconciliation, history tracking

#### MedicationRequest
Prescriptions and medication orders.

- **Purpose**: Prescriptions from healthcare providers
- **Key Fields**: Medication, dosage, instructions, substitution
- **Use Cases**: E-prescribing, pharmacy fulfillment

#### MedicationAdministration
Record of medication given to patients.

- **Purpose**: Actual administration events (especially inpatient)
- **Key Fields**: Medication, dose given, route, timing
- **Use Cases**: Hospital medication tracking, nursing records

### Procedure
Medical procedures, surgeries, and therapeutic interventions.

- **Purpose**: Operations, treatments, diagnostic procedures
- **Key Fields**: Code, performer, outcome, body site
- **Coding Systems**: CPT, SNOMED CT, ICD-10-PCS
- **Relationships**: Links to encounters, conditions, devices

## Specialized Resources

### AllergyIntolerance
Allergic reactions and adverse substance sensitivities.

- **Purpose**: Drug allergies, food intolerances, environmental reactions
- **Key Fields**: Substance, reaction type, severity, manifestations
- **Safety**: Critical for clinical decision support

### Immunization
Vaccination records and immunization history.

- **Purpose**: Vaccines administered, immunization schedules
- **Key Fields**: Vaccine code, date given, lot number, site
- **Public Health**: Important for disease surveillance

### FamilyMemberHistory
Genetic and familial health information.

- **Purpose**: Family medical history, genetic risk factors
- **Key Fields**: Relationship, condition, age of onset
- **Use Cases**: Risk assessment, genetic counseling

### ServiceRequest
Orders for diagnostic tests, procedures, and referrals.

- **Purpose**: Lab orders, imaging requests, specialist referrals
- **Key Fields**: Category, code, priority, requester
- **Workflow**: Drives healthcare delivery processes

### DiagnosticReport
Results and interpretations from diagnostic services.

- **Purpose**: Lab reports, imaging studies, pathology results
- **Key Fields**: Status, category, conclusion, observations
- **Relationships**: References service requests and observations

### CarePlan
Planned healthcare activities and goals.

- **Purpose**: Treatment plans, care coordination, goals
- **Key Fields**: Activities, goals, status, participants
- **Care Coordination**: Essential for managing complex conditions

## Appointment and Scheduling

### Appointment
Scheduled healthcare appointments and bookings.

- **Purpose**: Future healthcare encounters, scheduling
- **Key Fields**: Status, participants, time, location, reason
- **Workflow**: Healthcare scheduling and resource management

### Schedule
Provider and resource availability.

- **Purpose**: Available time slots, practitioner schedules
- **Key Fields**: Active period, service category, participants
- **Integration**: Works with appointment scheduling systems

## Implementation Patterns

### Resource Relationships
Most FHIR resources are interconnected:

```
Patient ← referenced by → Encounter
    ↓                         ↓
Condition ←              → Observation
    ↓                         ↓
MedicationRequest ←      → Procedure
```

### Coding and Terminologies

#### Standard Code Systems
- **ICD-10-CM**: Diagnosis codes
- **CPT**: Procedure codes  
- **SNOMED CT**: Clinical concepts
- **LOINC**: Laboratory and clinical observations
- **RxNorm**: Medications

#### Custom Coding
Scribe2FHIR supports custom code systems for:
- Internal facility codes
- Proprietary classification systems
- Legacy system mappings

### Data Quality and Validation

#### Required Elements
Each resource type has mandatory elements that must be provided:
- Patient: identifier, name
- Encounter: status, class, subject
- Condition: code, subject

#### Constraints and Profiles
FHIR profiles can add additional constraints:
- US Core profiles for interoperability
- Specialty-specific profiles (cardiology, oncology)
- Institutional implementation guides

## Best Practices

### Resource Selection
Choose the appropriate resource type based on:

1. **Clinical Intent**: What are you trying to represent?
2. **Workflow Integration**: How will it be used downstream?  
3. **Interoperability**: What do receiving systems expect?

### Data Modeling
Structure your data to:

1. **Separate Concerns**: Use distinct resources for different concepts
2. **Maintain Relationships**: Link related resources appropriately
3. **Follow Standards**: Use established coding systems where possible

### Performance Considerations
For large datasets:

1. **Bundle Resources**: Group related resources together
2. **Reference by ID**: Use logical references between resources
3. **Index Strategically**: Plan for search and retrieval patterns

## Next Steps

- **[Explore specific resource documentation](/resources/patient)** for detailed implementation
- **[Review Python SDK examples](/python-sdk)** for code samples
- **[Check FHIR specification compliance](/fhir-specification)** for standards details
