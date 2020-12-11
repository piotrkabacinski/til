[Back](../README.md)

# TypeScript's Type Predicates

<small>2020-12-10</small>

One of the cool things in TypeScript is [type predicates](https://www.typescriptlang.org/docs/handbook/advanced-types.html#user-defined-type-guards) - user-defined, [duck typed](https://en.wikipedia.org/wiki/Duck_typing) expressions allowing to define the type based on its properties or any condition. Let's dive into an example use case.

```TypeScript
interface Planet {
  radius: number;
  name: string;
};

interface Earth extends Planet {
  humansPopulation: number;
}

function getPlanet(): Planet | Earth {
  return {
    radius: 6371,
    name: 'Earth',
    humansPopulation: 7.8e9
  }
}

function getHumans(earth: Earth) {
  return earth.humansPopulation;
}

const planet = getPlanet();
const humans = getHumans(planet); // ⚠️ Argument of type 'Planet | Earth' is not assignable to parameter of type 'Earth'.
```

Problem here is using returned value of `getPlanet` function as an argument for `getHumans` function. Event if we're sure that `getPlanet` will return `Earth` typed object TypeScript's not aware of it. How to handle it? Instead of type casting we can create a simple type guard:

```TypeScript
function isEarth(planet: Planet | Earth): planet is Earth {
  return planet.hasOwnProperty('humansPopulation');
}

const planet = getPlanet();

if (isEarth(planet)) {
  const humans = getHumans(planet); // No errors! 🎉
}
```
