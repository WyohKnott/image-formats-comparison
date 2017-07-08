#Evolution of the AV1 codec between October 2016 and July 2017

The VMAF metric significatively deteriorated however all other metrics improved. The encoding time increased by a factor of around 
7,3.

###Lossless compression ratio and Weissman score:

| codec             | compression ratio | avg. enc. time | avg. dec. time | Reduction gain (from PNG) | Weissman score |
|:------------------|:-----------------:|:--------------:|:--------------:|:-------------------------:|:--------------:|
| Daala             |   4,249   |   0,13    |   0,13    |   183,62% |   3,10    |
| AV1 (oct 2016)    |   4,436   |   1,27    |   0,09    |   196,13% |   2,20    |
| AV1 (jul 2017)    |   4,565   |   9,25    |   0,13    |   204,73% |   1,77    |
| Reference (PNG)   |   1,498   |   0,20    |   0,11    |   0,00%   |   1,00    |
| BPG               |   1,752   |   0,72    |   0,68    |   16,93%  |   0,94    |

####Bits per pixel at equivalent quality according to VMAF

![Bits per pixel at equivalent quality according to VMAF](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20VMAF%20-%20comparison%20oct2016-jul2017.svg)

####Bits per pixel at equivalent quality according to Y-PSNR-HVS-M

![Bits per pixel at equivalent quality according to Y-PSNR-HVS-M](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20Y-PSNR-HVS-M%20-%20comparison%20oct2016-jul2017.svg)

####Bits per pixel at equivalent quality according to Y-MSSSIM

![Bits per pixel at equivalent quality according to Y-MSSSIM](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20Y-MSSSIM%20-%20comparison%20oct2016-jul2017.svg)

####Bits per pixel at equivalent quality according to Y-SSIM

![Bits per pixel at equivalent quality according to Y-SSIM](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20Y-SSIM%20-%20comparison%20oct2016-jul2017.svg)

####Bits per pixel at equivalent quality according to RGB-SSIM

![Bits per pixel at equivalent quality according to RGB-SSIM](http://wyohknott.github.io/image-formats-comparison/Bits%20per%20pixel%20at%20equivalent%20quality%20according%20to%20RGB-SSIM%20-%20comparison%20oct2016-jul2017.svg)




