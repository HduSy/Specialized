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

# Reference
