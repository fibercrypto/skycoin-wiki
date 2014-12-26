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
- printers are connected to the network and in the future will contain a full CPU and operating system. Governments will require mandatory printer backdoors for law enforcement, to enable monitoring, surveillance and as a vector for originating attacks on targets from within their network.
- all mobile phones contain several backdoors and exploits. Pictures of private key QR codes stored on phones should not be considered secure.
- the cellular phones and computers which currently used to store private keys may be subject to theft or confiscation. Confiscation of private keys at border crossings for profit, by corrupt officials may become common in the future.

Therefore we need a brain wallet standard that allows 
- wallet seeds to be committed to memory
- is robust to errors in memory (redundancy, error correction)
- does not rely upon computers, cell phones or printers
- allows wallets to optionally be committed to paper for secure long term storage

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
│─2─4─1 │ 2─3─3 │
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

```
█████████
█     │ █
█─2─3─3─█
█   │   █
█─2─4─1 █
█   │   █
█ 1─2 2─█
█     │ █
█████████
```