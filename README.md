# Pica/PinCP

## 安装 repo

1. macOS 可以使用 Homebrew 安装：
```bash
brew install repo
```

2. [installing-repo](https://source.android.com/source/downloading?hl=zh-cn#installing-repo)

3. 克隆官方仓库
```bash
git clone https://android.googlesource.com/tools/repo
# OR:
git clone https://gerrit.googlesource.com/git-repo
```

推荐使用方式 1，省去自行配置环境变量。

## repo 命令

[Repo 命令参考资料](https://source.android.com/docs/setup/create/repo?hl=zh-cn)

## 拉取 repo

```bash
repo init -b master -u git@github.com:y4n9b0/RepoManifest.git
repo sync
```

## 配置 maven 账号密码

打开文件 ${HOME}/.gradle/gradle.properties
```bash
MAVEN_USERNAME=***
MAVEN_PASSWORD=***
```

## 发布 catalog
1. 修改 buildScript/mvn-publish-catalog.gradle 的 catalog 版本号：catalog_version
2. 发布

    ```bash
    ./gradlew clean publishCatalogPublicationToMavenRepository
    ```
3. 提交 buildScript 修改

## 发布 lib

1. 修改对应 lib module build.gradle 的 artifact 版本号：POM_VERSION_NAME
2. 发布

    ```bash
    ./gradlew clean :${module}:publishReleasePublicationToMavenRepository
    ```

   **Note** 不要直接复制，使用对应的 module name 替换 `${module}`

## lib 开发联调

有三种方式：
1. 发布 SNAPSHOT 版本到 [maven](http://maven.qa.changbaops.com/artifactory/libs-snapshot-local)，项目使用 maven 依赖
2. 发布 SNAPSHOT 版本到 mavenLocal，项目使用 mavenLocal 依赖。 mavenLocal 位于本地机 `${HOME}/.m2/repository/`
3. 复合构建 [gradle composite build](https://docs.gradle.org/current/userguide/composite_builds.html)

    ```txt
    // 配置在 app 工程 settings.gradle
    includeBuild('../Y4n9b0Repo/') { // 替换为自己本地的 repo 路径
        dependencySubstitution {
            // 替换 widget 模块调试
            substitute module('com.y4n9b0:widget') using project(':widget')
        }
    }
    ```

三种方式倒序优先推荐。

## 其他

1. 三方库需要二次开发发布到 maven 时，尽可能的引入三方库原有的 git 记录，方便后续升级。
2. to be continue...