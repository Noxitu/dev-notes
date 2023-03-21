This page explains the algorithm used in TOTP, together with example (not real) intermeiate values. Code in Python will be used

# Resources

TOTP relies on following resources:

 - RFC 6238: TOTP: Time-Based One-Time Password Algorithm - main TOTP specification
 - RFC 4226: HOTP: An HMAC-Based One-Time Password Algorithm - main HOTP specification, which TOTP buils upon
 - RFC 4648: The Base16, Base32, and Base64 Data Encodings - definition of base32 encoding, which is de facto standard encoding for secrets
 - [Google Authenticator / Key Uri Format](https://github.com/google/google-authenticator/wiki/Key-Uri-Format) - typical content included in TOTP QR codes

# Example

A example of a single, TOTP generation looks as follows. Value names are same as in the code below.

```
>>> totp('c', 1679428478.26)
key        = [c1 59 e2 36 43 d2 fe b5 d1 df]

timestamp  = 1679428478.260000
timestamp* = 2023-03-21T19:54:38.260000+00:00
counter    = 55980949

message    = [00 00 00 00 03 56 33 95]
hash       = [67 9c d7 8c e6 5d f6 b1 91 4c ac 1e 8a 9a 3b f7 d2 eb 3b cd]
                                                                        ^ 
offset     = 0xd

indices    = [00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f 10 11 12 13]
hash       = [67 9c d7 8c e6 5d f6 b1 91 4c ac 1e 8a 9a 3b f7 d2 eb 3b cd]
                                                     ^^ ^^ ^^ ^^
truncated  = [9a 3b f7 d2]
truncated* = [1a 3b f7 d2]
value      = 0x1a3bf7d2
value*     = 440137682
code       = 137_682
```

# Code

TOTP keys are generally random bytes, so for convinience they are encoded as base32. Base32 works nice for this purpose as it doesn't allow for easy mistakes - only upper case letters is used, and digits similar to letters (`0`, `1` and `8`) are not used.

Some authenticator implementations do accept secrets without padding. Microsoft Authenticator did accept `AE` (as `AE======` representing number 1), but it also allowed for `B` that was invalid and generated same codes as `AA` (representing number 0).

```py
def _decode_secret(secret):
    n = -len(secret) % 8
    return base64.b32decode(secret + n * '=')
```

TOTP defines only the usage of seconds since Epoch divided by 30 to get counter input as required by HOTP:

```py
def totp(secret, timestamp=None, *, timestep=30):
    if timestamp is None:
        timestamp = time.time()
    
    counter = int(timestamp / timestep)
    
    return hotp(secret, counter)
```

HOTP algorithm relies fully on HMAC with SHA1 digest. Counter is 8-byte integer, that is encoded in Big-Endian for HMAC.

After truncating hash, result is interpreted as 31-bit integer (again using Big-Endian).

```py
def hotp(secret, counter):
    key = _decode_secret(secret)
    message = struct.pack('>Q', counter)
    hash = hmac.digest(key, message, 'sha1')
    value = struct.unpack('>I', _dynamic_truncate(hash))[0] & 0x7fffffff
    code = value % 1_000_000
    
    return code
```

Dynamic truncation uses 4 least significant bits of last byte of hash to determine offset at which 4 bytes should be selected from the hash.

```py
def _dynamic_truncate(data):
    offset = data[-1] & 0xf

    # Note: RFC suggests discarding most significant bit here, to avoid problems with sign.
    truncated = data[offset: offset+4]
    
    return truncated
```

## Print statements

```py
    print(f'offset     = {hex(offset)}')
    print(f'truncated  = {_my_hex(truncated)}')

    print(f'key        = {_my_hex(key)}')
    print(f'message    = {_my_hex(message)}')
    print(f'hash       = {_my_hex(hash)}')
    print(f'value      = 0x{value:04x}')
    print(f'value*     = {value}')
    print(f'code       = {code:,}'.replace(',', '_'))

    print(f'timestamp  = {timestamp:.6f}')
    print(f'timestamp* = {datetime.datetime.fromtimestamp(timestamp, tz=datetime.timezone.utc).isoformat()}')
    print(f'counter    = {counter}')
```

# URI

Following URI format is encoded as QR code for adding secret to authenticators:

    otpauth://totp/<Issuer>:<User>?secret=<Secret>&issuer=<Issuer>
    
For example, if secret used in example was provided by GitHub (for my account), it would use following URI:

    otpauth://totp/GitHub:Noxitu?secret=YFM6ENSD2L7LLUO7&issuer=GitHub

More detailed specification of this URI, as well as some more (although not reasonably supported) fields are documented on [Google Authenticator / Key Uri Format wiki page](https://github.com/google/google-authenticator/wiki/Key-Uri-Format).
