```ts
interface PromiseCapability<T, E> {
  resolve: (fulfilled: T) => void
  reject: (err: E) => void
}

type CapablePromise<T, E> = Promise<T> & PromiseCapability<T, E>;

const createCapablePromise = <T, E>(): CapablePromise<T, E> => {
  let resolve_ = (fulfilled: T) => {
    //
  };
  let reject_ = (err: E) => {
    //
  };
  const p = new Promise((resolve, reject) => {
    resolve_ = resolve;
    reject_ = reject;
  })

  //@ts-ignore
  p.resolve = resolve_;
  //@ts-ignore
  p.reject = reject_;

  return p as CapablePromise<T, E>
}

class PromiseState<T extends Promise<U>, U> {
  protected state: "pending" | "fulfilled" | "rejected" = "pending"

  constructor(public readonly promise: T) {
    promise.then((v) => {
      this.state = "fulfilled";
      return v
    }, (err) => {
      this.state = "rejected";
      throw err
    })
  }

  getState() {
    return this.state
  }

  isPending() {
    return this.state === "pending"
  }
  
  isFulfilled() {
    return this.state === "fulfilled"
  }
  
  isRejected() {
    return this.state === "rejected"
  }

  isSettled () {
    return this.state === "fulfilled" || this.state === "rejected"
  }
}
```

```ts
void (async () => {
  const p = new PromiseState(createCapablePromise<string, unknown>());
  p.promise.then(console.log);

  console.log(p.getState()); // [LOG]: "pending" 
  p.promise.resolve("foobarbaz");
  await new Promise(r => setTimeout(r, 0)) 
  // [LOG]: "foobarbaz" 
  console.log(p.getState()); // [LOG]: "fulfilled" 
})();
```
