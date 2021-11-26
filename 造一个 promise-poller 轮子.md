# 造一个 promise-poller 轮子

[造一个 promise-poller 轮子](https://zhuanlan.zhihu.com/p/360247799)

```js
/**
 * @param taskFn: Function //回调函数
   @param interval: number //轮询时长
   @param shouldContinue: (err: string | null, result?: any) => boolean //当次轮询后是否需要继续
   @param masterTimeout?: number // 整个轮询过程的 timeout 时长
   @param taskTimeout?: number // 轮询任务的 timeout
   @param retries?: number // 轮询任务失败后重试次数
   @param progressCallback?: (retriesRemain: number, error: Error) => unknown // 剩余次数回调
 */

const timeout = (promise, interval) => {
  return new Promise((resolve, reject) => {
    const timeoutId = setTimeout(() => reject('Task timeout'), interval);

    promise.then(result => {
      clearTimeout(timeoutId);
      resolve(result);
    });
  });
};

const delay = interval =>
  new Promise(resolve => {
    setTimeout(resolve, interval);
  });

function promisePoller(options) {
  const {
    taskFn,
    masterTimeout = 30000,
    taskTimeout = 2000,
    shouldContinue,
    interval = 3000,
    retries = 5
  } = options;

  let timeoutId;
  let polling = true;
  let retriesRemain = retries;

  return new Promise((resolve, reject) => {
    if (masterTimeout) {
      timeoutId = window.setTimeout(() => {
        reject(new Error('Master timeout'));
        polling = false;
      }, masterTimeout);
    }

    const poll = () => {
      let taskResult = taskFn();
      let taskPromise = Promise.resolve(taskResult);
      if (taskTimeout) {
        taskPromise = timeout(taskPromise, taskTimeout);
      }
      taskPromise
        .then(result => {
          if (--retriesRemain === 0 || shouldContinue() || !timeoutId) {
            if (retriesRemain === 0) {
              reject('请稍后重试！');
            } else if (!timeoutId) {
              reject('请求超时，请稍后重试！');
            }
            reject('已完成！');
          } else if (polling) {
            delay(interval).then(poll);
          } else {
            console.log('polling false');
            clearTimeout(timeoutId);
            timeoutId = undefined;
            reject('请稍后重试！');
          }
        })
        .catch(error => {
          polling = false;
          console.log(error);
        });
    };
    // debounce(poll, 3000);
    poll();
  });
}

export default promisePoller;
```

