### ant-pro资产使用

1. **开始使用**

   `npm install --save-dev @umijs/preset-ui`  打开开发模式下页面右下角的小气泡，方便添加区块和模版等pro资产。

2. 这里的资产指的就是一系列快速搭建页面的代码片段，可以帮助用户快速的初始化好一个页面，更快速的开发代码。可以理解为一些项目中经常会用到的典型页面的模版。

3. **区块和模版**

   区块相当于一个组件，模版相当于一个页面。

   区块现在支持所有 antd 中的 demo，可以更加快速的将 demo 导入到项目中去。

### ant-pro国际化

1. pro 通过 umi 插件`@umijs/plugin-locale`来实现全球化的功能，并且默认开启。 `@umijs/plugin-locale` 约定 在 src/locales 中引入 相应的 js，例如 en-US.ts 和 zh-CN.ts，并且在 `config/config.ts` 中做如下配置：

   ```tsx
   plugins:[
   ...,
   locale: {
     default: 'zh-CN',
     baseNavigator: true,
     antd: true,
   },
   ...,
   ]
   # default: 默认语言，当检测不到具体语言时，展示指定的语言。
   # antd: boolean, 开启后，支持antd国际化。
   # baseNavigator: boolean, 开启浏览器语言检测。
   # 	默认情况下，当前语言是按照localStorage.umi_locale > 浏览器检测 > default设置的默认语言 > 中文
   ```

2. 组件中使用

   ```tsx
   import { FormattedMessage } from 'umi';
   
   export default () => {
     return (
       <div>
         <FormattedMessage id="navbar.lang" />
       </div>
     );
   };
   ```

   官方建议：不要直接使用formatMessage这个语法糖：

   ​	虽然 formatMessage 使用起来会非常方便，但是它脱离了 react 的生命周期，最严重的问题就是切换语言	时无法触发 dom 重新渲染。为了解决这个问题，我们切换语言时会刷新一下浏览器，用户体验很差，所以	推荐大家使用 [`useIntl`](https://umijs.org/zh-CN/plugins/plugin-locale#useIntl) 或者 [`injectIntl`](https://github.com/formatjs/formatjs/blob/main/website/docs/react-intl/api.md#injectintl-hoc)，可以实现同样的功能。

   ```tsx
   import React, { useState } from 'react';
   import { useIntl } from 'umi';
   
   export default function () {
     const intl = useIntl();
     return (
       <button type="primary">
         {intl.formatMessage(
           {
             id: 'name',
             defaultMessage: '你好，旅行者',
           },
           {
             name: '旅行者',
           },
         )}
       </button>
     );
   }
   ```

3. `setLocale(lange, realReload)`

   * lang：需要切换的语言；
   * realReload：是否需要刷新页面；

4. `getLocale()`

   获取当前的语言

5. 删除国际化

   `npm run i18n-remove`