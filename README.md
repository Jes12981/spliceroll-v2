# SpliceRoll v2

A new, dependency-free production workspace inspired by the workflows in SpliceRoll-App. Local review edition; no deployment or cloud service is configured.

## Run in VSCode on Windows

Install Node.js 20 or newer and Git. In the VSCode PowerShell terminal:

```powershell
git clone https://github.com/Jes12981/spliceroll-v2.git
cd spliceroll-v2
code .
npm start
```

Open http://localhost:5173. No `npm install` is needed. If PowerShell blocks npm.ps1, use `npm.cmd start`. Alternatively press F5 in VSCode with the included launch configuration. Stop with Ctrl+C.

```powershell
npm test
```

## Tools and differences

- **Winding:** measured full construction caliper, actual core outside diameter, repeat pitch and lane count; rounds to whole repeats.
- **Splicing:** two to four rolls; subtracts duplicate core areas; rejects rolls smaller than the core.
- **Run time:** explicitly counts press frames, with editable web advance, actual speed and setup minutes. Frames and individual labels are different quantities.
- **Material:** includes gaps between lanes, edge trim on both sides, whole repeats, additive waste allowance and fixed setup length.
- **Estimator:** editable stock/laminate dollars per MSI, press hourly rate and clicks per color per repeat. All rates start at zero; enter actual rates. Shows production cost, not selling price. No private pricing configuration has been copied.
- **Taper:** shows a linear reduction from operator-entered total tension; does not invent recommended machine settings.
- **Proofing:** opens the existing proofs.spliceroll.com tool. No artwork handling is implemented here.

All dimensional inputs convert in place between inches/feet and millimeters/meters without changing the internal inch-based values. Tension remains in lbf and rates remain in USD/MSI. Eight recent saved calculations stay in this browser's local storage. Copy and print controls are included. The application uses local assets and no third-party scripts, fonts or API requests. Proofing is an external link.

## Assumptions

Winding: `D = sqrt(coreOD² + 4 × lengthIn × caliperIn × packingFactor / π)`.
Splicing: `D = sqrt(coreOD² + sum(sourceOD² - coreOD²))`, for identical web widths, constructions and packing density.
Material lanes: `floor((web - 2 × trim + gap) / (labelWidth + gap))`.
Material length: `ceil(quantity / lanes) × pitch / 12 × (1 + allowance/100) + setupFeet`.
The allowance is extra material added to net requirements, not a percentage of gross material lost.
Runtime: `frames × webAdvanceIn / 12 / feetPerMinute + setupMinutes`.
Estimator clicks are charged once per color per down-web repeat including planned waste, rounded up; adjust rates to your actual billing basis. Excludes markup, tax and unentered finishing expenses.
Taper: `T(d) = Tstart × (1 - taper/100 × (d-coreOD)/(finishOD-coreOD))`.

Measured values and machine-approved settings should replace sample inputs. Nominal 3-inch and 6-inch core sizes normally refer to the bore; enter actual outside diameter for roll geometry.

## Files

- `index.html`, `styles.css`: responsive workspace.
- `js/ui.js`: live forms, unit conversion, history and result visualizations.
- `js/calculators.js`: pure calculation functions and validation.
- `tests/calculators.test.js`: independent numeric expectations, invalid inputs and metric equivalence.
- `server.js`: local static preview server. For development only; binds port 5173.

The original repository and live site are unchanged. This implementation has no database, backend account, deployment workflow or service-worker cache. Browser rendering QA and numeric test results are documented in the delivery message.
