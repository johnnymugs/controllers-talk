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
I'm a software mentor at Groundwork, a new mentorship-focused software consultancy in New York City
Maybe elaborate.
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
  the last three or four years I've worked on dozens of Rails projects
{:/comment}

# Almost no one knows how to test controllers
{::comment}
  the reason for all this background is...
- I've worked on countless Rails projects
- No one seems to know what to do with controller tests
- Why I came to give this talk
{:/comment}

# Controller Testing Hall of Shame
{::comment}
  before we get into it though let's look at some strategies that don't work
{:/comment}

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

    describe "#show" do
      subject { -> { get :show, id: id } }
      let(:id) { '77' }
      let(:pizza) { Pizza.new }

      context "with an existing pizza" do
        before { Pizza.should_receive(:find).with(id).and_return(pizza) }
        it { assigns(:pizza).should == pizza }
      end

      context "with a non-existent pizza" do
        before { Pizza.should_receive(:find).with(id).and_raise_error(ActiveRecord::RecordNotFound)
        it { should raise_error(ActiveRecord::RecordNotFound) }
      end
    end
{: lang="ruby"}
{::comment}
  - so think about this in terms of what we just talked about
  - too coupled to implementation
  - does nothing for OO design
{:/comment}

# Everything is integration

    As a user
    Given there is a pepperoni pizza
    When I visit the pizza index page
    And I click on "pepperoni"
    Then I should see the pepperoni pizza
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

# Often the things that really matter are untested
{::comment}
  the worst thing about all these approaches is that they test things common to ALL rails controllers
  and they miss the things that are unique to specific controllers
  TODO: is this the right place for this?
{:/comment}

# Why does this happen?
{::comment}
  I've thought a lot about why this happens...
  Controllers are a funny thing. They are glue.

  TODO: expand on this!
- Why are you doing these wrong
- There is something unique about Rails controllers that confuses people
- there's not a lot of logic, or there shouldn't be!
TODO: there really is room for improvement/expansion here
TODO: the order of these few slides could be improved

{:/comment}

# Controllers are hard to test in isolation

That's what leads to these anti-patterns

# People are confused about what to test

# So was I!

# One day...
{::comment}
  one day I started working on a project with a dev much senior to me
  he had strong opinions about controllers
  he employed a unique strategy **TODO: own slide or what?
{:/comment}

# Rails controllers (+ responders) are awesomely declarative
{::comment}
TODO: the things that make controllers confusing to test are due to how awesome rails controllers can be
Let's talk about what's great about controllers!
{:/comment}

# What do we really mean when we say declarative
{::comment}
this is a term i'll toss around a lot so let's define it
{:/comment}

- Describe properties of the thing we want
- No logic (really!)

# Imperative / Declarative
{::comment}
  declarative as a spectrum
{:/comment}

# Imperative

"When deleting a user, if the current user is an admin user, then allow the deletion; if the current user is not an admin, do not allow the deletion to finish."

# Declarative

"Only admin users can delete another user."

# Imperative

"When a request for a resource comes in, if the request is for JSON, then fetch the resource and render it from the JSON template; if the request is for HTML, then fetch the resource and render the HTML template; if the request is for another format like PDF, return an error."

# Declarative

"This controller returns a resource represented as JSON or HTML."

# Ruby is imperative but it lets us write declarative code

# Look at how declarative Rails controllers can be

# before_filter

    # let's us do things like

    before_filter :authenticate_user!
{: lang="ruby"}

# before_filter

    # Using Authority gem

    before_filter :authorize_actions_for SomeResource 
{: lang="ruby"}

# before_filter

    # Using CanCan gem

    before_filter :load_and_authorize_resource :some_resource
{: lang="ruby"}

# Rails 4 + Responders
{::comment}
everyone knows about Rails responders at this point I hope
{:/comment}

# respond_to
    # quickly declare formats

    respond_to :html, :json
{: lang="ruby"}

# NEW

    def new
      respond_with(@pizza = Pizza.new)
    end
{: lang="ruby"}


# CREATE

    def create
      respond_with(@pizza = Pizza.create(pizza_params))
    end
{: lang="ruby"}

# UPDATE

    def create
      respond_with(@pizza.update_attributes(pizza_params))
    end
{: lang="ruby"}

# SHOW

    def create
      respond_with(@pizza = Pizza.find_by_param!(params[:id]))
    end
{: lang="ruby"}

# ...and so on...

# Little to no logic in controllers
{::comment}
TODO: why?
again, it's a spectrum obvi
{:/comment}

# And this is great!
{::comment}
TODO: why?
because controllers should be a simple link between views and models
{:/comment}

# But it's not what 90% of the controllers I come across look like

# Because of muddying these nice declarative controllers with business logic

# Business logic belongs in models
You've heard this many times already

# But I thought this talk was about testing?

# Yes!

# Tests should help us write better code

# Declarative controller? Declarative tests!
{::comment}
Let's put the discussion of logic on hold for a second here
and talk about some tests we can write
to match Rails' awesome declarative controllers
{:/comment}

# What do we really care about in controllers?
{::comment}
okay, how do we do it?
{:/comment}

# AAPR

# Authentication

# Authorization

# Presence of resource

# Response
{::comment}
Something people often miss is that we can request an object in many formats, it should be the same basic resource
{:/comment}

# What does it look like in action?
{::comment}
You'll notice these are very repetitive in implementation
{:/comment}

# Shared examples

{::comment}
TODO: consider doing before / after
{:/comment}

# Authentication

# Authentication shared example
{::comment}
TODO: fil this out
{:/comment}

# Authentication shared example in action
{::comment}
TODO: fil this out
{:/comment}

# Authorization

# Authorization shared example
{::comment}
TODO: fil this out
{:/comment}

# Authorization shared example in action
{::comment}
TODO: fil this out
{:/comment}

# Presence

# Presence shared example
{::comment}
TODO: fil this out
{:/comment}

# Presence shared example in action
{::comment}
TODO: fil this out
{:/comment}

# Response

# Response shared example
{::comment}
TODO: fil this out
{:/comment}

# Response shared example in action
{::comment}
TODO: fil this out
{:/comment}

# Your test is like a check list
{::comment}
"build to spec" like in the actual meaning of the term
{:/comment}

# But Johnny, what about...

- Likes/Bookmarks/Ratings
- Bulk creates
- Merging records

# "Skinny controller, fat model"

- Ever since I began Rails work people have been saying this

# 5/6 projects suffer from bloated controllers

- The reason is that the controller is a lazy place to stash stuff
- Laziness is a programmer virtue
- Take my advice and set yourself up for long term laziness

# A divergence
{::comment}
TODO: let's talk about motherfucking resources now.
TODO: a better transition here would be great
{/::comment}

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
{::comment}
TODO: show the spec and it's slow divergence from nice controller
{::/comment}


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
{::comment}
TODO: code code code
{::/comment}

# And our controller is back to being nice and simple

# This might seem contrived but I've seen this and worse
- just a password_reset action on the users controller


# The lesson
{::comment}
this is what it's all about, make your web app about resources
{:/comment}

# Think nouns (resources), not verbs

# HTTP gives you all the verbs you need

# Footwork

- No gem!
- This will vary from project to project
- Figure out how your project will handle these situations

# Habbits

# Hence the controller checklist
{::comment}
  it just helps you develop a habbit
{:/comment}

# Easier to test

# Simpler controllers

# Logic goes in models where it belongs

# No confusion about where things go (bulk creates, likes, etc)

# Uniform controllers == more time spent on things that matter

{::comment}
  TODO: stronger summary
Your testing strategy should help drive good code
Your controllers should be simple and resource driven
You can employ a testing strategy that helps you achieve this
{:/comment}

# Thanks!

Get in touch!

* Jonathan Mukai-Heidt

* Groundwork

* @johnnymukai

* johnny@buildgroundwork.com

