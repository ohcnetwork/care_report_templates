# Treatment Summary Report

A Jinja HTML template that generates a printable A4 **Treatment Summary** for a patient encounter. It consolidates the patient's clinical data — allergies, symptoms, diagnoses, medications, diagnostic reports, and a freeform reports section — into a single document.

## What It Renders

### Patient & Encounter Info
Displays the patient's name, age, gender, address, official identifier, admission/discharge date-time.

### Allergies
Lists all allergy/intolerance records, excluding those with `clinical_status="inactive"` or `verification_status="entered_in_error"`. Columns: allergy name, clinical status, criticality, verification status, note.

### Symptoms
Lists all symptoms, excluding inactive or erroneously entered ones. Columns: symptom name, clinical status, verification status, onset date, note.

### Diagnoses
Lists all diagnoses, excluding inactive or erroneously entered ones. Columns: diagnosis name, clinical status, verification status, onset date, note.

### Medications
Lists all **active medications** from the encounter's medication prescriptions. Each row shows medicine name, dosage, frequency, and duration. Only medications with `status="active"` and at least one dosage instruction are included.

### Service Requests (Diagnostic Reports)
Lists all diagnostic reports with `status="final"`. Grouped by service request title. Each observation row shows: date, title, result value, reference range (with unit), and interpretation. Component-level sub-rows (indented) are shown for panel observations. Optional note and conclusion are rendered at the end of each report.

### Reports (Questionnaire)
Fetches questionnaire responses filtered by `questionnaire_slug` and renders every question-answer pair as a labelled block. The last editor's name is shown at the bottom.

---

## Questionnaire Slug Configuration

Set the slug at the top of the template:

```jinja
{% set questionnaire_slug = "enter_specified_questionnaire_slug" %}
```

Responses are fetched using `.filter(slug=questionnaire_slug)` and only the **first matching response** is used. All question-answer pairs from that response are rendered under the **Reports** section.

> There is no bundled questionnaire JSON for this template. The questionnaire used here is facility/workflow-specific. Create or configure a questionnaire in your system and set `questionnaire_slug` to its slug.

---

## Template Variables

| Variable | Description |
|---|---|
| `encounter` | The patient encounter object (patient info, facility, start/end time, allergies, symptoms, diagnoses, medication prescriptions, diagnostic reports, questionnaire responses) |
| `logo_img` | URL of the facility logo image |

## Filters / Functions Used

| Filter / Function | Usage |
|---|---|
| `current_date()` | Renders today's date in the header |
| `\|datetime` | Formats admission/discharge datetime strings |
| `\|date` | Formats onset and observation dates |
| `\|capitalize` | Capitalises status and interpretation values |
| `\|float` | Casts reference range min/max to float |
| `\|default` | Fallback value for missing interpretation |
| `\|selectattr` | Filters patient identifiers and reference ranges by attribute |
| `.filter(exclude_clinical_status=..., exclude_verification_status=...)` | Excludes inactive/errored allergies, symptoms, and diagnoses |
| `.filter(status="active")` | Filters medications to active ones only |
| `.filter(status="final")` | Filters diagnostic reports to finalised ones only |
| `.filter(slug=...)` | Filters questionnaire responses to the specified questionnaire |
