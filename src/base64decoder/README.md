## Base64 Decoder

Strict base64 decoder as of RFC4648 (optional padding, no whitespace separator support).

### Features
- chunkwise decoder
- optimized uint32 reads and writes
- SIMD version (currently commented out, still needs testing and a browser support switch)
- error detection - stops on input domain error

### Usage
```typescript
import { Base64Decoder } from 'import_package_yet_to_create';

// create reusable decoder instance with a keepSize
const decoder = new Base64Decoder(1048576);

// init decoder with decoded byte size
decoder.init(decodedSize);
for (chunk of data) {
  // load chunks of data
  if (decoder.put(chunk, start, end)) {
    throw new Error('base64 data error');
  }
}
// finalize decoding
if (decoder.end()) {
  throw new Error('base64 data error');
}
// grab decoded bytes (borrowed, might need a copy step for further processing)
const bytes = decoder.data8;
// release memory (will destroy data if exceeding keepSize)
decoder.release();

// next base64 data: new init|put|release cycle
```

### Benchmark
```text
   Context "lib/base64decoder/base64.benchmark.js"
      Context "Base64"
         Context "Node - Buffer"
            Case "decode - 256" : 100 runs - average throughput: 25.08 MB/s
            Case "decode - 4096" : 100 runs - average throughput: 232.96 MB/s
            Case "decode - 65536" : 100 runs - average throughput: 516.78 MB/s
            Case "decode - 1048576" : 100 runs - average throughput: 551.90 MB/s
         Context "Base64Decoder"
            Case "decode - 256" : 100 runs - average throughput: 38.30 MB/s
            Case "decode - 4096" : 100 runs - average throughput: 426.29 MB/s
            Case "decode - 65536" : 100 runs - average throughput: 1155.06 MB/s
            Case "decode - 1048576" : 100 runs - average throughput: 1395.79 MB/s
```


### TODO
- more user friendly interface
  - single decode call
  - string support
- ESM build
- maybe: lift strictness - allow whitespaces (SP|CR|LF)?
