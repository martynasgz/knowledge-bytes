# bcrypt

## Case

Attackers can use a dictionary attack - using a pre-arranged list of words (a rainbow table), such as the entries from the English dictionary, with their computed hash, the attacker easily compares the hashes from a stolen passwords table with every hash on the list.

## Solution

Add a different salt to each password before the hash, so the attacker needs to compute a different rainbow table for each user.

## How It Works

1. Generate a random salt. A "cost" factor has been pre-configured. Collect a password.
2. Derive an encryption key from the password using the salt and cost factor. Use it to encrypt a well-known string. Store the cost, salt, and cipher text. Because these three elements have a known length, it's easy to concatenate them and store them in a single field, yet be able to split them apart later.
3. When someone tries to authenticate, retrieve the stored cost and salt. Derive a key from the input password, cost and salt. Encrypt the same well-known string. If the generated cipher text matches the stored cipher text, the password is a match.

Bcrypt operates in a very similar manner to more traditional schemes based on algorithms like PBKDF2. The main difference is its use of a derived key to encrypt known plain text; other schemes (reasonably) assume the key derivation function is irreversible, and store the derived key directly.

Stored in the database, a bcrypt "hash" might look something like this:

> $2a$10$vI8aWBnW3fID.ZQ4/zo1G.q1lRps.9cGLcZEiGDMVr5yUP1KUOYTa

This is actually three fields, delimited by "$":

- `2a` identifies the `bcrypt` algorithm version that was used.
- `10` is the cost factor; $2^{10}$ iterations of the key derivation function are used (which is not enough, by the way. I'd recommend a cost of 12 or more.)
- `vI8aWBnW3fID.ZQ4/zo1G.q1lRps.9cGLcZEiGDMVr5yUP1KUOYTa` is the salt and the cipher text, concatenated and encoded in a modified Base-64. The first 22 characters decode to a 16-byte value for the salt. The remaining characters are cipher text to be compared for authentication[^1].

[^1]: [Stack Overflow answer](https://stackoverflow.com/a/6833165).