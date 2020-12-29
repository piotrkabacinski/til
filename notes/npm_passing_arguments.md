[Back](../README.md)

# NPM scripts - arguments passing

<small>2020-12-29</small>

Thanks to [NPM scripts](https://docs.npmjs.com/cli/v6/commands/npm-run-script#description) command structure we can easily reuse scripts between each other, pass arguments for specific cases and avoid possible redundancy issues:

```Sh
npm run-script <command> [--silent] [-- <args>...]
```

`package.json`:

```JSONC
{
  // ...
  "scripts": {
    "typeorm": "node ./node_modules/typeorm/cli.js",
    "migration:run": "npm run typeorm -- migration:run",
    "migration:generate": "npm run typeorm -- migration:generate -n PostRefactoring"
  }
}
```
