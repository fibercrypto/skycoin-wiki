Data sent over the network and data saved to the database is stored in a custom binary encoding format.

The goal of the format is to produce deterministic output in a simple manner.

A description of the format is as follows:

* Integer values (uint8, uint16, uint32, uint64, int8, int16, int32, int64) are stored little-endian.
* Bools are stored as 1 byte with 1 equal to `true` and 0 equal to `false`. Any other value is invalid.
* Floats values (float32, float64) are converted to their IEEE 754 binary representation and stored little-endian as a uint32 or uint64, respectively.
* Variable-length arrays (i.e. slices), including strings have a 4-byte length prefix (little-endian) followed by the contents of the array, in sequence
* Fixed-length arrays have no prefix, only the contents of the array, in sequence
* Maps are serialized as a sequence of key, value pairs. **NOTE: The use of maps causes the serialized data to be non-deterministic. Do not use where determinism is required**
* Structs add no overhead

Varints (as used in protobuf and other serializers) are not used. The encoding is optimized for speed over space, after simplicity and determinism.

The decoder and encoder behavior is specified:

* A decoder must return an error if the data buffer does not have enough data to decode a full object.
* A decoder may return an error, or the number of bytes remaining, if the data buffer has additional bytes after decoding a full object.
* A decoder must return an error if a bool value is not 0 or 1

Decoding Options
================

The Go decoder uses struct tags with the name `enc` for encoding and decoding options.

The struct tag has the format `enc:"name,options"`. `name` is not required and has only one meaningful value, `-`. 
If `name` is `-`, the field is ignored when encoding or decoding.

The comma is required prior to declaring options, even if there is no name.

The options are:

* `maxlen=N`: it an error to encode or decode an object with a length greater than N. Only valid for slices, strings and maps.
* `omitempty`: Applicable only to a final, variable-length field in a top level struct, the encoder will not write a length prefix if the field is empty. 
  Similarly, the decoder will not return an error if the data buffer stops where an empty `omitempty` field would be.

Variable-length objects (slices, strings and maps) may add a `maxlen=N` 

Implementation Specifics
========================

* Decoding an empty map or variable-length arrays leaves the variable set to `nil`. It does not allocate an empty object.
* `[]struct{}` cannot be used (but `map[T]struct{}` is allowed)
* Unexported struct fields are ignored

Libraries
=========

* Reflect-based runtime encoder: https://godoc.org/github.com/skycoin/skycoin/src/cipher/encoder
* Compile-time code generator: https://github.com/skycoin/skyencoder