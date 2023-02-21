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


### TODO
- more user friendly interface
  - single decode call
  - string support
- ESM build
- maybe: lift strictness - allow whitespaces (SP|CR|LF)?
