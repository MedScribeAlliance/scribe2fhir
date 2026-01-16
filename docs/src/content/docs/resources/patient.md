---
title: "Patient Resource"  
description: "Complete guide to creating and managing Patient resources in Scribe2FHIR with examples and best practices."
---
The Patient resource is the cornerstone of FHIR-based healthcare systems, representing individuals who receive healthcare services. In Scribe2FHIR, the Patient resource serves as the foundation that links all other clinical information.

## Overview

Every healthcare interaction begins with a patient, making this resource essential for:

- **Identity Management**: Unique identification across systems
- **Demographics**: Basic information for care delivery  
- **Contact Information**: Communication and emergency contacts
- **Administrative Data**: Insurance, preferences, and consent

## Core Implementation

### Basic Patient Creation

```python
from scribe2fhir.core.document_builder import DocumentBuilder
from scribe2fhir.core.types import PatientInfo
from datetime import date

builder = DocumentBuilder()

# Create basic patient information
patient_info = PatientInfo(
    first_name="John",
    last_name="Doe",
    date_of_birth=date(1980, 5, 15),
    gender="male",
    mrn="MRN12345678"
)

# Add patient to document
patient = builder.add_patient(patient_info)
print(f"Created patient with ID: {patient.id}")
```

### Advanced Patient Information

```python
from scribe2fhir.core.types import PatientInfo, ContactInfo, Address

# Comprehensive patient information
patient_info = PatientInfo(
    first_name="Jane",
    last_name="Smith", 
    middle_name="Marie",
    date_of_birth=date(1985, 12, 3),
    gender="female",
    mrn="MRN87654321",
    
    # Multiple identifiers
    ssn="123-45-6789",
    
    # Contact information
    phone="555-123-4567",
    email="jane.smith@email.com",
    
    # Address information
    address=Address(
        line1="123 Main Street",
        line2="Apt 4B",
        city="Springfield",
        state="IL",
        postal_code="62704",
        country="US"
    ),
    
    # Emergency contact
    emergency_contact=ContactInfo(
        name="Robert Smith",
        relationship="spouse",
        phone="555-987-6543"
    ),
    
    # Additional demographics
    marital_status="married",
    preferred_language="en",
    race="white",
    ethnicity="not-hispanic-latino"
)

patient = builder.add_patient(patient_info)
```

## Required vs Optional Elements

### FHIR Requirements
The FHIR Patient resource has minimal required elements:
- **identifier**: At least one identifier (MRN, SSN, etc.)
- **name**: At least one name (can be partial)

### Scribe2FHIR Requirements
Scribe2FHIR requires additional elements for practical use:
- **first_name**: Given name
- **last_name**: Family name  
- **date_of_birth**: Birth date
- **gender**: Administrative gender
- **mrn**: Medical record number

### Recommended Elements
For comprehensive healthcare records:
- **Contact information**: Phone, email, address
- **Emergency contacts**: For critical situations
- **Insurance information**: Coverage details
- **Demographics**: Race, ethnicity, language preferences

## Identifiers and Identity Management

### Primary Identifiers

```python
# Medical Record Number (primary)
patient_info = PatientInfo(
    first_name="John",
    last_name="Doe",
    mrn="MRN12345678",  # Primary facility identifier
    # ... other fields
)

# Multiple facility identifiers
patient_info = PatientInfo(
    first_name="John", 
    last_name="Doe",
    mrn="MRN12345678",
    additional_identifiers={
        "hospital_id": "HSP001122334",
        "clinic_id": "CLN998877665", 
        "insurance_id": "INS555666777"
    }
)
```

### Government Identifiers

```python
# Social Security Number
patient_info = PatientInfo(
    first_name="Jane",
    last_name="Smith",
    ssn="123-45-6789",  # US Social Security Number
    mrn="MRN87654321"
)

# International identifiers
patient_info = PatientInfo(
    first_name="Marie",
    last_name="Dubois", 
    mrn="MRN11223344",
    additional_identifiers={
        "passport": "FR1234567890",
        "national_id": "1850512345678"  # French INSEE number
    }
)
```

## Demographics and Personal Information

### Gender and Sex
FHIR distinguishes between administrative gender and biological sex:

```python
# Administrative gender (required)
patient_info = PatientInfo(
    gender="female",  # male | female | other | unknown
    # ... other fields
)

# For more complex gender identity scenarios
patient_info = PatientInfo(
    gender="other",
    additional_demographics={
        "gender_identity": "non-binary",
        "pronouns": "they/them",
        "birth_sex": "female"
    }
)
```

### Race and Ethnicity
Following US Core guidelines:

```python
patient_info = PatientInfo(
    race="white",  # OMB race categories
    ethnicity="not-hispanic-latino",  # OMB ethnicity categories
    additional_demographics={
        "detailed_race": ["irish", "german"],
        "detailed_ethnicity": []
    }
)
```

### Language and Communication

```python
patient_info = PatientInfo(
    preferred_language="es",  # ISO 639-1 language code
    additional_demographics={
        "communication_preferences": {
            "language": "es",
            "preferred": True,
            "mode": ["written", "spoken"]
        },
        "interpreter_required": True
    }
)
```

