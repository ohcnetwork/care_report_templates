# Certificate of Medical Fitness Report

A Jinja HTML template that generates a printable A4 **Certificate of Medical Fitness** for a patient encounter. The certificate is authored by the examining doctor and confirms the patient is fit to resume duties from a specified date.

## What It Renders

### Header
Facility name, address, phone number, and logo.

### Certificate Body
A formal declaration paragraph containing:
- **Doctor's name** — pulled from the questionnaire response's `updated_by.full_name`.
- **Patient's name and address** — from the encounter's patient record.
- **Fit-from date** — the `"From Date"` answer extracted from the questionnaire response.
- Today's date and a placeholder for the applicant's signature.

### Doctor's Sign-off
Displays the doctor's name and their **Registration No** (extracted from the `"Registration No"` answer in the questionnaire response), left-aligned at the bottom of the certificate.

---

## Questionnaire Slug Configuration

Set the slug at the top of the template:

```jinja
{% set medication_fitness_questionnaire = "enter_specified_questionnaire_slug" %}
```

The questionnaire responses are filtered using `.filter(slug=medication_fitness_questionnaire)` and only the **first matching response** is used.

### Questionnaire JSON

A sample questionnaire definition is provided in this folder:
**[`certificate-of-medical-fi.json`](certificate-of-medical-fi.json)**

| Field | Value |
|---|---|
| `title` | Certificate Of Medical Fitness |
| `slug` | `certificate-of-medical-fi` |
| `subject_type` | `encounter` |
| `status` | `active` |

#### Questions

| Link ID | Text | Type | Purpose in template |
|---|---|---|---|
| `Q-1772784513132` | From Date | `date` | Rendered as the fit-to-resume date in the certificate body |
| `Q-1773648510107` | Registration No | `string` | Rendered as the doctor's registration number in the sign-off |

To use this questionnaire, import `certificate-of-medical-fi.json` into your system and set `medication_fitness_questionnaire` to `certificate-of-medical-fi`.

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
| `.filter(slug=...)` | Filters questionnaire responses to the fitness certificate questionnaire |
