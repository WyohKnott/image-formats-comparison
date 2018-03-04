##Lossless compression ratio and Weissman score:

|   codec    |avg. compression ratio|avg. space saving|wavg. encode time|wavg. decode time|Weissman score|
|:-----------|:-------------------:|:--------------:|:--------------:|:--------------:|:------------:|
|daala       |                2.798|          64.26%|          0.8049|          0.7280|        3.3381|
|vp9         |                2.905|          65.58%|          3.9375|          0.4193|        2.8011|
|av1-20160930|                2.912|          65.66%|          4.7511|          0.4838|        2.7455|
|av1-20170809|                2.984|          66.49%|         20.4157|          0.6900|        2.4003|
|kdu         |                1.564|          36.06%|          0.3875|          0.2892|        2.0946|
|jxr         |                1.560|          35.89%|          0.4058|          0.3628|        2.0730|
|av1-20180222|                2.943|          66.02%|         97.6492|          1.0156|        2.0444|
|flif        |                2.473|          59.57%|         22.1088|          4.3042|        1.9732|
|openjpeg    |                1.564|          36.06%|          2.1181|          1.3794|        1.6300|
|webp        |                2.124|          52.93%|         37.9675|          2.4799|        1.6079|
|bpg         |                1.150|          13.03%|          3.7503|          3.5251|        1.1151|
|mozjpeg     |                1.137|          12.05%|          8.7385|          0.4144|        1.0000|


##Lossy compression and speed

![Encoding time in function of bits per pixel](subset1.encoding_time.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20180222,pik\).svg)