## Contact Information Management

### Primary Contact

```python
from scribe2fhir.core.types import Address, ContactInfo

# Primary address
address = Address(
    line1="456 Oak Avenue", 
    line2="Suite 12",
    city="Chicago",
    state="IL", 
    postal_code="60601",
    country="US",
    use="home"  # home | work | temp | old
)

patient_info = PatientInfo(
    phone="312-555-0123",
    email="patient@example.com", 
    address=address
)
```

### Multiple Contacts

```python
patient_info = PatientInfo(
    # Primary contact
    phone="312-555-0123",
    email="patient@example.com",
    
    # Additional contacts
    additional_contacts={
        "work_phone": "312-555-9876",
        "mobile": "312-555-5555",
        "work_email": "john.doe@company.com"
    }
)
```

### Emergency Contacts

```python
emergency_contact = ContactInfo(
    name="Sarah Johnson",
    relationship="daughter", 
    phone="312-555-7890",
    email="sarah@example.com",
    address=Address(
        line1="789 Pine Street",
        city="Evanston",
        state="IL",
        postal_code="60201"
    )
)

patient_info = PatientInfo(
    emergency_contact=emergency_contact,
    # ... other fields
)
```

## Clinical and Administrative Context

### Marital Status

```python
patient_info = PatientInfo(
    marital_status="married",  # married | single | divorced | widowed | separated
    # ... other fields
)
```

### Insurance and Coverage

```python
patient_info = PatientInfo(
    insurance_info={
        "primary_insurance": {
            "plan_name": "Blue Cross Blue Shield",
            "member_id": "ABC123456789",
            "group_number": "GRP001",
            "subscriber": "self"
        },
        "secondary_insurance": {
            "plan_name": "Medicare",
            "member_id": "1EG4-TE5-MK73",
            "part_a": True,
            "part_b": True
        }
    }
)
```

## Data Validation and Quality

### Required Field Validation
Scribe2FHIR automatically validates required fields:

```python
try:
    patient_info = PatientInfo(
        first_name="John",
        # Missing last_name, date_of_birth, gender, mrn
    )
    patient = builder.add_patient(patient_info)
except ValidationError as e:
    print(f"Validation error: {e}")
```

### Date Validation

```python
from datetime import date

# Valid birth date
patient_info = PatientInfo(
    date_of_birth=date(1990, 6, 15),  # Valid past date
    # ... other fields
)

# Invalid future birth date will raise validation error
try:
    patient_info = PatientInfo(
        date_of_birth=date(2030, 1, 1),  # Future date
        # ... other fields
    )
except ValidationError as e:
    print(f"Invalid birth date: {e}")
```

### Phone and Email Validation

```python
# Valid formats
patient_info = PatientInfo(
    phone="(555) 123-4567",  # Various formats accepted
    email="patient@domain.com",  # Standard email format
)

# Alternative phone formats
patient_info = PatientInfo(
    phone="+1-555-123-4567",  # International format
    # phone="555.123.4567",   # Dot notation
    # phone="5551234567",     # Numbers only
)
```

## Integration Patterns

### Patient Matching and Deduplication

```python
def find_or_create_patient(builder, patient_info):
    """Find existing patient or create new one"""
    
    # Search by MRN first
    existing = builder.find_patient_by_mrn(patient_info.mrn)
    if existing:
        return existing
        
    # Search by name and DOB
    existing = builder.find_patient_by_demographics(
        first_name=patient_info.first_name,
        last_name=patient_info.last_name, 
        date_of_birth=patient_info.date_of_birth
    )
    if existing:
        # Update with new information
        return builder.update_patient(existing.id, patient_info)
    
    # Create new patient
    return builder.add_patient(patient_info)
```

### Batch Patient Import

```python
def import_patient_batch(patient_data_list):
    """Import multiple patients efficiently"""
    builder = DocumentBuilder()
    results = []
    
    for patient_data in patient_data_list:
        try:
            patient_info = PatientInfo(**patient_data)
            patient = builder.add_patient(patient_info)
            results.append({
                'status': 'success',
                'patient_id': patient.id,
                'mrn': patient_info.mrn
            })
        except Exception as e:
            results.append({
                'status': 'error', 
                'error': str(e),
                'data': patient_data
            })
    
    return results
```

## Privacy and Security Considerations

### Sensitive Information
Patient resources contain highly sensitive PHI (Protected Health Information):

```python
# Minimal patient for privacy-conscious scenarios
patient_info = PatientInfo(
    first_name="John",
    last_name="D",  # Abbreviated last name
    date_of_birth=date(1980, 1, 1),  # Year only
    gender="male",
    mrn="MRN12345678"
    # Omit address, phone, SSN for privacy
)
```

### Consent and Preferences

```python
patient_info = PatientInfo(
    # ... basic information
    consent_preferences={
        "data_sharing": False,
        "marketing_communications": False,
        "research_participation": True,
        "organ_donation": True
    }
)
```

