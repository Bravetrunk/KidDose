# KidDose / DoseDek

KidDose is a bilingual Thai/English pediatric dosage and safety calculator designed for fast, weight- and age-based medication checks in a mobile-friendly web app.

> **Clinical safety notice:** KidDose is an aid for trained healthcare professionals, not a prescribing authority or a substitute for a current drug reference, local protocol, or clinical judgment. Always verify the medicine, indication, formulation, patient context, and final dose before administration. Do not use the calculator to self-medicate a child.

## What it does

- Calculates liquid doses from patient weight and medicine concentration.
- Supports age-band dosing where a weight-based formula is not appropriate.
- Applies configured maximum single-dose caps.
- Flags age and minimum-weight restrictions.
- Warns for configured G6PD and renal-safety concerns.
- Groups medicines by clinical category and supports Thai/English search.
- Lets users pin favorites, switch between grid and compact views, inspect calculation details, and copy a dose summary for an EMR.
- Includes a Fact Check page with separate physician/prescriber and user/caregiver verification checklists linked to current WHO, FDA, and CDC guidance.
- Includes a responsive PWA shell with an installable manifest and service worker.

The current embedded catalog contains 43 medicine records across OPD Common, Antibiotics, ER / Resuscitation, Asthma / Respiratory, GI / Digestive, Vitamins / Supplements, and Neuro / Seizure categories. The catalog is maintained in `MASTER_DRUGS` inside `index.html` and must be clinically reviewed before production use.

## Run locally

This is a static site with no build step and no runtime backend. Serve it over HTTP so the service worker can register:

```bash
python3 -m http.server 8080
```

Then open [http://localhost:8080](http://localhost:8080). Opening `index.html` directly from the filesystem will prevent the PWA service worker from working.

If you use VS Code, the repository includes a Chrome launch configuration in `.vscode/launch.json` for the same local server.

## How to use it

1. Choose the patient context (infant, child, or adolescent) or enter the patient values directly.
2. Enter weight in kilograms and age in months.
3. Turn on G6PD deficiency or renal impairment when applicable.
4. Search for a medicine or browse by category.
5. Select the dispensed concentration and review the calculated amount, frequency, dose cap, and safety messages.
6. Open the detail view and verify the result against an authoritative current reference before prescribing or administering.

## Project structure

```text
.
├── index.html                 # React UI, embedded drug catalog, and dose engine
├── manifest.json              # PWA metadata and install icons
├── sw.js                      # App-shell service worker
├── icons/                     # PWA and browser icon sizes
├── docs/                      # Product, design, technical, and dose references
├── .github/workflows/         # Dependency-free repository validation
└── .vscode/launch.json        # Optional local Chrome launch configuration
```

## Implementation notes

- React 18, ReactDOM, Babel Standalone, Tailwind CSS, Google Fonts, and Lucide are loaded from CDNs in `index.html`.
- The core dose calculation and safety evaluation run in the browser; there is no application server or database.
- The service worker caches the local app shell after the first successful load. CDN-hosted libraries and fonts remain external runtime dependencies, so test offline behavior on the target devices before relying on it in a clinical setting.
- The reference PDFs in `docs/` are project inputs, not a guarantee that the embedded dosing data is current. Review and version clinical data separately from UI changes.
- The Fact Check page is a safety-reference layer, not a clinical certification. Recheck its linked guidance against current local policy, hospital protocol, and product labeling before deployment.

## Verification

The repository validation workflow checks JSON syntax, required PWA assets, and whitespace errors on pushes and pull requests. For local checks, run:

```bash
python3 -m json.tool manifest.json >/dev/null
test -f index.html && test -f sw.js && test -f manifest.json
git diff --check
```

Also exercise the calculator in a browser with representative weights, age limits, G6PD/renal flags, concentration changes, search, favorites, detail modal, and copy-to-EMR behavior.

## Reference material

See [`docs/README.md`](docs/README.md) for the project specifications, design system, and source dose table included with the repository.

## License

No license has been declared yet. Treat the repository as all-rights-reserved until the project owner adds a license and confirms the licensing status of the included reference material.
