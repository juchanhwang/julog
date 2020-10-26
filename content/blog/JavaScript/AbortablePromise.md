---
title: "AbortablePromise"
date: 2020-10-26 16:00:00
category: 'JavaScript'
draft: false
---

api요청의 취소가 가능한 AbortablePromise를 구현해보았다.

```ts
export interface ExecutorFunction<T> {
  (resolve: (value?: PromiseLike<T> | T) => void, reject: (reason?: any) => void): void;
}

export interface AbortableExecutorFunction<T> {
  (resolve: (value?: PromiseLike<T> | T) => void, reject: (reason?: any) => void, abortSignal: AbortSignal): void;
}

export class AbortablePromise<T> extends Promise<T> {
  private _abortController: AbortController;
  private _onfinally: Function[];

  constructor(executor: AbortableExecutorFunction<T>, abortController?: AbortController) {
    if (!abortController) {
      abortController = new AbortController();
    }
    const abortSignal = abortController.signal;

    const normalExecutor: ExecutorFunction<T> = (resolve, reject) => {
      abortSignal.addEventListener('abort', () => {
        reject(new AbortError());
        this._onfinally.forEach(onfinally => onfinally());
      });

      executor(resolve, reject, abortSignal);
    };

    super(normalExecutor);
    Object.setPrototypeOf(this, new.target.prototype);
    this._abortController = abortController;
    this._onfinally = [];
  }

  then<TResult1 = T, TResult2 = never>(onfulfilled?: ((value: T) => TResult1 | PromiseLike<TResult1>) | undefined | null, onrejected?: ((reason: any) => TResult2 | PromiseLike<TResult2>) | undefined | null): AbortablePromise<TResult1 | TResult2> {
    return new AbortablePromise((resolve, reject, signal) => {
      const onSettled = (
        status: 'resolved' | 'rejected',
        value: any,
        callback: ((v: any) => TResult1 | PromiseLike<TResult1> | TResult2 | PromiseLike<TResult2>) | undefined | null
      ) => {
        if ("function" === typeof callback) {
          value = callback(value);
          if (value instanceof AbortablePromise) {
            Object.assign(signal, value._abortController.signal);
          }
          return resolve(value);
        }
        "resolved" === status && resolve(value);
        "rejected" === status && reject(value);
      };

      super.then(
        value => onSettled("resolved", value, onfulfilled),
        reason => onSettled("rejected", reason, onrejected)
      );
    }, this._abortController);
  }

  catch<TResult = never>(onrejected?: ((reason: any) => TResult | PromiseLike<TResult>) | undefined | null): AbortablePromise<T | TResult> {
    return new AbortablePromise((resolve, reject, signal) => {
      const onSettled = (
        value: any,
        callback: ((reason: any) => TResult | PromiseLike<TResult>) | undefined | null
      ) => {
        if ("function" === typeof callback) {
          value = callback(value);
          if (value instanceof AbortablePromise) {
            Object.assign(signal, value._abortController.signal);
          }
          return reject(value);
        }
        reject(value);
      };

      super.then(
        value => onSettled(value, onrejected)
      );
    }, this._abortController);
  }

  finally(onfinally?: (() => void) | undefined | null): AbortablePromise<T> {
    if (onfinally) {
      this._onfinally.push(onfinally);
    }
    super.finally(onfinally);
    return this;
  }

  abort() {
    this._abortController.abort();
  }
}

export class AbortError extends PayPortCustomError {
  constructor(message: string = 'Aborted') {
    super(message);
    this.name = 'AbortError';
  }
}
```

<br>

> ## 참조 및 출처
>
> - [promise-abortable](https://github.com/dondevi/promise-abortable)