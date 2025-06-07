# Random-Number-Generator-

Overview

This project implements a self-contained SHA-256 hashing class and a custom random number generator (RNG) that uses a rolling 512-bit entropy buffer derived from CPU timing fluctuations. Designed without external dependencies, it provides cryptographic hashing and bit-level entropy capture purely in C++.

The code features:

A full implementation of the SHA-256 algorithm.

Digest outputs in both hexadecimal and binary string formats.

A rolling entropy window of 512 bits stored using a std::deque<int>.

A custom RNG that uses nanosecond-level timing variance to generate input entropy.

---

📌 Components

1. SHA256 Class

This class implements the complete SHA-256 hashing algorithm as specified in FIPS PUB 180-4. It processes arbitrary-length input and produces a 256-bit (32-byte) digest.

Key Methods

update(const uint8_t* data, size_t len): Feeds raw data into the hash.

update(const std::string& data): Convenience overload for strings.

digest() -> std::string: Returns the hash as a 64-character hexadecimal string.

digestBinary() -> std::string: Returns the hash as a 256-character binary string ('0' and '1').

reset(): Resets the internal state for reuse.


The internal SHA-256 state includes:

A working buffer of 512 bits (64 bytes).

Eight 32-bit hash words (h[0] to h[7]).

Standard constants K[64] used per round.

---

2. RandomNumberGenerator Class

This class generates bits using high-resolution timing from std::chrono::high_resolution_clock. Each bit is derived by comparing a short loop's execution time against a rolling average. If the current timing is above average, a '1' is recorded; otherwise, a '0'.

Entropy Collection

The entropy is accumulated in a 512-bit rolling buffer (std::deque<int>).

Once full, the buffer is packed into 64 bytes and passed to the SHA256 class.

The result is a digest in either:

Binary (256 bits),

Or Hexadecimal (64 characters).


Timing-based Entropy Algorithm

int bit = (duration < globalAvg) ? 0 : 1;

This simple yet effective comparison introduces variability from environmental and hardware-level noise (e.g., thermal fluctuations, CPU scheduling), making it a lightweight source of entropy.

---

🧪 Example Output

Once 512 bits have been collected, a SHA-256 hash is computed and printed.

Binary digest output example:

010101010110011001011011...

---

🔧 Technical Highlights

Pure C++17: No dependencies beyond the standard library.

Deterministic SHA-256: Matches known-good test vectors.

Bit-level entropy sourcing: Suitable for seeding or augmenting PRNGs.

Compact and modular: ~ 244 lines total for SHA-256 and RNG.

---

🔐 Use Cases

Lightweight random number generation for embedded or constrained environments.

Cryptographic preprocessing and seed derivation.

Hash-based fingerprinting of volatile system conditions.

---

📜 License

MIT License.



