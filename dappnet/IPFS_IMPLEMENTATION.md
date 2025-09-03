# IPFS Protocol Support Implementation

## Overview
This implementation adds native support for `ipfs://` URIs in Chromium, which are automatically redirected to a local IPFS gateway running on `http://0.0.0.0:8080`.

## Implementation Details

### Files Modified

1. **chrome/common/url_constants.h**
   - Added `kIPFSScheme` constant for "ipfs" scheme

2. **chrome/common/chrome_content_client.cc**
   - Added IPFS to `kChromeStandardURLSchemes` array
   - Registered IPFS as a secure scheme to avoid mixed content warnings

3. **chrome/browser/chrome_content_browser_client.cc**
   - Added `HandleIPFSURLRewrite` function to redirect IPFS URLs to local gateway
   - Registered the handler in `BrowserURLHandlerCreated`

### How It Works

1. **URL Registration**: The IPFS scheme is registered as a standard URL scheme in Chromium
2. **URL Rewriting**: When a user navigates to an `ipfs://` URL, it's intercepted and rewritten to `http://0.0.0.0:8080/ipfs/[CID]/[path]`
3. **Security**: IPFS is marked as a secure scheme to prevent mixed content warnings

### Example URL Transformations

- `ipfs://QmTWHDsZKruRDBYFKy7CNNR17SLkhWdvhBoiW9YJtsozeT/` → `http://0.0.0.0:8080/ipfs/QmTWHDsZKruRDBYFKy7CNNR17SLkhWdvhBoiW9YJtsozeT/`
- `ipfs://QmTWHDsZKruRDBYFKy7CNNR17SLkhWdvhBoiW9YJtsozeT/index.html` → `http://0.0.0.0:8080/ipfs/QmTWHDsZKruRDBYFKy7CNNR17SLkhWdvhBoiW9YJtsozeT/index.html`

## Building and Testing

### Prerequisites
1. IPFS daemon running locally with gateway on port 8080
   ```bash
   ipfs daemon
   ```

2. Build Chromium with the changes:
   ```bash
   autoninja -C out/Default chrome
   ```

### Testing

1. Launch the built Chromium:
   ```bash
   out/Default/chrome
   ```

2. Open the test file:
   - File → Open → Navigate to `dappnet/test-ipfs.html`
   - Or visit `file:///path/to/chromium/src/dappnet/test-ipfs.html`

3. Click on the IPFS links to verify they redirect to the local gateway

4. Verify in DevTools:
   - Open Developer Tools (F12)
   - Go to Network tab
   - Click an IPFS link
   - Verify the request goes to `http://0.0.0.0:8080/ipfs/...`

### Troubleshooting

1. **Links not working**: Ensure IPFS daemon is running and gateway is accessible at `http://0.0.0.0:8080`

2. **Build errors**: Clean and rebuild:
   ```bash
   gn clean out/Default
   gn gen out/Default
   autoninja -C out/Default chrome
   ```

3. **URL not recognized**: Check that all three files were modified correctly

## Future Enhancements

- Add support for `ipns://` protocol
- Make gateway URL configurable via settings
- Add fallback to public gateways if local gateway is unavailable
- Integrate with browser security policies