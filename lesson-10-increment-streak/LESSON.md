# Incrementing a Streak Counter

The goal of this lesson is to write the logic to increment our streak.

## Intro

Remember our flow chart? Well in this lesson we need to add the logic that will increment our streak.

We first need to think about what logic we'll use to determine if an increment is the right move. Then we'll need logic to actually increment and keep track of the streak.

To us, a streak is days in a row. For example, if you log in on 5/21 and then again on 5/22, that's a streak of two days. But if you next login on 5/24, that is no longer a streak. So in order to track this we need a start date for the streak and a last login date, both of which we have.

This is why we pass in `date` to our `streakCounter` function - so that we can use it to check if the streak is still active.

One new TypeScript concept we're going to utilize is called a [string literal](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types). This is handy when you want to narrow a type from any `string` to specific strings. Let's look at an example:

```typescript
// Taken from TS docs
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}
printText("Hello, world", "left");
printText("G'day, mate", "centre");
// Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.
```

As you can see, the second parameter type can be one of three strings: "left", "right" or "center".

When someone tries to use it, but uses the wrong type (or wrong spelling in this case), TS will complain that the types don't match. Super cool, right?

There are two other TypeScript concepts I want to mention as well: [return type annotations](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#return-type-annotations) and [union types](typescriptlang.org/docs/handbook/2/everyday-types.html#union-types).

Similar to parameter type annotations, return types are for the function return type. Here's an example:

```typescript
function getFavoriteNumber(): number {
  return 26;
}
```

Notice the `: number` at the end of the function, before the opening block curly bracket. This simply means this function returns a `number`.

Union type is when you want to say something is type X or type Y. Two examples:

```typescript
function printId(id: number | string) {
  console.log("Your ID is: " + id);
}
```

The `|` (pipe symbol as I say), indicates a union type. Here, the parameter `id` can be of type `number` or `string`.

Keep these concepts in mind for the next challenge! I used all three in my solution, but you don't have to.

### Challenge

Using TDD, complete these tests by making the appropriate changes to your `streakCounter` function:

```typescript
// File: __tests__/index.test.ts
describe("streakCounter", () => {
  // previous tests omitted...

  // Separate suite to test different scenario
  describe("with a pre-populated streak", () => {
    // TODO: populate localStorage with a streak
    it("should increment the streak", () => {
      // It should increment because this is the day after
      // the streak started and a streak is days in a row.
      const date = new Date("12/13/2021");
      const streak = streakCounter(mockLocalStorage, date);

      expect(streak.currentCount).toBe(2);
    });
    it("should not increment the streak when login days not consecutive", () => {
      // It should not increment because this is two days after
      // the streak started and the days aren't consecutive.
      const date = new Date("12/14/2021");
      const streak = streakCounter(mockLocalStorage, date);

      expect(streak.currentCount).toBe(1);
    });
    it("should save the incremented streak to localStorage", () => {
      const key = "streak";
      const date = new Date("12/13/2021");
      // Call it once so it updates the streak
      streakCounter(mockLocalStorage, date);

      const streakAsString = mockLocalStorage.getItem(key);
      // Normally you should wrap in try/catch in case the JSON is bad
      // but since we authored it, we can skip here
      const streak = JSON.parse(streakAsString || "");

      expect(streak.currentCount).toBe(2);
    });
  });
});
```

Hints:

- I made a helper function in `index.ts` called `shouldIncrementOrResetStreakCount` which returns `increment` or `undefined`

### Solution

Here is the solution I came up with: https://gist.github.com/jsjoeio/08a304acdf8a2411ab6510fc118e98a8

### Extra Credit

Imagine you're explaining this to a new JavaScript developer learning TypeScript:

- What are string literal types?
- What are examples of ways you might use them?
