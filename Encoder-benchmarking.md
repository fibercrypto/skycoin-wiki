Benchmarking of the binary encoder (https://github.com/skycoin/skycoin/tree/develop/src/cipher/encoder) is done with https://github.com/gz-c/gosercomp.  The benchmarks require a lot of setup for the various encoders, but is described in the README of that repo.

Results generated on Mid-2015 Macbook Pro base model:

```
BenchmarkMarshalByJson-8                       	 2000000	       688 ns/op	     128 B/op	       2 allocs/op
BenchmarkUnmarshalByJson-8                     	  500000	      2212 ns/op	     216 B/op	       8 allocs/op
BenchmarkMarshalByXml-8                        	  500000	      3726 ns/op	    4800 B/op	      11 allocs/op
BenchmarkUnmarshalByXml-8                      	  100000	     13710 ns/op	    3027 B/op	      73 allocs/op
BenchmarkMarshalByMsgp-8                       	20000000	       122 ns/op	      80 B/op	       1 allocs/op
BenchmarkUnmarshalByMsgp-8                     	10000000	       222 ns/op	      32 B/op	       5 allocs/op
BenchmarkMarshalByProtoBuf-8                   	10000000	       233 ns/op	      48 B/op	       1 allocs/op
BenchmarkUnmarshalByProtoBuf-8                 	 3000000	       606 ns/op	     160 B/op	      10 allocs/op
BenchmarkMarshalByGogoProtoBuf-8               	10000000	       132 ns/op	      48 B/op	       1 allocs/op
BenchmarkUnmarshalByGogoProtoBuf-8             	 3000000	       387 ns/op	     144 B/op	       8 allocs/op
BenchmarkMarshalByFlatBuffers-8                	 5000000	       361 ns/op	      16 B/op	       1 allocs/op
BenchmarkUnmarshalByFlatBuffers-8              	2000000000	         0.32 ns/op	       0 B/op	       0 allocs/op
BenchmarkUnmarshalByFlatBuffers_withFields-8   	10000000	       126 ns/op	       0 B/op	       0 allocs/op
BenchmarkMarshalByThrift-8                     	 3000000	       446 ns/op	      64 B/op	       1 allocs/op
BenchmarkUnmarshalByThrift-8                   	 1000000	      1300 ns/op	     656 B/op	      11 allocs/op
BenchmarkMarshalByAvro-8                       	 3000000	       541 ns/op	      48 B/op	       6 allocs/op
BenchmarkUnmarshalByAvro-8                     	  500000	      3257 ns/op	    1672 B/op	      62 allocs/op
BenchmarkMarshalByGencode-8                    	30000000	        44.6 ns/op	       0 B/op	       0 allocs/op
BenchmarkUnmarshalByGencode-8                  	10000000	       121 ns/op	      32 B/op	       5 allocs/op
BenchmarkMarshalByUgorjiCodecAndCbor-8         	 2000000	       727 ns/op	     112 B/op	       3 allocs/op
BenchmarkUnmarshalByUgorjiCodecAndCbor-8       	 2000000	       673 ns/op	      48 B/op	       6 allocs/op
BenchmarkMarshalByUgorjiCodecAndMsgp-8         	 2000000	       692 ns/op	     112 B/op	       3 allocs/op
BenchmarkUnmarshalByUgorjiCodecAndMsgp-8       	 2000000	       722 ns/op	      48 B/op	       6 allocs/op
BenchmarkMarshalByUgorjiCodecAndBinc-8         	 2000000	       807 ns/op	     112 B/op	       3 allocs/op
BenchmarkUnmarshalByUgorjiCodecAndBinc-8       	 1000000	      1107 ns/op	     824 B/op	      10 allocs/op
BenchmarkMarshalByUgorjiCodecAndJson-8         	 2000000	      1046 ns/op	     112 B/op	       3 allocs/op
BenchmarkUnmarshalByUgorjiCodecAndJson-8       	 2000000	       813 ns/op	      48 B/op	       6 allocs/op
BenchmarkMarshalByEasyjson-8                   	 5000000	       337 ns/op	     128 B/op	       1 allocs/op
BenchmarkUnmarshalByEasyjson-8                 	 3000000	       535 ns/op	      32 B/op	       5 allocs/op
BenchmarkMarshalByFfjson-8                     	 1000000	      1064 ns/op	     424 B/op	       9 allocs/op
BenchmarkUnmarshalByFfjson-8                   	 1000000	      1535 ns/op	     480 B/op	      13 allocs/op
BenchmarkMarshalByJsoniter-8                   	 2000000	       659 ns/op	      96 B/op	       2 allocs/op
BenchmarkUnmarshalByJsoniter-8                 	 2000000	       647 ns/op	      32 B/op	       5 allocs/op
BenchmarkUnmarshalByGJSON-8                    	 1000000	      1885 ns/op	     624 B/op	       7 allocs/op
BenchmarkMarshalByGoMemdump-8                  	  300000	      5328 ns/op	    1032 B/op	      30 allocs/op
BenchmarkUnmarshalByGoMemdump-8                	 1000000	      1534 ns/op	    2400 B/op	      12 allocs/op
BenchmarkMarshalBySky-8                        	 1000000	      1140 ns/op	     208 B/op	      13 allocs/op
BenchmarkUnmarshalBySky-8                      	  500000	      3175 ns/op	     883 B/op	      31 allocs/op
BenchmarkMarshalByColfer-8                     	50000000	        33.6 ns/op	       0 B/op	       0 allocs/op
BenchmarkUnmarshalByColfer-8                   	 5000000	       212 ns/op	      96 B/op	       6 allocs/op
BenchmarkMarshalByZebrapack-8                  	10000000	       240 ns/op	     182 B/op	       0 allocs/op
BenchmarkUnmarshalByZebrapack-8                	 5000000	       303 ns/op	      32 B/op	       5 allocs/op
BenchmarkMarshalByGotiny-8                     	 3000000	       465 ns/op	     184 B/op	       6 allocs/op
BenchmarkUnmarshalByGotiny-8                   	 5000000	       261 ns/op	      88 B/op	       2 allocs/op
BenchmarkMarshalByHprose-8                     	 3000000	       462 ns/op	     210 B/op	       1 allocs/op
BenchmarkUnmarshalByHprose-8                   	 2000000	       643 ns/op	     288 B/op	       9 allocs/op
BenchmarkMarshalBySereal-8                     	 1000000	      2288 ns/op	     792 B/op	      22 allocs/op
BenchmarkUnmarshalBySereal-8                   	 2000000	       755 ns/op	      80 B/op	       6 allocs/op
BenchmarkMarshalByMsgpackV2-8                  	 1000000	      2124 ns/op	     192 B/op	       4 allocs/op
BenchmarkUnmarshalByMsgpackv2-8                	 1000000	      1657 ns/op	     232 B/op	      11 allocs/op
BenchmarkMarshalMsgZColorGroup-8               	20000000	        81.4 ns/op	      80 B/op	       1 allocs/op
BenchmarkAppendMsgZColorGroup-8                	30000000	        44.7 ns/op	 313.29 MB/s	       0 B/op	       0 allocs/op
BenchmarkUnmarshalZColorGroup-8                	20000000	        86.9 ns/op	 161.02 MB/s	       0 B/op	       0 allocs/op
BenchmarkEncodeZColorGroup-8                   	20000000	        75.8 ns/op	 184.78 MB/s	      16 B/op	       1 allocs/op
BenchmarkDecodeZColorGroup-8                   	10000000	       136 ns/op	 102.57 MB/s	       0 B/op	       0 allocs/op
```


## Size of marshalled results


```
    gosercomp_test.go:99: json:				 	65 bytes
    gosercomp_test.go:102: xml:				 	137 bytes
    gosercomp_test.go:105: msgp:				47 bytes
    gosercomp_test.go:108: protobuf:			36 bytes
    gosercomp_test.go:111: gogoprotobuf:		36 bytes
    gosercomp_test.go:115: flatbuffers:			108 bytes
    gosercomp_test.go:121: thrift:				63 bytes
    gosercomp_test.go:135: avro:				32 bytes
    gosercomp_test.go:144: gencode:				34 bytes
    gosercomp_test.go:150: UgorjiCodec_Cbor:	47 bytes
    gosercomp_test.go:156: UgorjiCodec_Msgp:	47 bytes
    gosercomp_test.go:162: UgorjiCodec_Bin:		53 bytes
    gosercomp_test.go:164: UgorjiCodec_Json:	91 bytes
    gosercomp_test.go:167: easyjson:			65 bytes
    gosercomp_test.go:170: ffjson:				65 bytes
    gosercomp_test.go:173: jsoniter:			65 bytes
    gosercomp_test.go:177: memdump:				200 bytes
    gosercomp_test.go:180: colfer:				35 bytes
    gosercomp_test.go:183: sky:				 	52 bytes
    gosercomp_test.go:186: zebrapack:			48 bytes
    gosercomp_test.go:189: gotiny:				32 bytes
    gosercomp_test.go:194: hprose:				32 bytes
    gosercomp_test.go:198: sereal:				76 bytes
    gosercomp_test.go:201: msgpackv2:			47 bytes
```


*Original issue: https://github.com/skycoin/skycoin/issues/1872*