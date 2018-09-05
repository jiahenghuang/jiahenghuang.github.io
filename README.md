# steps to take in a new computer
## 1.setup hexo environment

```shell
npm install hexo-cli --save #在新电脑上配置hexo
npm install hexo --save
npm install hexo-server --save
npm install hexo-generator-search --save
npm install hexo-deployer-git --save
npm install hexo-asset-image --save
#注：需要设置站点配置文件post_asset_folder: true
```

## 2.Create a new note

```shell
git pull
hexo new post "new blog"
hexo clean #clear old html file
```

## 3.view in local

```shell
hexo s -g
```

## 4.add new blog sources
```shell
git add --all
git commit -m "add new blog"
git push origin hexo //push to remote branch hexo
```

## 5.push to remote
```shell
hexo d -g
```



