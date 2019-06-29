---
title: SOLID - Building scalable software
date: "2019-06-27"
description: Building software is messy. Following certain principles and patterns can help remove some of the mess. SOLID is where to start.
---

If there's anything that I've realized over my time developing software, it's that all code is trash. Code is written by humans. Humans are smart (mostly) and don't write bad code on purpose. As we develop software, many times, the requirements change. As we iterate, it's natural that we find that our product is all together different from stakeholder expectations. So we deviate from the plan - which is okay, we're agile. But let me share a mind-shaking statistic. The average tenure of a software developer at any given company, is two years. This means, at any given time, your colleagues are just as clueless about major design decisions as you (I'm not sure if that's terrifying or a relief). When we combine these two issues, with problem-solving techniques that vary across developers, our codebase moves from the appearing like the Mona Lisa, to spaghetti, and then spaghetti that someone put in a blender... then the dishwasher.

Software is like any system. As a codebase ages, it naturally descends into [chaos](https://en.wikipedia.org/wiki/Chaos_theory). Not all hope is lost. If you implement a few design principles in your codebase, you can certainly alleviate these pains.

## Code Rot - How do you know you have a problem?

When something smells in your house, it's pretty easy to find the cause. You open the door and wrinkle your nose. After that first whiff, you walk to the kitchen. Sniffing twelve times a second, you try to identify the culprit. Is it the trash? No, it smells like old pizza but you've been immune to old pizza smell since college. Is it the sink? The fridge? Is there a rotten potato that fell into a hidden corner of your pantry? Whatever the case, finding the issue is usually harder than removing it. Sometimes our codebase smells the same. When a new developer clones the repo, what will she smell? Will there be a whiff of rotten spaghetti in the corner? Maybe even overwhelmed because she just walked into a binary garbage can? Or will she walk into a pristine system? Will your codebase still have that _new code smell_ in three or four years? Like a house, no codebases are pefect. Trust me, one of the primary reasons that my wife and I have friends over is because we'd never clean without them (I'm not sure if I'm joking here or not). So let's look at some of the ways that you can detect these smells in your code.

#### Rigidity

> Rigidity is the tendency for software to be difficult to change - Robert C. Martin (RCM)

Does your codebase have files, folders, or classes that you just don't touch? Minute changes to a single class or function thunders forth with an avalanche of bugs and defects. It may have been an easy fix, I mean - You did tell everyone at standup that it would only take a couple days. Small modules turn into behemoths and simple changes to marathons. When software behaves this way, managers fear to allow engineers to fix non-critical problems. But even if managers let their devs fix those issues, the codebase can turn into a roach motel. Engineers may check in but don't check out.

When these fears become so accute that the team refuses to allow changes to software, rigidity sets in. We have officially moved from design defficiency to issue aversion.

#### Fragility

> Fragility is the tendency of the software to break in many places every time it is changed. - RCM

It seems like we hear it all the time, "I'm not sure how this is broken, I didn't touch anything over here". As time moves on, the more fragile the codebase becomes. Every fix only seems to break something else, or introduces new issues. The team is tired of fixing each other's bugs and the code review becomes much more intensive. Developers lose credibility. Managers pull their hair out. Releases get pushed back.

#### Immobility

> Immobility is the inability to reuse software from other projects or from parts of the same project - RCM

We've written the same thing over and over again but it never quite satisfies the requirements of this project. We want to build reusable tool but just don't have the time. So we extend and mangle and finagle previously written code into our new system but it has too much baggage to be dependable. Or we find that it's too dependent on another chunk of code. After a few weeks of trying to make it work, we simply just rewrite it.

#### Viscosity

This comes in two flavors: design and environment. When _design_ is viscous, changes to the codebase are slow and executing minute tasks are tedious. It's not clear what variables are named so it takes a while to understand code for maintainence. Maybe modules are hidden. Or a too strict style guide is implemented. Awkward file structures can even lead to design viscosity. The _environment_ is viscous when the reflected changes take ages to be reflected in the codebase. Sometimes this happens through poor code review or a lack of collaboration. Sometimes developers may have to copy files from one directory to another, then run a series of manual build commands to see a change locally. Long compile times may also result in a viscous environment.

These four symptoms are the acrid aromas of poor architecture. When our applications suffer from one (or several) of them, we know that it's a matter of time before they rot into obscurity. As mentioned before, these aren't typically caused by idiots or monkeys typing code. There are a whole host of reasons why a system degrades. By no means do I mean to lash the individuals responsible for powering the future. While these problems may be introduced unknowingly or by the changing landscape of our development environments, it is up to the developers - the command-line heroes - to make the world right again. But how?

## SOLID - Engineering for the future

Introduced by Robert C. Martin, the SOLID design principles are set of basic ideas that are meant to make your code scalable and maintainable. These are widely applied to the Object Oriented Programming paradigm, but the underlying principles have a place in every codebase - even an HTML/JQuery web component. The most basic ideas include things like - _don't make your class or function do things that it shouldn't_, and _don't depend too much on implementation detail of a class or function you can't change_. The ideas that I'll be presenting in this article, may include heavy tech jargon. I'll try to make it relatable, I swear.

#### (S)ingle Responsibility Principle

> Do one thing and do it well

When I was in college, I had a coffee maker. But I was really into coffee, so it was an expensive coffee maker. There was a deluxe steaming wand dangling off the side. It had a grinder up top and a drip tray below. It could dispense hot water into medium grind beans and I could attach the espresso head and in just a few minutes, have a hobbit's cup worth of Cafe Bustelo (the rich man's Lavazza). This coffee machine was one of the neatest pieces of tech that I owned in my early life. So neat, that I only used it a dozen times. The machine was huge. It was hard to move and took up a ton of room on the counter. The grinder was loud but you couldn't swap it out for another. The steam wand was really cool, but when improper cleaning clogged the nozzle, it was a pain to take apart. So, to my shame, I bought an [Aeropress](https://aeropress.com/). I won't go into the details, but the aeropress only does one thing. It's small, and assembled from a few pieces. It's basically just a tube with a plunger. I fell in love with the thing and took it everywhere. It makes the best coffee because it only makes coffee one way.

