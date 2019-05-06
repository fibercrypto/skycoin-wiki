Salt sources are XORed with entropy sources in an "entropy pool mixer" to harden the RNG.
If the entropy source has some hidden structure that makes it vulnerable to attack, these
salt sources will interfere with that attack.

The entropy pool mixer takes in the sha256 hash of a salt value and XORs it with the sha256 hash of entropy gathered 
from the system's RNG. Each subsequent call will mix in more entropy, and optionally more salt values, if available.

Example salt values are process ID, system time, environment variables, an incrementing program counter, thermal sensors,
and hardware IDs. Salt values that are static, such as hardware IDs, can be mixed during initialization.  Salt values that
are not static such as thermal sensor readings can be mixed in more frequently.

The XOR mixing mechanism cannot reduce the entropy of the output. Even if the salt is entirely predictable, it will
not reduce the entropy of the original value obtained from system's RNG.

This salt mixing is not a primary security defense, but is a backup defense against certain kinds of RNG attacks.
