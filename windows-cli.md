#### windows-cli
查看端口占用
netstat -aon|findstr "9050"

#### wmic
```
$ wmic diskdrive list brief
Caption                 DeviceID            Model                   Partitions  Size
WDC WD5000LPLX-08ZNTT0  \\.\PHYSICALDRIVE1  WDC WD5000LPLX-08ZNTT0  1           500105249280
KBG30ZMT128G TOSHIBA    \\.\PHYSICALDRIVE0  KBG30ZMT128G TOSHIBA    3           128034708480

$ wmic diskdrive get Name,Model
Model                   Name
WDC WD5000LPLX-08ZNTT0  \\.\PHYSICALDRIVE1
KBG30ZMT128G TOSHIBA    \\.\PHYSICALDRIVE0
```