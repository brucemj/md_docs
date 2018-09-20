
### 2018-1-26 取阿里代码
```
repo init -u git@172.21.5.248:dev/ali/mtk6753_pbase_konka_R1/repo/pbase/platform -b codebase_ctnr_mtk6763_konka
repo init -u git@172.21.5.248:dev/ali/mtk6753_pbase_konka_R1/repo/yunos/tianmu -b codebase_host_mtk6763_konka

repo sync
ctnr:
repo forall -c ' git  branch  --track dev_801_ctnr_mtk6763_konka  extyunos/dev_801_ctnr_mtk6763_konka ; git co dev_801_ctnr_mtk6763_konka '
host:
repo forall -c ' git  branch  --track codebase_host_mtk6763_konka extyunos/codebase_host_mtk6763_konka; git co codebase_host_mtk6763_konka'
```


### mtk仓库
```
测试命令
repo init -u git@172.21.5.248:dev/mtkrepo/8.0/platform/manifest -b kk_manifest -m kk_base_manifest.xml

repo sync alps/device/mediatek/common
repo sync alps/device/mediatek/common --optimized-fetch

```
