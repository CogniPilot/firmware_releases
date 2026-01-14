# CogniPilot Firmware Releases

This repository hosts firmware binaries for CogniPilot hardware platforms. It is deployed to [firmware.cognipilot.org](https://firmware.cognipilot.org) via GitHub Pages.

## Repository Structure

```
firmware_releases/
├── CNAME                           # Custom domain: firmware.cognipilot.org
├── .github/
│   └── workflows/
│       └── pages.yml               # GitHub Pages deployment
├── {board}/
│   └── {app}/
│       ├── latest.json             # Firmware manifest
│       ├── {version}.bin           # Firmware binary (MCUboot signed)
│       └── ...
```

## Manifest Format

Each firmware has a `latest.json` manifest:

```json
{
  "board": "mr_mcxn_t1",
  "app": "optical-flow",
  "latest": {
    "version": "1.2.3",
    "date": "2026-01-13T12:00:00Z",
    "sha": "abc123...",
    "size": 123456,
    "url": "https://firmware.cognipilot.org/mr_mcxn_t1/optical-flow/1.2.3.bin",
    "changelog": "Fixed sensor calibration issue"
  },
  "previous": [
    {
      "version": "1.2.2",
      "date": "2026-01-05T12:00:00Z",
      "sha": "def456...",
      "url": "https://firmware.cognipilot.org/mr_mcxn_t1/optical-flow/1.2.2.bin"
    }
  ]
}
```

## Releasing Firmware

Use the `west release_image` command from the spinali workspace:

```bash
# Build your firmware
west build -b mr_mcxn_t1 app/optical_flow

# Release to this repository
west release_image -d build/mr_mcxn_t1/optical_flow

# Or release and push in one command
west release_image -d build/mr_mcxn_t1/optical_flow --push
```

### Environment Variables

- `FIRMWARE_RELEASES_PATH`: Path to this repository (optional, auto-detected)

## HCDF Integration

Devices can specify a custom firmware update URI in their HCDF definition:

```xml
<mcu name="spinali-001" hwid="0x12345678">
  <board>spinali</board>
  <firmware_update_uri>https://firmware.cognipilot.org/spinali/cerebri</firmware_update_uri>
</mcu>
```

This allows devices to override the default firmware source (based on board/app) with a specific URL.

## URL Patterns

- Manifest: `https://firmware.cognipilot.org/{board}/{app}/latest.json`
- Binary: `https://firmware.cognipilot.org/{board}/{app}/{version}.bin`

## License

Apache-2.0
