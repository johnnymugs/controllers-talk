# Controller Testing

subtitle
:   "You're doing it wrong"

author
:   Jonathan Mukai-Heidt

# Me

* Jonathan Mukai-Heidt

* @johnnymukai

* johnny@buildgroundwork.com

# Groundwork
{::comment}
  the mentorship schpiel
TODO: logo, website link
{:/comment}
http://buildgroundwork.com

# ...some history
{::comment}
  before Groundwork I...
{:/comment}

- Two years consulting at Pivotal Labs
- Kicked around NYC start up scene
- Freelance software developer

# Many, many, many different projects
{::comment}
  at least six last year
{:/comment}

# Almost no one knows how to test controllers
{::comment}
  the reason for all this background is...
- I've worked on countless Rails projects
- No one seems to know what to do with controller tests
- Why I came to give this talk
{:/comment}

# One day... (あの日。。。)
{::comment}
  one day I started working on a project with a dev much senior to me
  he had strong opinions about controllers
  thisishowwedoit.png
{:/comment}

# Controller Testing Hall of Shame

# Wait, testing... why?
{::comment}
  - rails has awesome testing culture
  - but just to make sure we're on the same page
  - why do we write tests?
{:/comment}

# Catching regressions

# Spec as documentation

# (And so on)

# Developing code in isolation!!!
{::comment}
  - testing gives us a framework to write modular code in isolation
  - good design, modular, composable
{:/comment}

# Back to the Hall of Shame
{::comment}
  with the above in mind
{:/comment}

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
- there's not a lot of logic, or there shouldn't be!

{:/comment}

# Controllers are hard to test in isolation

That's what leads to these anti-patterns

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

# A divergence
TODO: let's talk about motherfucking resources now.

# ActiveModel

# Use it!

# An illustrative example

# Password Reset

Client wants to implement a simple password reset

`/password_reset_requests/new`
`/password_reset_requests/create`

# Suspend your dis-belief, they are not using Devise yet

# Legacy code base

`User.send_password_reset_email!`

# Too simple to break out into a model?
{::comment}
There is existing code for tokens and stuff or what?
{::/comment}

{::comment}
TODO:look this code up from a2
{::/comment}

# Okay, it just fires off a job

# Requirements always change
"Ah but wait, we want to tell users if they put in their e-mail wrong."

# Seems easy enough to just add in the controller...

# Add to our spec...

`````
context "with an e-mail for an existing user" do
...
end

context "with an e-mail for a _____________" do
...
end

`````
{::comment}
TODO: finish up here
{::/comment}

# Add to the implementation...

`````
if user = User.find_by_email(params[:email])
  user.send_password_reset_email!
else
  flash[:alert] = "Looks like you don't have an account yet"
end
`````

# Of course requirements change again
"If the user is locked out of their account, we shouldn't send a password reset."

# Ok, just change the spec...

{::comment}
TODO: finish up here
{::/comment}

# Uhhhhh...
{::comment}
TODO: finish up here
{::/comment}

# So where's your great controller strategy now Johnny?

# ActiveModel makes it simple

# this might seem contrived but I've seen this and worse
- just a password_reset action on the users controller


# The lesson
{::comment}
this is what it's all about, make your web app about resources
{:/comment}

# Think nouns (resources), not verbs

# HTTP gives you all the verbs you need

# Habbits

# The wins
- Easier to test
- Simpler controllers
- Logic goes in models where it belongs
- No confusion about where things go (bulk creates, likes, etc)
- Uniform controllers == Less time to write


# Thanks!

Get in touch!

* Jonathan Mukai-Heidt

* Groundwork

* @johnnymukai

* johnny@buildgroundwork.com

