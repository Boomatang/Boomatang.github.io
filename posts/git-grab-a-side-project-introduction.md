# Git-grab, a side project introduction

Git-grab is a side project that I work on in my spare time or when I feel like it.
It is an CLI tool written in python and here are some usefully links for getting started.
(Add linkings in here)

Why am I talking about this, it's a tool that most people wont have the need for or even know it exists.
But yet it's a tool I use nearly daily.

In this post I am not going to try sell you the tool. I already assume you are using it.
And that is the main point I assume everyone is using git-grab.
This post is about what I as the creator and maintainer gets out of git-grab.

First of all I should give a shout out to Brian Okien of pod cast test&code (add link). 
This project is based/forked from a sample CLI toll he created for his show. 
It was nice to have a working starting point to build from.


From working on this project there has, there was a lot to learn and still more to learn.
My guess is, I will use git-grab as example in future posts so I want try explain some of the things learned in this post.
I will go over some of the basic stuff.

## Getting Started
Before I could start to create my own tool I had to figure out how the original tool was developed.
This meant working out how to install the tool so I could develop using it.
Under standing what the commands in the *Make* file do and how I should use them.

The key take away point here would be how to run the tool in development mode was documented.
This idea of documenting will drive its self home again and again as the project advanced.

## Publishing the CLI tool

For me this was and is very important. 
Why is was this important.
Well it's a nice feeling seeing your ideas be more than just a concept in your mind but a real thing is shared with others.

Sharing with other is the important part.
Any feature that is add could be used by anyone in what ever way they want.
Which might be a way you never thought about.
The first release version had a commands that I thought I would need.
But after trying to use it as a user I soon found out there were many edge case that would be hard to fix and harder to maintain.
These commands have sense been taken out but it was hard to justify breaking the CLI for possible user, even on version 0.1.0.

Doing the release forces you to learn how to do a release and what issue you may face doing it.
It really drives home the need for automation around releasing.
If it's hard to make a release you wont release often.
If you don't release often you will have larger change set which will possible have some bug that was missed.
This would require a rushed release on a project that's not easy to release because of a bug caused indirectly from having a hard to release project.
This is no fun, stressfully and less likely to happen.

What you would end up with is buggy released software for user to get upset with.
And you as the project maintainer wont want to work on due to it been hard to get release out but also you maybe getting more backlash from unhappy users.

Just to be clear having a project that is easy to release does **not** mean you won't have buggy code. It just means its easier to fix the bugs making it more likely to happen.

## Adding new features
We all like to be working on something new.
We all want to be adding new features but you can't with out it coming at a cost.
The cost is maintenance.

To add a new feature you really need to know why, what & how. 
And yes I will describe these next plus the way I found to focus this work so as not to waste time.
Remember this is a side project, you always have something better that you can be doing.

Before you ask the questions why, what & how, the answer is **no** you don't need the feature.
Your aim is to understand why you need what you may want.

### Why
Know why a thing is being done and I mean really know why will help focus the mind when difficult challenges come up, and they will.
*Because you can* is not an answer.
Look at this question with the view of years down the road.
The sad thing about this question is most times there is no true reason why.
So you most stay as no.
You might think this is a bad thing but it's not, for it allows you to say yes to the more important, more variable things.

### What
So from looking at why you should do some thing and after coming to terms that this is something you should do.
What exactly are you saying yes to.
Answering the what will narrow the focus of what you should be working on.
What does the user see after running a command?
What should the documentation say about the command?
What are the known edge cases around what your doing?

I know I am speaking as if we are working on software but it does not have to be.
You could be throwing a dinner party.
What veg are in session?
What time is a good time for starters?

### How
With knowing the *what* you can now answer the most important question, *How*?
This may be a list of tasks that need to be done.
The more detail the better.
The *how* will keep you on track when you see the next shiny thing.
The *how* should describe what is needed to achieve the *what*.

So to bring this to a real example, an example in git-grab.
There is a list function which lists the number of repos that have been recorded in the system.
It is really usefully (speaking as a daily user).

How the code is written doesn't matter what matters is how the user uses the feature.

- How do you create the list?
- How do you filter the list?
- How do you display more information?
- How do you simply display the list

In the questions above you is the user and they are all valid questions.

The easiest why I found to answer these question was to write the documentation first.
Yes, documentation the thing that is so hard to write.
Doing this at the start focused me on what was needed to be done.
There was examples of the output that I could compare against.
All the flags I needed were listed.

When I got around to implementing the feature it was easy.
There was steps and examples to follow.
I didn't have to solve the problem of how it looks at the same time as making it work.
It stopped me from trying to add in things I thought of while trying to create the feature.
The work was scoped and I had a definition of done.

There is something very important to point out.
My documentation was not 100% correct in the start.
While working I found issues edge cases that I had not thought of.
Even with these mistakes the picture was far clearer of what was needed to be done over doing it on the fly.

And on the Plus side the to curation was 80% done.
There was a few updates required.
It would have been much harder go back over and document things when the feature is done.
At that stage you just want to hit release.

## Wrapping up
To wrap this up.
Have a look at git-grab (add in the links).
Check out test&cod pod cast, you never know what you might learn.

But most importantly know *why* you are going to do something. Before you ever know *what* your going to do.
And follow up with *how* your going to do it.

Then the tip is if you can, for the *how* write the documentation first. 
It really can focus you on the end goal.
