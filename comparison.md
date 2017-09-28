#Evolution of the AV1 codec between October 2016 and July 2017



###Lossless compression ratio and Weissman score:

|   format   |avg_bpp|avg_compression_ratio|avg_space_saving|wavg_encode_time|wavg_decode_time|weissman_score|
|------------|------:|--------------------:|---------------:|---------------:|---------------:|-------------:|
|daala       |  5.791|                2.798|          0.6426|          1.4952|          1.2960|        3.2145|
|av1-20160930|  5.565|                2.912|          0.6566|         31.1063|          1.6304|        2.3640|
|av1-20170809|  5.366|                3.020|          0.6689|        118.6229|          1.1642|        2.1707|
|mozjpeg     | 14.252|                1.137|          0.1205|         14.0222|          0.6776|        1.0000|
|bpg         | 14.095|                1.150|          0.1303|         21.4230|          5.8602|        0.9682|

###Encoding times:


![Encodinq time in function of bits per pixel](http://wyohknott.github.io/image-formats-comparison/subset1.encoding_time.(av1-20160930,av1-20170809).svg)


###Bits per pixel at equivalent quality according to VMAF

![Bits per pixel at equivalent quality according to VMAF](http://wyohknott.github.io/image-formats-comparison/subset1.vmaf.(av1-20160930,av1-20170809).svg)

###Bits per pixel at equivalent quality according to Y-PSNR-HVS-M

![Bits per pixel at equivalent quality according to Y-PSNR-HVS-M](http://wyohknott.github.io/image-formats-comparison/subset1.psnr-hvs-m.(av1-20160930,av1-20170809).svg)

###Bits per pixel at equivalent quality according to Y-MSSSIM

![Bits per pixel at equivalent quality according to Y-MSSSIM](http://wyohknott.github.io/image-formats-comparison/subset1.ms-ssim.(av1-20160930,av1-20170809).svg)

###Bits per pixel at equivalent quality according to Y-SSIM

![Bits per pixel at equivalent quality according to Y-SSIM](http://wyohknott.github.io/image-formats-comparison/subset1.y-ssim.(av1-20160930,av1-20170809).svg)

###Bits per pixel at equivalent quality according to RGB-SSIM

![Bits per pixel at equivalent quality according to RGB-SSIM](http://wyohknott.github.io/image-formats-comparison/subset1.rgb-ssim.(av1-20160930,av1-20170809).svg)




