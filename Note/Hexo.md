# Hexo

## 博客搭建

- [Node.js](https://link.juejin.cn/?target=http%3A%2F%2Fnodejs.org%2F) 下载

- [Git](https://link.juejin.cn/?target=http%3A%2F%2Fgit-scm.com%2F) 下载

- 安装`Hexo`

  ```sh
  npm install -g hexo-cli`
  ```

- 初始化`Hexo`

  ```sh
  $ hexo init <folder>
  $ cd <folder>
  $ npm install
  ```

- 启动服务器

  ```sh
  hexo server
  ```

  

- 注意

  - hexo相关命令均在`站点目录`下，用`Git Bash`运行
  - 站点配置文件
    - `_config.yml`
  - 主题配置文件：
    - 站点目录下的`themes`文件夹下的，主题文件夹下的`_config.yml`

## 部署到GithubPages

- 创建[Github](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com)账号

- 创建仓库， 仓库名为：<Github账号名称>.github.io

- 将本地Hexo博客推送到GithubPages

  - 安装`hexo-deployer-git`插件

    ```sh
    $ npm install hexo-deployer-git --save
    ```

  - 添加`SSH key`

    - 创建一个 `SSH key`

      ```sh
      $ ssh-keygen -t rsa -C "邮箱地址"
      ```

    - 添加到 `github`

    - 测试是否添加成功

      ```sh
      $ ssh -T git@github.com
      $ yes
      ```

  - 修改`_config.yml`

    ```
    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repo: git@github.com:<Github账号名称>/<Github账号名称>.github.io.git
      branch: master
    ```

  - 推送到`GithubPages`

    ```sh
    $ hexo g
    $ hexo d
    ```

  - 访问网址： `https://<Github账号名称>.github.io`

## 常用操作

### 创建文章

- 命令

  ```sh
  $ hexo new [layout] <title>
  ```

- 参数说明

  | 参数名 | 功能                    | 文章路径       |
  | ------ | ----------------------- | -------------- |
  | post   | 新建博文                | source/_posts  |
  | page   | 新建页面（如404，分类） | source         |
  | draft  | 草稿                    | source/_drafts |

  - 草稿可通过一下命令发布

    ```sh
    $ hexo publish [layout] <title>
    ```

  - title

    - 是博文markdown文件的名字

### Front-matter

- 博文最上方以 `---` 分隔的那部分

  | 参数         | 描述                 | 默认值       |
  | ------------ | -------------------- | ------------ |
  | `layout`     | 布局                 |              |
  | `title`      | 标题                 |              |
  | `date`       | 建立日期             | 文件建立日期 |
  | `updated`    | 更新日期             | 文件更新日期 |
  | `comments`   | 开启文章的评论功能   | true         |
  | `tags`       | 标签（不适用于分页） |              |
  | `categories` | 分类（不适用于分页） |              |
  | `permalink`  | 覆盖文章网址         |              |

### 文章添加分类与标签

```markdown
categories:
- 个人博客（第一层级）
- Hexo博客（第二层级）
tags:
- Hexo
- 博客
```

- 添加多个分类

  ```markdown
  categories:
  - [日常, 生活]
  - [日常, 随想]
  - [日记]
  ```

### 常用命令

- 清除缓存：`hexo clean`
- 生成静态文件：`hexo generate`可简写为`hexo g`
- 启动服务器：`hexo server`或者 `hexo s` 常用参数：`-p（--port）`重设端口
- 部署：hexo deploy`可简写为`hexo d















