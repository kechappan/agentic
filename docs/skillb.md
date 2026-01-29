import os, re

def find_orphans():
    # 1. Collect all UI components registered in XML
    registered = set()
    for root, _, files in os.walk("."):
        for file in files:
            if file.endswith(".xml"):
                with open(os.path.join(root, file), 'r', errors='ignore') as f:
                    # Match activity/fragment names in Manifest or NavGraph
                    matches = re.findall(r'android:name="([^"]+)"', f.read())
                    for m in matches:
                        registered.add(m.split('.')[-1])

    # 2. Check disk for files not in that set
    print("| Module | Orphaned Native File | Path |")
    print("| :--- | :--- | :--- |")
    for root, _, files in os.walk("."):
        if "src" in root:
            for file in files:
                if file.endswith(("Activity.kt", "Activity.java", "Fragment.kt", "Fragment.java")):
                    name = file.split('.')[0]
                    if name not in registered:
                        module = root.split(os.sep)[1] if len(root.split(os.sep)) > 1 else "root"
                        print(f"| {module} | {file} | {os.path.join(root, file)} |")

find_orphans()
