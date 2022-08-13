<h1 align="center">Antd Multi Tabs Admin</h1>

<div align="center">🛠️Antd多标签页后台管理模板🛠️</div>

- 尝试多种主题色切换或取色器动态换肤功能（调研开发中👀）
- 集成单元测试 jest + enzyme，争取覆盖率100%💪🏻💪🏻（学习中🤫）

## 动态主题

目前 `antd@4.17.0-alpha.0` 已支持[动态主题](https://ant.design/docs/react/customize-theme-variable-cn)（实验性），感兴趣的话可以去玩玩，互相探讨。
<br />

### 预览图

![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-1.png)
![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-2.png)
![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-3.png)
![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-4.png)
![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-5.png)
<br />

## 使用

### 使用命令行

```bash
$ npm install -g typescript
$ git clone https://github.com/hsl947/react-antd-multi-tabs-admin.git
$ yarn install
$ yarn start         # 访问 http://localhost:666
```

### 权限控制

<p>如果不需要权限控制，可自行注释去掉权限功能。</p>

```
# src/components/common/menu/index.tsx

// 创建可展开的第一级子菜单
const creatSubMenu = (data: any): JSX.Element => {
  // const menuItemList = []
  // data.routes.map((item: any) => {
  //   const arr = permission.filter((ele: any) => item.key === ele.code)
  //   if (arr.length > 0) {
  //     menuItemList.push(renderMenu(item))
  //   }
  //  return arr
  // })

  const menuItemList = data.routes.reduce(
    (prev: any, next: any) => [...prev, renderMenu(next)],
    []
  )

  return menuItemList.length > 0 ? (
    <SubMenu key={data.key} title={subMenuTitle(data)}>
      {menuItemList}
    </SubMenu>
  ) : null
}
```

```
# src/pages/container/index.tsx

// 检查权限
const checkAuth = (newPathname: string): boolean => {
  // 不需要检查权限的
  // if (noCheckAuth.includes(newPathname)) {
  //   return true
  // }
  // const { tabKey: currentKey } = getKeyName(newPathname)
  // return isAuthorized(currentKey)

  // 一定返回 true
  return !!newPathname
}

```

```
# src/components/common/tabPanes/index.tsx

// 移除tab
const remove = (targetKey: string): void => {
  const delIndex = panes.findIndex(
    (item: CommonObjectType) => item.key === targetKey
  )
  panes.splice(delIndex, 1)

  // 删除非当前tab
  if (targetKey !== activeKey) {
    const nextKey = activeKey
    setPanes(panes)
    setActiveKey(nextKey)
    storeTabs(panes)
    return
  }

  // 删除当前tab，地址往前推
  const nextPath = curTab[delIndex - 1]
  const { tabKey } = getKeyName(nextPath)
  // 如果当前tab关闭后，上一个tab无权限，就一起关掉
  // if (!isAuthorized(tabKey) && nextPath !== '/') {
  //   remove(tabKey)
  //   history.push(curTab[delIndex - 2])
  // } else {
  //   history.push(nextPath)
  // }
  history.push(nextPath)
  setPanes(panes)
  storeTabs(panes)
}
```

### Redux/Toolkit 的使用说明

你仍然可以使用 `redux` @Reduxjs/Toolkit是 Redux的封装.使用如下 Reduxjs/Toolkit 用于减少redux模板代码, 按照`就近原则`, 分割代码. 一个状态管理容器切片示例如下.

```
# 在/src/store/slicers
import { createSlice } from '@reduxjs/toolkit'
import type { RootState } from '@/store' // typescript intelligence

export interface AppState {
  theme: string
  collapsed: boolean // 菜单收纳状态, 用于垂直布局
  menuMode: 'horizontal' | 'vertical' // 菜单模式, 用于水平布局
}

const initialState: AppState = {
  collapsed: false,
  theme: 'dark',
  menuMode: 'horizontal'
}

export const appSlice = createSlice({
  name: 'app',
  initialState,
  // The `reducers` field lets us define reducers and generate associated actions
  reducers: {
    setTheme(state, action) {
      state.theme = action.payload
    },
    // ... other setters
  }
})

// actions
export const { setCollapsed, setTheme, setMenuMode } = appSlice.actions

// getters
export const selectTheme = (state: RootState) => state.app.theme
export const selectCollapsed = (state: RootState) => state.app.collapsed
export const selectMenuMode = (state: RootState) => state.app.menuMode
// 返回 已切片的 reducer
export default appSlice.reducer

```

### 路由/菜单配置

```
# 所有路由写在 /src/route/routes.ts （包括菜单栏的路由）
  用于路由权限控制

# 菜单路由基于路由生成, 路由项上增加额外字段标识: 是否在菜单栏展示,及图标等信息

# 你也可以分开写, 增强带单定制功能.
```

### 关于换肤配置

> 本框架是使用 less.js 实现动态切换主题，js文件在 /public/less.min.js

```
# 主题配置文件在 /public/color.less

引用了 antd 组件后，基本不需要自己额外自定义主题样式，因为主题文件里都有。
但是！！！
如果自己写了自定义组件，切换主题后样式显示不正常，
则需要自己在 color.less 底部添加深浅主题对应的样式，具体参考主题文件内额外配置。

```
