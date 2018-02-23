##Lossless compression ratio and Weissman score:

|   codec   |avg. compression ratio|avg. space saving|wavg. encode time|wavg. decode time|Weissman score|
|:----------|:--------------------:|:---------------:|:---------------:|:---------------:|:------------:|
|daala               |  2.798|          64.26%|          1.4952|          1.2960|        3.2145|
|vp9                 |  2.905|          65.58%|         10.6933|          0.9304|        2.6297|
|av1-20160930        |  2.912|          65.66%|         31.1063|          1.6304|        2.3640|
|av1-20170809        |  3.020|          66.89%|        118.6229|          1.1642|        2.1707|
|av1-20180222        |  2.943|          66.02%|         97.6492|          1.0156|        2.1509|
|jxr                 |  1.560|          35.89%|          0.7528|          0.6119|        1.9776|
|flif                |  2.473|          59.57%|         44.8139|          6.7067|        1.9391|
|kdu                 |  1.564|          36.06%|          0.9992|          0.9789|        1.9015|
|webp                |  2.124|          52.93%|         81.4525|          4.1099|        1.5775|
|openjpeg            |  1.564|          36.06%|          4.1349|          2.2590|        1.5771|
|Reference (mozjpeg) |  1.137|          12.05%|         14.0222|          0.6776|        1.0000|
|bpg                 |  1.150|          13.03%|         21.4230|          5.8602|        0.9682|


##Lossy compression and speed

![Encoding time in function of bits per pixel](http://wyohknott.github.io/image-formats-comparison/subset1.encoding_time.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20180222,pik\).svg)