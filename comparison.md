#Evolution of the AV1 codec between October 2016, July 2017 and February 2018



###Lossless compression ratio and Weissman score:

|   codec   |avg. compression ratio|avg. space saving|wavg. encode time|wavg. decode time|Weissman score|
|:----------|:--------------------:|:---------------:|:---------------:|:---------------:|:------------:|
|daala               |  2.798|          64.26%|          1.4952|          1.2960|        3.2145|
|av1-20160930        |  2.912|          65.66%|         31.1063|          1.6304|        2.3640|
|av1-20170809        |  3.020|          66.89%|        118.6229|          1.1642|        2.1707|
|av1-20180222        |  2.943|          66.02%|         97.6492|          1.0156|        2.1509|
|Reference (mozjpeg) |  1.137|          12.05%|         14.0222|          0.6776|        1.0000|
|bpg                 |  1.150|          13.03%|         21.4230|          5.8602|        0.9682|

###Encoding times:


![Encodinq time in function of bits per pixel](http://wyohknott.github.io/image-formats-comparison/subset1.encoding_time.(av1-20160930,av1-20170809,av1-20180222\).svg)


###Bits per pixel at equivalent quality according to VMAF

![Bits per pixel at equivalent quality according to VMAF](http://wyohknott.github.io/image-formats-comparison/subset1.vmaf.(av1-20160930,av1-20170809,av1-20180222\).svg)

###Bits per pixel at equivalent quality according to Y-PSNR-HVS-M

![Bits per pixel at equivalent quality according to Y-PSNR-HVS-M](http://wyohknott.github.io/image-formats-comparison/subset1.psnr-hvs-m.(av1-20160930,av1-20170809,av1-20180222\).svg)

###Bits per pixel at equivalent quality according to Y-MSSSIM

![Bits per pixel at equivalent quality according to Y-MSSSIM](http://wyohknott.github.io/image-formats-comparison/subset1.ms-ssim.(av1-20160930,av1-20170809,av1-20180222\).svg)

###Bits per pixel at equivalent quality according to Y-SSIM

![Bits per pixel at equivalent quality according to Y-SSIM](http://wyohknott.github.io/image-formats-comparison/subset1.y-ssim.(av1-20160930,av1-20170809,av1-20180222\).svg)

###Bits per pixel at equivalent quality according to RGB-SSIM

![Bits per pixel at equivalent quality according to RGB-SSIM](http://wyohknott.github.io/image-formats-comparison/subset1.rgb-ssim.(av1-20160930,av1-20170809,av1-20180222\).svg)




