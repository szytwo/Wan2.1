## fork

https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner

## 安装

```
conda create --prefix ./venv python==3.11
conda activate ./venv

pip install -r ./api_requirements.txt -i https://mirrors.aliyun.com/pypi/simple

nvidia-smi -L  # 查看GUID

modelscope download Wan-AI/Wan2.1-T2V-1.3B --local_dir ./models/Wan2.1-T2V-1.3B
modelscope download Qwen/Qwen2.5-7B-Instruct --local_dir ./models/Qwen2.5-7B-Instruct
modelscope download Qwen/Qwen2.5-3B-Instruct --local_dir ./models/Qwen2.5-3B-Instruct

```

## Docker镜像操作

```

docker build -t wan2.1:1.0 . --progress=plain # 构建镜像
docker build -t wan2.1:1.0 --cache-from wan2.1:1.0 .
docker load -i wan2.1-1.0.tar # 导入镜像
docker save -o wan2.1-1.0.tar wan2.1:1.0 # 导出镜像
docker-compose up -d # 后台运行容器
docker builder prune -a #强制清理所有构建缓存

docker compose run --rm wan2.1 python generate.py  --task t2v-1.3B --size 832*480 --ckpt_dir ./models/Wan2.1-T2V-1.3B --sample_shift 8 --sample_guide_scale 6 --prompt "Two anthropomorphic cats in comfy boxing gear and bright gloves fight intensely on a spotlighted stage." --save_file results/cats_boxing.mp4


docker compose run --rm wan2.1 torchrun --nproc_per_node=2 generate.py --task t2v-1.3B --size 832*480 --ckpt_dir ./models/Wan2.1-T2V-1.3B --dit_fsdp --t5_fsdp --ulysses_size 2 --sample_shift 8 --sample_guide_scale 6 --prompt "Two anthropomorphic cats in comfy boxing gear and bright gloves fight intensely on a spotlighted stage." --save_file results/cats_boxing.mp4

```

## 安全压缩 WSL2/Docker 虚拟磁盘

```

wsl --shutdown # 关闭Docker/WSL
diskpart # 进入磁盘管理工具
select vdisk file="D:\Docker\DockerDesktopWSL\disk\docker_data.vhdx" # 选择虚拟磁盘文件（即 Docker 的 WSL2 数据文件
attach vdisk readonly # 以只读模式挂载磁盘
compact vdisk # 压缩虚拟磁盘文件
detach vdisk # 卸载磁盘
exit # 退出 diskpart

```

## GIT

```
git pull # 拉取
git push # 推送

git submodule add https://github.com/szytwo/fastText.git # 添加子模块
git submodule add https://github.com/szytwo/hanziconv.git # 添加子模块
git submodule update --init --recursive # 初始化子模块
git pull --recurse-submodules  # 拉取主项目更新，并递归更新子模块

git branch -r # 查看分支
git branch -m main # 重命名分支
git branch --set-upstream-to=origin/main main #关联远程分支origin/main 

git remote -v # 查看远程仓库
git remote remove origin # 移除远程仓库连接，origin，upstream

# 添加新的远程仓库，origin，upstream
git remote add upstream https://github.com/MontrealCorpusTools/Montreal-Forced-Aligner.git 

git fetch upstream # 从远程仓库拉取更新，origin，upstream
git checkout main # 切换到主分支
git merge upstream/main # 合并到本地分支,主分支名称可能是 ，origin，upstream，master,main 

git reset --hard origin/main # 强制覆盖本地代码

```