# Controller Testing

subtitle
:   You're doing it wrong

author
:   Jonathan Mukai-Heidt

# Me

## Jonathan Mukai-Heidt
## @johnnymukai
## johnny@buildgroundwork.com

# Groundwork
{::comment}
  the mentorship schpiel
{:/comment}

# ...some history

- Two years consulting at Pivotal Labs
- Kicked around NYC start up scene
- Freelance software developer

# Many, many, many different projects
{::comment}
  at least six last year
{:/comment}

# Why I came to give this talk
{::comment}
  the reason for all this background is...
- I've worked on countless Rails projects
- No one seems to know what to do with controller tests
{:/comment}

# No one knows how to test controllers

# Controller Testing Hall of Shame

# Wait, testing... why?
{::comment}
  - rails has awesome testing culture
  - but just to make sure we're on the same page
  - why do we write tests?
{:/comment}

# Catching regression

# Developing code in isolation
{::comment}
  - testing gives us a framework to write modular code in isolation
  - good design, modular, composable
{:/comment}

# Back to the Hall of Shame

# Stub all the things

    # comment
    def method_name
      body
      # what is the what is the what
    end
{: lang="ruby"}
{::comment}
  - so think about this in terms of what we just talked about
  - too coupled to implementation
  - does nothing for OO design
{:/comment}

# Everything is integration
As a user...
{::comment}
  - Despite DHH, no.
  - Does this drive good design? No! Your controller could be utter spaghetti code
  - "But it works" -- not if you have to maintain it
{:/comment}

# No tests at all
...
{::comment}
  You would be surprised how common this is, even among hardcore TDDers
{:/comment}

# Why does this happen?
{::comment}
  Controllers are a funny thing. They are glue.

  TODO: expand on this!
- Why are you doing these wrong
- There is something unique about Rails controllers that confuses people
{:/comment}

# there's not a lot of logic, or there shouldn't be!

# but there's hope!

# Rails controllers (+ responders) are awesomely declarative

# What do we really mean when we say declarative
- describe properties of the thing we want
- no logic (really!)

# Imperative <-> Declarative
{::comment}
  declarative as a spectrum
{:/comment}

# Look at how declarative Rails controllers can be
{::comment}
TODO: fill this out
{:/comment}

# NEW

# CREATE

# UPDATE

# SHOW

# INDEX

# DESTROY

# And this is great!
{::comment}
TODO: why?
{:/comment}

# But it's not what 90% of the controllers I come across look like

# Because of muddying these nice declarative controllers with business logic

# But I thought this talk was about testing?

# Yes!

# Declarative tests!
{::comment}
Let's put the discussion of logic on hold for a second here
and talk about some tests we can write
to match Rails' awesome declarative controllers
{:/comment}

# What do we really care about in controllers?

# AAPR

# Authentication

# Authorization

# Presence of resource

# Response

# What does it look like in action?

# Shared examples

# Authentication

# Authentication shared example
{::comment}
fil this out
{:/comment}

# Authentication shared example in action
{::comment}
fil this out
{:/comment}

# Authorization

# Authorization shared example
{::comment}
fil this out
{:/comment}

# Authorization shared example in action
{::comment}
fil this out
{:/comment}

# Presence

# Presence shared example
{::comment}
fil this out
{:/comment}

# Presence shared example in action
{::comment}
fil this out
{:/comment}

# Response

# Response shared example
{::comment}
fil this out
{:/comment}

# Response shared example in action
{::comment}
fil this out
{:/comment}

# Why is this such a good thing?
{::comment}
section - should the following slides go after examples?
{:/comment}

# Easier to test

# Simpler controllers

# Logic goes in models where it belongs

# No confusion about where things go (bulk creates, likes, etc)

# Uniform controllers == more time spent on things that matter

# What is the solution?

# Footwork

- No gem!
- This will vary from project to project
- Figure out how your project will handle these situations

# "Skinny controller, fat model"

- Ever since I began Rails work people have been saying this

# 5/6 projects suffer from bloated controllers

- The reason is that the controller is a lazy place to stash stuff
- Laziness is a programmer virtue
- Take my advice and set yourself up for long term laziness

# But Johnny, what about...

- likes
- bulk creates
- merging records

# ActiveModel

# Use it!

# Thanks!

## Jonathan Mukai-Heidt
## @johnnymukai
## johnny@buildgroundwork.com


# junk bin

- everything after this slide is just notes

# What do we actually mean by REST?

# The hidden agenda is REST

# Why REST? (* seriously)

- what makes this approach better?


