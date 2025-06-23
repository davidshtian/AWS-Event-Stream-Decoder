# AWS Event Stream Decoder

A web-based tool for parsing and decoding AWS Event Stream binary data, particularly useful for debugging AWS Bedrock API streaming responses.

![image](https://github.com/user-attachments/assets/ab962afd-8b7b-4b9d-b221-241642034135)

## 🔍 Overview

This tool provides a user-friendly interface to decode binary messages that follow the [AWS Event Stream Protocol](https://github.com/awslabs/aws-c-event-stream/?tab=readme-ov-file#encoding). It's especially valuable for debugging AWS Bedrock API streaming responses, automatically parsing message headers, validating CRC checksums, and displaying payload content in a readable format.

<img width="856" alt="image" src="https://github.com/user-attachments/assets/4be68456-bc3a-41ab-96ac-c7f9cb728d76" />

## ✨ Features

- **Python Bytes Format Support**: Designed specifically for Python bytes format input
- **Smart Header Processing**: Automatically detects and adds byte length prefixes for header names when needed
- **CRC Validation**: Verifies both prelude and message CRC checksums
- **JSON Formatting**: Automatically formats JSON payloads for better readability
- **Mixed Input Support**: Handles multiple message segments in a single input
- **Detailed Analysis**: Shows complete message structure including headers, payload, and metadata
- **Error Handling**: Gracefully handles incomplete or malformed messages

## 🚀 Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- No additional dependencies required - runs entirely in the browser

### Usage

1. Open `aws-event-stream-decoder.html` in your web browser
2. Paste your AWS Event Stream binary data into the input textarea
3. Click "🔍 Parse Messages" to decode the data
4. View the parsed results with detailed message information

### Supported Input Format

#### Python Bytes Format
```python
b'\x00\x00\x00\x95\x00\x00\x00R\xf9\xa1G\xd1\x0b:event-type\x07\x00\x0cmessageStart'
```

You can also input multiple message segments:
```python
b':content-type\x07\x00\x10application/json'
b'\x0d:message-type\x07\x00\x05event...'
```

## 🛠️ Technical Details

### AWS Event Stream Protocol

The tool implements the AWS Event Stream Protocol specification, which consists of:

1. **Prelude** (8 bytes): Total length + Header length
2. **Prelude CRC** (4 bytes): CRC32 checksum of prelude
3. **Headers**: Variable-length key-value pairs
4. **Payload**: Message content
5. **Message CRC** (4 bytes): CRC32 checksum of entire message (excluding message CRC)

### Header Processing Intelligence

The tool automatically handles header byte length prefixes:

- **Auto-detection**: Determines if headers already include byte length prefix
- **Smart Addition**: Adds appropriate byte length for common headers:
  - `:message-type` (13 bytes) → `\x0d`
  - `:content-type` (13 bytes) → `\x0d`
  - `:event-type` (11 bytes) → `\x0b`
  - `:error-code` (11 bytes) → `\x0b`
  - `:error-message` (14 bytes) → `\x0e`
- **Preservation**: Keeps existing byte length prefixes when detected

### CRC32 Validation

Uses the standard IEEE 802.3 CRC32 algorithm (same as GZIP) to validate:
- Prelude integrity
- Complete message integrity

## 📊 Output Information

For each parsed message, the tool displays:

### Message Overview
- Event type and message type
- Message index and status

### Prelude Information
- Total message length
- Header section length
- Payload length
- Byte offset position
- CRC validation results

### Header Details
- All header key-value pairs
- Header metadata (type, lengths)
- Truncation indicators if applicable

### Payload Content
- Formatted JSON (when applicable)
- Raw hexadecimal for binary data
- UTF-8 decoded text

## 📝 Example Usage

### Input
```python
b'\x00\x00\x00\x95\x00\x00\x00R\xf9\xa1G\xd1\x0b:event-type\x07\x00\x0cmessageStart'
b':content-type\x07\x00\x10application/json'
b':message-type\x07\x00\x05event{"p":"abcdefghijklmnopqrstuvwx","role":"assistant"}\xfb\xd5\xa5\xb0'
```

### Output
The tool will parse and display:
- Message structure with validated CRCs
- Headers: `:event-type`, `:content-type`, `:message-type`
- JSON-formatted payload
- Complete message statistics

## 🔗 References

- [AWS Event Stream Protocol Specification](https://github.com/awslabs/aws-c-event-stream/?tab=readme-ov-file#encoding)
- [AWS Bedrock API Documentation](https://docs.aws.amazon.com/bedrock/latest/APIReference/)
- [CRC32 Algorithm (IEEE 802.3)](https://en.wikipedia.org/wiki/Cyclic_redundancy_check)

---

**Note**: This tool processes data entirely in your browser - no data is sent to external servers, ensuring privacy and security of your AWS event stream data.
