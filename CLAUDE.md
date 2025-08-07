# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains `sx`, a bash script that serves as a command-line wrapper for the OSINT.SX API service. The script performs OSINT (Open Source Intelligence) searches on emails, phone numbers, and usernames across multiple platforms and services.

## Architecture

- **Single Script**: The entire functionality is contained in one bash script (`sx`)
- **API Wrapper**: Acts as a client for the OSINT.SX REST API (https://api.osint.sx/search)
- **Output Formatting**: Provides both formatted human-readable output and raw JSON stream output
- **Service Detection**: Contains extensive pattern matching for 50+ services including social media, gaming, productivity, and financial platforms

## Configuration

- **API Key**: Requires `OSINT_SX_API_KEY` environment variable to be set
- **No config files**: All configuration is handled through environment variables and command-line arguments

## Usage

```bash
# Basic usage
./sx email user@domain.com
./sx username johndoe  
./sx phone +15551234567

# JSON output for programmatic use
./sx --json email user@domain.com
```

## Key Components

### Service Formatters (lines 58-453)
The `format_result()` function contains platform-specific formatters for services like:
- Google, PayPal, GitHub, Twitter/X, Discord
- Gaming platforms (Twitch, Roblox, Minecraft)
- Social media (TikTok, Snapchat, Instagram)
- Professional tools (Trello, Adobe, Medium)

### API Integration
- POST requests to OSINT.SX API with JSON payloads
- Streams results in real-time as they arrive
- Handles both registration checks and detailed profile data

## Development Notes

- **Dependencies**: Requires `curl` and `jq` for API calls and JSON processing
- **Error Handling**: Basic error handling for missing API keys and malformed arguments
- **Output**: Uses ANSI color codes for terminal formatting
- **Temporary Files**: Uses `/tmp/sx_registered_$$` for collecting registration data

## Testing

Test the script functionality:
```bash
# Test help output
./sx --help

# Test with invalid arguments (should show usage)
./sx

# Test JSON mode (requires valid API key)
./sx --json email test@example.com
```