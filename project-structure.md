# Project Structure

```
fal/
├── README.md                           # Complete setup and usage guide
├── QUICK_SETUP.md                      # Quick start guide
├── n8n-falai-workflow.json            # Main n8n workflow configuration
├── project-structure.md               # This file
│
├── config/                             # Configuration files
│   ├── n8n-credentials-template.json  # n8n credentials template
│   └── environment.example.env        # Environment variables template
│
├── functions/                          # Individual function files
│   ├── process-input.js               # Input validation and base64 conversion
│   ├── check-response-type.js         # API response analysis
│   ├── process-poll-result.js         # Polling result handler
│   └── format-final-response.js       # Final response formatter
│
└── examples/                           # Usage examples and tests
    ├── javascript-example.js          # JavaScript/TypeScript integration
    ├── python-example.py              # Python integration
    ├── curl-examples.sh               # cURL command examples
    ├── postman-collection.json        # Postman test collection
    └── test-interface.html            # Web-based test interface
```

## File Descriptions

### Core Files
- **`n8n-falai-workflow.json`**: The main n8n workflow configuration that you import into your n8n instance
- **`README.md`**: Comprehensive documentation with setup instructions, API reference, and troubleshooting
- **`QUICK_SETUP.md`**: Minimal setup guide for quick deployment

### Configuration
- **`config/n8n-credentials-template.json`**: Template for n8n API credentials
- **`config/environment.example.env`**: Environment variables for advanced configuration

### Functions
- **`functions/process-input.js`**: Handles input validation and base64 to buffer conversion
- **`functions/check-response-type.js`**: Analyzes Fal.ai API responses and determines processing path
- **`functions/process-poll-result.js`**: Manages polling for async jobs with retry logic
- **`functions/format-final-response.js`**: Formats the final webhook response with metadata

### Examples & Testing
- **`examples/javascript-example.js`**: Complete JavaScript client with browser and Node.js support
- **`examples/python-example.py`**: Python client with batch processing examples
- **`examples/curl-examples.sh`**: Command-line testing examples
- **`examples/postman-collection.json`**: Postman collection for API testing
- **`examples/test-interface.html`**: Interactive web interface for testing the workflow

## Usage Flow

1. **Setup**: Import `n8n-falai-workflow.json` into n8n and configure credentials
2. **Integration**: Use examples in `examples/` directory to integrate with your application
3. **Testing**: Use `test-interface.html` or Postman collection for testing
4. **Customization**: Modify functions in `functions/` directory as needed
5. **Configuration**: Adjust settings using `config/environment.example.env` template

## Key Features

- ✅ Single webhook endpoint for all Fal.ai models
- ✅ Support for both base64 and URL inputs
- ✅ Automatic async job polling
- ✅ Comprehensive error handling
- ✅ Multiple language examples
- ✅ Interactive testing interface
- ✅ Production-ready configuration