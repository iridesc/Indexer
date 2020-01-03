# indexer

将非结构化的文本数据快速结构化，并进行文本索引。


# 安装
克隆项目到本地
# 本地使用
## 数据库建立
浏览器打开indexer/dits/index.html
点击datamaker
在左侧文本框粘贴条目文本，下面会出现文本关键字的列表。
点击选择关键字，选中的关键字将作为索引的关键字。
在右侧文本框会实时显示所编辑的条目的数据。
在右侧的输入框可自定义该条目关键字。
点击push item按钮将会把条目加入数据库。
点击copy database 将会将数据库复制到粘贴板。
在indexer/assets/下建立myDatabasesName.json，并将粘贴板中的数据粘贴进文件。
在DBs.json中加入myDataBaseName。
然后npm run build
刷新浏览器页面，你的数据库将展现在首页。选中即可进行索引。



## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
