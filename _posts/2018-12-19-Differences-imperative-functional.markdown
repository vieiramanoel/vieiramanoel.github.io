---
title: "The difference between imperative and functional languages"
layout: post
date: 2018-12-20 03:39
headerImage: false
tag:
- haskell
- imperative
- functional
- programming
star: true
category: blog
author: vieira
---

When we are moving from one paradigm to another it's difficult to learn how to start thinking from a different viewing point. Think about it: when you had your first classes at college about C and then in the next semester you needed to learn C++ or Java and its classes, hierarchy, composition, constructors and so on. It should taken a little time from thinking in a procedural way since ever to starting design your code based on OO architecture. What about moving from OO designed architecture to functional programming? Let's see.

I'll describe in this post the most important breaking point in these early studies about functional programming using Haskell until now.

No more blablabla:

In imperative languages (you use at least one of them if you are here), such as C/C++, Java, Golang. You describes **HOW TO** achieve your desired data from an input.

In the other hand, functional languages (like haskell, elm-lang, etc.) you describes **WHAT IS** your desired data for a certain input.

This can be a little bit abstract, so let's see an example.

First of all I'll define my dictionary this way:

*A list of pairs (pair: tuple containing two values) where the first one is a string for the person's name and second one is its age*

So in C++ I would define a tuple like this

{% highlight c %}

struct tuple {
    tuple(std::string name, int age) {
        key = name;
        value = age;
    }
    std::string key;
    int value;
};

{% endhighlight %}


*Assuming we don't have all lovely functional tools from C++14 (lists, tuples, ...). I'm trying to describe an example for any imperative language and while some have functional tools (c++14, python), others don't (c, golang)

Then my dictionary instantiated on main would be:

{% highlight c %}

int main(int argc, char const *argv[])
{
    std::vector<tuple> dict = {tuple("John", 32), tuple("Mary", 21), tuple("Josh", 40)};
    return 0;
}

{% endhighlight %}


Take a breath, we will continue after that. I know it's getting boring, but stay with me a little bit more.

At last, for getting a person's age from my dict structure I must search like this:

{% highlight c %}

int getAge (std::vector<tuple> dict, std::string name){
    for (auto i = dict.begin(); i < dict.end(); i++) {
        if (i->key == name) {
            return i->value;
        }
    }
    return -1;
}

{% endhighlight %}


My outputs are then

{% highlight c %}

getAge(dict, "John") // evaluates 32
getAge(dict, "Mary") // evaluates 21
getAge(dict, "Josh") // evaluates 40

{% endhighlight %}


Ok we got a lot of work until here, you can check complete code at [this gist](https://gist.github.com/vieiramanoel/3de09d6aaa6964fe28c161d18749dda0)

We defined our structure, and **HOW TO** operate on that.

Well, the fun starts now.

I'm assuming you're not familiar with Haskell syntax, so I'll be a little verbose.

Let's get back our dict definition

*A list of pairs (pair: tuple containing two values) where the first one is a string for the person name and second one is its age*

For haskell lists and tuples are concepts well defined, because, hm... it's a functional language.

> Note: GHCi is a haskell interactive shell, something pretty much like python interpreter

So our dict would be simply:

{% highlight hs %}

dict = [("John", 32), ("Mary", 21), ("Josh", 40)]

{% endhighlight %}

A list of pairs!

Nice, we defined our structure. Despite we don't had to define the tuple type like in C++, things are going in the same way.

UNTIL NOW.

## Step 1: Filter your list removing undesired elements

{% highlight hs %}

dict = [("John", 32), ("Mary", 21), ("Josh", 40)]
filterByKey key dict = filter (\(k,v) -> k == key) dict
-- filterByKey takes two arguments and returns all
-- elements where first element is equal to requested key
-- This is pretty similar to SQL

-- On GHCi
*Main> :l modules.hs
*Main> filterByKey "John" dict
[("John",32)]
{% endhighlight %}

*I've saved my hs file as `modules.hs`

Now we have a list elements that interests to us.

In C we defined that first element containing the key returns its value, let's define same behavior in haskell.

## Step 2: Take only first element from a list. You just need to **head list**

{% highlight hs %}

dict = [("John", 32), ("Mary", 21), ("Josh", 40)]*
filterByKey key dict = head . filter (\(k,v) -> k == key) dict
-- This dot between `head` and `filter` means: "extract first element
-- from list returned by filter"

-- On GHCi
*Main> :l modules.hs
*Main> filterByKey "John" dict
("John",32)

{% endhighlight %}

Ok, now we got first tuple on our dict with `key="John"`

## Step 3: Returns age from pair (name, age)

Once we want just its age, then `filterByKey` changes to getAge

{% highlight hs %}

dict = [("John", 32), ("Mary", 21), ("Josh", 40)]
getAge name dict = snd . head . filter (\(k,v) -> k == name) dict
-- I changed function name to `getAge`
-- and parameter from `key` to `name` in order to be more readable
-- snd is a function which returns second element from a pair (pretty useful, hm?)

-- On GHCi
*Main> :l modules.hs
*Main> getAge "John" dict
32

{% endhighlight %}

Voilà, we've extracted data we want from our structure saying **WHAT** our desired data is, not **HOW TO** obtain it.

First mistake that people like me who are starting a new paradigm makes is try to associate the known thinking process to a completely different thing that doesn't have any relation between them.

So, next time you're writing a functional program think: "What's my output and what functions must I apply to my input to produce the data I want?" instead of "What steps are necessary to achieve my goal?".
