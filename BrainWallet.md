Skycoin wallets are deterministic. The addresses are generated from a 256 bit seed. Minimum entropy of the wallet generation seed should be 256 bits.

A typical wallet seed is 64 characters in hex and appears as
```
f399bd1b78792da9cc49b1157c73016450c949df565ce3ddbf2f9d65fd8f0dac
```

This is difficult to commit to memory.
- writing down wallet seeds enables easy theft or confiscation
- printing wallet seeds is insecure as many computers save print jobs
- cloud printing reports print jobs to central authority. Future printers may clandestinely save or upload print jobs to central authorities, even for local print jobs. Cloud printing going through a central server is the only option for printing from Chromebook or on the iPad.
- printers are insecure and may be compromised or back doored

Therefore we need a brain wallet standard that allows 
- wallet seeds to be committed to memory
- is robust to errors in memory (redundancy, error correction)
- does not rely upon computers, printers or paper

The standard should allow a person to commit a wallet seed to paper, by hand and place the seed within a safe for long term cold storage. The standard should also allow individuals to commit the seed to memory if required.

## Standard Definition

This is a visual standard for representing 256 bit wallet seeds in a visual format that can be committed to memory. The format is designed to be easy to remember and has built in redundancy to allow for error correction.

----
|
|

```
┌─┐
│


```