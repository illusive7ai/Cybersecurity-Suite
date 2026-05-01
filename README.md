# Cybersecurity-Suite
Multi-Tool Security Toolkit | Encryption | Hashing | Encoding

---

## Table of Contents

1. Overview
2. Features
3. Technical Specifications
4. Installation
5. Usage Guide
6. Module Documentation
7. Security Architecture
8. Browser Compatibility
9. Screenshots
10. Keyboard Shortcuts
11. Troubleshooting
12. Security Notice
13. License

---

## Overview

CYBER SUITE is a client-side, browser-based security toolkit that provides cryptographic operations, encoding utilities and hashing functions for security professionals, developers and system administrators. The tool operates entirely within the user's browser using the Web Crypto API, ensuring that sensitive data never leaves the local machine.

The suite includes Base64 encoding/decoding, AES-GCM encryption/decryption and SHA-256/SHA-512 hashing capabilities, all presented through a modern glassmorphism interface with real-time character tracking and operation history.

---

## Features

### Core Modules

| Module | Functionality | Key Features |
|--------|--------------|--------------|
| Base64 | Encoding & Decoding | Unicode support, file upload, drag-and-drop |
| AES-GCM | Symmetric Encryption | 256-bit keys, PBKDF2 key derivation, IV+salt embedding |
| Hashing | Cryptographic Digests | SHA-256, SHA-512, hexadecimal output |

### Additional Capabilities

- File drag-and-drop with automatic Base64 conversion
- Operation history with one-click output restoration
- Output export as .txt file
- Local storage persistence for operation history
- Keyboard shortcuts (Ctrl+Enter for primary action)
- Dark/Light theme toggle
- Client-side only operation - no data transmitted
- Responsive design for desktop and mobile devices

---

## Technical Specifications

### Cryptographic Standards

| Component | Standard | Implementation |
|-----------|----------|----------------|
| Encryption | AES-GCM (Galois/Counter Mode) | NIST SP 800-38D |
| Key Derivation | PBKDF2 | 100,000 iterations, SHA-256 |
| Key Length | 256-bit | AES-256 |
| IV Length | 96-bit (12 bytes) | Recommended for GCM |
| Salt Length | 128-bit (16 bytes) | Random per encryption |
| Hash Algorithms | SHA-256, SHA-512 | FIPS PUB 180-4 |

### Data Formats

| Operation | Input Format | Output Format |
|-----------|--------------|---------------|
| Base64 Encode | Plain text / Binary | Base64 string |
| Base64 Decode | Base64 string | UTF-8 text |
| AES Encrypt | Plain text | Base64-encoded JSON (iv+salt+ciphertext) |
| AES Decrypt | Base64 JSON object | Plain text |
| Hash | Plain text | Hexadecimal string |

---

## Installation

### Zero Installation Required

CYBER SUITE is a static HTML/CSS/JavaScript application that requires no server-side components, build steps, or package managers.

### Method 1: Direct Browser Access

Save the `index.html` file to your local machine and open it with any modern web browser:

```
File > Open File > Select index.html
```

### Method 2: Local Web Server (Optional)

For the most reliable experience, serve the file via a local HTTP server:

**Python 3:**
```bash
python -m http.server 8080
```

**Python 2:**
```bash
python -m SimpleHTTPServer 8080
```

**Node.js (http-server):**
```bash
npx http-server -p 8080
```

Then navigate to `http://localhost:8080`

### Method 3: Offline Usage

The application requires no internet connection except for:
- Loading Google Fonts (Poppins, Orbitron) - fails gracefully
- Web Crypto API - available offline

All cryptographic operations are performed locally using the browser's native Web Crypto API.

---

## Usage Guide

### Module Selection

Use the tab navigation at the top of the main card to switch between three modules:

| Tab | Purpose |
|-----|---------|
| Base64 | Convert between text and Base64 representation |
| AES-GCM | Encrypt or decrypt data using AES-256 in GCM mode |
| Hashing | Generate SHA-256 or SHA-512 cryptographic hashes |

### Base64 Module

**Encode to Base64:**

1. Enter plain text in the input textarea
2. Click the "Encode to Base64" button
3. The Base64 output appears in the output field
4. Use "Copy Output" to copy to clipboard

**Decode from Base64:**

1. Paste a Base64 string in the input textarea
2. Click the "Decode from Base64" button
3. The decoded text appears in the output field

**File Upload:**

- Drag and drop a file onto the drop zone
- Or click the drop zone to browse for a file
- File contents are converted to Base64 automatically

**Supported Characters:** Unicode (UTF-8) is fully supported for encoding and decoding.

### AES-GCM Module

**Encrypt Data:**

1. Enter plaintext in the input textarea
2. Enter a strong passphrase in the "Secret Key" field
3. Click the "Encrypt (AES-GCM)" button
4. The encrypted output appears as a Base64 string

**Decrypt Data:**

