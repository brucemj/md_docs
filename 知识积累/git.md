
### repo仓库地址变更
#### 更改仓库地址
repo forall -c 'git remote set-url outsidegit ssh://Dh_Konka@120.55.12.197:29418/$REPO_PROJECT'

#### 更改清单仓库地址
git remote set-url outsidegit  ssh://Dh_Konka@120.55.12.197:29418/repo/pbase/platform

#### 更改清单文件地址
<remote fetch="../.." name="outsidegit" review="http://120.55.12.197:8080"/>
