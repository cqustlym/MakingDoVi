必要工具：ffmpeg、MakeMKV、dovi_tool

* 1 制作一个带DV的Remux，可以用MakeMKV做或者其他地方下一个Remux

* 2 用dovi_tool分离DV层

​		dovi_tool GitHub地址：https://github.com/quietvoid/dovi_tool

```bash
ffmpeg -i "remux的地址" -c:v copy -vbsf hevc_mp4toannexb -f hevc - | dovi_tool -m 2 -c extract-rpu - -o RPU.bin
```

例：

```bash
ffmpeg -i "D:\\Demux_out_of_the_blue\\00005.track_4117.mkv” -c:v copy -vbsf hevc_mp4toannexb -f hevc - | dovi_tool -m 2 -c extract-rpu - -o RPU.bin
```

* 3 压制时候添加x265参数:

```bash
--dolby-vision-profile 8.1 --dolby-vision-rpu "link to RPU.bin"
```

例：

```
--vbv-bufsize 160000 --vbv-maxrate 160000 --high-tier  --bframes 16 --rd 4 --me star --subme 5 --merange 57 --ipratio 1.3 --pbratio 1.2 --aq-mode 4 --aq-strength 1.6 --qcomp 0.6 --psy-rd 1.8 --psy-rdoq 1.6 --ctu 32 --rc-lookahead 60 --deblock -3:-3 --cbqpoffs 0 --crqpoffs 0 --qg-size 8 --range limited --no-frame-dup --selective-sao 0 --no-cutree --tu-intra-depth 4 --tu-inter-depth 4  --no-tskip --no-early-skip --rect --amp --no-sao  --aud --repeat-headers --hrd --hdr-opt --colorprim bt2020 --colormatrix bt2020nc --transfer smpte2084 --master-display "G(13250,34500)B(7500,3000)R(34000,16000)WP(15635,16450)L(10000000,1)" --max-cll=0,0 --chromaloc 0 --pmode --dolby-vision-profile 8.1 --dolby-vision-rpu "D:\Demux_out_of_the_blue\RPU.bin"
```

HDR常规参数见PTer压制组内HDR教程