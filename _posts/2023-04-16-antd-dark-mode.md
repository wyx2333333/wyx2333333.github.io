---
title: Antd-v5中使用暗黑模式
date: 2023-04-16 15:57:26 +0800
categories: [Web, Antd]
tags: [Antd]
---

- theme.defaultAlgorithm: 默认主题
- theme.darkAlgorithm: 对应 v4 版本的暗黑主题
- theme.compactAlgorithm: 对应 v4 版本的紧凑主题

{% raw %}

```tsx
import { PropsWithChildren } from "react";

import { ConfigProvider, theme } from "antd";

const AntdThemeProvider: React.FC<PropsWithChildren> = ({ children }) => (
  <ConfigProvider
    theme={{
      algorithm: theme.darkAlgorithm,
    }}
  >
    {children}
  </ConfigProvider>
);

export default AntdThemeProvider;
```
{: file='AntdThemeProvider.tsx'}

{% endraw %}
