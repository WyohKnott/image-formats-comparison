#Image formats comparison

##Introduction

This study compares 8 differents image formats, AOM AV1, BPG, Daala, FLIF, JPEG XR, JPEG 2000, JPEG and WebP. We use five algorithms in order to compare each format:

  * VMAF: Video Multi-Method Assessment Fusion: [http://techblog.netflix.com/2016/06/toward-practical-perceptual-video.html](http://techblog.netflix.com/2016/06/toward-practical-perceptual-video.html)
  * Y-SSIM: Structural Similarity algorithm applied to the luma channel
  * RGB-SSIM: Structural Similarity algorithm applied to the R, G and B channels
  * Y-MSSIM: Multi-Scale Structural Similarity algorithm applied to the luma channel
  * PSNR-HVS-M: Peak Signal to Noise Ratio taking into account Contrast Sensitivity Function (CSF) and between-coefficient contrast masking of DCT basis functions

## Materials

###Image set

The image set is comprised of 50 images from [the subset 1 and subset 2 maintened by Xiph](https://wiki.xiph.org/Daala_Quickstart#Test_Media). All images are YCbCr 4:2:0 Y4M files.

###Codecs

  * Alliance for Open Media AV1: `https://aomedia.googlesource.com/aom/`. The versions used are built from GIT revision `02affd269df5d8abbfc75f5bdad0c080308e0ce1` (october 2016), `ce7272d2d00f224475849c1b1bca0a97b70ea0c4` (july 2017) and `7d3bd8daba6e51566f0458e3f842e246a559ea82` (february 2018).
  * Fabrice Bellard BPG: `https://github.com/mirrorer/libbpg`. The version used is 0.9.7.
  * Xiph Daala: `https://git.xiph.org/?p=daala.git`. The version used is built from GIT revision `72783687ce4963478b8ab4d97809510f40c7c855`.
  * Jon Sneyers FLIF: `https://github.com/FLIF-hub/FLIF`. The version used is built from GIT revision `c17459bab5399ed5009c262e9954d474f275db7f`.
  * Microsoft JxrLib: `https://jxrlib.codeplex.com/`. The version used is built from GIT revision `e922fa50cdf9a58f40cad07553bcaa2883d3c5bf`.
  * Kakadu JPEG2000: `http://kakadusoftware.com/downloads/`. The version used is 7.10.2.
  * Mozilla JPEG Encoder: `https://github.com/mozilla/mozjpeg`. The version used is 3.3.1.
  * Google VP9: `https://chromium.googlesource.com/webm/libvpx`. The version used is 1.7.0.
  * Google WebP: `https://chromium.googlesource.com/webm/libwebp`. The version used is 0.6.1.
  * Google Pik: `https://github.com/google/pik`. The version used is built from GIT revision `52f2d45cc8e35e45278da54615bb8b11b5066f16`.

###Metrics

  * The VMAF (Video Multi-Method Assessment Fusion) metric is computed using `vmafossexec`, provided by Netflix: `https://github.com/Netflix/vmaf`. The version used is built from GIT revision `7ebbde0c64493af978da66cb7ebe2946fb12dec2`.

    `vmafossexec` compares two YUV files, given their subsampling and dimensions.
    
  * Y-MSSIM, Y-SSIM, RGB-SSIM and PNSR-HVS-M are computed by the tools `dump_msssim`, `dump_ssim` and `dump_psnrhvs`, provided by the Daala repository: `https://git.xiph.org/?p=daala.git`. The version used is built from GIT revision `05243557bc3e59872fd043c99dc4c17ca33bcb1b`.

    Each metric compares two Y4M files.

###Tools

  * `ffmpeg` is used for image formats conversion. The version used is ffmpeg 3.3.2.
  * `gm identify` is used to determine the width and height of images. The version used is GraphicsMagick 1.3.25.
  *  Python 3 timeit is used to mesure computing times.
  *  Python 3 Pandas and Numpy are used to compute means.
  *  Python 3 Matplot is used to plot graphs.

##Methods

###Image conversion

Each Y4M image is exported to 4:2:0 PNG, YUV and PPM files with FFMPEG:

`ffmpeg -loglevel quiet -y -i [input] -pix_fmt yuv420p [output]`

###Image compression

All images are compressed losslessly and over a range of qualities for each codec:

  * BPG: 
  
    - lossless: `bpgenc -m 8 -f 420 -lossless -o [output] [input(PNG)]`
    - between q=3 and q=45: `bpgenc -m 8 -f 420 -q $q -o [output] [input(PNG)]`
  
  * AV1:
  
    - lossless: `aomenc --cpu-used=2 --tile-columns=4 --passes=2 --lossless=1 -o [output] [input(Y4M)]`
    - between q=5 and q=63: `aomenc --cpu-used=2 --tile-columns=4 --passes=2 --end-usage=q --cq-level=$q -o [output] [input(Y4M)]`
  
  * Daala:
  
    - lossless: `encoder_example -v 0 -o [output] [input(Y4M)]`
    - between q=5 and q=85: `encoder_example -v $q -o [output] [input(Y4M)]`
  
  * FLIF:
  
    - lossless: `flif -Q 100 [input(PNG)] [output]`
    - between q=-329 and q=79, with a step of 12: `flif -Q $q  [input(PNG)] [output]`
  
  * JPEG2000:
  
    - lossless: `kdu_compress -no_info Creversible=yes -slope 0 -o [output] -i [input(PPM)]`
    - between q=38912 and q=45056, with a step of 64: `kdu_compress -no_info -slope $q -o [output] -i [input(PPM)]`
  
  * JPEG XR:
  
    - lossless: `JxrEncApp -d 1 -q 1 -o [output] -i [input(PPM)]`
    - between q=5 and q=85: `JxrEncApp -d 1 -q $q -o [output] -i [input(PPM)]`
  
  * MozJPEG:
  
    - lossless: `cjpeg -rgb -quality 100 [input(PNG)] > [output]`
    - between q=5 and q=95: `cjpeg -quality $q [input(PNG)] > [output]`

  * Pik:
  
    - between q=0.5 and q=3.0: `cpik [input(PNG)] [output] --distance $q`
  
  * VP9:
  
    - lossless: `aomenc --cpu-used=2 --tile-columns=4 --passes=2 --lossless=1 -o [output] [input(Y4M)]`
    - between q=5 and q=63: `aomenc --cpu-used=2 --tile-columns=4 --passes=2 --end-usage=q --cq-level=$q -o [output] [input(Y4M)]`
  
  * WebP:
  
    - lossless: `cwebp -mt -z 9 -lossless -o [output] [input(PNG)]`
    - between q=5 and q=95: `cwebp -mt -q $q -o [output] [input(PNG)]`

The Python script used to generate the compressed images are available on [the GIT repository](https://github.com/WyohKnott/image-comparison-sources).

###Image selection

The images which will be displayed on the website are then chosen among all compressed images, using the following criteria:

  * The BPG file at quality 24 is chosen to be the reference filesize for *large* quality images.
  * The filesize for *medium* quality images is 60% of the *large* filesize.
  * The filesize for *small* quality images is 60% of the *medium* filesize.
  * The filesize for *tiny* quality images is 60% of the *small* filesize.

The Python script used to select the compressed images are available on [the GIT repository](https://github.com/WyohKnott/image-comparison-sources).


###Encoding and decoding speeds:

####Lossless compression and speed
For each codec and image, the encoding and decoding speeds for lossless compression are sampled using Python `timeit`.
The arithmetic mean of encoding and decoding speeds are calculated over the entire image set. We then determine a [Weissman score](https://en.wikipedia.org/wiki/Weissman_score) for each codec using the following formula:

$$W = \alpha {r \over \overline{r}} {\log{\overline{T}} \over \log{T}}$$

where `r` is the compression ratio over PPM filesize, `T` the time required to compress, `̅r` and `̅T` the same metrics for the standard compressor, and alpha is a scaling constant.

The standard compressor used is the compression of a JPG image using mozjpeg.

###Lossy metrics

For each codec and image, we apply the following metrics, Y-SSIM, RGB-SSIM, Y-MSSSIM, PSNR-HVS-M and VMAF, over 15 image samples of increasing quality. For VMAF, we use the trained model `nflxall_vmafv4.pkl` given by Netflix.

For each sample, we first decode the compressed image, then export the resulting file to 4:2:0 Y4M and YUV format using FFMPEG (`ffmpeg -loglevel quiet -y -i [input] -pix_fmt yuv420p [output]`). Finally we apply the metrics over each sample, comparing it to the original image.

For each codec, we calculate the arithmetic mean of each metric over the entire set of images, weighted by the area of the corresponding picture, for the 15 samples of increasing quality:

$$\overline{metric}_{quality\ q} = \frac{ \sum\limits_{picture=1}^{50} metric_{qp} \times area_{p}}{\sum\limits_{p=1}^{50} area_{p}}$$

We also determine the average bits per pixel for each quality sample:

$$\overline{bpp}_{quality\ q} = \frac{ \sum\limits_{picture=1}^{50} filesize_{qp}}{\sum\limits_{picture=1}^{50} area_{p}}$$

##Results

###Raw data

The following archives contain the raw data in csv format for subset1 and subset2:

  * [Subset1](subset1.tar.gz) (updated February 2018)
  * [Subset2](subset2.tar.gz) (outdated)

###Lossless compression ratio and Weissman score:

|   codec   |avg. compression ratio|avg. space saving|wavg. encode time|wavg. decode time|Weissman score|
|:----------|:--------------------:|:---------------:|:---------------:|:---------------:|:------------:|
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


###Lossy compression and speed

![Encoding time in function of bits per pixel](subset1.encoding_time.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20180222,pik\).svg)

###Lossy metrics

For each comparison algorithms, we plot the quality in dB in function of the mean bits per pixel on a logarithmic scale. We can then visualize which codec gives the best quality at a given bit per pixel (top left is better).

####Bits per pixel at equivalent quality according to VMAF

![Bits per pixel at equivalent quality according to VMAF](subset1.vmaf.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20180222,pik\).svg)

####Bits per pixel at equivalent quality according to Y-PSNR-HVS-M

![Bits per pixel at equivalent quality according to Y-PSNR-HVS-M](subset1.psnr-hvs-m.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20180222,pik\).svg)

####Bits per pixel at equivalent quality according to Y-MSSSIM

![Bits per pixel at equivalent quality according to Y-MSSSIM](subset1.ms-ssim.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20180222,pik\).svg)

####Bits per pixel at equivalent quality according to Y-SSIM

![Bits per pixel at equivalent quality according to Y-SSIM](subset1.y-ssim.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20180222,pik\).svg)

####Bits per pixel at equivalent quality according to RGB-SSIM

![Bits per pixel at equivalent quality according to RGB-SSIM](subset1.rgb-ssim.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20180222,pik\).svg)



