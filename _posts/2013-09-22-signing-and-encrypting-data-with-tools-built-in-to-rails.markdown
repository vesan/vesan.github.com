---
layout: post
title: Signing and encrypting data with tools built-in to Rails
tags: [programming, ruby, rails]
---

I have released that many people working with Ruby on Rails are not aware that it provides classes for signing and verifying messages and encrypting and decrypting them. `ActiveSupport::MessageVerifier` and `ActiveSupport::MessageEncryptor` are used by Rails to implement encrypted session stored in a browser cookie, but you can use them yourself to encrypt data.

Using both classes is simple. Here is an example of using `MessageVerifier`:

```ruby
verifier = ActiveSupport::MessageVerifier.new('your-secret')
message = "String that is prevented from tampering."
# Sign the message...
signed_message = verifier.generate(message)
# and verify it.
verified = verifier.verify(signed_message)

# Verified message equals the original message
verified == message #=> true
```

If the string is tampered `#verify` will raise `ActiveSupport::MessageVerifier::InvalidSignature` exception.

`MessageEncryptor` works in similar way:

```ruby
salt  = SecureRandom.random_bytes(64)
key   = ActiveSupport::KeyGenerator.new('password').
          generate_key(salt)

message = "Encrypted string."
encryptor = ActiveSupport::MessageEncryptor.new(key)
# Encrypt the message...
encrypted_message = encryptor.encrypt_and_sign(message)
# and decrypt it.
decrypted = encryptor.decrypt_and_verify(encrypted_message)

# Decrypted message equals the original message
decrypted == message #=> true
```

If the string is tampered `#decrypt_and_verify` will raise `ActiveSupport::MessageEncryptor::InvalidMessage` exception.

Documentation is available for both [MessageVerifier](http://api.rubyonrails.org/classes/ActiveSupport/MessageVerifier.html) and [MessageEncryptor](http://api.rubyonrails.org/classes/ActiveSupport/MessageEncryptor.html). For real world example of using these facilities you can view how Rails uses them to [implement encrypted cookies](https://github.com/rails/rails/blob/c91d266cdfcdc15d101d3d4360ef6c5a730d36e9/actionpack/lib/action_dispatch/middleware/cookies.rb#L425).

So the next time you need to encrypt data, you don't have to implement your own (probably insecure) solution but you can reach for ActiveSupport.
