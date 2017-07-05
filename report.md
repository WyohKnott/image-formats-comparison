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

  * Alliance for Open Media AV1: `https://aomedia.googlesource.com/aom/`. The versions used are built from GIT revision `02affd269df5d8abbfc75f5bdad0c080308e0ce1` (october 2016) and `7ebbde0c64493af978da66cb7ebe2946fb12dec2` (july 2017).
  * Fabrice Bellard BPG: `https://github.com/mirrorer/libbpg. The version used is 0.9.7.
  * Xiph Daala: `https://git.xiph.org/?p=daala.git`. The version used is built from GIT revision `05243557bc3e59872fd043c99dc4c17ca33bcb1b`.
  * Jon Sneyers FLIF: `https://github.com/FLIF-hub/FLIF`. The version used is built from GIT revision `5bff895fcab219d0ae56a300ae8bada5b546bd54`.
  * Microsoft JxrLib: `https://jxrlib.codeplex.com/`. The version used is built from GIT revision `e922fa50cdf9a58f40cad07553bcaa2883d3c5bf`.
  * Kakadu JPEG2000: `http://kakadusoftware.com/downloads/`. The version used is 7.8.
  * Mozilla JPEG Encoder: `https://github.com/mozilla/mozjpeg`. The version used is 3.1
  * Google WebP: `https://chromium.googlesource.com/webm/libwebp`. The version is built from GIT revision `28ce3043448bd3a941989939521cd333b6a6ae39`.

###Metrics

  * The VMAF (Video Multi-Method Assessment Fusion) metric is computed using `vmafossexec`, provided by Netflix: `https://github.com/Netflix/vmaf`. The version used is built from GIT revision `7ebbde0c64493af978da66cb7ebe2946fb12dec2`.

    `vmafossexec` compares two YUV files, given their subsampling and dimensions.
    
  * Y-MSSIM, Y-SSIM, RGB-SSIM and PNSR-HVS-M are computed by the tools `dump_msssim`, `dump_ssim` and `dump_psnrhvs`, provided by the Daala repository: `https://git.xiph.org/?p=daala.git`. The version used is built from GIT revision `05243557bc3e59872fd043c99dc4c17ca33bcb1b`.

    Each metric compares two Y4M files.

###Tools

  * `ffmpeg` is used for image formats conversion. The version used is ffmpeg 3.1.4.
  * `gm identify` is used to determine the width and height of images. The version used is GraphicsMagick 1.3.25.
  * `bc` is used for BASH inline calculations . The version used is bc 1.06.95.
  *  GNU `time` is used to mesure computing times. The version used is GNU time 1.7.
  *  A spreedsheet application, either Excel or LibreOffice Calc.

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

