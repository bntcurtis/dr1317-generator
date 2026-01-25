# Colorado DR 1317 & Thank You Letter Generator

A web-based tool for Colorado nonprofits to automatically generate Child Care Contribution Tax Credit forms (DR 1317) and personalized thank-you letters for donors.

## What it does

This single-file HTML application helps Colorado childcare nonprofits:
- **Generate DR 1317 forms** — Colorado Child Care Contribution Tax Credit Certification forms with donor information, donation amounts, and credit calculations
- **Create thank-you letters** — Personalized letters on your organization's letterhead with custom messaging
- **Process donations in bulk** — Handle hundreds of donors at once with automatic PDF generation
- **Support multiple CSV formats** — Works with exports from Harness, Colorado Gives Day, and other donation platforms

Instead of manually filling out forms or writing individual letters, process dozens or hundreds of donors in minutes.

## Features

- ✅ **Single standalone HTML file** — No server, build process, or dependencies to install
- ✅ **Runs entirely in your browser** — All processing happens locally; your donor data never leaves your computer
- ✅ **Support for two CSV formats** — Automatically detects and handles Harness or Colorado Gives Day exports
- ✅ **Batch processing** — Generate hundreds of documents without browser freezing
- ✅ **Smart file naming** — `LastName_FirstName_DocType_Campaign_Date.pdf` format for easy organization
- ✅ **ZIP download option** — Get all documents in a single ZIP file for large batches
- ✅ **Signature embedding** — Automatically adds signature images to both forms and letters
- ✅ **Responsive UI** — Works on desktop and tablet devices
- ✅ **Progress tracking** — Real-time progress bar shows generation status

## Use case

This tool was built for Colorado nonprofits that qualify for the **Colorado Child Care Contribution Tax Credit** (C.R.S. 39-22-121). If your organization:
- Provides licensed childcare or early childhood education in Colorado
- Receives donations that qualify for the 50% state tax credit
- Needs to issue DR 1317 forms to donors for tax filing
- Wants to send personalized thank-you letters efficiently

...then this tool can save you hours of manual document preparation.

## Quick start

### 1. Download the tool

Download `dr1317-generator.html` from this repository. That's it — it's a single file that contains everything needed.

### 2. Open it in your browser

Simply double-click the HTML file to open it in your default browser, or drag it into an open browser window.

### 3. Prepare your files

