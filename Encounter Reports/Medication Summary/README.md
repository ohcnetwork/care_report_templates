# Medication Summary Report

A Jinja HTML template that generates a printable A4 **Medication Summary** for a patient encounter. It combines the patient's active medication prescriptions with free-text medical advice recorded by a doctor.

## What It Renders

### Patient & Encounter Info
Displays the patient's name, age, gender, address, official identifier, admission/discharge date-time, and bed location.

### Medications
Lists all **active medications** from the encounter's medication prescriptions (`encounter.medication_prescriptions`). Each row shows:
- Medicine name
- Dosage
- Frequency
- Duration

Only medications with `status="active"` and at least one dosage instruction are shown.

### Medical Advice
Fetches questionnaire responses filtered by `medication_questionnaire_slug` and renders the answer to the `"Medication Advice"` question as free text. The advising doctor's name (`updated_by.full_name`) is displayed below the advice.

---

## Questionnaire Slug Configuration

Set the slug at the top of the template:

```jinja
{% set medication_questionnaire_slug = "enter_your_questionnaire_slug_on_medication_advice" %}
```

Responses are fetched using `.filter(slug=medication_questionnaire_slug)` and only the **first matching response** is used. Only the `"Medication Advice"` question from that response is rendered.

### Questionnaire JSON

A sample questionnaire definition is provided in this folder:
**[`medication-and-advice.json`](medication-and-advice.json)**

| Field | Value |
|---|---|
| `title` | Medication and Advice |
| `slug` | `medication-and-advice` |
| `subject_type` | `encounter` |
| `status` | `active` |

#### Questions

| Link ID | Text | Type | Purpose in template |
|---|---|---|---|
| `Q-1769607421179` | Medication | `structured` (`medication_request`) | Not directly rendered in the template — medications are sourced from `encounter.medication_prescriptions` instead |
| `Q-1769607453695` | Medication Advice | `text` | Rendered as free-text medical advice below the medications table |

To use this questionnaire, import `medication-and-advice.json` into your system and set `medication_questionnaire_slug` to `medication-and-advice`.

---

## Template Variables

| Variable | Description |
|---|---|
| `encounter` | The patient encounter object (patient info, facility, start/end time, medication prescriptions, questionnaire responses) |
| `logo_img` | URL of the facility logo image |

## Filters / Functions Used

| Filter / Function | Usage |
|---|---|
| `current_date()` | Renders today's date in the header |
| `\|datetime` | Formats admission/discharge datetime strings |
| `\|selectattr` | Filters patient identifiers to find the official one |
| `.filter(status="active")` | Filters medications to only active ones |
| `.filter(slug=...)` | Filters questionnaire responses to the medication advice questionnaire |
| `\|map(attribute='value')` | Extracts values from the advice answer list |
| `\|join('<br>')` | Joins multiple advice lines with line breaks |
| `\|safe` | Renders the joined HTML string without escaping |
