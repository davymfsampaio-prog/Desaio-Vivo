---
name: Streamlit workflow config
description: How to configure Streamlit workflows correctly in Replit to avoid startup failures
---

## Rule
Configure Streamlit workflows WITHOUT `waitForPort`. Do NOT use pydeck for maps.

**Why:** Replit's `configureWorkflow` health-check verifies the port within a short window before Streamlit's HTTP server is ready to serve requests, causing a false TASK_FAILED/DIDNT_OPEN_A_PORT result even though the process is healthy. Removing `waitForPort` lets the workflow start and stay running. For maps, use `plotly.graph_objects.Scattergeo` — pydeck requires a Mapbox API token or it will error.

**How to apply:**
```javascript
await configureWorkflow({
    name: "Start application",
    command: "cd /abs/path/to/streamlit-app && streamlit run app.py",
    outputType: "webview"
    // no waitForPort
});
```
For maps: use `plotly.graph_objects.Scattergeo` or `plotly.express.scatter_geo` with `scope="south america"` (no external token needed).
