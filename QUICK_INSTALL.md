# VoiceInk Quick Install (For Already Modified Code)

If you already have the `bypass` branch with the licensing modifications, here's the quick install process.

## Prerequisites
- Xcode installed
- Apple Developer certificate (free Apple ID works)

## Step 1: Clone and Get the Bypass Branch

```bash
# Clone your fork
git clone https://github.com/ugolafosse/VoiceInk.git
cd VoiceInk

# Checkout the bypass branch (already has the license modifications)
git checkout bypass

# Disable the problematic file
mv VoiceInk/Services/NativeAppleTranscriptionService.swift VoiceInk/Services/NativeAppleTranscriptionService.swift.disabled
```

## Step 2: Get Your Team ID

```bash
# Find your signing identity
security find-identity -v -p codesigning

# Get your team ID (replace with your email)
security find-certificate -c "your-email@example.com" -p | openssl x509 -noout -text | grep "Subject:" | grep -oE 'OU=[^,]+' | cut -d= -f2
```

## Step 3: Build and Install Script

Save this as `build_and_install.sh`:

```bash
#!/bin/bash

# CHANGE THESE VALUES
TEAM_ID="YOUR_TEAM_ID"  # Replace with your team ID (e.g., "FFMQU322ZW")
BUNDLE_ID="com.YOUR_USERNAME.VoiceInk"  # Replace with unique bundle ID

echo "Building VoiceInk..."

# Archive
xcodebuild -scheme VoiceInk \
    -configuration Release \
    archive \
    -archivePath ~/Desktop/VoiceInk.xcarchive \
    -allowProvisioningUpdates \
    DEVELOPMENT_TEAM="$TEAM_ID" \
    PRODUCT_BUNDLE_IDENTIFIER="$BUNDLE_ID"

# Create export options
cat > exportOptions.plist << EOF
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>method</key>
    <string>development</string>
    <key>teamID</key>
    <string>$TEAM_ID</string>
    <key>signingStyle</key>
    <string>automatic</string>
    <key>uploadBitcode</key>
    <false/>
    <key>uploadSymbols</key>
    <false/>
    <key>compileBitcode</key>
    <false/>
</dict>
</plist>
EOF

# Export
xcodebuild -exportArchive \
    -archivePath ~/Desktop/VoiceInk.xcarchive \
    -exportPath ~/Desktop \
    -exportOptionsPlist exportOptions.plist

# Install
cp -R ~/Desktop/VoiceInk.app /Applications/

# Clean up
rm -rf ~/Desktop/VoiceInk.xcarchive
rm -rf ~/Desktop/VoiceInk.app
rm exportOptions.plist

echo "✅ VoiceInk installed to /Applications/"
```

## Step 4: Run It

```bash
chmod +x build_and_install.sh
./build_and_install.sh
```

That's it! The app will be in `/Applications/VoiceInk.app`

## One-Liner After Setup

Once you've set up the script with your team ID, you can just run:

```bash
cd VoiceInk && ./build_and_install.sh
```