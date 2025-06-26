# JWT Robot Framework Library

[![CI](https://github.com/yourusername/jwt-robotframework-library/workflows/CI/badge.svg)](https://github.com/yourusername/jwt-robotframework-library/actions)
[![PyPI version](https://badge.fury.io/py/robotframework-jwtlibrary.svg)](https://badge.fury.io/py/robotframework-jwtlibrary)
[![Python Support](https://img.shields.io/pypi/pyversions/robotframework-jwtlibrary.svg)](https://pypi.org/project/robotframework-jwtlibrary/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Downloads](https://pepy.tech/badge/robotframework-jwtlibrary)](https://pepy.tech/project/robotframework-jwtlibrary)

A comprehensive Robot Framework library for JSON Web Token (JWT) operations, enabling robust testing of JWT-based authentication and authorization in your test automation.

## 🚀 Features

- **Complete JWT Lifecycle**: Generate, decode, validate, and analyze JWT tokens
- **Multiple Algorithms**: Support for HMAC, RSA, ECDSA, and PSS algorithms
- **Advanced Validation**: Comprehensive claim validation, expiration checking, and signature verification
- **Security-First**: Built-in protection against common JWT vulnerabilities
- **Easy Integration**: Simple keyword interface designed for Robot Framework
- **Extensive Documentation**: Complete keyword reference with examples
- **Error Handling**: Graceful error handling with detailed error messages
- **Performance Optimized**: Efficient token operations for test automation

## 📦 Installation

### Using pip

```bash
pip install robotframework-jwtlibrary
```

### From source

```bash
git clone https://github.com/yourusername/jwt-robotframework-library.git
cd jwt-robotframework-library
pip install -e .
```

### Dependencies

- Python 3.7+
- Robot Framework 4.0+
- PyJWT 2.0+

## 🏃 Quick Start

### Basic Usage

```robotframework
*** Settings ***
Library    JWTLibrary

*** Variables ***
${SECRET_KEY}    your-secret-key-here

*** Test Cases ***
Basic JWT Operations
    # Create payload
    ${payload}=    Create Dictionary    user_id=123    role=admin
    
    # Generate token
    ${token}=    Generate JWT Token    ${payload}    ${SECRET_KEY}
    
    # Decode and verify
    ${decoded}=    Decode JWT Payload    ${token}    ${SECRET_KEY}
    Should Be Equal    ${decoded['user_id']}    123
    
    # Validate token
    ${is_valid}=    Verify JWT Token    ${token}    ${SECRET_KEY}
    Should Be True    ${is_valid}
```

### Advanced Example

```robotframework
Advanced JWT Validation
    # Create comprehensive payload
    ${payload}=    Create Dictionary
    ...    iss=auth-service
    ...    sub=user-12345
    ...    aud=api-service
    ...    user_id=12345
    ...    role=admin
    ...    permissions=["read", "write", "delete"]
    
    # Generate token with custom expiration
    ${token}=    Generate JWT Token    ${payload}    ${SECRET_KEY}    expiration_hours=2
    
    # Comprehensive validation
    ${exp_info}=    Check JWT Expiration    ${token}
    Should Be Equal    ${exp_info['is_expired']}    ${False}
    
    ${claims_valid}=    Validate JWT Claims    ${token}    
    ...    {"role": "admin", "user_id": 12345}    ${SECRET_KEY}    ${True}
    Should Be True    ${claims_valid}
    
    ${aud_valid}=    Validate JWT Audience    ${token}    api-service
    Should Be True    ${aud_valid}
```

## 📚 Available Keywords

### Token Generation
- `Generate JWT Token` - Creates JWT tokens with custom payloads
- `Generate JWT Token With Claims` - Creates tokens using keyword arguments
- `Generate JWT Token Without Expiration` - Creates non-expiring tokens
- `Generate JWT Token With Custom Expiration` - Creates tokens with specific expiration

### Token Decoding
- `Decode JWT Payload` - Decodes token payloads with optional verification
- `Decode JWT Header` - Decodes token headers
- `Get JWT Claim` - Extracts specific claims from tokens
- `Get Multiple JWT Claims` - Extracts multiple claims
- `Extract All JWT Claims` - Gets all claims with metadata

### Token Validation
- `Verify JWT Token` - Validates token signatures and expiration
- `Check JWT Expiration` - Checks token expiration status
- `Validate JWT Claims` - Validates expected claim values
- `Check JWT Algorithm` - Validates token algorithm
- `Validate JWT Structure` - Validates token format
- `Check JWT Not Before` - Validates nbf claim
- `Validate JWT Audience` - Validates audience claim

### Utilities
- `Create JWT Payload` - Helper to create payload dictionaries
- `Get JWT Token Info` - Gets comprehensive token information
- `Compare JWT Tokens` - Compares two tokens
- `Extract JWT Timestamps` - Extracts timestamp claims
- `Generate Current Timestamp` - Creates current timestamp
- `Generate Future Timestamp` - Creates future timestamp
- `Convert Timestamp To Datetime` - Converts timestamps to datetime

## 🔧 Supported Algorithms

| Family | Algorithms | Description |
|--------|------------|-------------|
| HMAC | HS256, HS384, HS512 | Symmetric signing |
| RSA | RS256, RS384, RS512 | Asymmetric signing |
| ECDSA | ES256, ES384, ES512 | Elliptic curve signing |
| PSS | PS256, PS384, PS512 | RSA-PSS signing |

## 🎯 Use Cases

### API Testing
```robotframework
Test API Authentication
    ${token}=    Generate JWT Token    {"user_id": 123}    ${API_SECRET}
    
    # Use token in API requests
    ${headers}=    Create Dictionary    Authorization=Bearer ${token}
    ${response}=    GET    ${API_URL}/protected    headers=${headers}
    Should Be Equal As Integers    ${response.status_code}    200
```

### Microservices Testing
```robotframework
Test Service-to-Service Communication
    ${service_payload}=    Create Dictionary
    ...    iss=service-a
    ...    aud=service-b
    ...    scope=read:data
    
    ${service_token}=    Generate JWT Token    ${service_payload}    ${SERVICE_SECRET}
    
    # Validate token at receiving service
    ${claims_valid}=    Validate JWT Claims    ${service_token}
    ...    {"iss": "service-a", "aud": "service-b"}
    Should Be True    ${claims_valid}
```

### Security Testing
```robotframework
Test Token Security
    ${token}=    Generate JWT Token    {"user_id": 123}    ${SECRET_KEY}
    
    # Test with tampered token
    ${tampered_token}=    Replace String    ${token}    .    X    count=1
    ${is_valid}=    Verify JWT Token    ${tampered_token}    ${SECRET_KEY}
    Should Be Equal    ${is_valid}    ${False}
    
    # Test token expiration
    ${expired_token}=    Generate JWT Token    {"user_id": 123}    ${SECRET_KEY}    
    ...    expiration_hours=0.001
    Sleep    1s
    ${exp_info}=    Check JWT Expiration    ${expired_token}
    Should Be True    ${exp_info['is_expired']}
```

## 🛡️ Security Considerations

- **Secret Management**: Never hardcode secrets in test files
- **Algorithm Validation**: Always verify the algorithm matches expectations
- **Expiration Checking**: Validate token expiration in security tests
- **Claim Validation**: Verify all security-relevant claims
- **Signature Verification**: Always verify signatures in production scenarios

## 📖 Documentation

- [Installation Guide](docs/installation.md)
- [User Guide](docs/user_guide/basic_usage.md)
- [API Reference](docs/api_reference/keywords.md)
- [Examples](examples/)
- [Contributing Guidelines](docs/contributing.md)

## 🔍 Examples

Check out the [examples directory](examples/) for comprehensive usage examples:

- [Basic Usage](examples/basic_usage/)
- [Advanced Features](examples/advanced_usage/)
- [Real-world Scenarios](examples/real_world_scenarios/)

## 🧪 Testing

Run the test suite:

```bash
# Install development dependencies
pip install -r requirements-dev.txt

# Run unit tests
pytest tests/unit/

# Run Robot Framework tests
robot tests/robot/acceptance/

# Run all tests
make test
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](docs/contributing.md) for details.

### Development Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/jwt-robotframework-library.git
cd jwt-robotframework-library

# Setup development environment
make dev-setup

# Install pre-commit hooks
pre-commit install

# Run tests
make test

# Check code quality
make lint
```

## 📝 Changelog

See [CHANGELOG.md](CHANGELOG.md) for a detailed history of changes.

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## 🌟 Support

- **Documentation**: [Read the docs](https://jwt-robotframework-library.readthedocs.io/)
- **Issues**: [GitHub Issues](https://github.com/yourusername/jwt-robotframework-library/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/jwt-robotframework-library/discussions)
- **Stack Overflow**: Tag your questions with `robotframework` and `jwt`

## 🏆 Acknowledgments

- Built on top of the excellent [PyJWT](https://github.com/jpadilla/pyjwt) library
- Inspired by the Robot Framework community
- Thanks to all contributors and users

## 📊 Project Stats

![GitHub stars](https://img.shields.io/github/stars/yourusername/jwt-robotframework-library?style=social)
![GitHub forks](https://img.shields.io/github/forks/yourusername/jwt-robotframework-library?style=social)
![GitHub issues](https://img.shields.io/github/issues/yourusername/jwt-robotframework-library)
![GitHub pull requests](https://img.shields.io/github/issues-pr/yourusername/jwt-robotframework-library)

---

**Made with ❤️ for the Robot Framework community**
