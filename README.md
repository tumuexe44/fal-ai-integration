# Fal.ai Integration with n8n - Complete Setup Guide

## Overview

This n8n workflow provides a seamless integration with Fal.ai services, allowing you to process images through various AI models via a single webhook endpoint. The workflow handles both synchronous and asynchronous processing, automatic polling, and comprehensive error handling.

## Features

- ✅ Single webhook endpoint for all Fal.ai models
- ✅ Support for base64, URL, and binary image inputs (including Postman form-data)
- ✅ Automatic async job polling with smart retry logic
- ✅ Comprehensive error handling and validation
- ✅ Detailed response formatting with metadata
- ✅ Support for multiple AI models with user-friendly aliases
- ✅ Enhanced binary data handling for direct file uploads
- ✅ Model validation and auto-resolution

## Supported Models

The workflow supports all Fal.ai models with convenient aliases:

### Background Removal
- `background/remove`, `remove-background`, `bria/background-removal`

### Image Enhancement
- `retoucher`, `enhance`, `retouch`
- `face-restore`, `gfpgan` (face restoration)

### Image Upscaling
- `upscaler`, `upscale`, `super-resolution`

### Creative Effects
- `colorizer`, `colorize`, `add-color` (colorization)
- `style-transfer`, `artistic-style` (style transfer)
- `object-removal`, `inpaint`, `remove-object` (object removal)

### Custom Models
- Any Fal.ai model can be used with its full name (e.g., `fal-ai/custom-model`)

## Setup Instructions

### 1. Prerequisites

- n8n instance (self-hosted or cloud)
- Fal.ai API account and API key
- Basic understanding of n8n workflows

### 2. Import the Workflow

1. Copy the content of `n8n-falai-workflow.json`
2. In n8n, go to **Workflows** → **Import from file**
3. Paste the JSON content and click **Import**

### 3. Configure Credentials

1. In n8n, go to **Credentials** → **Add Credential**
2. Create a new credential of type **HTTP Header Auth**
3. Name it `falaiApi`
4. Set the header name to `Authorization`
5. Set the header value to `Key YOUR_FAL_AI_API_KEY`

### 4. Activate the Workflow

1. Open the imported workflow
2. Click **Activate** in the top right corner
3. Note the webhook URL (will be something like `https://your-n8n-instance.com/webhook/falai-process`)

## Webhook API Reference

### Endpoint

```
POST /webhook/falai-process
```

### Request Body

#### Option 1: JSON with Base64
```json
{
  "model": "background/remove",
  "image_base64": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..."
}
```

#### Option 2: JSON with URL
```json
{
  "model": "upscaler",
  "image_url": "https://example.com/image.jpg"
}
```

#### Option 3: Form-Data (Binary Upload)
```
Content-Type: multipart/form-data

model: enhance
image: [file upload]
```

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `model` | string | Yes | Fal.ai model to use (supports aliases like "enhance", "upscale") |
| `image_base64` | string | Conditional | Base64 encoded image (one of the three input methods required) |
| `image_url` | string | Conditional | URL to image (one of the three input methods required) |
| `image` | file | Conditional | Binary file upload via form-data (one of the three input methods required) |

### Response Format

#### Success Response
```json
{
  "status": "success",
  "model": "background/remove",
  "result_url": "https://fal.media/files/result-image.png",
  "completed_at": "2025-08-29T10:30:00.000Z",
  "processing_time_ms": 5240,
  "processing_type": "asynchronous",
  "metadata": {
    "poll_count": 3,
    "total_processing_time": 5240
  }
}
```

#### Error Response
```json
{
  "status": "error",
  "error": "Invalid base64 image data",
  "model": "background/remove",
  "error_code": "validation_error",
  "timestamp": "2025-08-29T10:30:00.000Z",
  "processing_time_ms": 120,
  "metadata": {
    "poll_count": 0,
    "max_polls_reached": false
  }
}
```

## Workflow Architecture

### Node Flow

1. **Webhook Entry** - Receives POST requests
2. **Process Input** - Validates and processes input parameters
3. **Model Router** - Routes to appropriate API configuration
4. **Fal.ai API Call** - Makes the initial API request
5. **Check Response Type** - Determines if polling is needed
6. **Polling Loop** - Handles async job polling (if needed)
7. **Format Final Response** - Formats the final output
8. **Webhook Response** - Returns the result

### Key Functions

- **Input Validation**: Ensures required parameters are present
- **Base64 Processing**: Converts base64 strings to binary data
- **Smart Polling**: Automatically polls async jobs with exponential backoff
- **Error Handling**: Comprehensive error detection and reporting
- **Response Formatting**: Consistent response structure with metadata

## Error Handling

The workflow handles various error scenarios:

- Missing or invalid parameters
- Invalid base64 data
- API rate limits
- Job failures
- Network timeouts
- Maximum polling attempts reached

## Performance Considerations

- **Polling Interval**: 3 seconds between polls (configurable)
- **Maximum Polls**: 30 attempts (configurable)
- **Timeout**: Approximately 90 seconds total
- **Concurrent Jobs**: Supports multiple simultaneous requests

## Troubleshooting

### Common Issues

1. **"Model parameter is required"**
   - Ensure you're sending the `model` parameter in the request body

2. **"Either image_base64 or image_url is required"**
   - Include either a base64 encoded image or a URL to an image

3. **"Invalid base64 image data"**
   - Check that your base64 string is properly formatted
   - Ensure it includes the data URL prefix (e.g., `data:image/png;base64,`)

4. **"Maximum polling attempts reached"**
   - The job is taking longer than expected
   - Check the Fal.ai dashboard for job status
   - Consider increasing the `max_polls` value

### Debug Mode

To enable detailed logging:

1. Open the workflow in n8n
2. Add `console.log()` statements in the function nodes
3. Check the execution logs for detailed information

## Integration Examples

See the `examples/` directory for integration examples with:

- JavaScript/TypeScript applications
- Python applications
- cURL commands
- Postman collections

## Security Considerations

- Always use HTTPS for webhook endpoints
- Store API keys securely in n8n credentials
- Validate file sizes and types before processing
- Implement rate limiting if necessary
- Monitor API usage and costs

## Support

For issues related to:

- **n8n Workflow**: Check the workflow logs and function outputs
- **Fal.ai API**: Consult the Fal.ai documentation
- **Integration Issues**: Review the examples and troubleshooting guide

## License

This workflow configuration is provided as-is for educational and commercial use.