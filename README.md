# Mount Opacity

一个 Minecraft Java Edition Fabric 客户端 Mod，用来调节本地玩家当前骑乘生物的透明度。

## 功能

- 通过 Mod Menu 打开配置界面。
- 生物坐骑透明度和矿车/船透明度分开配置，范围均为 `0.0` 到 `1.0`。
- `0.0` 表示完全透明。
- `1.0` 表示完全不透明。
- 配置会保存到客户端 `config/mount-opacity.json`。
- 只影响本地玩家当前骑乘的生物类坐骑，例如马、猪、骆驼等。
- 也支持本地玩家当前乘坐的矿车、船和运输船。

## 目标版本

当前工程已适配到 `Minecraft Java Edition 26.2 + Fabric`。Fabric 官方 26.2 迁移说明建议开发者使用 `Loom 1.17` 和 `Gradle 9.5.1`，玩家端 Fabric Loader 当前稳定版为 `0.19.3`。

26.x 不再按旧 Yarn 流程开发，当前工程已迁移到 Mojang 官方命名，并使用新的 `net.fabricmc.fabric-loom` 插件。

当前版本配置在 `gradle.properties`：

- `minecraft_version=26.2`
- `loader_version=0.19.3`
- `fabric_api_version=0.152.0+26.2`
- `modmenu_version=20.0.0-alpha.1`
- `cloth_config_file_id=6351757`

如果你需要改成其他版本，请修改 `gradle.properties` 中的这些字段：

- `minecraft_version`
- `loader_version`
- `fabric_api_version`
- `modmenu_version`
- `cloth_config_file_id`

## 构建

需要 Java 25。项目已包含 Gradle Wrapper，会自动使用 `Gradle 9.5.1`，不需要再单独安装系统级 Gradle。

如果你还没有配置 Java 25，可以先运行项目内的一键安装脚本。

Windows：

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\setup-java25-windows.ps1
```

macOS / Linux：

```bash
./scripts/setup-java25-unix.sh
```

安装脚本会把 JDK 25 安装到当前用户目录下的 `.jdks/jdk-25`，并自动配置 `JAVA_HOME`。脚本运行结束后，请关闭并重新打开终端。

macOS / Linux：

```bash
./gradlew build
```

Windows：

```bat
gradlew.bat build
```

构建成功后，Mod 文件会生成在：

```text
build/libs/
```

把生成的 `.jar` 放入客户端的 `mods` 文件夹，并同时安装 Fabric Loader、Fabric API、Mod Menu、Cloth Config。

## 适配说明

- 已把 Loom 从 `1.6-SNAPSHOT` 升级到 `1.17.13`。
- 已把 Loom 插件 ID 从旧的 `fabric-loom` 改为 26.x 推荐的 `net.fabricmc.fabric-loom`。
- 已把 Minecraft 依赖从 `1.20.2` 升级到 `26.2`。
- 已把 Fabric Loader 从 `0.15.11` 升级到 `0.19.3`。
- 已把 Java 目标版本从 `17` 升级到 `25`。
- 已添加 Gradle Wrapper，固定使用 `Gradle 9.5.1`。
- 已从 Yarn 包名迁移到 Mojang 官方命名包名，例如 `net.minecraft.client.Minecraft`、`net.minecraft.world.entity.LivingEntity`。
- Cloth Config 改为通过 CurseMaven 文件 ID `6351757` 引入，避免使用不存在的 Shedaniel Maven 版本号。
- `fabric.mod.json` 中的 Minecraft 与 Loader 依赖会跟随 `gradle.properties` 自动展开。
- 透明坐骑逻辑仍通过 `LivingEntityRenderer` Mixin 修改本地玩家当前坐骑的模型 alpha。
- 矿车、船和运输船逻辑通过 `AbstractMinecartRenderer`、`AbstractBoatRenderer` 和模型提交 Mixin 应用独立透明度。

## 主要源码

- `src/main/java/com/trae/mountopacity/MountOpacityClient.java`
- `src/main/java/com/trae/mountopacity/config/MountOpacityConfig.java`
- `src/main/java/com/trae/mountopacity/config/MountOpacityModMenu.java`
- `src/main/java/com/trae/mountopacity/mixin/LivingEntityRendererMixin.java`
