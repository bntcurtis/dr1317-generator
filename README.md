# Colorado DR 1317 and Thank You Letter Generator

A browser-based tool that generates:
- Colorado DR 1317 tax credit certification forms
- Personalized thank-you letters

Version 5 is aligned to the current Colorado DR 1317 layout (09/18/25 form design) and Wild Bear workflows.

## What This Version Does

- Uses a 4-page flat DR 1317 template and automatically copies pages 3 and 4 into a 2-page output form.
- Fills DR 1317 Sections A, B, C, and D using donor CSV data plus signer/org inputs.
- Calculates DR 1317 amounts as whole dollars:
  - Line 2 = `ceil(Intended Amount)`
  - Line 3 = `ceil(Line 2 * 0.5)`
- Places signature in Section D inside the signature band.
- Auto-trims signature image margins (white/transparent edges) before embedding, so signature placement is consistent.
- Generates thank-you letters from a PDF letterhead + text template with placeholders.
- Supports individual PDF downloads or one ZIP download.

## Main File

Use:
- `dr1317-generator.html`

## Required Inputs

### Always required
- Donor CSV
- Signature image (`.png`, `.jpg`, `.jpeg`)
- At least one document type selected

### If generating DR 1317
- DR 1317 template PDF (4-page flat template)

### If generating thank-you letters
- Letterhead PDF
- Letter text template (`.txt`)

## DR 1317 Template Requirement

Use the 4-page flat template for the new form design.

The app expects:
- Template page 3 = DR 1317 output page 1 (Sections A and B)
- Template page 4 = DR 1317 output page 2 (Sections C and D)

If you upload a different layout, field placement and signature location will be off.

## CSV Formats Supported

The app auto-detects format from headers.

### 1) Harness format
Expected columns:
- `Date`
- `First Name`
- `Last Name`
- `Street Address 1`
- `City`
- `State`
- `Zip Code`
- `Total Donation`
- `Fees Covered`
- `Intended Amount`

### 2) Colorado Gives Day format
Expected columns:
- `Date`
- `Donor First Name`
- `Donor Last Name`
- `Address`
- `City`
- `State`
- `Zip`
- `Total Donation`
- `Covered Cost`
- `Amount`

### 3) Auto-Gen format
Expected columns:
- `Date`
- `First Name`
- `Last Name`
- `Mailing Address`
- `City`
- `State`
- `ZIP`
- `Total Donation`
- `Fees Covered`
- `Intended Amt`

## Signer and Org Inputs Used in DR 1317

### Organization information (Section B)
- Organization name
- FEIN
- CDEC license number
- Phone
- Street address
- City
- County
- State
- ZIP

### Signer information (Section D)
- Name
- Title
- Phone
- Signing date (`MM/DD/YY`)
- Signature image

## Letter Template Placeholders

Supported placeholders in the `.txt` letter template:
- `{{date}}`
- `{{firstName}}`
- `{{amount}}`
- `{{donationDate}}`
- `{{preparerName}}`
- `{{preparerTitle}}`

Notes:
- `{{amount}}` uses `Total Donation` (includes covered fees).
- Signature placement in letters is triggered by the exact line:
  - `Thank you and enjoy your day.`

## Output File Naming

### DR 1317 PDFs
`LastName_FirstName_DR1317_Campaign_YYYYMMDD.pdf`

### Thank-you PDFs
`LastName_FirstName_TY_Campaign_YYYYMMDD.pdf`

### ZIP output
`WBNC_Donor_Documents_Campaign_YYYYMMDD.zip`

If campaign is blank, default is `WBNC`.

## Quick Start

1. Open `dr1317-generator.html` in a modern browser.
2. Fill organization info (Section B defaults can be edited).
3. Fill signer info (Section D).
4. Upload signature image.
5. Upload donor CSV.
6. Select document types.
7. Upload required template files.
8. Choose output format (individual or ZIP).
9. Generate documents.
10. Review a few samples before distribution.

## Troubleshooting

### Generate button stays disabled
Check required inputs based on selected document types.

### Signature appears too high/low in DR 1317
Check:
- You are using the correct 4-page flat DR 1317 template.
- Signature file is a valid PNG/JPG.
- Re-upload the signature (v5 trims margins on upload).

### Loaded donor count is wrong
Check CSV header row and required columns for your format.

### Wrong amounts in DR 1317
Confirm CSV `Intended Amount` (or `Intended Amt` for Auto-Gen) contains qualifying amount values.

## Privacy and Security

- All processing runs locally in your browser.
- Donor data is not uploaded to a server by this app.

## Browser Support

Tested in current versions of:
- Chrome / Edge
- Firefox
- Safari

## Changelog

### v5.0 (March 2026)
- Updated for DR 1317 09/18/25 form design.
- Added 4-page template extraction (pages 3 and 4 only).
- Added Section D fields: signing date, signer title, signer phone.
- Added Auto-Gen CSV support.
- Updated DR 1317 coordinate mapping for Sections A-D.
- Updated signature logic for Section D writable band.
- Added signature image normalization (auto-trim margins) for stable placement.

### v4.x (January 2026)
- Configurable org info and campaign naming.
- Thank-you letter amount uses total donation.
- Colorado Gives Day CSV support.

## License

MIT. See `LICENSE`.
