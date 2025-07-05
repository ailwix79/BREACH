# BREACH Attack Implementation

## Overview

This project implements a BREACH (Browser Reconnaissance and Exfiltration via Adaptive Compression of Hypertext) attack demonstration. BREACH is a security vulnerability that exploits HTTP compression to extract sensitive information from encrypted HTTPS responses.

## ⚠️ Security Notice

**This code is for educational and research purposes only.** It demonstrates a known vulnerability for learning about web security. Do not use this code for malicious purposes or against systems you don't own or have explicit permission to test.

## How It Works

The BREACH attack exploits the fact that HTTP compression algorithms (like gzip) compress repeated patterns more efficiently. By observing the compressed response length, an attacker can infer whether their guessed characters match the secret data.

### Attack Methodology

1. **Baseline Measurement**: Establishes the original response length without test characters
2. **Character Testing**: Tests each possible character from a predefined ASCII set
3. **Compression Analysis**: Compares response lengths to detect compression efficiency changes
4. **Secret Extraction**: Builds the secret character by character based on compression patterns
5. **Collision Handling**: Uses dynamic padding to resolve ambiguous results

## Code Structure

### Key Components

- **`get_length()`**: Sends HTTP requests and measures raw response length
- **`test_char()`**: Tests individual characters and updates verdict array
- **`execute_threads()`**: Manages parallel testing of all possible characters
- **`append_secret()`**: Adds discovered characters to the extracted secret

### Configuration Variables

- `multiplier`: Controls spacing multiplier for padding
- `secret`: Stores the progressively discovered secret
- `scope`: Range for random multiplier generation
- `spacing`: Anti-Huffman padding characters
- `url`: Target URL with vulnerable parameter
- `ascii_list`: Possible ASCII values for the secret

## Requirements

```python
import random
import requests
import threading
import time
```

## Usage

### Basic Execution

```python
python breach_attack.py
```

### Target Configuration

Update the `url` variable to point to your test target:

```python
url = "http://your-test-server.com/vulnerable-endpoint/?param="
```

### Character Set Customization

Modify `ascii_list` to include characters relevant to your target:

```python
ascii_list = [39, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 97, 98, 99, 100, 101]
# Represents: ', 0-9, a-e
```

## Example Output

```
[INFO] EXTRACTED SECRET: b
[INFO] EXTRACTED SECRET: bb
[INFO] EXTRACTED SECRET: bb6
[INFO] EXTRACTED SECRET: bb63
[WARN] CHANGED MULTIPLIER TO 89
[INFO] EXTRACTED SECRET: bb63e4ba67e24dab81ed425c5a95b7a2'
[END] Expected ending character detected. Program finished.
[END] Elapsed time is 24.564345121383667 seconds.
```

## Key Features

- **Multi-threaded**: Parallel testing of character possibilities
- **Adaptive Padding**: Dynamic collision resolution
- **Progress Tracking**: Real-time secret extraction display
- **Collision Detection**: Handles ambiguous compression results
- **Timing Analysis**: Performance measurement

## Limitations

- Requires HTTP compression to be enabled on the target
- Success depends on the predictability of the secret format
- May produce false positives with certain padding configurations
- Network latency can affect timing-based measurements

## Defense Strategies

To protect against BREACH attacks:

1. **Disable HTTP Compression**: Turn off compression for sensitive responses
2. **CSRF Tokens**: Use unpredictable, per-request tokens
3. **Padding**: Add random padding to responses
4. **Separate Domains**: Isolate sensitive data from user-controlled content
5. **Length Hiding**: Implement consistent response lengths

## Educational Context

This implementation demonstrates:
- Side-channel attack principles
- HTTP compression vulnerabilities
- Timing-based information extraction
- Multi-threaded attack optimization
- Collision resolution techniques

## References

- [BREACH Attack Paper](https://breachattack.com/)
- [RFC 7457 - Summarizing Known Attacks on TLS](https://tools.ietf.org/html/rfc7457)
- [OWASP - Testing for Padding Oracle](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle)

## License

This project is provided for educational purposes. Use responsibly and in accordance with applicable laws and regulations.

## Contributing

If you find improvements or have educational enhancements, please submit pull requests or open issues for discussion.

---

**Remember**: Always obtain proper authorization before testing security vulnerabilities on any system.
