# Discharge Summary Report

A Jinja HTML template that generates a printable A4 discharge summary for a patient encounter.

## What It Renders

### Patient & Encounter Info
Displays the patient's name, age, gender, address, official identifier, admission/discharge date-time, and bed location.

### Clinical Summary
Fetches questionnaire responses filtered by `questionnaire_clinical_summary_slug`. Each question-answer pair in the response is rendered as a labelled block. The last editor's name is shown at the bottom of this section.

### Diagnostic Reports
Lists all diagnostic reports with status `final` for the encounter. Each report shows:
- Service request title as a group heading.
- Per-observation rows: date, title, result value, reference range, and interpretation.
- Component-level sub-rows (indented) for panel observations.
- Optional note and conclusion at the end of each report.

### Doctor's Advice
Fetches questionnaire responses filtered by `questionnaire_discharge_advice_slug`. Responses are **grouped by the doctor (updated_by)** and only answers to the question `"Discharge Advice"` are rendered as bullet points under each doctor's name.

### Care Team Signature
Displays the name and role of the first member of the encounter's care team, right-aligned, as a sign-off.

---

## Questionnaire Slug Configuration

Two slugs are configured at the top of the template. Replace the placeholder values with the actual slugs from your system:

```jinja
{% set questionnaire_clinical_summary_slug = "enter_your_questionnaire_slug_on_clinical_summary" %}
{% set questionnaire_discharge_advice_slug  = "enter_your_questionnaire_slug_on_discharg_advice" %}
```

| Variable | Purpose |
|---|---|
| `questionnaire_clinical_summary_slug` | Slug of the questionnaire used to record the clinical summary (chief complaint, history, diagnosis, etc.) |
| `questionnaire_discharge_advice_slug` | Slug of the questionnaire used to record discharge advice given by doctors |

Questionnaire responses are filtered at render time using `.filter(slug = <slug>)`, so only responses belonging to the specified questionnaire are shown in each section.

---

## Template Variables

| Variable | Description |
|---|---|
| `encounter` | The patient encounter object (patient info, facility, start/end time, questionnaire responses, diagnostic reports, care team) |
| `logo_img` | URL of the facility logo image |

## Filters / Functions Used

| Filter / Function | Usage |
|---|---|
| `current_date()` | Renders today's date in the header |
| `\|datetime` | Formats a datetime string |
| `\|date` | Formats a date string |
| `\|float` | Casts a numeric value to float |
| `\|capitalize` | Capitalises the first letter of a string |
| `\|selectattr` | Filters lists by attribute (identifiers, reference ranges) |
| `\|groupby` | Groups doctor's advice responses by the doctor's full name |
| `.filter(slug=...)` | Filters questionnaire responses to a specific questionnaire slug |
