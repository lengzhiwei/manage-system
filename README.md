<h1 align="center">Antd Multi Tabs Admin</h1>

<div align="center">ð ï¸Antdå¤æ ç­¾é¡µåå°ç®¡çæ¨¡æ¿ð ï¸</div>

- å°è¯å¤ç§ä¸»é¢è²åæ¢æåè²å¨å¨ææ¢è¤åè½ï¼è°ç å¼åä¸­ðï¼
- éæååæµè¯ jest + enzymeï¼äºåè¦çç100%ðªð»ðªð»ï¼å­¦ä¹ ä¸­ð¤«ï¼

## å¨æä¸»é¢

ç®å `antd@4.17.0-alpha.0` å·²æ¯æ[å¨æä¸»é¢](https://ant.design/docs/react/customize-theme-variable-cn)ï¼å®éªæ§ï¼ï¼æå´è¶£çè¯å¯ä»¥å»ç©ç©ï¼äºç¸æ¢è®¨ã
<br />

### é¢è§å¾

![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-1.png)
![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-2.png)
![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-3.png)
![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-4.png)
![](https://github.com/hsl947/hsl947.github.io/raw/master/images/antd-admin-5.png)
<br />

## ä½¿ç¨

### ä½¿ç¨å½ä»¤è¡

```bash
$ npm install -g typescript
$ git clone https://github.com/hsl947/react-antd-multi-tabs-admin.git
$ yarn install
$ yarn start         # è®¿é® http://localhost:666
```

### æéæ§å¶

<p>å¦æä¸éè¦æéæ§å¶ï¼å¯èªè¡æ³¨éå»ææéåè½ã</p>

```
# src/components/common/menu/index.tsx

// åå»ºå¯å±å¼çç¬¬ä¸çº§å­èå
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

// æ£æ¥æé
const checkAuth = (newPathname: string): boolean => {
  // ä¸éè¦æ£æ¥æéç
  // if (noCheckAuth.includes(newPathname)) {
  //   return true
  // }
  // const { tabKey: currentKey } = getKeyName(newPathname)
  // return isAuthorized(currentKey)

  // ä¸å®è¿å true
  return !!newPathname
}

```

```
# src/components/common/tabPanes/index.tsx

// ç§»é¤tab
const remove = (targetKey: string): void => {
  const delIndex = panes.findIndex(
    (item: CommonObjectType) => item.key === targetKey
  )
  panes.splice(delIndex, 1)

  // å é¤éå½åtab
  if (targetKey !== activeKey) {
    const nextKey = activeKey
    setPanes(panes)
    setActiveKey(nextKey)
    storeTabs(panes)
    return
  }

  // å é¤å½åtabï¼å°åå¾åæ¨
  const nextPath = curTab[delIndex - 1]
  const { tabKey } = getKeyName(nextPath)
  // å¦æå½åtabå³é­åï¼ä¸ä¸ä¸ªtabæ æéï¼å°±ä¸èµ·å³æ
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

### Redux/Toolkit çä½¿ç¨è¯´æ

ä½ ä»ç¶å¯ä»¥ä½¿ç¨ `redux` @Reduxjs/Toolkitæ¯ Reduxçå°è£.ä½¿ç¨å¦ä¸ Reduxjs/Toolkit ç¨äºåå°reduxæ¨¡æ¿ä»£ç , æç§`å°±è¿åå`, åå²ä»£ç . ä¸ä¸ªç¶æç®¡çå®¹å¨åçç¤ºä¾å¦ä¸.

```
# å¨/src/store/slicers
import { createSlice } from '@reduxjs/toolkit'
import type { RootState } from '@/store' // typescript intelligence

export interface AppState {
  theme: string
  collapsed: boolean // èåæ¶çº³ç¶æ, ç¨äºåç´å¸å±
  menuMode: 'horizontal' | 'vertical' // èåæ¨¡å¼, ç¨äºæ°´å¹³å¸å±
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
// è¿å å·²åçç reducer
export default appSlice.reducer

```

### è·¯ç±/èåéç½®

```
# ææè·¯ç±åå¨ /src/route/routes.ts ï¼åæ¬èåæ çè·¯ç±ï¼
  ç¨äºè·¯ç±æéæ§å¶

# èåè·¯ç±åºäºè·¯ç±çæ, è·¯ç±é¡¹ä¸å¢å é¢å¤å­æ®µæ è¯: æ¯å¦å¨èåæ å±ç¤º,åå¾æ ç­ä¿¡æ¯

# ä½ ä¹å¯ä»¥åå¼å, å¢å¼ºå¸¦åå®å¶åè½.
```

### å³äºæ¢è¤éç½®

> æ¬æ¡æ¶æ¯ä½¿ç¨ less.js å®ç°å¨æåæ¢ä¸»é¢ï¼jsæä»¶å¨ /public/less.min.js

```
# ä¸»é¢éç½®æä»¶å¨ /public/color.less

å¼ç¨äº antd ç»ä»¶åï¼åºæ¬ä¸éè¦èªå·±é¢å¤èªå®ä¹ä¸»é¢æ ·å¼ï¼å ä¸ºä¸»é¢æä»¶éé½æã
ä½æ¯ï¼ï¼ï¼
å¦æèªå·±åäºèªå®ä¹ç»ä»¶ï¼åæ¢ä¸»é¢åæ ·å¼æ¾ç¤ºä¸æ­£å¸¸ï¼
åéè¦èªå·±å¨ color.less åºé¨æ·»å æ·±æµä¸»é¢å¯¹åºçæ ·å¼ï¼å·ä½åèä¸»é¢æä»¶åé¢å¤éç½®ã

```