Classes follow this example. It's so easy for us to put everything in a class because we think related functionality is the classes responsibility. But that's not exactly true. Let's look at an example.

```py
class Note:
    def __init__(self, title, content):
        self.title = title
        self.content = content

    def __str__(self):
        return f'{self.title}\n{self.content}`

    def save(self, filename)
        f = open(filename, 'w+')
        f.write(f'{self.title}\n')
        f.write(f'{self.content}\n')
        f.close()
```

This is a simple example of what not to do. While at first glance, this seems to be an excellent use of lines, it breaks our SRP. The Note is now no longer concerned its creation and any _note specific_ actions. We have broken it and given it a save class. If we gave it a save, we'd probably give it a delete and an update as well. But instead of all of this, let's just create a persistence manager.

```py
class Note:
    def __init__(self, title, content):
        self.title = title
        self.content = content

    def __str__(self):
        return f'{self.title}\n{self.content}`

class PersistenceManager:
    @staticmethod
    def save(item, filename)
        # I've used a context manager here to show a little variety
        with open(filename) as f:
            f.write(item)
```

Now, to use it:

```py
pm = PersistenceManager()
note = Note('I write notes', 'But not a lot of content')
pm.save(note, 'note.txt')
```

Now, we have a persistence manager that works across several different classes and does not care about the implementation detail of the client.

#### (O)pen/closed principle

Our classes or functions should be open for extension but closed for modification. This is a pretty straightfoward concept but so easy to break. I'm not sure if you've ever played minecraft. But I've seen pictures of people building these huge items (castles, or dolphins, or something) in the sky and I could never figure out how they were doing it. But the mystery was simple. They just built a few blocks into the air, and then removed all of the blocks below them except the one they were standing on.

Software is not minecraft. Every class you build that depends on another is another block in the chain. We can compose excellent things, but if we try to modify the blocks below us, everything around us will crumble. Let's look at an example.

If I were manufacturing Baseball gloves, I might want to have the ability to create gloves, and then filter through my inventory based on various criteria, like size or color.

```py
class Glove:
    def __init__(self, model, color, size):
        self.model = model
        self.color = color
        self.size = size

class GloveFilter:
    def filter_by_size(self, gloves, size):
        return [g for g in gloves if g.size == size]

    def filter_by_color(self, gloves, color):
        return [g for g in gloves in g.color == color]

    def filter_by_color_and_size(self, gloves, color, size):
        pass # you get the idea
```

This is okay for our current scenario, but what if we need to filter by color and size together? And then filter by model, color, and size? Now we have nine specific methods to build because we have nine combinations of items to filter and we have to modify and test the GloveFilter class every time we make a change because we don't want our other implementations to break.

So we'll use an enterprise pattern called the `Specification` Pattern to get help us stick to our principle. I will have a future post explaining the Specification Pattern in more detail.

```py
class Specification:
    """This is an abstract method. Just for illustration"""
    def is_satisfied(self, item):
        pass

class Filter:
    """This is an abstract method. Just for illustration"""
    def filter(self, items, spec):
        pass

# Let's create a specification for filtering
# a specification just lets us know if our item
# satisfies a certain condition
class ColorSpec(Specification):
    def __init__(self, color):
        self.color = color

    def is_satisfied(self, item):
        return item.color == self.color

class SizeSpec(Specification):
    def __init__(self, size):
        self.size = size

    def is_satisfied(self, item):
        return item.size == self.size

# We create a filter based on our specification
class Filter(Filter):
    def filter(self, items, spec):
        return [item for item in items if spec.is_satisfied(item)]

```

Now let's implement:

```py
my_glove = Glove('baseball', 'tan', 'large')
wife_glove = Glove('baseball', 'pink', 'small')
son_glove = Glove('football', 'tan', 'small')

