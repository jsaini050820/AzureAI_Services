Azure AI Search reads the content, builds an index, and provides autocomplete for users typing into a search bar.

🧭 Step-by-Step Setup
🔹 1. Prepare Blob Storage
Create a Storage Account

Create a Blob Container (e.g., documents)

Upload files (PDF, DOCX, etc.)

🔹 2. Create Azure AI Search Index from Blob
Use the Import Data Wizard:

Go to your Search service in Azure

Click Import Data

Choose:

Data source: Azure Blob Storage

Skillset: Use AI enrichment (optional for content extraction)

Index: Create new

Indexer: Schedule or run now

Ensure your index includes:

metadata_storage_name

metadata_storage_path (used for linking)

content (text extracted from files)

title (optional: extracted metadata or custom tagging)

🔹 3. Add a Suggester to Your Index
In the portal or JSON schema, define a suggester:

json
Copy
Edit
"fields": [
  { "name": "id", "type": "Edm.String", "key": true },
  { "name": "metadata_storage_name", "type": "Edm.String", "searchable": true },
  { "name": "content", "type": "Edm.String", "searchable": true },
  { "name": "metadata_storage_path", "type": "Edm.String" }
],
"suggesters": [
  {
    "name": "sg",
    "sourceFields": [ "metadata_storage_name", "content" ]
  }
]
✅ This lets the user type and get document title/content suggestions.

🔹 4. Add CORS if Needed
In the Azure Portal → Search Service → CORS → Add your frontend origin
