# elevate-crypto

Pure-Rust crypto for the elevate tools: password hashing, file hashing and
signature checks. There's no OpenSSL dependency. The one exception is the
optional `system_crypt` feature, which calls the system `crypt(3)` for old
`/etc/shadow` hash formats that the Rust crates don't handle.

- **Argon2id** for password hashing, with salts from `getrandom`. bcrypt and
  SHA-crypt verification are there for existing shadow files.
- **Blake3** for hashing files, e.g. to notice changes to `/etc/passwd`,
  `/etc/shadow` and `/etc/group`.
- **Ed25519** for signing and verifying data.

## Usage

```rust
use elevate_crypto::{hash_password, verify_password};

let hash = hash_password("SecretPassword123!").unwrap();
assert!(verify_password("SecretPassword123!", &hash).unwrap_or(false));
```

Add it as a git dependency:

```toml
elevate-crypto = { git = "https://github.com/Zainium-Dynamics/elevate-crypto", tag = "v1.0.1" }
```

Features are `hash`, `sign`, `password`, `legacy_shadow` and `system_crypt`
(all on by default), plus `std` / `alloc`. Turn off `system_crypt` if you
don't want to link libcrypt.

Used by [elevate-pam](https://github.com/Zainium-Dynamics/elevate-privilege)
and elevate-umbra.

## License

MIT OR Apache-2.0, see [LICENSE-MIT](LICENSE-MIT) and
[LICENSE-APACHE](LICENSE-APACHE).
