# Medication Certificate Report

A Jinja HTML template that generates a printable A4 **Medication Certificate** (Medical Leave Certificate) for a patient encounter. The certificate is authored by the examining doctor and certifies that the patient requires a specified number of days of rest due to their diagnosis.

## What It Renders

### Header
Facility name, address, phone number, and logo.

### Certificate Body
A formal declaration paragraph containing:
- **Patient's name and address** — from the encounter's patient record.
- **Diagnosis** — the `"Diagnosis"` answer extracted from the questionnaire response.
- **Number of days** of absence recommended — from the `"Number Of Days"` answer.
- **From date** — the `"From Date"` answer (formatted as a date).
- Today's date and a placeholder for the applicant's signature.

### Doctor's Sign-off
Displays the doctor's name (`updated_by.full_name` from the questionnaire response) and their **Registration No**, left-aligned at the bottom.

---

## Questionnaire Slug Configuration

Set the slug at the top of the template:

```jinja
{% set medication_certificate_questionnaire = "enter_specified_questionnaire_slug" %}
```

Responses are fetched using `.filter(slug=medication_certificate_questionnaire)` and only the **first matching response** is used.

### Questionnaire JSON

A sample questionnaire definition is provided in this folder:
**[`medical-certificate.json`](medical-certificate.json)**

| Field | Value |
|---|---|
| `title` | Medical Certificate |
| `slug` | `medical-certificate` |
| `subject_type` | `encounter` |
| `status` | `active` |

#### Questions

| Link ID | Text | Type | Purpose in template |
|---|---|---|---|
| `Q-1772715238857` | From Date | `date` | Start date of recommended rest period |
| `Q-1772715324919` | Diagnosis | `string` | Illness/condition stated in the certificate body |
| `Q-1772715373635` | Registration No | `string` | Doctor's registration number shown in the sign-off |
| `Q-1772715676325` | Number Of Days | `string` | Duration of recommended absence from duty |

To use this questionnaire, import `medical-certificate.json` into your system and set `medication_certificate_questionnaire` to `medical-certificate`.

---

## Template Variables

| Variable | Description |
|---|---|
| `encounter` | The patient encounter object (patient info, facility, questionnaire responses) |
| `logo_img` | URL of the facility logo image |

## Filters / Functions Used

| Filter / Function | Usage |
|---|---|
| `current_date()` | Renders today's date on the certificate |
| `\|date` | Formats the "From Date" answer value |
| `.filter(slug=...)` | Filters questionnaire responses to the medication certificate questionnaire |
