# Customization Guide (v5)

This guide covers customizing `dr1317-generator.html` for the current DR 1317 form design.

## Scope

This document focuses on:
- DR 1317 template and field mapping
- Signature behavior (Section D)
- Thank-you letter layout and placeholders
- CSV format mappings
- File naming and ZIP naming

## 1) DR 1317 Template Assumptions

The generator is built for the 4-page flat DR 1317 template used by this project.

Current behavior:
- Loads the uploaded template PDF
- Copies pages `[2, 3]` (0-based) into output
- Writes all fields to those two pages

Code location:
- `generateDR1317(donor)`

Current lines:
```javascript
const [pg1, pg2] = await outDoc.copyPages(templateDoc, [2, 3]);
```

If your source template changes page order, update these indices first.

## 2) DR 1317 Field Coordinates

Coordinates are hardcoded in `generateDR1317`.

Coordinate system:
- Origin `(0,0)` is bottom-left of page
- Units are PDF points (`72 pt = 1 in`)

Examples in v5:
- Section A/B donor and org fields on page 1
- Section C amounts/checkbox on page 2
- Section D signer fields on page 2

If text is misaligned:
1. Confirm you uploaded the expected template.
2. Adjust only the relevant `x` and `y` values by small increments (1-3 pt).
3. Regenerate one donor and verify.

## 3) Section D Signature Behavior (v5)

### What changed

v5 normalizes uploaded signature images before embedding:
- Reads image
- Detects visible ink bounds
- Trims extra white/transparent margins
- Stores trimmed PNG for consistent placement

This prevents visual drift caused by large blank margins in the source signature file.

### Where to edit

Normalization code:
- `normalizeSignatureImage(file)`
- helpers: `readFileAsDataURL`, `loadImageFromDataURL`, `findInkBounds`

Section D draw code:
```javascript
const sigBox = { x: 39, y: 372, width: 414, height: 24 };
const bottomPadding = 0.5;
const topPadding = 0.5;
const maxW = sigBox.width;
const maxH = sigBox.height - topPadding - bottomPadding;
```

### Safe adjustments

- Move signature horizontally: change `sigBox.x`
- Move signature row vertically: change `sigBox.y`
- Limit max signature height: change `sigBox.height` or paddings

Keep `maxH` at or below the writable band height so signatures stay inside Section D.

## 4) DR 1317 Amount Logic

Inside `generateDR1317`:
```javascript
const intendedRaw = parseAmount(donor['Intended Amount'] || '0');
const line2 = Math.ceil(intendedRaw);
const line3 = Math.ceil(line2 * 0.5);
```

If your policy changes (for example rounding rules), update these expressions.

## 5) CSV Format Detection and Mapping

Code path:
- CSV upload handler
- `normalizeDonorData(raw, isCGD, isAutoGen)`

Detection:
- CGD if header contains `Donor First Name`
- Auto-Gen if not CGD and header contains `Mailing Address` or `Intended Amt`
- Otherwise treated as Harness

### Add another format

1. Add detection condition in CSV upload handler.
2. Add a mapping branch in `normalizeDonorData`.
3. Map incoming fields to internal keys:
   - `First Name`, `Last Name`, `Date`, `Street Address 1`, `City`, `State`, `Zip Code`
   - `Total Donation`, `Fees Covered`, `Intended Amount`

## 6) Thank-You Letter Layout

Code path:
- `generateThankYouLetter(donor)`

Current text area settings:
```javascript
const textX = 183;
const textY = 672;
const maxWidth = 377;
const normalLineHeight = 14;
const normalFontSize = 11;
const footnoteFontSize = 9;
```

Current signature area in letters:
```javascript
const maxSigWidth = 200;
const maxSigHeight = 60;
```

Signature appears only when the template text contains the exact line:
- `Thank you and enjoy your day.`

## 7) Placeholders

Supported placeholders:
- `{{date}}`
- `{{firstName}}`
- `{{amount}}`
- `{{donationDate}}`
- `{{preparerName}}`
- `{{preparerTitle}}`

Replacement logic lives in `generateThankYouLetter`.

To add a placeholder:
1. Compute value in code.
2. Add a replacement line.
3. Use placeholder in `.txt` template.

Example:
```javascript
const campaign = document.getElementById('campaignName').value || 'WBNC';
letterText = letterText.replace(/\{\{campaign\}\}/g, campaign);
```

## 8) File Naming

### DR 1317
```javascript
const fileName = `${donorLastName}_${donorFirstName}_DR1317_${campaign}_${dateForFile}.pdf`;
```

### Thank-you
```javascript
const fileName = `${donorLastName}_${donorFirstName}_TY_${campaign}_${dateForFile}.pdf`;
```

### ZIP
```javascript
link.download = `WBNC_Donor_Documents_${campaign}_${timestamp}.zip`;
```

## 9) Validation and Required Inputs

`checkReadyToGenerate()` requires:
- donor list loaded (`donors.length > 0`)
- at least one output type checked
- signature uploaded
- DR template uploaded (if DR output enabled)
- letterhead + letter text uploaded (if letter output enabled)

If you add new required fields, update this function.

## 10) Testing Checklist After Any Customization

1. Load one donor CSV row.
2. Generate DR 1317 only.
3. Verify Section A-D alignment.
4. Verify signature stays within Section D band.
5. Generate thank-you only.
6. Verify text wrapping and signature placement in letter.
7. Run a 20+ donor batch and test ZIP output.

## 11) Common Failure Modes

- Wrong template version uploaded:
  - Symptoms: all coordinates shifted, signature appears misplaced.
- CSV headers not matching format mapping:
  - Symptoms: donor count loads but fields blank/wrong.
- Letter template missing trigger line:
  - Symptoms: no signature on thank-you letter.

## 12) Recommended Editing Workflow

1. Copy `dr1317-generator-v5.html` to a dated backup.
2. Make one change at a time.
3. Test with 1 donor.
4. Commit/document change notes before the next tweak.
