NeuraltalkAI – Automated Invoice Processing Workflow

📌 Overview

NeuralAI2 is an end-to-end automated invoice processing system built on n8n
.
It integrates Telegram, Upstage OCR, Google Sheets, Google Drive, and custom logic to streamline invoice management.

The workflow allows users to:

Send invoices (PDFs, images) directly to a Telegram Bot.

Extract structured data using OCR + AI.

Validate against existing records for duplicates.

Store files in Google Drive and metadata in Google Sheets.

Provide real-time feedback via Telegram (✅ Success / ❌ Error).

⚙️ Features

📥 Telegram Integration – Upload invoices as images or PDFs.

🔎 OCR via Upstage AI – Extracts invoice details like shop, date, items, and totals.

🧾 Duplicate Detection – Prevents duplicate entries in Google Sheets.

☁️ Cloud Storage – Uploads invoice files to Google Drive with clean filenames.

📊 Data Management – Logs invoice data in Google Sheets (Expense Tracker).

🤖 Error Handling – Notifies users when extraction fails or mandatory fields are missing.

⚡ Custom Logic – Uses JavaScript code nodes for filename formatting, duplicate checks, and data normalization.

🏗️ Architecture
flowchart LR
    A[📲 Telegram User] -->|Sends Invoice| B[🤖 Telegram Bot (n8n Trigger)]
    B --> C{Check Input Type}
    C -->|Image/PDF| D[📄 Upstage OCR API]
    D --> E{Check Extraction Error}
    E -->|Error| F[❌ Send Error Message via Telegram]
    E -->|Success| G[🔎 Check for Duplicates in Google Sheets]
    G --> H[🧮 Process Duplicate Check (JS Code)]
    H --> I[📂 Get File ID from Telegram]
    I --> J[🌐 Telegram File API - GetFile]
    J --> K[⬇️ Download File]
    K --> L[📝 Generate Clean Filename (JS Code)]
    L --> M[☁️ Upload to Google Drive]
    M --> N[🔗 Update Receipt Link in Data]
    N --> O[📊 Save Final Data to Google Sheets]
    O --> P[✅ Send Success Message via Telegram]

🚀 Setup Instructions
1. Clone Repository
git clone https://github.com/your-username/neuralai2.git
cd neuralai2

2. Prerequisites

n8n
 installed and running

A Telegram Bot (via BotFather
)

API Key for Upstage Document AI OCR

Access to Google Drive & Google Sheets API (OAuth2 credentials)

3. Import Workflow

Open your n8n instance.

Import the provided workflow JSON (neuralai2.json).

Update credentials for:

Telegram API

Google Drive API

Google Sheets API

Upstage OCR API

4. Configure Environment Variables

In .env:

TELEGRAM_API_KEY=your_bot_api_key
UPSTAGE_API_KEY=your_upstage_key
GOOGLE_DRIVE_FOLDER_ID=your_drive_folder
GOOGLE_SHEETS_DOC_ID=your_sheets_id

5. Run the Workflow

Start n8n:

n8n start


Send a test invoice (image/PDF) to your Telegram bot.

📊 Data Flow

Input → Telegram message (photo/document/text).

Processing → OCR extracts text → AI structures JSON → JS checks duplicates.

Storage → Files in Google Drive, metadata in Google Sheets.

Output → Success/error message sent to Telegram.

📌 Example

User Action: Uploads receipt image via Telegram.
System Response:

Extracts invoice details (Shop Name, Date, Item, Amount).

Saves file to Google Drive.

Appends structured data to Google Sheets.

Sends success confirmation ✅.

🛠️ Tech Stack

n8n (workflow automation)

Telegram Bot API (user interaction)

Upstage AI (OCR & document parsing)

Google Drive (file storage)

Google Sheets (expense tracking)

Custom JS Code Nodes (logic handling)

📌 Future Enhancements

🔐 Authentication for approved Telegram users.

📈 Analytics dashboard (Google Data Studio / Looker).

🤝 Multi-language OCR support.

🔔 Automated reminders for expense approvals.








from graphviz import Digraph

# Create a directed graph
dot = Digraph("NeuralAI2_Architecture", format="png")
dot.attr(rankdir="LR", size="8")

# Nodes
dot.node("A", "📲 Telegram User", shape="box", style="filled", fillcolor="#D6EAF8")
dot.node("B", "🤖 Telegram Bot\n(n8n Trigger)", shape="box", style="filled", fillcolor="#AED6F1")
dot.node("C", "Check Input Type", shape="diamond", style="filled", fillcolor="#F9E79F")
dot.node("D", "📄 Upstage OCR API", shape="box", style="filled", fillcolor="#FAD7A0")
dot.node("E", "Check Extraction Error", shape="diamond", style="filled", fillcolor="#F5B7B1")
dot.node("F", "❌ Error Message\nvia Telegram", shape="box", style="filled", fillcolor="#F1948A")
dot.node("G", "🔎 Duplicate Check\n(Google Sheets)", shape="box", style="filled", fillcolor="#D2B4DE")
dot.node("H", "🧮 JS Logic:\nDuplicate Validation", shape="box", style="filled", fillcolor="#D5DBDB")
dot.node("I", "📂 Get File ID\nfrom Telegram", shape="box", style="filled", fillcolor="#A9CCE3")
dot.node("J", "🌐 Telegram File API - GetFile", shape="box", style="filled", fillcolor="#7FB3D5")
dot.node("K", "⬇️ Download File", shape="box", style="filled", fillcolor="#85C1E9")
dot.node("L", "📝 Generate Clean Filename\n(JS Code)", shape="box", style="filled", fillcolor="#F7DC6F")
dot.node("M", "☁️ Upload to Google Drive", shape="box", style="filled", fillcolor="#82E0AA")
dot.node("N", "🔗 Update Receipt Link\n(JS Code)", shape="box", style="filled", fillcolor="#73C6B6")
dot.node("O", "📊 Save Data to Google Sheets", shape="box", style="filled", fillcolor="#BB8FCE")
dot.node("P", "✅ Success Message\nvia Telegram", shape="box", style="filled", fillcolor="#58D68D")

# Edges
dot.edge("A", "B", label="Sends Invoice")
dot.edge("B", "C")
dot.edge("C", "D", label="Image/PDF")
dot.edge("D", "E")
dot.edge("E", "F", label="Error")
dot.edge("E", "G", label="Success")
dot.edge("G", "H")
dot.edge("H", "I")
dot.edge("I", "J")
dot.edge("J", "K")
dot.edge("K", "L")
dot.edge("L", "M")
dot.edge("M", "N")
dot.edge("N", "O")
dot.edge("O", "P")

# Save to file
output_path = "/mnt/data/architecture"
dot.render(output_path, format="png", cleanup=True)

output_file = output_path + ".png"
output_file
