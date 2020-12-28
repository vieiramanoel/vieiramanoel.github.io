---
title: "Iterating over a string literal type in typescript"
layout: post
date: 2020-12-27 21:18
headerImage: false
tag:
- typescript
star: true
category: blog
author: vieira
description: 
---

In typescript literal types are amazing, I personally love to limit certain fields from a form using string literals. Something like this:

```typescript
type Company = 'google' | 'facebook' | 'apple';
```

But very often I want to iterate over the possible values of `Company` - imagine a Select button as an example.

So, for this case most answers would suggest that you use an enum type instead as it provides ways to iterate over it's items. That's a possible solution, but in this post I'd like to offer a different one at compiler time and so that the type `Company` behaves like it was defined literally:

```typescript 
const companies = ['google', 'facebook', 'apple'] as const;
type Company = typeof companies[number];
```

Typescript 3.4 introduces a new concept called const assertion to indicate to compiler that the value is constant and should be replaced at compilation. Now that typescript knows all the possible values for `companies` it can define a literal type from that value (as it's constant etc etc).

The second line is called [indexed access types](https://microsoft.github.io/TypeScript-New-Handbook/chapters/types-from-extraction/#indexed-access-types) and we can define our type extracting it from an object or array item (more examples on the link).

At the end you should see that the type of `Company` is the same as it were if we'd defined it literally 

```typescript
type Company = 'google' | 'facebook' | 'apple';
// is the same as 
const companies = ['google', 'facebook', 'apple'] as const;
type Company = typeof companies[number];
```

And now you can iterate over `Company` accepted values using the const variable `companies` 😄