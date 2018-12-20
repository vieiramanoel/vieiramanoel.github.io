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

When we are moving from one paradigm from another is difficult to learn how to think from a different viewing point. Think about it: when you had your first classes at college about C and then in the next semester you needed to learn C++ or Java and its classes, hierarchy, composition, constructors and so on. It should taken a little time from thinking in a procedural way since ever to design your code based on OO architecture. What about moving from OO designed architecture to functional programming? Let's see.

I'll describe in this post the most important breaking point in these early studies about functional programming using Haskell until now.

No more blablabla:

In imperative languages (you use at least one of them if you are here), such as C/C++, Java, Golang. You describes **HOW TO** achieve your desired data from an input.

In the other hand, functional languages (like haskell, elm-lang, etc.) you describes **WHAT IS** your desired data for a certain input.

This can be a little bit abstract, so let's see an example.

First of all I'll define my dictionary list this way:
    *A list of pairs (a tuple containing two values) where the first one is a string for the person name and second one is its age*
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

{% endhightlight %}

*Assuming we don't have all lovely functional tools from C++14 (lists, tuples, ...)

And then my dictionary instantiated on main would be:

{% highlight c %}

int main(int argc, char const *argv[])
{
    std::vector<tuple> dict = {tuple("John", 32), tuple("Mary", 21), tuple("Josh", 40)};
    return 0;
}

{% endhightlight %}

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

{% endhightlight %}

My outputs are

{% highlight c %}

getAge(dict, "Jonh") // evaluates 32
getAge(dict, "Mary") // evaluates 21
getAge(dict, "Josh") // evaluates 40

{% endhightlight %}