1. Paste the encrypted Base64 string in the input textarea
2. Enter the same passphrase used for encryption
3. Click the "Decrypt" button
4. The decrypted plaintext appears in the output field

**Key Security Notes:**

- Keys are never stored, transmitted, or logged
- Each encryption generates a unique random salt and IV
- Encrypted output contains the ciphertext, IV, and salt embedded together
- 100,000 PBKDF2 iterations provide defense against brute-force attacks
- AES-GCM provides authenticated encryption (integrity verification)

### Hashing Module

**Generate SHA-256 Hash:**

1. Enter text in the input textarea
2. Click the "SHA-256" button
3. The 64-character hexadecimal hash appears in the output field

**Generate SHA-512 Hash:**

1. Enter text in the input textarea
2. Click the "SHA-512" button
3. The 128-character hexadecimal hash appears in the output field

**Hash Characteristics:**

| Algorithm | Output Length | Security Level |
|-----------|---------------|----------------|
| SHA-256 | 64 hex chars (256 bits) | 128-bit collision resistance |
| SHA-512 | 128 hex chars (512 bits) | 256-bit collision resistance |

### General Features

**Operation History:**

- The last 5 operations are stored in the right sidebar
- Click any history entry to restore its output to the current module
- History persists across browser sessions via localStorage
- Use "Clear History" to remove all entries

**Download Output:**

- Click "Download Output as .txt" to save the current output field content
- Filename format: `cyber_output_[module].txt`

**Reset All:**

- Clears all input and output fields for the active module

**Theme Toggle:**

- Switch between dark and light themes using the button in the top-right corner
- Preference is not persisted (uses default dark theme on reload)

---

## Module Documentation

### Base64 Module

**Encoding Process:**

```
Input String → UTF-8 Encoding → Binary Data → Base64 Encoding → Output String
```

**Decoding Process:**

```
Base64 String → Base64 Decoding → Binary Data → UTF-8 Decoding → Output String
```

**API Reference:**

| Function | Description | Error Handling |
|----------|-------------|----------------|
| `btoa(unescape(encodeURIComponent(str)))` | Encodes UTF-8 strings to Base64 | Handles all Unicode characters |
| `decodeURIComponent(escape(atob(str)))` | Decodes Base64 to UTF-8 | Throws error on invalid input |

### AES-GCM Module

**Encryption Flow:**

```
Plaintext + Passphrase
        ↓
Random Salt Generation (16 bytes)
        ↓
PBKDF2 Key Derivation (100,000 iterations)
        ↓
Random IV Generation (12 bytes)
        ↓
AES-GCM Encryption
        ↓
JSON Structure {iv, salt, ciphertext}
        ↓
Base64 Encoding
        ↓
Output String
```

**Decryption Flow:**

```
Base64 Input + Passphrase
        ↓
JSON Parsing
        ↓
IV + Salt Extraction
        ↓
PBKDF2 Key Derivation
        ↓
AES-GCM Decryption
        ↓
Plaintext Output
```

**Security Parameters:**

| Parameter | Value | Justification |
|-----------|-------|---------------|
| PBKDF2 Iterations | 100,000 | OWASP recommended minimum (2024) |
| Salt Length | 128 bits | Prevents rainbow table attacks |
| IV Length | 96 bits | Standard for AES-GCM |
| Key Length | 256 bits | Maximum AES key size |
| Authentication | GCM mode | Provides integrity checking |

### Hashing Module

**Process Flow:**

```
Input String → UTF-8 Encoding → Web Crypto digest() → Hex Encoding → Output
```

**Algorithm Specifications:**

| Algorithm | Block Size | Output Size | Internal State |
|-----------|------------|-------------|----------------|
| SHA-256 | 512 bits | 256 bits | 256 bits |
| SHA-512 | 1024 bits | 512 bits | 512 bits |

---

## Security Architecture

### Client-Side Only Operation

