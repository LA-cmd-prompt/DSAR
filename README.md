# DSAR
Data Subject Access Rights
# DSAR Automation Case Study

## Overview
This repository demonstrates how a company can automate **Data Subject Access Requests (DSARs)** — user requests to access, correct, or delete their personal data.  

Under:
- **PIPEDA**: Individuals have the right to access and correct their personal information.  
- **GDPR**: Articles 12–22 grant access, rectification, deletion, and portability rights.  
- **CPRA (California)**: Expands access and deletion rights for consumers.  

Automation reduces human error, speeds up responses, and ensures compliance with strict deadlines.

---

## Workflow

1. **Intake**: User submits DSAR via web form.  
2. **Verification**: System verifies identity (email token / ID check).  
3. **Triage**: Request type identified (access, correction, deletion, portability).  
4. **Fulfilment**: Data pulled from systems (CRM, HR, databases).  
5. **Response**: Standardized letter sent to user.  
6. **Record-Keeping**: Request logged in DSAR Register (PIPEDA requires accountability).  

📸 See `workflow-diagram.png` for the full process.

---
import csv
from datetime import datetime, timedelta

# File where DSARs will be logged
TRACKER_FILE = "dsar-log.csv"

def log_dsar(requester_name, request_type, email):
    """Logs a DSAR into the tracker with due date (30 days under PIPEDA)."""
    
    # Set due date (30 days from today)
    due_date = (datetime.now() + timedelta(days=30)).strftime("%Y-%m-%d")
    
    # Create log entry
    entry = {
        "Date Received": datetime.now().strftime("%Y-%m-%d"),
        "Requester": requester_name,
        "Email": email,
        "Request Type": request_type,
        "Status": "Open",
        "Due Date": due_date
    }
    
    # Append entry to CSV file
    with open(TRACKER_FILE, mode="a", newline="") as file:
        writer = csv.DictWriter(file, fieldnames=entry.keys())
        
        # Write header if file is empty
        if file.tell() == 0:
            writer.writeheader()
        
        writer.writerow(entry)
    
    print(f"✅ DSAR logged for {requester_name} ({request_type}) due by {due_date}")

# Example usage
log_dsar("Jane Doe", "Access Request", "jane.doe@email.com")

## 📂 Key Deliverables

- **`dsar-form-template.html`** → Sample intake form for users.  
- **`dsar-intake-script.py`** → Mock Python script logging DSARs into a tracker.  
- **`dsar-response-template.md`** → Standardized letter for fulfilling access/deletion requests.  
- **`dsar-tracker.xlsx`** → Excel tool to monitor DSARs (status, due dates, system owner).  
- **`risk-matrix.png`** → Risks if DSARs mishandled (missed deadlines, incorrect disclosure).  

---

## 🎯 Why This Matters
- **For Clients:** Demonstrates ability to operationalize compliance, not just write policies.  
- **For Hiring Managers:** Shows technical + legal integration — you understand DSAR rights *and* can translate them into workflows/tools.  
- **For Peers:** A practical framework others can adapt.  

---

⚡ Built by [LA-cmd-prompt](https://github.com/LA-cmd-prompt)
