# ripstop
ripgrep setup for chromium for much faster local code searches

Probably will suggest this to chromium some time

# The `ripstop` Execution Tiers

| Execution Profile | Philosophy | Est. Search Space | What It Scans (Includes) | What It Drops (Excludes) | Cold Cache Speed | Best Used For (Agent Task) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Baseline** (`.gitignore`) | *"Search everything Git tracks."* | **~30GB** | Core browser, Blink, V8, Skia, WebRTC, all test data, mobile implementations. | Compiled binaries (`out/`). | **1 to 3 minutes** (Hangs the agent) | **Never.** Causes catastrophic token-limit crashes. |
| **Standard** (Blacklist) | *"Safe exploration."* | **~5 to 8GB** | Core browser, Blink, lightweight dependencies, utility scripts. | `out/`, `testing/`, `node_modules`, V8, Skia, WebRTC, heavy vendors. | **5 to 10 seconds** | **General SWE tasks.** Feature exploration, tracing standard API calls across the browser. |
| **High** (Whitelist) | *"Web Platform focus."* | **~4GB** | Core C++ architecture (`chrome/`, `content/`, `base/`), **AND** `third_party/blink/`. | ALL other `third_party/` directories (V8, graphics engines), massive test suites. | **2 to 4 seconds** | **Frontend/Rendering tasks.** CSS layout bugs, DOM manipulation, Blink-to-Browser IPC. |
| **Full** (Strict Whitelist) | *"Total isolation."* | **< 2GB** | **ONLY** core C++ infrastructure (`base/`, `chrome/`, `content/`, `net/`, `ui/`, `components/`). | **EVERYTHING ELSE.** Drops Blink, all vendors, all tests, all mobile cruft. | **1 to 3 seconds** | **Deep Backend tasks.** Thread scheduling, raw network protocols, low-level memory allocation. |

(This estimation is not based on actual run on chromium code)
