Skycoin wallets are deterministic. The addresses are generated from a 256 bit seed. Minimum entropy of the wallet generation seed should be 256 bits.

A typical wallet seed is 256 bits, 64 characters in hex and appears as
```
f399bd1b78792da9cc49b1157c73016450c949df565ce3ddbf2f9d65fd8f0dac
```

This is difficult to commit to memory.
- writing down wallet seeds enables easy theft or confiscation
- printing wallet seeds is insecure as many computers save finished print jobs. Even deleted print jobs can be recovered using disc forensics techniques.
- printers will become targets hackers compromise to steal coins
- cloud printing reports print jobs to central authority. Future printers may clandestinely save or upload print jobs to central authorities, even for local print jobs. Cloud printing going through a central server is the only option for printing from Chromebook or on the iPad.
- printers are insecure and may be compromised or backdoored.
- printers are connected to the network and in the future will contain a full CPU and operating system. Governments will require mandatory printer backdoors, to enable monitoring, surveillance and as a vector for originating attacks on targets from within their own networks.
- all mobile phones contain several backdoors and exploits. Pictures of private key QR codes stored on phones should not be considered secure.
- the cellular phones and computers which currently used to store private keys may be subject to theft or confiscation. Confiscation of private keys at border crossings for profit, by corrupt officials may become common in the future.

Therefore we need a brain wallet standard that allows 
- wallet seeds to be committed to memory
- is robust to errors in memory (redundancy, error correction)
- does not rely upon computers, cell phones or printers
- allows wallets to optionally be committed to paper for secure long term storage
- has enough bits that the seed cannot be brute forced

The standard should allow a person to commit a wallet seed to paper, by hand and place the seed within a safe for long term cold storage. The standard should also allow individuals to commit the seed to memory if required.

## Graphical Wallet Representations

This is a proposal for a wallet "sigil" format for storing deterministic wallet seeds that 
- is easy to commit to paper by hand
- can be committed to memory
- has level of redundancy/error correction
- has simple implementation as hardware device (does not require keyboard, hardware wallet, separate private key storage/signature operations on to secure device).

## Standard Definition

This is a visual standard for representing wallet seeds in a visual format that can be committed to memory. The format is designed to be easy to remember and has built in redundancy to allow for error correction.

Draw a 3"x3" square. Sub divide the square into 4 quadrants (in blue ink). Each quadrant will contain a 3x3 or 4x4 grid of dots in black ink.

```
┌ ─ ─ ─ ┐ ─ ─ ─ ┐

│ · · · │ · · · │

│ · · · │ · · · │

│ · · · │ · · · │

└ ─ ─ ─ ┘ ─ ─ ─ ┘

│ · · · │ · · · │

│ · · · │ · · · │

│ · · · │ · · · │

└ ─ ─ ─ ┘ ─ ─ ─ ┘
```

Draw random lines between the dots (north, south, east, west), within the quadrants.

```
┌ ─ ─ ─ ┐ ─ ─ ─ ┐
      │     │     
│─·─·─·─│─· · · │
    │     │   │ 
│─·─·─· │ ·─·─· │
    │       │ │  
│ ·─· ·─│─·─· · │
      │     │    
└ ─ ─ ─ ┘ ─ ─ ─ ┘
    |       | |     
│─·─· · │─·─· · │
    |       │     
│ · ·─·─│─·─·─·─│
      │          
│─·─·─· │ · ·─·─│
      │     │     
└ ─ ─ ─ ┘ ─ ─ ─ ┘
```

Draw a number over the dots for how many lines are adjacent to the dots.

```
┌ ─ ─ ─ ┐ ─ ─ ─ ┐
      │     │     
│─2─3─3─│─2 1 1 │
    │     │   │ 
│─2─4─2─│ 2─3─3 │
    │       │ │  
│ 1─2 2─│─2─3 1 │
      │     │    
└ ─ ─ ─ ┘ ─ ─ ─ ┘
    |       | |     
│─2─3 0 │─2─3 1 │
    |       │     
│ 0 2─3─│─2─3─2─│
      │          
│─2─2─3 │ 0 2─2─│
      │     │     
└ ─ ─ ─ ┘ ─ ─ ─ ┘
```

Take a piece of papers and draw a square, with enough room for a 3x3 grid of numbers. The spacing between the numbers should equal the width and height of the numbers (on graph paper a 5x5 grid).

```
█████████
█░ ░ ░│░█
█─2─3─3─█
█░ ░│░ ░█
█─2─4─2─█
█░ ░│░ ░█
█ 1─2 2─█
█░ ░ ░│░█
█████████
```

Draw the grid and outline on a piece of paper. Commit the outline of a quadrant to memory.

