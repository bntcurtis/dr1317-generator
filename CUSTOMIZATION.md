# Customization guide

This guide provides detailed instructions for customizing the Colorado DR 1317 & Thank You Letter Generator for your organization.

## Table of contents

1. [Creating your letterhead](#creating-your-letterhead)
2. [Customizing letter text](#customizing-letter-text)
3. [Adjusting signature sizes](#adjusting-signature-sizes)
4. [Modifying field positions](#modifying-field-positions)
5. [Adding new placeholders](#adding-new-placeholders)
6. [Changing file naming](#changing-file-naming)

## Creating your letterhead

Your letterhead PDF serves as the template for thank-you letters. Follow these guidelines for best results.

### Design considerations

**Essential elements:**
- Organization logo
- Organization name and address
- Any branding elements (colors, patterns)
- Optional: Board members list, tagline, website

**Critical spacing:**
- Leave a blank rectangular area for letter text
- Recommended: 4-5 inches wide × 7-8 inches tall
- Position: Usually upper-left or center of page
- The tool writes text starting from coordinates (183, 672) with max width 377px

### Design tools

You can create your letterhead in:
- **Microsoft Word** — Use header/footer for branding, leave body blank
- **Google Docs** — Same approach as Word
- **Adobe InDesign** — Professional design option
- **Canva** — User-friendly design platform
- **LibreOffice Writer** — Free alternative to Word

### Step-by-step in Google Docs

1. Create a new document
2. Set margins: File → Page setup → Set custom margins
3. Insert → Header & footer → Header
4. Add your logo and organization info to the header
5. Optionally add a sidebar by:
   - Insert → Drawing → New
   - Create a rectangle for the sidebar
   - Add text boxes with board member names, contact info, etc.
   - Save and close
6. Leave the main body area blank
7. File → Download → PDF Document

### Step-by-step in Microsoft Word

1. Create a new document
2. Insert → Header
3. Add logo and organization info
4. Use text boxes (Insert → Text Box) to create a sidebar if desired
5. Leave the main body blank
6. File → Save As → PDF

### Testing your letterhead

After creating your letterhead PDF:
1. Use it to generate a test letter with sample data
2. Check that text doesn't overlap with logos or branding
3. Verify signature placement looks good
4. Adjust if needed and regenerate

## Customizing letter text

Letter text templates use placeholders that get replaced with actual donor data.

### Available placeholders

- `{{date}}` — Current date when letter is generated (e.g., "January 21, 2026")
- `{{firstName}}` — Donor's first name
- `{{amount}}` — Total donation amount including fees (e.g., "$259.97")
- `{{donationDate}}` — Date of donation (e.g., "January 2, 2025")
- `{{preparerName}}` — Signer's name (from form input)
- `{{preparerTitle}}` — Signer's title (from form input)

### Example template structure

```
{{date}}

Dear {{firstName}},

[Opening paragraph with {{amount}} and {{donationDate}}]

[Body paragraphs about impact and mission]

[Optional quote or testimonial]

[Closing paragraph]

Thank you and enjoy your day.

{{preparerName}}
{{preparerTitle}}

*[Tax-deductible disclaimer and organization info]*
```

### Writing guidelines

**Do:**
- Keep paragraphs short (3-5 sentences)
- Use the donor's first name for personalization
- Include specific impact statements
- Add a call to action or invitation
- Keep total length under 400 words
- Use the special line "Thank you and enjoy your day." to trigger signature placement

**Don't:**
- Hardcode specific donor names or amounts
- Make the letter too long (readability decreases)
- Forget the tax disclaimer at the bottom
- Remove or modify the placeholder syntax (`{{placeholder}}`)

### Tax disclaimer template

```
*[Your Organization Name] is a 501(c)3 non-profit organization, and your donation is 100% tax deductible. It may also be eligible for the Colorado Child Care Tax Credit. Please consult your tax advisor. Note that no goods or services were exchanged for this contribution. This letter may be used for your IRS tax records. Tax ID # [Your EIN].*
```

## Adjusting signature sizes

Signatures are scaled proportionally to fit within maximum dimensions.

### Finding the code

Open `dr1317-generator.html` in a text editor and search for:

**For DR 1317 forms:**
```javascript
// Add signature
if (signatureImg && signatureDimensions) {
    const maxWidth = 200;  // Adjust this
    const maxHeight = 50;  // Adjust this
```

**For thank-you letters:**
```javascript
if (signatureImg) {
    const maxSigWidth = 200;  // Adjust this
    const maxSigHeight = 60;  // Adjust this
```

### Making adjustments

To make signatures **larger**:
- Increase both maxWidth and maxHeight proportionally
- Example: Change 200 → 250 and 50 → 63

To make signatures **smaller**:
- Decrease both maxWidth and maxHeight proportionally
- Example: Change 200 → 150 and 50 → 38

**Important:** Keep the aspect ratio similar to avoid distortion. If you double the width, roughly double the height.

### Testing

After making changes:
1. Save the HTML file
2. Open it in your browser (refresh if already open)
3. Generate a test document
4. Check signature size
5. Adjust again if needed

## Modifying field positions

The tool draws text at specific coordinates on the DR 1317 form.

### Understanding coordinates

Coordinates are in PDF points (72 points = 1 inch):
- X coordinate: Left to right (0 = left edge)
- Y coordinate: Bottom to top (0 = bottom edge)
- Origin (0,0) is at bottom-left corner of page

### Field coordinate table

The tool uses these coordinates for DR 1317 forms:

```javascript
const fieldCoordinates = {
    'Organization Name': { x: 37.2095, y: 624.72, width: 540.2945, height: 16.476 },
    'License Number or Colorado Account Number': { x: 37.3399, y: 600.6, width: 269.4181, height: 16.837 },
    'FEIN': { x: 305.888, y: 600.6, width: 271.243, height: 16.837 },
    // ... more fields
};
```

### Adjusting positions

If text appears misaligned:

1. Find the field in the `fieldCoordinates` object
2. Adjust x and y values:
   - Increase x to move right
   - Decrease x to move left
   - Increase y to move up
   - Decrease y to move down
3. Adjust by small amounts (2-5 points at a time)
4. Test and iterate

### Text area for letters

The letter text area is defined by:

```javascript
const textX = 183;  // Left edge of text area
const textY = 672;  // Top edge of text area
const maxWidth = 377; // Width of text area
```

Adjust these if your letterhead has different spacing requirements.

## Adding new placeholders

To add custom placeholders to letter templates:

### 1. Add to CSV data structure

If the data comes from your CSV, it's already available. If it's calculated or comes from another source, you'll need to add it.

### 2. Update placeholder replacement

Find this section in the code:

```javascript
// Replace placeholders in template
let letterText = letterTextTemplate;
letterText = letterText.replace(/\{\{date\}\}/g, currentDate);
letterText = letterText.replace(/\{\{firstName\}\}/g, donorFirstName);
letterText = letterText.replace(/\{\{amount\}\}/g, `$${totalDonation.toFixed(2)}`);
```

Add your new placeholder:

```javascript
letterText = letterText.replace(/\{\{yourNewPlaceholder\}\}/g, yourNewValue);
```

### 3. Update template

Add the placeholder to your letter text template:

```
Thank you for your {{amount}} donation to support {{yourNewPlaceholder}}.
```

### Example: Adding organization name

```javascript
// In the letter generation function, add:
const orgName = document.getElementById('orgName').value;
letterText = letterText.replace(/\{\{orgName\}\}/g, orgName);
```

Then use in template:
```
Thank you for your donation to {{orgName}}.
```

## Changing file naming

File names are generated in the `generateDR1317` and `generateThankYouLetter` functions.

### Current format

- DR 1317: `LastName_FirstName_DR1317_Campaign_Date.pdf`
- Thank you: `LastName_FirstName_TY_Campaign_Date.pdf`

### Customizing format

Find these lines:

```javascript
// DR 1317 naming
const fileName = `${donorLastName}_${donorFirstName}_DR1317_${campaign}_${dateForFile}.pdf`;

// Thank you naming
const fileName = `${donorLastName}_${donorFirstName}_TY_${campaign}_${dateForFile}.pdf`;
```

### Examples of alternative formats

**Reverse order (FirstName_LastName):**
```javascript
const fileName = `${donorFirstName}_${donorLastName}_DR1317_${campaign}_${dateForFile}.pdf`;
```

**Add organization abbreviation:**
```javascript
const fileName = `ORG_${donorLastName}_${donorFirstName}_DR1317_${dateForFile}.pdf`;
```

**Different date format:**
```javascript
// Use the original date string instead of formatted version
const fileName = `${donorLastName}_${donorFirstName}_DR1317_${donor['Date']}.pdf`;
```

**Include donation amount:**
```javascript
const fileName = `${donorLastName}_${donorFirstName}_${totalDonation}_DR1317.pdf`;
```

## Advanced customizations

### Supporting additional CSV formats

To add support for another CSV format:

1. Update the `normalizeDonorData` function
2. Add detection logic based on column names
3. Map columns to the standard internal format

### Changing fonts

The tool currently uses Helvetica (DR 1317) and Times Roman (letters). To change:

```javascript
// DR 1317 - find this line:
const font = await pdfDoc.embedFont(StandardFonts.Helvetica);

// Change to:
const font = await pdfDoc.embedFont(StandardFonts.TimesRoman);

// Available standard fonts:
// - Helvetica
// - TimesRoman
// - Courier
// - HelveticaBold
// - TimesRomanBold
```

### Adjusting text size

Find these constants:

```javascript
const normalFontSize = 11;
const footnoteFontSize = 9;
```

Increase or decrease as needed. Test with longer and shorter text to ensure wrapping works correctly.

## Getting help

If you're making customizations and run into issues:

1. Open the browser console (F12) to see error messages
2. Test changes incrementally (one at a time)
3. Keep a backup of the working version
4. Check the main README for troubleshooting tips
5. Open a GitHub issue describing your customization and the problem

## Contributing your customizations

If you've made improvements that would benefit others:

1. Fork the repository
2. Make your changes
3. Test thoroughly
4. Submit a pull request with a clear description
5. Include examples of what your changes enable

We welcome contributions that:
- Add new features
- Improve usability
- Fix bugs
- Enhance documentation
- Support additional use cases
