# Auto-Update Testing Guide

## ⚠️ Important: Bootstrap Required

**The Problem:**
- v1.0.7 and v1.0.8 do NOT have signature bypass configured
- They will reject unsigned updates (v1.0.9, v1.0.10, etc.)
- Cannot update from v1.0.7 → v1.0.9+ directly

**The Solution:**
- Manually install v1.0.9 or later FIRST
- Then test auto-update to newer versions

---

## 🎯 Testing Steps

### Step 1: Bootstrap (Manual Install)

**Install v1.0.9 manually:**
```bash
# Run the installer:
release\1.0.9\Agentic Browser-Windows-1.0.9-Setup.exe
```

**Why v1.0.9?**
- ✅ Has `forceDevUpdateConfig = true`
- ✅ Has `ELECTRON_UPDATER_ALLOW_UNSIGNED` env var
- ✅ Has `disableDifferentialDownload = true`
- ✅ Will accept unsigned updates

---

### Step 2: Test Auto-Update (v1.0.9 → v1.0.10)

Once v1.0.9 is installed:

1. **Launch the app**
   - Opens automatically after install

2. **Wait 5-10 seconds**
   - Auto-updater checks for updates
   - Should detect v1.0.10 is available

3. **Check console** (if DevTools enabled)
   ```
   [AutoUpdater] Update available: 1.0.10
   [AutoUpdater] Environment: ELECTRON_UPDATER_ALLOW_UNSIGNED=true
   ```

4. **Click "Download Update"**
   - Downloads v1.0.10 installer
   - No signature verification errors!

5. **Click "Install Update"**
   - App quits
   - Installer runs
   - App restarts with v1.0.10

---

## 🔍 Expected Results

### ✅ Success Indicators:

**Console Output:**
```
[AutoUpdater] Initializing auto-updater...
[AutoUpdater] Current version: 1.0.9
[AutoUpdater] ⚠️ WARNING: Running in dev mode
[AutoUpdater] Environment: ELECTRON_UPDATER_ALLOW_UNSIGNED=true
[AutoUpdater] Checking for updates...
[AutoUpdater] Update available: 1.0.10
```

**No Errors Like:**
```
❌ "New version X is not signed by the application owner"
❌ "SignerCertificate": null
❌ "is not digitally signed"
```

---

## ❌ Troubleshooting

### Error: "Not signed by application owner"

**Cause:** Old version installed (v1.0.7 or v1.0.8)

**Fix:**
1. Uninstall current version
2. Install v1.0.9 manually
3. Test update to v1.0.10

---

### Error: Update not detected

**Check:**
1. Internet connection active
2. GitHub releases accessible
3. Version number is correct
4. Wait at least 10 seconds after launch

**Debug:**
```javascript
// In DevTools console:
console.log('Current version:', process.versions.electron);
```

---

### Error: Download fails

**Check:**
1. GitHub release exists: https://github.com/PawanOzha/upgradable-browser/releases
2. Assets uploaded correctly
3. Internet connection stable

---

## 🎬 Quick Test Script

```bash
# 1. Uninstall old version (if any)
# Windows Settings → Apps → Agentic Browser → Uninstall

# 2. Install v1.0.9
release\1.0.9\Agentic Browser-Windows-1.0.9-Setup.exe

# 3. Launch app
# - Wait 10 seconds
# - Update notification should appear

# 4. Update to v1.0.10
# - Click "Download"
# - Click "Install"
# - Verify v1.0.10 is running
```

---

## 📊 Version Compatibility Matrix

| From Version | To Version | Auto-Update | Notes |
|--------------|------------|-------------|-------|
| v1.0.7 | v1.0.8 | ❌ | No bypass configured |
| v1.0.7 | v1.0.9 | ❌ | No bypass configured |
| v1.0.7 | v1.0.10 | ❌ | No bypass configured |
| v1.0.8 | v1.0.9 | ❌ | No bypass configured |
| v1.0.8 | v1.0.10 | ❌ | No bypass configured |
| **v1.0.9** | **v1.0.10** | ✅ | **Works!** |
| **v1.0.9** | **v1.0.11+** | ✅ | **Works!** |
| **v1.0.10** | **v1.0.11+** | ✅ | **Works!** |

**Key Insight:** You need v1.0.9+ installed to test auto-updates.

---

## 🚀 Production Deployment Strategy

**Phase 1: Bootstrap (Manual)**
- Distribute v1.0.9+ installer manually
- Email users, website download, etc.
- Ensure everyone gets the "bootstrap" version

**Phase 2: Auto-Update (Automatic)**
- Release v1.0.10, v1.0.11, etc. on GitHub
- Users on v1.0.9+ auto-update seamlessly
- No manual intervention needed

---

## 📝 Summary

**The Rule:**
> To test auto-update, you MUST have v1.0.9 or later installed first.

**Why:**
> Older versions (v1.0.7, v1.0.8) don't have signature bypass configured.

**Solution:**
> Install v1.0.9 manually, then test auto-update to v1.0.10.

---

## ✅ Next Steps

1. **Now:** Install v1.0.9 manually
2. **Wait:** Build completes for v1.0.10
3. **Test:** Launch v1.0.9 and let it update to v1.0.10
4. **Verify:** Check that v1.0.10 is running

Once this works, all future versions (v1.0.11, v1.0.12, etc.) will auto-update seamlessly!
