# Power Meter Configuration

This section describes how to connect external power meters via the web interface to read grid power data for the battery control system.

## HTTP/JSON Provider

The HTTP/JSON provider allows you to query power meters that expose their data locally via a REST API in JSON format (e.g., Shelly 3EM, Tasmota, etc.).

### Important Note on JSON Path Syntax (Arrays)

The internal parser utilizes a **JSON Pointer logic** and splits paths exclusively by forward slashes (`/`). The classic JavaScript dot-notation with directly attached brackets will not work and results in errors (e.g., `Unable to access JSON key...`).

If your meter's JSON response contains nested arrays/lists (identifiable by square brackets, e.g., for individual phase values), the array index must be isolated using forward slashes.

* **Incorrect:** `emeters[1].power` or `emeters[1]/power`
* **Correct:** `emeters/[1]/power` (or with a leading slash: `/emeters/[1]/power`)

### Example: Shelly 3EM
To read the power value of the second phase (L2) from a Shelly 3EM, the correct path configuration is:
`emeters/[1]/power`
