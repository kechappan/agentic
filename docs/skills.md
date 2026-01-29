# Claude Code: Automated Discovery Skills

Instruct Claude to create and run these scripts to extract data without manual file-by-file reading.

---

## Skill A: Bridge Mapping (`map_caps_bridge.py`)
**Goal:** Generates a comprehensive list of all JS-to-Native touchpoints.

```python
import os, re

def map_bridge():
    results = ["Module | File | JS Methods"]
    # Scan for bridge interfaces
    for root, _, files in os.walk("."):
        for file in files:
            if file.endswith((".kt", ".java")):
                path = os.path.join(root, file)
                with open(path, 'r', errors='ignore') as f:
                    content = f.read()
                    if "JavascriptInterface" in content and "Caps" in content:
                        methods = re.findall(r'@JavascriptInterface\s+(?:fun|public\s+void)\s+(\w+)', content)
                        # Extract module name
                        parts = path.split(os.sep)
                        module = parts[1] if len(parts) > 1 else "root"
                        results.append(f"{module} | {file} | {', '.join(methods)}")
    return results

print("\n".join(map_bridge()))
