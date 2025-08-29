# Quick Setup Guide

## Important: n8n Version Compatibility

**For n8n v1.0+**: Use `n8n-falai-workflow-compatible.json`
**For n8n v0.x**: Use `n8n-falai-workflow.json`

The main difference is the webhook response node type:
- v1.0+: Uses `respondToWebhook` node
- v0.x: Uses `webhookResponse` node

## 1. Import Workflow
1. Choose the correct workflow file based on your n8n version
2. Copy the content from the appropriate JSON file
3. In n8n: Workflows → Import from file
4. Paste JSON and import

## 2. Configure API Key
1. In n8n: Credentials → Add Credential
2. Type: **HTTP Header Auth** 
3. Name: `falaiApi`
4. Header Name: `Authorization`
5. Header Value: `Key YOUR_FAL_AI_API_KEY`

## 3. Activate Workflow
1. Open workflow
2. Click **Activate**
3. Note webhook URL

## 4. Test Integration

### Basic cURL Test
```bash
curl -X POST "https://your-n8n-instance.com/webhook/falai-process" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "background/remove",
    "image_url": "https://example.com/test-image.jpg"
  }'
```

### With Base64 Image
```bash
curl -X POST "https://your-n8n-instance.com/webhook/falai-process" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "background/remove",
    "image_base64": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..."
  }'
```

## 5. Integration in Your App

### JavaScript Example
```javascript
async function processImage(model, imageBase64) {
  const response = await fetch('https://your-n8n-instance.com/webhook/falai-process', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      model: model,
      image_base64: imageBase64
    })
  });
  
  return await response.json();
}

// Usage
const result = await processImage('background/remove', 'data:image/png;base64,...');
console.log(result.result_url);
```

### Expected Response
```json
{
  "status": "success",
  "model": "background/remove",
  "result_url": "https://fal.media/files/result.png",
  "completed_at": "2025-08-29T10:30:00.000Z",
  "processing_time_ms": 5240
}
```

## Supported Models
- `background/remove`
- `retoucher` 
- `upscaler`
- `colorizer`
- Any Fal.ai model

## Troubleshooting
- Check n8n execution logs
- Verify API key is correct
- Ensure image is valid base64 or accessible URL
- Check Fal.ai API documentation for model-specific parameters