Created Date：2022-12-17 22:08:05  
Last Modified：2022-12-17 22:08:04

# Tags

#Node

# Content

## nvm mac bug fixed

### 现象

`nvm install xx.xx.xx` 报 `Version 'xx.xx.xx' not found - try nvm ls-remote to browse available versions.` 错，`nvm ls-remote` 之后，列出的不是 node 版本而是 iojs 版本 ![[Pasted image 20230630213419.png]]

### 解决

`export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/dist`

# Reference
