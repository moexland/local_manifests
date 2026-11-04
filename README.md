# local_manifests

## 简览 | At a Glance

GrimoireOS的repo本地清单。它在GrapheneOS的清单之上，把bionic与hardened_malloc换成GrimoireOS的fork，并加入设备支持仓mdevtool。GrimoireOS是在努比亚红魔11 Pro+（NX809J）上运行的GrapheneOS移植。

The repo local manifest of GrimoireOS. On top of GrapheneOS's manifest, it swaps bionic and hardened_malloc for GrimoireOS's forks and adds the device support repository, mdevtool. GrimoireOS is a port of GrapheneOS to the nubia RedMagic 11 Pro+ (NX809J).

## 基线 | Base

GrimoireOS建立在两份上游之上：系统部分来自GrapheneOS的源码，vendor、odm、内核与启动镜像沿用红魔原厂固件。当前分支对应的版本如下：

GrimoireOS stands on two upstreams: the system comes from GrapheneOS's source, while the vendor, odm, kernel and boot images are kept from the stock RedMagic firmware. The versions for the current branch are:

| 上游 \| Upstream | 版本 \| Version |
|---|---|
| GrapheneOS | `2026110200` |
| 原厂固件 \| Stock firmware | `RedMagicOS11.0.23MR1` |

原厂固件的vendor构建指纹是`REDMAGIC/NX809J/NX809J:16/BQ2A.250705.001-BP2A.250605.031.A3/20260317.233524:user/release-keys`，装好GrimoireOS后`ro.vendor.build.fingerprint`仍是这个值。mdevtool按哈希核对这份固件里的镜像，换用别的原厂版本须先更新它的输入清单。

The vendor build fingerprint of the stock firmware is `REDMAGIC/NX809J/NX809J:16/BQ2A.250705.001-BP2A.250605.031.A3/20260317.233524:user/release-keys`, and `ro.vendor.build.fingerprint` keeps that value once GrimoireOS is installed. mdevtool checks the images from this firmware by hash, so moving to another stock version means updating its input list first.

## 使用 | Usage

先按`grimoireos.xml`开头注释里写明的GrapheneOS标签初始化源码树，再把本仓库放进`.repo/local_manifests`并同步：

First initialise the source tree at the GrapheneOS tag named in the comment at the top of `grimoireos.xml`, then put this repository in `.repo/local_manifests` and sync:

```bash
repo init -u https://github.com/GrapheneOS/platform_manifest.git -b refs/tags/2026110200
git clone -b 17 https://github.com/moexland/local_manifests.git .repo/local_manifests
repo sync -j8
```

生成vendor树与构建的步骤见mdevtool的README。

The steps to generate the vendor tree and build are in mdevtool's README.

## 构建号 | Build Numbers

GrimoireOS的构建号是所基于的GrapheneOS标签加两位序号：基于2026110200的构建依次是2026110201、2026110202……直到换下一个上游标签。构建号不会高于所基于的上游，因为GrapheneOS的应用商店按这个号判断系统是否满足包的版本要求。

A GrimoireOS build number is the GrapheneOS tag it is based on plus a two-digit sequence: builds on 2026110200 are 2026110201, 2026110202 and so on, until the next upstream tag. A build number never exceeds the upstream it is based on, because GrapheneOS's app store compares it against the system version that packages require.

每次发布时，各仓都以构建号打标签，标签说明里写明对应的GrapheneOS标签。

At each release, every repository is tagged with the build number, and the tag message names the corresponding GrapheneOS tag.
