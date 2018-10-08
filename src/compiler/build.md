## compiler/build.sh

有三点 
1. stable beta nightly : rust 环境
2. rustfmt clippy miri : 工具环境
3. false : 是否上传docker镜像, 当然这是作者账号的

主要做了

- `docker pull` 拉取镜像或者更新指定镜像 [更多](http://www.runoob.com/docker/docker-pull-command.html)
- `docker build` 用于使用 Dockerfile 创建镜像. [更多](http://www.runoob.com/docker/docker-build-command.html)
- `docker tag` 标记本地镜像，将其归入某一仓库 [更多](http://www.runoob.com/docker/docker-tag-command.html)
- `if [[ "${perform_push}" == 'true' ]]` - `docker push` 作者将本地的镜像上传到镜像仓库,要先登陆到镜像仓库 [更多](http://www.runoob.com/docker/docker-push-command.html)

``` bash
#!/bin/bash

set -euv -o pipefail

channels_to_build="${CHANNELS_TO_BUILD-stable beta nightly}" #  环境变量 CHANNELS_TO_BUILD
tools_to_build="${TOOLS_TO_BUILD-rustfmt clippy miri}"
perform_push="${PERFORM_PUSH-false}"
# echo $channels_to_build  //  stable beta nightly 
# $tools_to_build  // rustfmt clippy miri 
# $perform_push // false

repository=shepmaster

for channel in $channels_to_build; do
    cd "base"

    image_name="rust-${channel}"
    full_name="${repository}/${image_name}"

    docker pull "${full_name}"
    docker build -t "${full_name}" \
           --cache-from "${full_name}" \
           --build-arg channel="${channel}" \
           .
    docker tag "${full_name}" "${image_name}"

    if [[ "${perform_push}" == 'true' ]]; then
        docker push "${full_name}"
    fi

    cd ..
done

crate_api_base=https://crates.io/api/v1/crates

for tool in $tools_to_build; do
    cd "${tool}"

    image_name="${tool}"
    full_name="${repository}/${image_name}"

    docker pull "${full_name}"
    # 
    docker build -t "${full_name}" \
           --cache-from "${full_name}" \
           .
    docker tag "${full_name}" "${image_name}"

    if [[ "${perform_push}" == 'true' ]]; then
        docker push "${full_name}"
    fi

    cd ..
done

```