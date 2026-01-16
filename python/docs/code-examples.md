---
title: "Code Examples Registry"
description: "Collection of code examples for common Scribe2FHIR operations"
---

# Code Examples Registry

This registry contains code examples for common Scribe2FHIR operations.


## Basic Patient Creation

Create a simple patient record

```python
from scribe2fhir.core import DocumentBuilder
from scribe2fhir.core.types import PatientInfo

builder = DocumentBuilder()
patient_info = PatientInfo(
    first_name="John",
    last_name="Doe",
    date_of_birth=date(1980, 5, 15),
    gender="male",
    mrn="MRN12345678"
)
patient = builder.add_patient(patient_info)
```


## Complete Encounter Example

Create patient with encounter and clinical data

```python
# Create patient
patient = builder.add_patient(patient_info)

# Add encounter
encounter = builder.add_encounter(
    EncounterInfo(
        encounter_type="outpatient",
        encounter_date=datetime.now(),
        provider_name="Dr. Smith"
    ),
    patient_id=patient.id
)

# Add vital signs
builder.add_observation(
    ObservationInfo(
        observation_type="vital_signs",
        measurement_type="blood_pressure", 
        systolic_value=120,
        diastolic_value=80,
        unit="mmHg"
    ),
    patient_id=patient.id
)
```

