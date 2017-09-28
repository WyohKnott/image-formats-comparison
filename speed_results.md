##Lossless compression ratio and Weissman score:

| codec             | compression ratio | avg. enc. time | avg. dec. time | Reduction gain (from PNG) | Weissman score |
|:------------------|:-----------------:|:--------------:|:--------------:|:-------------------------:|:--------------:|
| Daala             |   4,249   |   0,13    |   0,13    |   183,62% |   3,10    |
| AV1 (oct 2016)    |   4,436   |   1,27    |   0,09    |   196,13% |   2,20    |
| VP9 mt            |   4,444   |   2,18    |   0,08    |   196,65% |   2,05    |
| VP9               |   4,444   |   2,54    |   0,08    |   196,65% |   2,01    |
| AV1 (jul 2017) mt |   4,584   |   3,98    |   0,14    |   203,99% |   1,94    |
| JPEG 2000         |   2,364   |   0,08    |   0,06    |   57,79%  |   1,91    |
| JPEG XR           |   2,330   |   0,08    |   0,09    |   55,50%  |   1,90    |
| AV1 (jul 2017)    |   4,565   |   9,25    |   0,13    |   204,73% |   1,77    |
| FLIF              |   3,782   |   4,54    |   0,87    |   152,44% |   1,59    |
| WebP              |   3,241   |  11,83    |   0,45    |   116,32% |   1,22    |
| Reference (PNG)   |   1,498   |   0,20    |   0,11    |   0,00%   |   1,00    |
| BPG               |   1,752   |   0,72    |   0,68    |   16,93%  |   0,94    |
| Mozjpeg           |   1,745   |   1,54    |   0,09    |   16,51%  |   0,84    | 


##Lossy compression and speed

![Encoding time in function of bits per pixel](http://wyohknott.github.io/image-formats-comparison/subset1.encoding_time.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20170809).svg)