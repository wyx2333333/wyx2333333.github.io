---
title: 根据WEB3钱包地址生成Blockies / Jazz风格头像 (同MetaMask)
date: 2022-09-09 10:21:45 +0800
categories: [Web3]
tags: [Web3]
---

## Blockies 风格头像

### 安装[blockies 库](https://github.com/download13/blockies)

```bash
npm i @download/blockies
or
yarn add @download/blockies
or
pnpm add @download/blockies
```

### 编写 blockie avatar component

{% raw %}

```tsx
import { CSSProperties, useEffect, useState } from "react";

import { createIcon } from "@download/blockies";

const BlockieAvatar: React.FC<{
  style?: CSSProperties;
  address: string;
  diameter?: number;
  alt?: string;
  rounded?: boolean;
}> = ({ style, address, diameter = 64, rounded = true, alt = "" }) => {
  const [imgSrc, setImgSrc] = useState("");

  useEffect(() => {
    const data: HTMLCanvasElement = createIcon({ seed: address.toLowerCase() });
    !!data && setImgSrc(data.toDataURL());
  }, [address]);

  return (
    <img
      style={{
        width: `${diameter}px`,
        height: `${diameter}px`,
        borderRadius: rounded ? 999 : 0,
        ...style,
      }}
      src={imgSrc}
      alt={alt}
    />
  );
};

export default BlockieAvatar;
```

{: file='BlockieAvatar.tsx'}
{% endraw %}

## Jazz 风格头像

### 安装依赖包

- [color](https://github.com/Qix-/color): 用于 color 转换
- [mersenne-twister](https://github.com/boo1ean/mersenne-twister): 用于生成伪随机数

```bash
npm i color mersenne-twister
npm i @types/color @types/mersenne-twister -D
or
yarn add color mersenne-twister
yarn add @types/color @types/mersenne-twister -D
or
pnpm add color mersenne-twister
pnpm add @types/color @types/mersenne-twister -D
```

### 编写 createJazzIcon.ts (修改来自 [jazzicon](https://github.com/danfinlay/jazzicon))

```ts
import Color from "color";
import MersenneTwister from "mersenne-twister";

const COLORS = [
  "#01888C", // teal
  "#FC7500", // bright orange
  "#034F5D", // dark teal
  "#F73F01", // orangered
  "#FC1960", // magenta
  "#C7144C", // raspberry
  "#F3C100", // goldenrod
  "#1598F2", // lightning blue
  "#2465E1", // sail blue
  "#F19E02", // gold
];

let generator: MersenneTwister;
let remainingColors: string[];

export default function createJazzIcon(address: string, diameter: number = 64) {
  generator = new MersenneTwister(parseInt(address.slice(2, 10), 16));

  const wobble = 30;
  const amount = generator.random() * 30 - wobble / 2;
  remainingColors = COLORS.slice().map((hex) => colorRotate(hex, amount));

  const bg = genColor();

  const svg = initShap(diameter);

  const shapeCount = 4;

  for (let i = 0; i < shapeCount - 1; i++) {
    const total = shapeCount - 1;
    const center = diameter / 2;

    const shape = initShap(diameter, "rect");

    const firstRot = generator.random();
    const angle = Math.PI * 2 * firstRot;
    const velocity =
      (diameter / total) * generator.random() + (i * diameter) / total;

    const translate = `translate(${Math.cos(angle) * velocity} ${
      Math.sin(angle) * velocity
    })`;

    // Third random is a shape rotation on top of all of that.
    const secondRot = generator.random();
    const rot = firstRot * 360 + secondRot * 180;
    const rotate = `rotate(${rot.toFixed(1)} ${center} ${center})`;

    shape.setAttribute("transform", `${translate} ${rotate}`);
    shape.setAttributeNS(null, "fill", genColor());

    svg.appendChild(shape);
  }

  return { svg, bg };
}

function initShap(diameter: number | string, qualifiedName: string = "svg") {
  const shap = document.createElementNS(null, qualifiedName);
  shap.setAttribute("x", "0");
  shap.setAttribute("y", "0");
  shap.setAttribute("width", diameter.toString());
  shap.setAttribute("height", diameter.toString());

  return shap;
}

function genColor() {
  generator.random();
  const idx = Math.floor(remainingColors.length * generator.random());
  const color = remainingColors.splice(idx, 1)[0];
  return color;
}

function colorRotate(hex: string, degrees: number) {
  const hsl = Color(hex).hsl().array();

  hsl[0] = (hsl[0] + degrees) % 360;
  hsl[0] = hsl[0] < 0 ? 360 + hsl[0] : hsl[0];

  return Color(hsl, "hsl").hex();
}
```

{: file='createJazzIcon.ts'}

### 编写 jazz avatar component

{% raw %}

```tsx
import { CSSProperties, useEffect, useState } from "react";

import createJazzIcon from "./createJazzIcon";

const JazzAvatar: React.FC<{
  style?: CSSProperties;
  address: string;
  diameter?: number;
  rounded?: boolean;
}> = ({ style = {}, address, diameter = 64, rounded = true }) => {
  const [containerBg, setContainerBg] = useState("transparent");
  const [avatarElStr, setAvatarElStr] = useState("");

  useEffect(() => {
    const { svg, bg } = createJazzIcon(address.toLowerCase(), diameter);

    !!svg?.outerHTML && setAvatarElStr(svg.outerHTML);
    !!bg && setContainerBg(bg);
  }, [address]);

  return (
    <div
      style={{
        width: `${diameter}px`,
        height: `${diameter}px`,
        background: containerBg,
        overflow: "hidden",
        borderRadius: rounded ? 999 : 0,
        ...style,
      }}
      dangerouslySetInnerHTML={{ __html: avatarElStr }}
    ></div>
  );
};

export default JazzAvatar;
```

{: file='JazzAvatar.tsx'}

{% endraw %}

## 使用效果

{% raw %}

```tsx
import BlockieAvatar from "@/components/BlockieAvatar";
import JazzAvatar from "@/components/JazzAvatar";

const AvatarDemo: React.FC = () => {
  const address = "0x4c8b5D17BaC56aEBbdB55F92Ced29Fc85F2F04EF";

  return (
    <div style={{ display: "flex", justifyContent: "center" }}>
      <BlockieAvatar address={address} />
      <BlockieAvatar
        style={{ margin: "0 12px" }}
        address={address}
        rounded={false}
      />

      <JazzAvatar address={address} />
      <JazzAvatar
        style={{ marginLeft: "12px" }}
        address={address}
        rounded={false}
      />
    </div>
  );
};

export default AvatarDemo;
```

{% endraw %}

![](/assets/img/blog/web3/avatar.png){: h="50"}
