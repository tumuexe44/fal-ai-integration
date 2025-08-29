# Troubleshooting Guide

## Common n8n Workflow Issues

### 1. "Unrecognized node type: n8n-nodes-base.webhookResponse"

**Problem**: This error occurs when using the old workflow format with newer n8n versions.

**Solution**: Use the compatible workflow file:
- **For n8n v1.0+**: Import `n8n-falai-workflow-compatible.json`
- **For n8n v0.x**: Import `n8n-falai-workflow.json`

**What changed**: n8n v1.0 replaced `webhookResponse` with `respondToWebhook` node.

### 2. Credential Configuration Issues

**Problem**: "Missing credentials" or authentication errors.

**Solution**:
1. Go to n8n **Credentials** → **Add Credential**
2. Choose **HTTP Header Auth**
3. Set exactly:
   - Name: `falaiApi`
   - Header Name: `Authorization`
   - Header Value: `Key YOUR_ACTUAL_API_KEY`

**Important**: Replace `YOUR_ACTUAL_API_KEY` with your real Fal.ai API key.

### 3. "listen for test event" Error

**Problem**: Workflow fails when testing the webhook.

**Solutions**:
1. **Check n8n Version**: Use the correct workflow file for your version
2. **Recreate Webhook Node**: Delete and recreate the webhook node
3. **Check Webhook Settings**:
   - Method: POST
   - Path: `falai-process`
   - Response Mode: `Response Node`

### 4. API Request Failures

**Problem**: HTTP requests to Fal.ai fail.

**Solutions**:
1. **Verify API Key**: Test your key in Fal.ai dashboard
2. **Check Model Name**: Ensure the model exists (e.g., `background/remove`)
3. **URL Format**: Should be `https://queue.fal.run/fal-ai/MODEL_NAME`

### 5. Base64 Image Issues

**Problem**: "Invalid base64 image data" error.

**Solutions**:
1. **Check Format**: Should start with `data:image/TYPE;base64,`
2. **Remove Spaces**: Ensure no spaces or line breaks in base64 string
3. **Validate Base64**: Test the string in an online base64 decoder

### 6. Polling Timeout Issues

**Problem**: Async jobs never complete.

**Solutions**:
1. **Check Poll URL**: Verify the status URL is correct
2. **Increase Max Polls**: Default is 30, increase if needed
3. **Check Fal.ai Status**: Look at job status in Fal.ai dashboard

### 7. JSON Parsing Errors

**Problem**: "Unexpected token" or JSON syntax errors.

**Solutions**:
1. **Validate JSON**: Use a JSON validator for request body
2. **Check Expressions**: Verify n8n expression syntax
3. **Escape Quotes**: Properly escape quotes in JSON strings

## Version-Specific Issues

### n8n v1.0+ Issues

**If Node**: Updated to version 2
- Old: `conditions.string`
- New: `conditions.conditions` with operator objects

**HTTP Request**: Updated to version 4
- Simplified authentication
- Changed JSON body format

### n8n v0.x Issues

**Webhook Response**: Uses `webhookResponse` node
**HTTP Request**: Uses version 3 with different parameter structure

## Debug Steps

### 1. Check Workflow Execution
1. Open workflow
2. Click on any failed node
3. Check the **Output** tab for error details
4. Look at **Input** tab to verify data flow

### 2. Test Components Individually
1. Test webhook with simple response first
2. Test API call with hardcoded values
3. Test each function node separately

### 3. Enable Debug Logging
Add console.log statements in function nodes:
```javascript
console.log('Debug data:', JSON.stringify($json, null, 2));
```

### 4. Validate Input Data
Check that incoming webhook data matches expected format:
```json
{
  "model": "background/remove",
  "image_base64": "data:image/png;base64,iVBORw0..."
}
```

## Getting Help

### Error Information to Collect
1. n8n version
2. Exact error message
3. Node where error occurs
4. Input data format
5. Workflow execution logs

### Support Channels
- n8n Community Forum
- n8n GitHub Issues
- Fal.ai Documentation
- This project's examples and documentation

## Prevention Tips

1. **Always backup** working workflows before updates
2. **Test thoroughly** after n8n updates
3. **Use version control** for workflow JSON files
4. **Monitor** Fal.ai API changes and limits
5. **Validate inputs** before processing
6. **Set appropriate timeouts** for long-running jobs

## Quick Fixes

### Recreate Problematic Nodes
1. Note the node configuration
2. Delete the problematic node
3. Add a new node of the same type
4. Reconfigure with noted settings
5. Reconnect to other nodes

### Reset Workflow State
1. Deactivate workflow
2. Save workflow
3. Reactivate workflow
4. Test functionality

### Clear n8n Cache
1. Restart n8n instance
2. Clear browser cache
3. Re-import workflow if needed