```
┌─────────────────────────────────────────────────────────────┐
│                      User's Browser                         │
│  ┌─────────────────────────────────────────────────────────┐│
│  │                   CYBER SUITE Application               ││
│  │  ┌───────────┐  ┌───────────┐  ┌─────────────────────┐  ││
│  │  │  Base64   │  │   AES     │  │      Hashing        │  ││
│  │  │  Module   │  │  Module   │  │      Module         │  ││
│  │  └─────┬─────┘  └─────┬─────┘  └──────────┬──────────┘  ││
│  │        │              │                   │             ││
│  │        ▼              ▼                   ▼             ││
│  │  ┌─────────────────────────────────────────────────────┐││
│  │  │              Web Crypto API (Native)                │││
│  │  │  - AES-GCM Encryption/Decryption                    │││
│  │  │  - PBKDF2 Key Derivation                            │││
│  │  │  - SHA-256/SHA-512 Hashing                          │││
│  │  └─────────────────────────────────────────────────────┘││
│  └─────────────────────────────────────────────────────────┘│
│                           │                                 │
│                     NO DATA TRANSMITTED                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Data Protection Guarantees

| Concern | Implementation |
|---------|----------------|
| Data Transmission | None - all operations local |
| Key Storage | Never persisted or logged |
| Input Privacy | No analytics or telemetry |
| History Storage | LocalStorage only (user-controlled) |
| Third-party Requests | None (Google Fonts optional, fails gracefully) |

### Content Security Policy

The application implements a strict Content Security Policy:

```
default-src 'self'
script-src 'self' 'unsafe-inline'
style-src 'self' 'unsafe-inline'
img-src data:
font-src 'self' data:
connect-src 'none'
```

---

## Browser Compatibility

### Supported Browsers

| Browser | Minimum Version | Web Crypto Support |
|---------|----------------|--------------------|
| Chrome | 37+ | Full support |
| Firefox | 34+ | Full support |
| Safari | 11+ | Full support |
| Edge | 79+ | Full support |
| Opera | 24+ | Full support |

### Mobile Support

| Platform | Browser | Status |
|----------|---------|--------|
| iOS | Safari | Full support |
| Android | Chrome | Full support |
| Android | Firefox | Full support |

### Fallback Behavior

- Google Fonts: If unavailable, system fallback fonts are used
- Web Crypto API: Requires modern browser (no fallback for cryptographic operations)
- LocalStorage: History feature degrades gracefully if unavailable

---

## Screenshots

### Screenshot 1: Main Interface

[Screenshot](main.jpg)

*Description: The CYBER SUITE main interface showing the glassmorphism card design with three module tabs (Base64, AES-GCM, Hashing). The Base64 module is active, displaying input and output textareas with the character counter. The operation history sidebar is visible on the right with recent operations. The dark theme is active with cyan accent colors and animated particle background.*

**Capture instructions:** Open the index.html file in a modern web browser. Ensure the Base64 tab is selected. The animated particle background should be visible. Capture the entire application window including the header with title and theme toggle, the main glass card with input/output areas, the advanced actions section, and the history sidebar.

---

## Keyboard Shortcuts

| Shortcut | Action | Context |
|----------|--------|---------|
| Ctrl + Enter | Execute primary action | Global |
| - | Encode to Base64 (when Base64 active) | Base64 module |
| - | Encrypt (when AES active) | AES module |
| - | SHA-256 Hash (when Hash active) | Hashing module |

*Note: Tab switching requires mouse interaction.*

---

## Troubleshooting

### Common Issues and Solutions

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| Base64 decode fails | Invalid Base64 string | Verify the input contains only valid Base64 characters (A-Z, a-z, 0-9, +, /, =) |
| AES decryption fails | Incorrect passphrase | Passphrase must exactly match the one used for encryption |
| AES decryption fails | Corrupted ciphertext | Ensure the full Base64 string is copied without modification |
| File upload not working | Browser security restrictions | Use drag-and-drop or file picker - both should work |
| History not persisting | LocalStorage disabled/private mode | History will work for current session only |
| Particles animation lag | Low-performance device | Animation is cosmetic; disable via browser if needed |
| Unicode characters display incorrectly | Encoding issue | Input/output uses UTF-8; should handle all Unicode |

### Debugging Tips

**Check Web Crypto API availability:**

Open browser console (F12) and run:
```javascript
console.log(window.crypto && window.crypto.subtle);
```
Returns `true` if supported.

**Verify localStorage access:**

```javascript
try {
    localStorage.setItem('test', 'test');
    console.log('localStorage available');
} catch(e) {
    console.log('localStorage unavailable');
}
```

---

## Security Notice

**Important Security Considerations**

1. **Key Management:** This tool does NOT store encryption keys. Users are responsible for securely managing their passphrases.

2. **Data Privacy:** All operations are performed locally in the browser. No data is transmitted to any server.

3. **No Warranty:** This tool is provided for educational and professional use. Users assume all responsibility for securing their data.

4. **Password Strength:** For AES encryption, use strong passphrases (minimum 12 characters, mixed case, numbers, symbols).

5. **IV Uniqueness:** Each encryption generates a new random IV. This is correct for GCM mode and prevents replay attacks.

6. **Authenticated Encryption:** AES-GCM provides both confidentiality and integrity protection. Tampering with ciphertext will cause decryption to fail.

7. **No Backdoors:** The source code is fully visible. No hidden functionality exists.

---

## License

This tool is provided open-source for educational and professional security purposes.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Current | Initial release: Base64, AES-GCM, SHA-256/512 modules, file upload, history, themes |

---

## File Structure

```
index.html          # Single-file application (all CSS, HTML, JavaScript)
```

No additional files, dependencies, or build artifacts are required.

---

## Acknowledgments

- Web Crypto API (W3C Specification)
- Font Awesome icons (via Unicode fallbacks)
- Google Fonts: Poppins, Orbitron

---

## Contact and Support

For issues, feature requests, or security concerns, please document:

- Browser name and version
- Operating system
- Steps to reproduce the issue
- Any error messages from browser console (F12)

---