gloves = [my_glove, wife_glove, son_glove]

glove_filter = Filter()

# This would produce a list of the tan gloves
tan = ColorSpec('tan')
tan_gloves = glove_filter.filter(gloves, tan)

# Let's implement filtering based on size
small = SizeSpec('small')
small_gloves = glove_filter.filter(gloves, small)
```

Now we don't need to modify our filters at all - like the first example, we can just extend and implement them. We can come up with a near infinite amount of filter for an infinite amount of gloves. And if we wanted to filter something else, like baseballs, we could use that as well.

#### (L)iskov Substitution Principle

This is an example that we don't often see because we naturally program around it well. LSP simply means that if we extend a series of classes, those classes should be able to work within the same functions. I won't even write a code example for this, as I think it's best shown through a series of design patterns. Using our glove example, if our glove had a `fingerless` property, we would just want to make sure that if it's accessible inside of the initial baseclass, that any extended class can access it.

#### (I)nterface Segregation Principle

Everyone who's ever owned a printer knows that they're the hardest machines to debug. There are a thousand different versions. The website says that the _Printer Model 100 series_ does everything. But then you look through the model 110 and it doesn't do half of the stuff that the base _Model 100_ does, even though the directions are the same. This is pretty much a violation of the Interface Segregation Principle.

With ISP, you want to make sure you keep only necessary methods and properties inside of your base classes. Let's look at this in example.

```py
class Machine:
    def print(self, document):
        raise NotImplementedError
    def fax(self, document):
        raise NotImplementedError
    def scan(self, document):
        raise NotImplementedError

class Printer(Machine):
    def print(self, document):
        pass # custom implementation here
```

In this example, we've extended the Machine's base class and overwritten the print method, but the printer also has access to two methods that it will never use. They'll actually throw an error in our program.

```py
printer = Printer()
printer.fax('document.txt') # This would throw a not implemented error, leaving the consumer confused
```

Here, the user has the _ability_ to utilize the `fax` method but can't actually execute it. We don't want consumers to be confused by the fact that our printer has access to the `fax` method but can't implement it. So instead, we create our base class to be as minimal as we can with only properties and methods necessary for all extensions of the class. So we might instead create a printer class (instead of a machine _god class_)

```py
class Printer:
    def print(self, document):
        raise NotImplementedError

class ScannerPrinter(Printer):
    def print(self, document):
        pass # custom implementation here
    def scan(self, document):
        pass
```

Here, if the user tried to use a `fax` method, it would be undefined rather than unimplemented.

```py
sp = ScannerPrinter()
sp.fax('document.txt') # fax is not defined
```

Both of these would blow up if you called an unsupported method, but it's certainly easier to debug one over the other.

#### (D)ependency Inversion Principle

The DIP proposes that we protect our code by making it independing of things that are fragile, volatile or out of our control. We really don't want to adapt to details or concrete implementations but the opposite. We want our implementations to adapt to our code via an abstraction. This might be some middle api. When using React, you might be tempted to grab a `ref` and force some your application to make use of some private methods to accomplish a task (those requirements can be tough). But that library was created as abstraction that you can grab and manipulate.

Issues arise when the dependencies of our classes may load or are called before the classes themselves are instantiated. We might get some runtime errors about one class loading before another, etc. To counter this, we might create a series of abstract base classes and `inject` dependencies into our classes at instantiation so that we don't have to worry about our dependencies being called/created in the wrong order.

For the most part, we don't see this issue in Python. Python is typically flexible enough and will allow us to provide our classes with particular data targets without having to comply with any specific interface. Python's dynamic typing keeps us flexible. However, we do actually have advantages when we create abstract base classes. Duck typing, for instance, is one of these advantages. Which makes our classes much more readable. Duck typing is basically - _if it looks like a duck, and it quacks like a duck, then it must be a duck_. Inheritance follows the rule of `is a`, so when you declare the abstract base class and extend it, you're effectively telling your users that your extended class _looks like a duck and quacks like a duck_.

#### So where do we go from here?

As I mentioned before, some of these things are incredibly technical and if you're in the Python or JavaScript space, you might not encounter many of these issues often.. But honestly that's sort of a problem. We have to be willing to hold to certain principles that can be used across the industry in order to maintain some semblance of order in our codebases. But you're in the middle of a project - how do you make code better from here? You'd be breaking the _Open/Closed princple_ if you started modifying everything.

Andrew Hunt, would tell us something along the lines of _clean up what you can when you walk into the room_. For instance, you may touch a certain part of the codebase that's messy and two pieces may be too tightly linked together. You might consider creating a minor abstraction to be the mediator between these functions, classes, or properties, instead of _monkey patching_ your code and moving onto the next bug. Good software is hard to build, especially if you start thinking about standards and design principles halfway through. I would encourage you, the next time that you implement a new feature, write _SOLID_ on sticky note and pin that sucker onto your monitor. It's never easy, but you can always start now.
