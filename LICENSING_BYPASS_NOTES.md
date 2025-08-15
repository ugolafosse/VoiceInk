# VoiceInk License Bypass Implementation Notes

## Summary
Successfully bypassed the 7-day trial limitation in VoiceInk for personal use.

## What Was Done

### 1. Repository Reset
- Reset fork to match upstream BeingPax/VoiceInk exactly
- Removed "remove licensing" commit that was causing issues
- Created clean development branch: `bypass`

### 2. License Bypass Implementation
**Location**: `/Users/ugo/_nexus/labs/VoiceInk/VoiceInk/Models/LicenseViewModel.swift`

**Changes Made**:
```swift
// Line 35-38: Always show as licensed in UI
private func loadLicenseState() {
    licenseState = .licensed
    return  // Skip all other logic
}

// Line 72-74: Always allow app usage
var canUseApp: Bool {
    return true  // Always allow app usage - bypass trial limitation
}
```

### 3. Build Configuration
- **Team ID**: FFMQU322ZW
- **Bundle ID**: Changed from `com.prakashjoshipax.VoiceInk` to `com.ugolafosse.VoiceInk`
- **Certificate**: Apple Development: ugoboss33130@gmail.com (VT5F7LJ58Y)
- **Method**: Development signing with automatic provisioning

### 4. Build Issues Fixed
- Disabled `NativeAppleTranscriptionService.swift` (future macOS 26 APIs)
- File renamed to `.swift.disabled` to prevent compilation errors

## Why This Works
- The licensing system is **cosmetic only** - no actual feature restrictions
- `canUseApp` property is not actively checked anywhere else in codebase
- Permissions and core functionality are completely independent of licensing
- Clean separation between licensing logic and app functionality

## Installation
1. App exported to: `~/Desktop/VoiceInk.app`
2. Move to Applications folder
3. All permissions (microphone, accessibility, screen recording) work normally

## Reverting Changes
To restore original licensing:
1. Checkout main branch
2. Reset to upstream/main
3. The bypass changes are only in the `bypass` branch

## Important Notes
- This is for **personal use only**
- The app developer deserves support - consider purchasing if you use it regularly
- All functionality works normally with this bypass
- No UserDefaults manipulation needed
- No risk to permissions or app stability