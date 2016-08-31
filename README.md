ansible test
============

# 概要

docker にて完結したansibleのテスト環境
docker network内で環境隔離しているのでdocker hostマシン内にてansibleを入れる必要はなし

master,targetのマシンが稼働し　master->targetにてansibleを実行する

# run
    docker-compose up -d


# sshkey change
    $ cat id_rsa | docker run -i <image> sh -c 'cat > /root/.ssh/id_rsa'
