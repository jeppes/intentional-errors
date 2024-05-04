# intentional-errors

When faced with a difficult bug, isolating the issue is key. One of the most efficient ways to do this is to remove progressively larger branches of the code until the issue is clear. 

Previously, I used to do this by commenting out the code. Unfortunately this has some issues:
- There is a risk of merging the commented-out code, even with pull request reviews
- You need to remember to later uncomment the code, which needlessly adds cognitive load

Recently, I've embraced the approach of intentionally adding errors to my code instead. Specifically, errors that are **detected by my linter** but **ignored by my compiler**. This means I can run the code just fine, but I can't merge it!

----------------------------

## Some examples

Skip over code with `if (false)`:
```typescript
line1()
if (false) { 
  line2() // <--- This line will no longer run
}
line3()
```

Return early to skip a function's implementation:
```typescript
function calculateTheThing(): number {
  return 5 // <--- The rest of the function is ignored

  return someCompexThing() + someOtherComplexThing() / more()
}
```

Forcing a conditional to be true by adding `true || ...`:
```typescript
true || conditional
```

Forcing a conditional to be false by adding `false && ...`:
```typescript
false && conditional
```

Prevent a component from rendering in React with `{ false && ... }`:
```typescript
<>
  { false && <SkipMe /> }
  <KeepMe />
</>
```

These examples are adapted to working with typescript, but you can likely adapt them to the language and tooling you work with.

----------------------------

## Conclusion

There is nothing wrong with introducing errors locally as long as your CI system is able to prevent you from later merging that code. These errors can help you focus on what matters in the moment without adding cognitive load. Even better, your linter will give you a trail of breadcrumbs to follow to undo your errors once you've found and fixed your bug.
