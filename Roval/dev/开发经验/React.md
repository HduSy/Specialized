Created Date：2023-12-14 19:39:39  
Last Modified：2023-12-14 19:39:38

# Tags

[[../NPM/React|React]]

# Content

## Custom Hooks

### useDuringHeartBeats

心跳

```ts
import { useEffect, useRef, useMemo } from 'react';
import { throttle } from 'lodash-es';

const HEART_BEAT_INTERVAL = 10 * 1000;
export function useDuringHeartBeats(opt?: {
  elm?: HTMLElement;
  interval?: number;
}) {
  let { elm = document.body, interval = HEART_BEAT_INTERVAL } = opt || {};
  const listenerPool = useRef([]);

  const throttledHandler = useMemo(() => {
    return throttle(
      () => {
        listenerPool.current.forEach((fn) => {
          fn();
        });
      },
      interval,
      { leading: false }
    );
  }, []);

  useEffect(() => {
    const mousemove = () => {
      throttledHandler();
    };
    elm = elm || document.body;
    elm.addEventListener('mousemove', mousemove);
    return () => {
      elm.removeEventListener('mousemove', mousemove);
    };
  }, [elm]);

  return (cb: any) => {
    listenerPool.current.push(cb);
    return () => {
      listenerPool.current = listenerPool.current.filter((item) => item !== cb); // filter从listenerPool清除当前cb
    };
  };
}
```

使用

```ts
const addDuringHeartBeats = useDuringHeartBeats({
    elm: iframeRef.current?.contentWindow
  });
  useEffect(() => {
    return addDuringHeartBeats(() => {
      report('message');
    });
  }, []);
```

## 服务端 API

### renderToStaticMarkup

将一个非交互式的 React 树渲染成 HTML 字符串  

[renderToStaticMarkup – React 中文文档](https://zh-hans.react.dev/reference/react-dom/server/renderToStaticMarkup)

## 过时 API

### createElement

调用 `createElement` 替代 `jsx` 来创建一个 `React` 组件，它有 `type`、`props` 和 `children` 三个参数，返回一个 `React` 元素

`type`：`type` 参数必须是一个有效的 `React` 组件类型，例如一个字符串标签名（如 `'div'` 或 `'span'`），或一个 `React` 组件（一个函数式组件、一个类式组件，或者是一个特殊的组件如 [`Fragment`](https://zh-hans.react.dev/reference/react/Fragment)）  

[createElement – React 中文文档](https://zh-hans.react.dev/reference/react/createElement)

# Reference
