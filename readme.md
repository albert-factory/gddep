## 命令列表
### 工作区相关
- gddep work set <_path>
- gddep work #打印信息
- gddep work quit

### 工件创建相关
- gddep create addon <group:id:version>
- gddep create reson <group:id:version>

### 工件依赖相关
- gddep dep install
- gddep dep add addon <group:id:version>
- gddep dep add reson <group:id:version>
- gddep dep remove addon <group:id:version>
- gddep dep remove reson <group:id:version>
- gddep dep over addon <group:id:version>
- gddep dep over reson <group:id:version>
- gddep dep rever addon <group:id:version>
- gddep dep rever reson <group:id:version>

### 工件管理
- gddep artifact install <group:id:version> <zip|path|url> <_path>
- gddep artifact installed
- gddep artifact pull <group:id:version>
- gddep artifact search <_name>
- gddep artifact avail <group:id:version>
- gddep artifact tree <group:id:version>
- gddep artifact build #打包当前工件

### 本地仓库管理
- gddep repo local <_path>
- gddep repo update
- gddep repo add <remote_repo_id> <remote_repo_url>
- gddep repo remove <remote_repo_id>
- gddep repo #打印信息

### 远程仓库管理
- gddep remote init <_name> <_manager> <_path>
- gddep remote set <_path> 
- gddep remote #打印信息
- gddep remote publish
- gddep remote add addon <_path_gddep.yml>
- gddep remote add reson <_path_gddep.yml>
- gddep remote remove addon <group:id>
- gddep remote remove reson <group:id>

## 项目结构列表
### 工件结构addon
- ARTIFACT_ROOT/
    - addons/
        - addon_id/
            - plugin.cfg
            - some.gd
            - ...
    - build/
        - group-id-version.zip
        - group-id-version.gddep.yml #版本配置
        - group-id.gddep.yml #简要配置
    - gddep.yml #版本配置
    - .gitignore
    - project.godot #支持godot4.5+

### 工件结构reson
- ARTIFACT_ROOT/
    - resons/
        - reson_id/
            - plugin.cfg
            - some.gd
            - ...
    - build/
        - group-id-version.zip #zip里仅包含resons/reson_id里面的全部代码
        - group-id-version.gddep.yml #工件项目版本配置
        - group-id.gddep.yml #工件简要配置
    - gddep.yml #工件项目版本配置
    - .gitignore
    - project.godot #支持godot4.5+

### 本地仓库结构
- GDDEP/ #在$user/.gddep/下，或者在环境变量$GDDEP_HOME/ 下
    - repo.yml #本地仓库描述
    - repo_onlines/
        - repo_id1.yml #远程仓库描述
        - repo_id1_onlines.yml #远程仓库工件可用列表
        - repo_id2.yml #远程仓库描述
        - repo_id2_onlines.yml #远程仓库工件可用列表
    - local_repo/
        - addons/
            - group/
                - id/
                    - group-id-version.zip
                    - group-id-version.gddep.yml #工件版本描述
                    - group-id.gddep.yml #工件简要描述
        - resons/
            - group/
                - id/
                    - group-id-version.zip
                    - group-id-version.gddep.yml #工件版本描述
                    - group-id.gddep.yml #工件简要描述

### 远程仓库结构
- REPO/
    - repo.yml #远程仓库描述
    - repo_onlines.yml #远程仓库工件可用列表
    - addons/
        - group/
            - id/
                - group-id.gddep.yml #工件简要描述
    - resons/
        - group/
            - id/
                - group-id.gddep.yml #工件简要描述

## 配置文件列表
### 工件简要配置
```yml
gddep: v1.0.0
artifact:
    group: project.group.name
    id: gdlibnameid
    type: addon|reson
    author: natsufumij
    arch: gdscript|csharp
    info: "This is artifact Infos.."
    fetch: 
        - files://somepath
        - https://gitee.com/some/id
        - https://github.com/some/id
        - https://some.com/abc/defg/${version}.zip
```

### 工件版本描述
```yml
gddep: v1.0
artifact:
    group: org.example
    id: gdlib0
    type: addon|reson
    author: natsufumij
    arch: gdscript #该项目使用的语言
    platform: 4.4+ #支持的godot引擎版本 
    info: "This is artifact Infos.."
    fetch: 
        - files://somepath
        - https://gitee.com/some/id
        - https://github.com/some/id
        - https://some.com/abc/defg/${version}.zip
    version: v1.0.0
    project: #自动继承本项目配置
        - autoload
        - gui
        - input
        - input_devices
        - layer_names
        - rendering
        - file_customization
        - ...
    deps:
        resons:
            - org.example:gdasset0:v1.0.0
            - org.example:gdasset1:v1.0.0
        addons:
            - org.example:updater:v1.0.0
            - org.example:netechange:v1.0.0
    overdeps:
        resons:
            - org.example:gdasset0-v2:v1.0.2
        addons:
            - org.example:updater-lib0:v1.2.0
```

### 本地仓库描述
```yml
gddep: v1.0
repo:
    local: D:/gddep/local_repo/
    remote: 
        - id: repo_id1
          url: http://github.com/group/id/master/
        - id: repo_id2
          url: http://github.com/group/id2/master/
```

### 远程仓库描述
```yml
gddep: v1.0
repo:
    manager: natsufumij
    name: repo_name
    publish: "202601181115"
```

### 远程仓库工件可用列表
```yml
gddep: v1.0
artifacts:
    addons:
        - group: groupname
          id: id
          author: natsufumij
          fetch:
            - url1
            - url2
        - group: groupname
          id: id
          author: natsufumij
          fetch: 
            - url1
            - url2
    resons:
        - group: groupname
          id: id
          author: natsufumij
          fetch:
            - url1
            - url2
        - group: groupname
          id: id
          author: natsufumij
          fetch: 
            - url1
            - url2
```

