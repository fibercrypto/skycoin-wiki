Skycoin wallets are deterministic. The addresses are generated from a 256 bit seed. Minimum entropy of the wallet generation seed should be 256 bits.

A typical wallet seed is 64 characters in hex and appears as
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

## Standard Definition

This is a visual standard for representing 256 bit wallet seeds in a visual format that can be committed to memory. The format is designed to be easy to remember and has built in redundancy to allow for error correction.

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

## Naive Entropy Estimates

Each symbol encodes 4 bits. A 3x3 grid encodes 36 bits. Four quadrants encode 144 bits.

A 4x4 grid encodes 64 bits. Four quadrants encode 256 bits.

However, the actual entropy is lower than this.  Each symbol places constraints on its neighbors.

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

Q4: How much entropy is lost from the representation if we require all columns and rows to add up to same number? 

Q5: How much entropy is lost if we require that quadrants tile? (12x12 for 3x3 quadrants), (16x16 for 4x4 quadrants). 

Q6: How much entropy is lost if we require that quadrants tile and wrap.

Q7: Can we put a forward error correcting code in here?

## End Goals

We want a format that is easy to remember. We want to encode ~300 bits, but with redundancy so that only 200 bits is required to uniquely the determine the full 300 bits. If a person has a memory error, we should be able to infer the correct sequence. However, the number of bits contained should be large enough that brute forcing is impossible.

We want variable sized, so that larger grids can be used in cold store committed to paper (absolute security) and smaller grids can be used for day to day, or hot wallets (at reduced security).

## Considerations

The strength depends on the hashing algorithm used. We have hashing algorithms that are so slow, that four letter passports cannot be brute forced. 

5x5 grids may have more data. Human memory may forgot more elements of the 5x5 grid quadrants, but more bits total may be remembered. 

