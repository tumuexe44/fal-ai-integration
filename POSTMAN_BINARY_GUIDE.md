# Postman Binary Upload Guide

## Overview

The enhanced n8n workflow now supports binary image uploads directly from Postman using form-data. This is especially useful for testing with actual image files without needing to convert them to base64.

## 🔧 Enhanced Features

### Process Input Function Updates
The `Process Input` function now handles:
- ✅ **Binary data** from Postman form-data uploads
- ✅ **Base64** encoded images
- ✅ **Image URLs**
- ✅ **All supported models** with aliases

### Supported Models and Aliases
```json
{
  "background/remove": "fal-ai/bria-ai/background/remove",
  "bria/background-removal": "fal-ai/bria-ai/background/remove", 
  "remove-background": "fal-ai/bria-ai/background/remove",
  
  "retoucher": "fal-ai/retoucher",
  "enhance": "fal-ai/retoucher",
  "retouch": "fal-ai/retoucher",
  
  "upscaler": "fal-ai/real-esrgan",
  "upscale": "fal-ai/real-esrgan",
  "super-resolution": "fal-ai/real-esrgan",
  
  "colorizer": "fal-ai/imagecolorizer",
  "colorize": "fal-ai/imagecolorizer",
  "add-color": "fal-ai/imagecolorizer",
  
  "face-restore": "fal-ai/gfpgan",
  "gfpgan": "fal-ai/gfpgan",
  
  "style-transfer": "fal-ai/arbitrary-style-transfer",
  "artistic-style": "fal-ai/arbitrary-style-transfer",
  
  "object-removal": "fal-ai/lama",
  "inpaint": "fal-ai/lama",
  "remove-object": "fal-ai/lama"
}
```

## 📝 How to Upload Binary Images in Postman

### Method 1: Form-Data Upload (Recommended for Binary)

1. **Open Postman** and create a new POST request
2. **Set URL** to your webhook: `https://your-n8n-instance.com/webhook/falai-process`
3. **Select Body tab** → **form-data**
4. **Add fields**:
   - `model` (Text): Choose from supported models (e.g., `background/remove`)
   - `image` (File): Select your image file

**Example form-data configuration:**
```
Key: model        | Type: Text | Value: background/remove
Key: image        | Type: File | Value: [Select your image file]
```

### Method 2: JSON with Base64 (Traditional)

1. **Set Body** → **raw** → **JSON**
2. **Add JSON**:
```json
{
  "model": "background/remove",
  "image_base64": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..."
}
```

### Method 3: JSON with URL

1. **Set Body** → **raw** → **JSON**
2. **Add JSON**:
```json
{
  "model": "upscaler",
  "image_url": "https://example.com/your-image.jpg"
}
```

## 🎯 Testing Different Models

### Background Removal
```json
POST /webhook/falai-process
Content-Type: multipart/form-data

model: remove-background
image: [file upload]
```

### Image Enhancement
```json
POST /webhook/falai-process
Content-Type: multipart/form-data

model: enhance
image: [file upload]
```

### Super Resolution
```json
POST /webhook/falai-process
Content-Type: multipart/form-data

model: super-resolution
image: [file upload]
```

### Face Restoration
```json
POST /webhook/falai-process
Content-Type: multipart/form-data

model: face-restore
image: [file upload]
```

## 📋 Expected Responses

### Success Response
```json
{
  "status": "success",
  "model": "background/remove",
  "actual_model": "fal-ai/bria-ai/background/remove",
  "result_url": "https://fal.media/files/processed-image.png",
  "completed_at": "2025-08-29T10:30:00.000Z",
  "processing_time_ms": 5240,
  "processing_type": "asynchronous",
  "metadata": {
    "poll_count": 3
  }
}
```

### Error Response (Missing Model)
```json
{
  "status": "error",
  "error": "Model parameter is required. Supported models: background/remove, retoucher, upscaler, colorizer, face-restore, style-transfer, object-removal, bria/background-removal, remove-background, enhance, retouch, upscale, super-resolution, colorize, add-color, gfpgan, artistic-style, inpaint, remove-object",
  "timestamp": "2025-08-29T10:30:00.000Z",
  "supported_models": ["background/remove", "retoucher", "upscaler", ...]
}
```

## 🔍 Debugging Binary Uploads

### Check Request in n8n Execution Log
The Process Input function will log:
```javascript
console.log('Processing binary input data');
```

### Verify Binary Data Structure
The function checks for: `$binary && $binary.data`

### Common Issues and Solutions

1. **"Image input required" Error**
   - **Cause**: No form field named 'image' or wrong field name
   - **Solution**: Ensure form-data field is named 'image'

2. **"Model parameter is required" Error**
   - **Cause**: Missing or incorrect model field
   - **Solution**: Add 'model' field in form-data

3. **Binary data not recognized**
   - **Cause**: n8n webhook not configured for multipart
   - **Solution**: Check webhook node settings

## 🧪 Testing Workflow

### 1. Test Model List
Send empty request to get supported models:
```json
POST /webhook/falai-process
Content-Type: application/json

{}
```

### 2. Test Binary Upload
Use form-data with:
- model: `background/remove`
- image: Upload a test image

### 3. Test Response Processing
Check that response includes:
- `input_type`: "binary"
- `has_binary`: true
- `actual_model`: Resolved model name

## 📁 Postman Collection

Import the enhanced `postman-collection.json` which includes:
- ✅ Binary upload examples for all models
- ✅ Organized by model categories
- ✅ Error testing scenarios
- ✅ Different input format examples

## 🔧 Troubleshooting

### Binary Data Not Processing
1. Check n8n execution logs
2. Verify webhook accepts multipart/form-data
3. Ensure form field is named 'image'
4. Check file size limits

### Model Not Found
1. Use exact model names from supported list
2. Check spelling and case sensitivity
3. Use aliases if preferred (e.g., 'enhance' vs 'retoucher')

### Large File Handling
1. Consider file size limits in n8n
2. Use compression for large images
3. Monitor processing timeouts

## 🎯 Best Practices

1. **Use descriptive model names**: Use aliases like 'enhance', 'upscale' for clarity
2. **Test with small files first**: Verify workflow before using large images
3. **Monitor processing times**: Async jobs may take longer
4. **Check supported formats**: Ensure image format is supported by the model
5. **Use form-data for files**: More efficient than base64 for file uploads

This enhanced workflow now provides complete flexibility for image processing with any input method you prefer!