## Common Use Cases and Examples

### New Patient Registration

```python
def register_new_patient(registration_form):
    """Register a new patient from intake form"""
    
    patient_info = PatientInfo(
        first_name=registration_form['first_name'],
        last_name=registration_form['last_name'],
        date_of_birth=registration_form['date_of_birth'], 
        gender=registration_form['gender'],
        mrn=generate_mrn(),  # Auto-generate MRN
        
        phone=registration_form.get('phone'),
        email=registration_form.get('email'),
        address=Address(**registration_form['address']),
        
        emergency_contact=ContactInfo(
            **registration_form['emergency_contact']
        ),
        
        insurance_info=registration_form.get('insurance')
    )
    
    builder = DocumentBuilder()
    patient = builder.add_patient(patient_info)
    
    return patient
```

### Patient Information Update

```python
def update_patient_contact(patient_id, new_phone, new_address):
    """Update patient contact information"""
    
    builder = DocumentBuilder()
    
    # Load existing patient
    existing_patient = builder.get_patient(patient_id)
    
    # Update contact information
    updated_info = PatientInfo(
        # Keep existing demographics
        first_name=existing_patient.name[0].given[0],
        last_name=existing_patient.name[0].family,
        date_of_birth=existing_patient.birthDate,
        gender=existing_patient.gender,
        mrn=get_mrn_from_identifiers(existing_patient.identifier),
        
        # Update contact info
        phone=new_phone,
        address=new_address
    )
    
    return builder.update_patient(patient_id, updated_info)
```

### Cross-System Patient Linking

```python
def link_patient_across_systems(local_mrn, external_system_id):
    """Link patient with external system identifier"""
    
    builder = DocumentBuilder()
    patient = builder.find_patient_by_mrn(local_mrn)
    
    if patient:
        # Add external system identifier
        builder.add_patient_identifier(
            patient.id,
            identifier_type="external_system",
            identifier_value=external_system_id,
            system="http://external-hospital.com/patient-ids"
        )
    
    return patient
```

## Error Handling and Troubleshooting

### Common Validation Errors

```python
from scribe2fhir.core.exceptions import ValidationError

def safe_patient_creation(patient_data):
    """Create patient with comprehensive error handling"""
    
    try:
        patient_info = PatientInfo(**patient_data)
        builder = DocumentBuilder()
        return builder.add_patient(patient_info)
        
    except ValidationError as e:
        if "date_of_birth" in str(e):
            print("Invalid birth date - must be in the past")
        elif "mrn" in str(e):
            print("MRN already exists or invalid format")
        elif "email" in str(e):
            print("Invalid email format")
        else:
            print(f"Validation error: {e}")
        return None
        
    except Exception as e:
        print(f"Unexpected error: {e}")
        return None
```

### Data Quality Checks

```python
def validate_patient_quality(patient_info):
    """Perform data quality checks"""
    issues = []
    
    # Check name completeness
    if len(patient_info.first_name) < 2:
        issues.append("First name too short")
    if len(patient_info.last_name) < 2:
        issues.append("Last name too short")
    
    # Check age reasonableness
    today = date.today()
    age = today.year - patient_info.date_of_birth.year
    if age > 150:
        issues.append("Age appears unrealistic")
    if age < 0:
        issues.append("Birth date is in the future")
    
    # Check contact information
    if not patient_info.phone and not patient_info.email:
        issues.append("No contact information provided")
    
    return issues
```

## Performance Considerations

### Efficient Patient Queries

```python
def bulk_patient_lookup(mrn_list):
    """Efficiently look up multiple patients"""
    builder = DocumentBuilder()
    
    # Batch query for better performance
    patients = builder.find_patients_by_mrns(mrn_list)
    
    return {p.identifier[0].value: p for p in patients}
```

### Memory Management for Large Datasets

```python
def process_large_patient_dataset(patient_file):
    """Process large patient datasets efficiently"""
    
    batch_size = 1000
    processed_count = 0
    
    with open(patient_file, 'r') as f:
        batch = []
        
        for line in f:
            patient_data = json.loads(line)
            batch.append(patient_data)
            
            if len(batch) >= batch_size:
                # Process batch
                results = import_patient_batch(batch)
                processed_count += len(results)
                
                # Clear batch to free memory
                batch = []
                
                print(f"Processed {processed_count} patients")
        
        # Process remaining batch
        if batch:
            results = import_patient_batch(batch)
            processed_count += len(results)
    
    return processed_count
```

## Related Resources

Patient resources typically link to:
- **[Encounter](encounter)**: Healthcare interactions
- **[Condition](condition)**: Medical problems
- **[Observation](observation)**: Clinical measurements
- **[MedicationStatement](medication)**: Current medications
- **[AllergyIntolerance](allergy)**: Known allergies

## Next Steps

1. **[Create your first patient](../examples/basic-patient)** - Step-by-step example
2. **[Learn about Encounters](encounter)** - Next resource to implement  
3. **[Explore the Python SDK](../python-sdk/readme)** - Full API documentation