You'll need:
- **CSV file** with donor data (exported from your donation platform)
- **DR 1317 template PDF** (blank form — included in this repo)
- **Letterhead PDF** (your organization's letterhead template)
- **Letter text template** (`.txt` file with placeholders — example included)
- **Signature image** (`.png` file with transparent background)

### 4. Fill in the form

1. Enter your organization information (name, license number, EIN, address, phone)
2. Enter signer information (name, title, campaign name)
3. Upload your signature image
4. Upload your donor CSV file
5. Select which documents to generate (DR 1317, thank-you letters, or both)
6. Upload the required templates
7. Choose output format (individual PDFs or ZIP file)
8. Click "Generate Documents"

### 5. Review and distribute

The tool generates PDFs that download automatically. Review a few to verify accuracy, then distribute to your donors.

## CSV format requirements

The tool supports two CSV formats:

### Harness format

Required columns:
- `Date` — Donation date (e.g., "Nov 16, 2025")
- `First Name` — Donor first name
- `Last Name` — Donor last name
- `Street Address 1` — Donor address
- `City` — Donor city
- `State` — Donor state (2-letter code)
- `Zip Code` — Donor zip code
- `Total Donation` — Total amount donated (e.g., "$15.92")
- `Fees Covered` — Non-qualifying fees (e.g., "$0.92")
- `Intended Amount` — Qualifying donation amount (e.g., "$15.00")

### Colorado Gives Day format

Required columns:
- `Date` — Donation date (MM/DD/YYYY format)
- `Donor First Name` — Donor first name
- `Donor Last Name` — Donor last name
- `Address` — Donor address
- `City` — Donor city
- `State` — Donor state (2-letter code)
- `Zip` — Donor zip code
- `Total Donation` — Total amount including fees
- `Fees Covered` — "Yes" or "No"
- `Covered Cost` — Fee amount if covered
- `Amount` — Qualifying donation amount

The tool automatically detects which format you're using.

## File structure

```
dr1317-generator/
├── dr1317-generator.html          (Main application - single file)
├── README.md                       (This file)
├── LICENSE                         (MIT License)
├── templates/
│   ├── dr1317-blank-template.pdf  (Blank DR 1317 form)
│   ├── sample-letterhead.pdf      (Example letterhead)
│   └── sample-letter-text.txt     (Example letter template)
├── examples/
│   ├── sample-donors.csv          (Example CSV with fake data)
│   └── sample-signature.png       (Example signature image)
└── docs/
    └── CUSTOMIZATION.md           (Detailed customization guide)
```

## Letter text template placeholders

Your letter text template (`.txt` file) can use these placeholders:

- `{{date}}` — Current date (when generated)
- `{{firstName}}` — Donor's first name
- `{{amount}}` — Donation amount (formatted as $XX.XX)
- `{{donationDate}}` — Date of donation (formatted as "November 16, 2025")
- `{{preparerName}}` — Signer's name
- `{{preparerTitle}}` — Signer's title

Example:
```
{{date}}

Dear {{firstName}},

Thank you so much for your donation of {{amount}} on {{donationDate}} to Our Nonprofit.

We could not have accomplished our goal without donors like you.

Thank you and enjoy your day.

{{preparerName}}
{{preparerTitle}}
```

## Technical details

### How it works

1. **All processing is local** — The tool runs entirely in your browser using JavaScript. No data is sent to any server.
2. **PDF generation** — Uses [pdf-lib](https://pdf-lib.js.org/) to fill forms and create documents
3. **ZIP creation** — Uses [JSZip](https://stuk.github.io/jszip/) to bundle multiple PDFs
4. **No installation needed** — pdf-lib and JSZip are embedded directly in the HTML file
5. **Browser compatibility** — Works in Chrome, Firefox, Safari, and Edge (recent versions)

### File sizes

- Main HTML file: ~656 KB (includes embedded libraries)
- Generated DR 1317: ~75-100 KB per document
- Generated thank-you letter: ~200-250 KB per document (depends on letterhead complexity)

### Performance

- Small batches (1-20 donors): Instant
- Medium batches (20-100 donors): 1-3 minutes
- Large batches (100-300 donors): 5-12 minutes
- Very large batches (300+ donors): Consider splitting into multiple runs

The tool processes documents in batches of 10 with short pauses to keep your browser responsive.

## Customization

### Creating your letterhead PDF

1. Design your letterhead in your preferred tool (Word, Google Docs, InDesign, etc.)
2. Include your logo, organization name, board members, address, etc.
3. **Leave blank space** in the main area for letter text (roughly 4-5 inches wide × 7-8 inches tall)
4. Export as PDF
5. Use this PDF as your letterhead template

### Creating a signature image

1. Sign on white paper with a dark pen
2. Scan or photograph at high resolution
3. Use an image editor (Photoshop, GIMP, Photopea) to:
   - Crop tightly around the signature
   - Remove the white background (make it transparent)
   - Save as PNG format
4. Upload to the tool

### Adjusting signature size

If signatures appear too small or large in generated documents, you can adjust the maximum dimensions in the HTML file. Look for these lines in the code:

For DR 1317 forms:
```javascript
const maxWidth = 200;  // Adjust this
const maxHeight = 50;  // Adjust this
```

For thank-you letters:
```javascript
const maxSigWidth = 200;  // Adjust this
const maxSigHeight = 60;  // Adjust this
```

## Troubleshooting

### "Generate" button won't activate

Check that you've uploaded:
- ✅ CSV file (shows "Loaded X donors")
- ✅ Signature image
- ✅ Selected at least one document type
- ✅ DR 1317 template (if generating forms)
- ✅ Letterhead PDF and letter text (if generating letters)
- ✅ Filled in organization information fields

### Wrong number of donors loaded

Your CSV might have:
- Extra blank rows (automatically skipped)
- Header rows in wrong position
- Missing required columns

Open your CSV in a text editor or spreadsheet to verify the structure.

### Amounts or dates are wrong

Check your CSV formatting:
- Amounts should include dollar signs: `$150.00`
- Dates should be formatted: `Nov 16, 2025` or `11/16/2025`
- Column names must match exactly (case-sensitive)

### Browser says "Page unresponsive"

This shouldn't happen with the current version, but if it does:
- You might be processing too many donors at once (try splitting into smaller batches)
- Close other browser tabs to free up memory
- Try using the ZIP download option instead of individual downloads

### Signature doesn't appear

Check that:
- File is PNG or JPEG format (not WebP)
- File isn't corrupted
- Signature has reasonable dimensions (not microscopic or gigantic)

### Documents look wrong

- Verify you're using the correct templates
- Check that your CSV columns match the expected format
- Review console output (press F12) for error messages

## Browser compatibility

Tested and working on:
- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+

Does **not** work on:
- ❌ Internet Explorer (any version)
- ❌ Very old browser versions

## Privacy and security

- **All processing happens locally** in your browser
- **No data is uploaded** to any server
- **No tracking or analytics** are included
- **Safe for sensitive donor information**

The tool only uses browser APIs to:
- Read uploaded files
- Generate PDFs
- Trigger downloads

Your donor data never leaves your computer.

## Contributing

Contributions are welcome! This tool was created for a specific use case but could be expanded. Potential improvements:

- Support for other state tax credit forms
- Additional CSV format support
- Batch configuration saving/loading
- Multi-page letter support
- Additional placeholder variables
- Internationalization

Please open an issue to discuss major changes before submitting a pull request.

## License

MIT License — see [LICENSE](LICENSE) file for details.

This means you can use, modify, and distribute this tool freely, even for commercial purposes. Attribution is appreciated but not required.

## Credits

Created by Brian Curtis for Wild Bear Nature Center (Nederland, CO) and open-sourced for the benefit of other Colorado nonprofits.

Built with:
- [pdf-lib](https://pdf-lib.js.org/) by Andrew Dillon
- [JSZip](https://stuk.github.io/jszip/) by Stuart Knightley
- [Tailwind CSS](https://tailwindcss.com/) for styling

## Support

For questions, issues, or suggestions:
- Open an issue on GitHub
- Check existing issues for solutions
- Review the troubleshooting section above

This is a volunteer project, so response times may vary.

## Changelog

### Version 4.0 (January 2026)
- Made organization info configurable (no longer hardcoded)
- Fixed thank-you letter amount to include covered fees
- State abbreviations truncated to 2 characters for DR1317
- New file naming convention: `LastName_FirstName_DocType_Campaign_Date`
- Support for Colorado Gives Day CSV format
- Added campaign name field for better file organization
- Embedded libraries for offline use (no CDN dependencies)
- Added batching to prevent browser freezing on large batches

### Version 3.0 (November 2025)
- Added thank-you letter generation
- ZIP file download option
- Progress tracking with visual progress bar
- Support for multiple CSV formats

### Version 2.0 (October 2025)
- Switched to flattened PDF template approach
- Improved signature aspect ratio preservation
- Credit computation fix for lines 1-4

### Version 1.0 (September 2025)
- Initial release
- Basic DR 1317 generation from CSV