```
┌───────┐
│░ ░ ░ ░│
│ · · · │
│░ ░ ░ ░│
│ · · · │
│░ ░ ░ ░│
│ · · · │
│░ ░ ░ ░│
└───────┘

█████████
█░ ░ ░│░█
█───┬─┴─█
█░ ░│░ ░█
█───┼───█
█░ ░│░ ░█
█ ──┘ ┌─█
█░ ░ ░│░█
█████████

┌─────┬─┐
│░ ░ ░│░│
│───┬─┴─│
│░ ░│░ ░│
│───┼───┤
│░ ░│░ ░│
│ ──┘ ┌─┤
│░ ░ ░│░│
└─────┴─┘

┌─────┬─┐
│░ ░ ░│░│
│─·─·─·─│
│░ ░│░ ░│
│─·─·─·─┤
│░ ░│░ ░│
│ ·─· ·─┤
│░ ░ ░│░│
└─────┴─┘
```

Redraw the quadrant with a number at each grid point. The number is number of lines adjacent to the dot

```
░│░    ░│░
 ·  is  1
░ ░    ░ ░

░ ░    ░ ░
─·─ is -2-
░ ░    ░ ░

░ ░    ░ ░
─·─ is -3-
░│░    ░ ░
```

So

```
┌─────┬─┐
│░ ░ ░│░│
│─2─3─3─│
│░ ░│░ ░│
│─2─4─2─┤
│░ ░│░ ░│
│ 1─2 2─┤
│░ ░ ░│░│
└─────┴─┘
```

Commit to memory the numbers and connecting dashes.

```
2 3 3
2 4 2
1 2 2 
```

Now attempt to draw the quadrant from memory. 

## Symbol Set

### 0-ary

1 symbol

```
░ ░
 0 
░ ░
```

### 1-ary

4 symbols

```
░│░  ░ ░  ░ ░  ░ ░
 ·    ·─   ·   ─· 
░ ░  ░ ░  ░│░  ░ ░

░│░  ░ ░  ░ ░  ░ ░
 1    1─   1   ─1 
░ ░  ░ ░  ░│░  ░ ░
```

### 2-ary

6 symbols

```
░│░  ░│░  ░│░
 ·─   ·   ─· 
░ ░  ░│░  ░ ░

░ ░  ░ ░
 ·─  ─·─
░│░  ░ ░

░│░
─· 
░ ░

```

### 3-ary

4 symbols
```
░ ░  ░│░  ░│░  ░│░
─·─  ─·   ─·─   ·─
░│░  ░│░  ░ ░  ░│░
```

### 4-ary
1 symbol
```
░│░
─·─
░│░
```
## Dictionary

16 symbols

```
░ ░
 0 
░ ░

░│░  ░│░  ░│░  ░│░
 1    2─   2   ─2 
░ ░  ░ ░  ░│░  ░ ░

░ ░  ░ ░  ░ ░
 1─   2─  ─2─
░ ░  ░│░  ░ ░

░ ░  ░░
 1   ─2 
░│░  ░│░

░ ░
─1 
░ ░

░ ░  ░│░  ░│░  ░│░
─3─  ─3   ─3─   3─
░│░  ░│░  ░ ░  ░│░

░│░
─4─
░│░

```

## Wallet Generation

Each of these 16 symbols is given a hex value, 0 to f. They are read off the grid row by row.

```
█████████
█░ ░ ░│░█
█─2─3─3─█
█░ ░│░ ░█
█─2─4─2─█
█░ ░│░ ░█
█ 1─2 2─█
█░ ░ ░│░█
█████████
```
produces 8bd 8f7 546

The hex values are concatenated and hashed, to form the deterministic wallet seed.

If a password is used, the password should hashed and used to salt the hashing of the deterministic wallet seed.

## Naive Entropy Estimates

Each symbol encodes 4 bits. A 3x3 grid encodes 36 bits. Four quadrants encode 144 bits.

A 4x4 grid encodes 64 bits. Four quadrants encode 256 bits.

However, the actual entropy is lower than this. Only a subset of symbol sequences are valid. Each symbol places constraints on its neighbors.

Q1: calculate the number of valid 3x3 grids over the symbol dictionary. Calculate effective number of bits stored per quadrant (log base 2 of the total number of valid grids).
Q2: calculate the number of valid 4x4 grids over the symbol dictionary. Calculate effective number of bits stored per quadrant (log base 2 of the total number of valid grids).

Q3:

Given an array
```
2 3 3
2 4 2
1 2 2 
```
How many valid 3x3 grids are there in the average case? What about for 4x4?

## Redundancy

The encoding should be easy to remember, but be robust against a small number of errors. Additional constraints that increase redundancy at the cost of reducing security.

We could require that the number grid rows and columns add to a constant.
```
2 3 3
2 4 2
1 2 2 
```

Q4: How much entropy is lost from the representation if we require all columns and rows to add up to same number? (human checkable, error correction)

