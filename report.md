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

  * Alliance for Open Media AV1: `https://aomedia.googlesource.com/aom/`. The versions used are built from GIT revision `02affd269df5d8abbfc75f5bdad0c080308e0ce1` (october 2016) and `ce7272d2d00f224475849c1b1bca0a97b70ea0c4` (july 2017).
  * Fabrice Bellard BPG: `https://github.com/mirrorer/libbpg. The version used is 0.9.7.
  * Xiph Daala: `https://git.xiph.org/?p=daala.git`. The version used is built from GIT revision `72783687ce4963478b8ab4d97809510f40c7c855`.
  * Jon Sneyers FLIF: `https://github.com/FLIF-hub/FLIF`. The version used is built from GIT revision `c17459bab5399ed5009c262e9954d474f275db7f`.
  * Microsoft JxrLib: `https://jxrlib.codeplex.com/`. The version used is built from GIT revision `e922fa50cdf9a58f40cad07553bcaa2883d3c5bf`.
  * Kakadu JPEG2000: `http://kakadusoftware.com/downloads/`. The version used is 7.9.
  * Mozilla JPEG Encoder: `https://github.com/mozilla/mozjpeg`. The version used is 3.2.
  * Google VP9: `https://chromium.googlesource.com/webm/libvpx`. The version used is built from GIT revision `6c375b9cd0647686ae5cc9bae8e94ec3d7c43e4b`.
  * Google WebP: `https://chromium.googlesource.com/webm/libwebp`. The version used is built from GIT revision `66ad84f0f9f36d38166a15a981fd7f9a1910a859`.

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
  
    - lossless: `aomenc --passes=2 --lossless=1 -o [output] [input(Y4M)]`
    - between q=5 and q=63: `aomenc --passes=2 --end-usage=q --cq-level=$q -o [output] [input(Y4M)]`
  
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
For each codec and image, the encoding and decoding speeds for lossless compression are sampled five times using GNU `time`
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

  * [Subset1](http://wyohknott.github.io/image-formats-comparison/subset1.tar.gz)
  * [Subset2](http://wyohknott.github.io/image-formats-comparison/subset2.tar.gz)

###Lossless compression ratio and Weissman score:


|   format   |avg_bpp|avg_compression_ratio|avg_space_saving|wavg_encode_time|wavg_decode_time|weissman_score|
|------------|------:|--------------------:|---------------:|---------------:|---------------:|-------------:|
|daala       |  5.791|                2.798|          0.6426|          1.4952|          1.2960|        3.2145|
|vp9         |  5.578|                2.905|          0.6558|         10.6933|          0.9304|        2.6297|
|av1-20160930|  5.565|                2.912|          0.6566|         31.1063|          1.6304|        2.3640|
|av1-20170809|  5.366|                3.020|          0.6689|        118.6229|          1.1642|        2.1707|
|jxr         | 10.389|                1.560|          0.3589|          0.7528|          0.6119|        1.9776|
|flif        |  6.553|                2.473|          0.5957|         44.8139|          6.7067|        1.9391|
|kdu         | 10.362|                1.564|          0.3606|          0.9992|          0.9789|        1.9015|
|webp        |  7.629|                2.124|          0.5293|         81.4525|          4.1099|        1.5775|
|openjpeg    | 10.362|                1.564|          0.3606|          4.1349|          2.2590|        1.5771|
|mozjpeg     | 14.252|                1.137|          0.1205|         14.0222|          0.6776|        1.0000|
|bpg         | 14.095|                1.150|          0.1303|         21.4230|          5.8602|        0.9682|


###Lossy compression and speed

![Encoding time in function of bits per pixel](http://wyohknott.github.io/image-formats-comparison/subset1.encoding_time.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20170809).svg)

###Lossy metrics

For each comparison algorithms, we plot the quality in dB in function of the mean bits per pixel on a logarithmic scale. We can then visualize which codec gives the best quality at a given bit per pixel (top left is better).

####Bits per pixel at equivalent quality according to VMAF

![Bits per pixel at equivalent quality according to VMAF](http://wyohknott.github.io/image-formats-comparison/subset1.vmaf.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20170809).svg)

####Bits per pixel at equivalent quality according to Y-PSNR-HVS-M

![Bits per pixel at equivalent quality according to Y-PSNR-HVS-M](http://wyohknott.github.io/image-formats-comparison/subset1.psnr-hvs-m.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20170809).svg)

####Bits per pixel at equivalent quality according to Y-MSSSIM

![Bits per pixel at equivalent quality according to Y-MSSSIM](http://wyohknott.github.io/image-formats-comparison/subset1.ms-ssim.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20170809).svg)

####Bits per pixel at equivalent quality according to Y-SSIM

![Bits per pixel at equivalent quality according to Y-SSIM](http://wyohknott.github.io/image-formats-comparison/subset1.y-ssim.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20170809).svg)

####Bits per pixel at equivalent quality according to RGB-SSIM

![Bits per pixel at equivalent quality according to RGB-SSIM](http://wyohknott.github.io/image-formats-comparison/subset1.rgb-ssim.(openjpeg,flif,vp9,daala,jxr,bpg,mozjpeg,webp,kdu,av1-20170809).svg)



