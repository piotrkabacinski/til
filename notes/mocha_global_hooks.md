[Back](../README.md)

# Mocha's Global Hooks

<small>2020-12-22</small>

[Mocha's hooks](https://mochajs.org/#hooks) are widely used to organize testing flow better. I used to use it within tests declaration, but I didn't know they can be launched globally as well, for example before all specs are run or ended. I started researching about it when I came up with the redundation problem of initiating a database for the tests:

`some.spec.ts`:

```TypeScript
describe("Some controller", () => {
  before(async () => {
    // Create Test DB
  });

  afterEach(async () => {
   // Reset Test DB
  });

  after(() => {
    // Destroy Test DB
  });

  it("Should...", () => {
    // ...
  });
});
```

As you can see, each spec will require to declare creation, reset, and destroy the test DB. When the number of tests is getting bigger maintenance of those hooks can be time-consuming. Fortunately, they can be declared globally:

`src/test/setupTests.ts`:

```TypeScript
  before(async () => {
    // create Test DB before all tests are run
  });

  afterEach(async () => {
   // Resets Test DB after each test
  });

  after(() => {
    // Destroy Test DB after all tests are done
  });
```

`some.spec.ts`:

```TypeScript
describe("Some controller", () => {
  it("Should...", () => {
    // ...
  });
});
```

`.mocharc.jsonc`:

```JavaScript
{
  // ...
  "file": "./src/test/setupTests.ts",
  "spec": "./src/**/*.spec.ts"
}
```

`package.json`:

```JSON
"test": "NODE_ENV=test ts-mocha --config .mocharc.jsonc"
```