Q5: How much entropy is lost if we require that quadrants tile? (12x12 for 3x3 quadrants), (16x16 for 4x4 quadrants). 

Q6: How much entropy is lost if we require that quadrants tile and wrap.

Q8: What about requiring exactly N lines in each column or row of each quadrant? How many bits are lost here and how effective is this as a human readable error correction code?

Q7: Can we put a forward error correcting code in here? Requiring that all rows add to same number and  number and all columns add to same number, is a form of error correction code that can be human checked.

Q8: What other forms of error correction code can be checked by hand easily? 

## End Goals

We want a format that is easy to remember. We want to encode up to ~300 bits, but with redundancy so that only 200 bits is required to uniquely the determine the full 300 bits. If a person has a memory error, we should be able to infer the correct sequence. However, the number of bits contained should be large enough that brute forcing is impossible.

We want variable sized sigil, to allow for different security levels. Larger grids can be used in cold store committed to paper (absolute security) and smaller grids can be used for day to day, or hot wallets (at reduced security).

## Considerations

We want enough redundancy, that we can correct a sigil with 30% to 50% symbol errors, but with enough entropy that it cannot be recovered by brute forcing the key space.

If a user forgets more pieces of the sigil, than are required for construction (is missing 16 bits), then ideally we should be to able to reconstruct the sigil by brute force search around the inputted sigil (2^16 candidates) to recover wallet (even if it takes a month).

The strength and entropy requirements depends on the hashing algorithm used and how long the key needs to remain secure for. We have hashing algorithms that are so slow, that four letter passwords cannot be brute forced easily. For storing a key for twenty years in a safe, a full 256 bits of entropy can encoded without trouble.

For a hot wallet committed to memory, a smaller number (64 bits?) may be enough for reasonable security with a slow enough hashing algorithm.

The combination of even a weak password as a salt and a strong hashing algorithm, allows even very small sigils to offer a good degree of security.

5x5 grids may have more data. Human memory may forgot more elements of the 5x5 grid quadrants, but more bits total may be remembered. We need to empirically determine the best grid size.

We also need to determine if human memory for sigils improves with practice. Best practice should require reciting the sigil at fixed time interval and checking human readable error correction code, to ensure that the sigil is not forgotten.

## Best Security Practices

A person should choose a salt. The salt can be a password, a birthday, phone number or something that is unlikely to be forgotten. Does not need to be a secure password, but should be relatively unique to user. This will be hashed and then hashed into the sigil hash to generate wallet (the hash will be salted). 

This prevents an attacker from running through all weak 64 bit wallet sigils with a brute force attack and looting them.
 
There is a trade off between security and the user forgetting access tokens and losing coins.
- strong passwords, offer greater security but are also more likely to be forgotten
- the risk of losing coins from forgetting a password or sigil, eventually becomes greater than the risk of theft from brute forcing over a particular level
- If both a strong password and a strong sigil are required, then the risk of losing access to the coins becomes even greater.
- Users not under particularly hostile security environments, should make physical copies of passwords or sigils and place them in a secure location to ensure availability.
- Users committing sigils to memory, without backups should be eternally aware of the potential for loss if they forgot the sigil.

The user should have a set of sigils at different security levels committed to memory. The user should be conscience of the effect of compromise of an individual sigil or compromise of the password salt.
- a long term, secure storage sigil, that is used for cold storage. Only to be used on secure, verified hardware controlled by user. Should only be used on hardware keyfob, which can perform signatures for transactions and relay them to computer or cell phone. This sigil should never touch the CPU on a cell phone, laptop or other potentially insecure hardware. The sigil should be rehearsed on a fixed schedule and error correction codes checked, to prevent lapse of memory.
- a sigil, for day to day transactions. This is a low security sigil for daily use on cell phones, computers and other insecure environments. This sigil should be small enough to be easily remembered and be convenient. This sigil may be within range of brute force recovery if the salt is known. The salt or sigil may be committed to paper. The sigil should be rotated out on a fixed schedule (monthly).

## Hardware Device

This sigil system is superior to phase based deterministic wallets, because it can be implemented on a hardware device without a full keyboard. Five buttons required are required for input. An up, down, left, right and center button. A screen is also needed.

A 32 bit arduino ($5), with SPI OLED screen ($5) and five buttons, may be suitable for prototype. The device should generate hash from the sigil. The hash should be used as seed for deterministic wallet generation.

The device should interface to a computer. The device should transmit a list of addresses it has private keys for, to the computer software. When a user attempts a transaction on the computer, the device receives transaction requests from the computer. The device displays the transaction and amount and requires physical input from user to approve the transaction. The device then produces the signature for the transaction.

The private keys should not leave the device. If the private keys do not leave the keyfob device, coins cannot be stolen, even if transactions are authorized on insecure or compromised hardware.