The BASH scripts used to generate the compressed images are available on [the GIT repository](https://github.com/WyohKnott/image-comparison-sources).

###Image selection

The images which will be displayed on the website are then chosen among all compressed images, using the following criteria:

  * The BPG file at quality 24 is chosen to be the reference filesize for *large* quality images.
  * The filesize for *medium* quality images is 60% of the *large* filesize.
  * The filesize for *small* quality images is 60% of the *medium* filesize.
  * The filesize for *tiny* quality images is 60% of the *small* filesize.

The BASH scripts used to select the compressed images are available on [the GIT repository](https://github.com/WyohKnott/image-comparison-sources).

###Lossless encoding and decoding speeds:

For each codec and image, the encoding and decoding speeds for lossless compression are sampled five times using GNU `time`. A BASH script [18_gather_lossless_metrics.sh](https://github.com/WyohKnott/image-comparison-sources/blob/master/18_gather_lossless_metrics.sh) is available on the GIT repository to perform this operation.

The arithmetic mean of encoding and decoding speeds are calculated over the entire image set. We then determine a [Weissman score](https://en.wikipedia.org/wiki/Weissman_score) for each codec using the following formula:

$$W = \alpha {r \over \overline{r}} {\log{\overline{T}} \over \log{T}}$$

where `r` is the compression ratio over PPM filesize, `T` the time required to compress, `̅r` and `̅T` the same metrics for the standard compressor, and alpha is a scaling constant.

The standard compressor used is the compression of a PPM image to a PNG image using FFMPEG: `ffmpeg -loglevel quiet -y -i [input(PPM)] -pix_fmt yuv420p [output(PNG)]`

###Lossy metrics

For each codec and image, we apply the following metrics, Y-SSIM, RGB-SSIM, Y-MSSSIM, PSNR-HVS-M and VMAF, over 15 image samples of increasing quality. For VMAF, we use the trained model `nflxall_vmafv4.pkl` given by Netflix.

For each sample, we first decode the compressed image, then export the resulting file to 4:2:0 Y4M and YUV format using FFMPEG (`ffmpeg -loglevel quiet -y -i [input] -pix_fmt yuv420p [output]`). Finally we apply the metrics over each sample, comparing it to the original image. A BASH script [19_gather_lossy_metrics.sh](https://github.com/WyohKnott/image-comparison-sources/blob/master/19_gather_lossy_metrics.sh) is available on the GIT repository to perform this operation.

For each codec, we calculate the arithmetic mean of each metric over the entire set of images, weighted by the area of the corresponding picture, for the 15 samples of increasing quality:

$$\overline{metric}_{quality\ q} = \frac{ \sum\limits_{picture=1}^{50} metric_{qp} \times area_{p}}{\sum\limits_{p=1}^{50} area_{p}}$$

We also determine the average bits per pixel for each quality sample:

$$\overline{bpp}_{quality\ q} = \frac{ \sum\limits_{picture=1}^{50} filesize_{qp}}{\sum\limits_{picture=1}^{50} area_{p}}$$

##Results

###Raw data

The following spreedsheets contain the raw data for lossless and lossy metrics:

  * [Lossless compression ratio and encoding speed](http://wyohknott.github.io/image-formats-comparison/lossless_results.ods)
  * [Lossy metrics](http://wyohknott.github.io/image-formats-comparison/lossy_results.ods)

###Lossless compression ratio and Weissman score:


| codec             | compression ratio | avg. enc. time | avg. dec. time | Reduction gain (from PNG) | Weissman score |
|:------------------|:-----------------:|:--------------:|:--------------:|:-------------------------:|:--------------:|
| Daala             |   4,249   |   0,19    |   0,19    |   183,62% |   3,05    |
| AV1 (oct 2016)    |   4,436   |   2,81    |   0,14    |   196,13% |   2,50    |
| AV1 (jul 2017)    |   4,565   |   9,68    |   0,13    |   204,74% |   2,22    |
| JPEG XR           |   2,330   |   0,12    |   0,15    |   55,50%  |   1,81    |
| JPEG 2000         |   2,364   |   0,14    |   0,14    |   57,79%  |   1,79    |
| FLIF              |   3,786   |   6,35    |   1,07    |   152,70% |   1,62    |
| WebP              |   3,237   |   4,17    |   0,59    |   116,07% |   1,46    |
| Reference (PNG)   |   1,498   |   0,28    |   0,16    |   0,00%   |   1,00    |
| BPG               |   1,750   |   2,01    |   0,89    |   16,84%  |   0,86    |
| Mozjpeg           |   1,745   |   2,43    |   0,13    |   16,51%  |   0,84    |

###Lossy metrics

For each comparison algorithms, we plot the quality in dB in function of the mean bits per pixel on a logarithmic scale. We can then visualize which codec gives the best quality at a given bit per pixel (top left is better).

####Bits per pixel at equivalent quality according to VMAF

![Bits per pixel at equivalent quality according to VMAF](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20VMAF.svg)

####Bits per pixel at equivalent quality according to Y-PSNR-HVS-M

![Bits per pixel at equivalent quality according to Y-PSNR-HVS-M](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20Y-PSNR-HVS-M.svg)

####Bits per pixel at equivalent quality according to Y-MSSSIM

![Bits per pixel at equivalent quality according to Y-MSSSIM](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20Y-MSSSIM.svg)

####Bits per pixel at equivalent quality according to Y-SSIM

![Bits per pixel at equivalent quality according to Y-SSIM](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20Y-SSIM.svg)

####Bits per pixel at equivalent quality according to RGB-SSIM

![Bits per pixel at equivalent quality according to RGB-SSIM](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20RGB-SSIM.svg)



