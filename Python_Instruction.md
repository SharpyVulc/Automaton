# XML Validator Using Python lxml Library
---
## Step 1: Install Python
 - Download from: https://www.python.org/downloads
 - Run the installer and install the software
 ---
Step 2: Add Python to PATH (if needed)
If python doesn’t work in Command Prompt, add these two paths manually (adjust based on your install location):
Example:
C:\Users\Local\Programs\Python\Python313
C:\Users\Local\Programs\Python\Python313\Scripts
To add them:
Open Control Panel > System > Advanced system settings
Click Environment Variables
Under System variables, select Path, then click Edit
Click New, add these two (adjust version if needed):
Click OK, then restart Command Prompt
Run: python --version to confirm installationStep 3: Install Required Library (lxml)
Open Command Prompt
Run the following command:
‘pip install lxml’ (This installs the lxml library for XML/XSD handling.)


 Step 4: Python Script — XML Validation
Here’s a sample Python script to validate your XML file using an XSD schema and generate an HTML report:
from lxml import etree
from datetime import datetime
import os

xml_file = "account.xml"
xsd_file = "account.xsd"

if not os.path.exists(xml_file):
    print(f"❌ XML file not found: {xml_file}")
    exit()
if not os.path.exists(xsd_file):
    print(f"❌ XSD file not found: {xsd_file}")
    exit()

timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
base_name = os.path.splitext(os.path.basename(xml_file))[0]
report_file = f"{base_name}_report_{timestamp}.html"

html = ["<html><head><title>Validation Report</title></head><body style='font-family:Arial;'>"]
html.append(f"<h2>Validation Report - {xml_file}</h2>")

try:
    schema = etree.XMLSchema(etree.parse(xsd_file))
    xml_doc = etree.parse(xml_file)
    schema.assertValid(xml_doc)
    print("✅ XML is valid.")
    html.append("<p style='color:green;'>✅ XML is valid.</p>")
except etree.DocumentInvalid as e:
    print("❌ XML is invalid.")
    html.append("<p style='color:red;'>❌ XML is invalid.</p><ul>")
    for error in schema.error_log:
        print(f"Line {error.line}, Column {error.column}: {error.message}")
        html.append(f"<li>Line {error.line}, Column {error.column}: {error.message}</li>")
    html.append("</ul>")

html.append("</body></html>")
with open(report_file, "w", encoding="utf-8") as f:
    f.write("\n".join(html))

print(f"\n📄 Validation report saved to: {report_file}")
Step 5: Run the Test in Command Prompt
Open Command Prompt
Navigate to the folder containing your Python script
Run the script:  python your_script_name.py

A validation report (.html format) will be saved in the same folder as your script.
