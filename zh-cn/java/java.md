# java

## 概述
本规范旨在为后端开发人员提供在Spring生态下的开发规范

## 代码结构说明
```tree
.
├── README.md                                    # 模块说明文件
├── build.sh                                     # 在终端格编译jar包的脚本
├── format.sh                                    # 在终端格式化java代码的脚本
├── k8s                                          # k8s相关文件
│   └── project.yaml                             # 项目部署k8s实例的配置文件
├── mvnw                                         # Maven工具文件
├── mvnw.cmd                                     # Maven工具文件
├── online.sh                                    # 在终部署正式服的脚本
├── pom.xml                                      # 项目依赖包配置文件
├── preview.sh                                   # 在终端署测试服的脚本
├── project.iml                                  # 项目配置文件
├── run.sh                                       # 在终端运行应用的脚本
├── src                                          # 源码目录
│   ├── main
│   │   ├── java
│   │   │   └── net
│   │   │       └── yichui
│   │   │           └── project                  # 模块主目录
│   │   │               ├── config               # 模块所有配置定义
│   │   │               ├── exception            # 模块所有错误枚举
│   │   │               ├── form                 # 模块所有表单实现与校验
│   │   │               ├── graphql              # graphql接口实现目录
│   │   │               │   ├── data             # DTO定义目录
│   │   │               │   ├── mutation         # 增删改数据接口实现
│   │   │               │   │   ├── me           # 用户增删改数据接口实现
│   │   │               │   │   ├── meAdminEdit  # 工作人员编辑数据接口实现
│   │   │               │   │   └── meAdminExec  # 工作人员增删数据接口实现
│   │   │               │   └── query            # 查询数据接口
│   │   │               │       ├── me           # 用户查询数据接口
│   │   │               │       └── meAdmin      # 工作人员查询数据接口
│   │   │               ├── model                # 数据库主目录
│   │   │               │   ├── orm              # DAO定义目录
│   │   │               │   ├── repo             # 基础数据仓库与高级操作方法
│   │   │               │   └── virtual          # 跨表数据仓库与高级操作方法
│   │   │               ├── service              # 模块逻辑主目录
│   │   │               └── utils                # 工具函数主目录
│   │   └── resources                            # 模块资源目录
│   │       ├── application-any.yml              # 外网开发环境配置
│   │       ├── application-dev.yml              # 内网开发环境配置
│   │       ├── application-prod.yml             # 生产环境配置
│   │       ├── application-test.yml             # 测试环境配置
│   │       ├── application.yml                  # 共享配置
│   │       └── graphql                          # graphql接口定义
│   │           ├── admin.graphqls               # 工作人员接口定义
│   │           ├── input.graphqls               # 所有接口输入表单定义
│   │           ├── me.graphqls                  # 工作人员接口定义
│   │           ├── pub.graphqls                 # 开放接口定义
│   │           ├── public.graphqls              # 所有数据返回结构定义
│   │           ├── result.graphqls              # 所有数据列表类型定义
│   │           └── root.graphqls                # grapqhl入口定义
│   └── test
│       └── java
│           └── net
│               └── yichui
│                   └── project                  # 测试
└── test.sh                                      # 在终端运行测试的脚本
```
