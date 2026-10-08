# Django PBKDF2 Password Authentication Compatibility

To ensure zero user interruption, FuraOJ v2.0 implements native cryptographic verification of Django PBKDF2/SHA256 password hashes.

## Hash String Format

Django stores password hashes in the format:

```
pbkdf2_sha256$<iterations>$<salt>$<base64_encoded_hash>
```

For example:
```
pbkdf2_sha256$260000$furaojsalt123456$hfk5jpjQnNJqDoI7Emmu7fOoBXuxQuaoy/MO0+Nbtms=
```

## Rust Verification Implementation

The `src/auth/password.rs` module implements this standard using constant-time comparison to prevent timing side-channel attacks:

```rust
pub fn verify_django_password(password: &str, encoded: &str) -> bool {
    let parts: Vec<&str> = encoded.split('$').collect();
    if parts.len() != 4 || parts[0] != "pbkdf2_sha256" {
        return false;
    }
    let iterations: u32 = match parts[1].parse() {
        Ok(i) => i,
        Err(_) => return false,
    };
    let salt = parts[2].as_bytes();
    let expected_hash = match BASE64.decode(parts[3]) {
        Ok(h) => h,
        Err(_) => return false,
    };

    let mut derived_key = vec![0u8; expected_hash.len()];
    pbkdf2_hmac::<Sha256>(password.as_bytes(), salt, iterations, &mut derived_key);

    derived_key.ct_eq(&expected_hash).into()
}
```

This guarantees existing user accounts can sign in seamlessly without resetting their passwords.
