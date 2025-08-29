# fal-ai-integration
n8n workflow + sample integrations for Fal.ai integration (JS, Python, Postman, HTML test interface)

# ⚡️ Fal.ai Integration for n8n

This repository contains **n8n workflow integration** and sample usage files prepared for **Fal.ai API** models.  
Customers can start using the integration with one click by adding their API key. 🚀
---
## 📂 Repo Structure

fal-ai-integration/
├── n8n-falai-all-models-workflow.json # Main workflow
├── README.md # Installation guide
├── QUICK_SETUP.md # Quick start
├── TROUBLESHOOTING.md # Troubleshooting
├── config/
│ └── n8n-credentials-template.json # API key template
├── examples/
│ ├── javascript-example.js # JS integration
│ ├── python-example.py # Python integration
│ ├── postman-collection.json # Test collection
│ └── test-interface.html # Web test interface
└── docs/
└── API-REFERENCE.md # API documentation


---

## 🚀 Quick Start

### 1. n8n Setup
```bash
# With Docker
docker run -it --rm --name n8n -p 5678:5678 n8nio/n8n

# or with npm
npm install n8n -g
n8n start


2. Workflow Import
- log in to the n8n panel
- Import n8n-falai-all-models-workflow.json
- Add API key (config/n8n-credentials-template.json)

API Key Configuration

{
  "name": "falaiApi",
  "type": "httpHeaderAuth", 
  "data": {
    "name": "Authorization",
    "value": "Key CUSTOMER_FAL_AI_API_KEY"
  }
}


🔌 Sample Integrations
JavaScript (React)

const processImage = async (model, imageFile) => {
  const formData = new FormData();
  formData.append('model', model);
  formData.append('image', imageFile);

  const response = await fetch('http://localhost:5678/webhook/falai-process', {
    method: 'POST',
    body: formData
  });

  return await response.json();
};

// Usage
const result = await processImage('background/remove', userSelectedFile);
console.log(result.result_url);

Python (Django/Flask)

import requests

def process_image(model, image_file):
    files = {'image': image_file}
    data = {'model': model}
    
    response = requests.post(
        'http://localhost:5678/webhook/falai-process',
        files=files,
        data=data
    )
    return response.json()

# Usage
result = process_image('retoucher', open('image.jpg', 'rb'))
print(result['result_url'])

✨ Supported AI Models
- ✅ background/remove → Delete background
- ✅ retoucher → Image enhancement
- ✅ photo-restoration → Old photo repair
- ✅ object-removal → Object deletion
- ✅ inpaint → Painting/correction
- ✅ clarity-upscaler → Quality enhancement
- ✅ product-shot → Product photo
- ✅ background/replace → Change background

📸 Test Interface You can test all models via a simple web UI by opening the file
examples/test-interface.html.
For each model:
- Photo upload
- Process initiation
- Result preview (before/after)

🛠 Troubleshooting
For common errors and solutions → TROUBLESHOOTING.md

💼 Customer Value Proposition
- Plug & Play → Just add API key and start using immediately
- 8 AI Models → Access to all models with a single endpoint
- Flexible Input → Binary, base64, URL support
- Production Ready → Error management + retry logic
- Comprehensive Docs → Detailed user guide and examples

