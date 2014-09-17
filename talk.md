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

# "Big" concerns vs. "small" concerns

# "Big" concerns should be the same!

- create
- read
- update
- destroy

# "Small" concerns are actually the most important
- Require authentication?
- Who is authorized?
- What formats?

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

    # let's us do things like

    before_filter :load_some_model, except: [:new, :index]
{: lang="ruby"}


# Authorization

    # Using Authority gem

    authorize_actions_for SomeResource 
{: lang="ruby"}

# before_filter

    # Using CanCan gem

    load_and_authorize_resource :some_resource
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
TODO: look over these and make sure they still make sense
{:/comment}

# Authentication

    describe CommentsController do
      let(:current_user) { users(:claude) }
      let(:blog_post) { blog_posts(:skinny_controller_fat_wallet) }


      describe "#new" do
        subject { -> { get :new, blog_post_id: blog_post } }

        context "with a logged in user" do
          it "should not redirect to the login page" do
            response.should_not be_redirect
          end
        end

        context "with an unauthenticated user" do
          it "should redirect to the login page" do
            response.should be_redirect_to(sign_in_path)
          end
        end
      end
    end
{: lang="ruby"}

# Authentication shared example

    shared_examples_for "an action that requires a login" do
      before { sign_out :user }
      it { should respond_with_redirect_to(sign_in_path) }
    end

{: lang="ruby"}
{::comment}
  obviously you could have a non-navigational  shared example for JSON
{:/comment}

# Authentication shared example in action

    describe CommentsController do
      let(:current_user) { users(:claude) }
      let(:blog_post) { blog_posts(:skinny_controller_fat_wallet) }


      describe "#new" do
        subject { -> { get :new, blog_post_id: blog_post } }

        it_should_behave_like "an action that requires a login"
      end
    end
{: lang="ruby"}

# Authorization

    describe "#create" do
      subject { -> { post :create, blog_post_id: blog_post, comment: params } }

      let(:params) { { body: "What a great post. I loved the part about shared examples." } }

      before { sign_in :user, current_user }

      context "with an authorized user" do
        let(:current_user) { users(:bob) }

        it "should respond with created" do
          response.should respond_with 201
        end
      end

      context "with an unauthorized user" do
        let(:current_user) { users(:mallory) }

        it "should respond with 404" do
          response.should respond_with 404
        end
      end
    end
{: lang="ruby"}

# "Malicious Mallory"

# Authorization shared example

    shared_examples_for "an action that requires authorization" do
      before { sign_in :user, users(:mallory) }
      it { should respond_with 404 }
    end
{: lang="ruby"}

# Authorization shared example in action

    describe "#create" do
      subject { -> { post :create, blog_post_id: blog_post, comment: params } }

      let(:params) { { body: "What a great post. I loved the part about shared examples." } }

      before { sign_in :user, users(:bob) }

      it_should_behave_like "a non-navigation action that requires a login"
      it_should_behave_like "an action that requires authorization"
    end
{: lang="ruby"}

# Presence
{::comment}
TODO: fill this out -- maybe
{:/comment}
{: lang="ruby"}

# Presence shared example

    shared_examples_for "an action that requires" do |*resources|
      resources.each do |resource|
        context "with an invalid or missing #{resource}" do
          let(resource) { double(to_param: "does-not-exist", reload: nil) }
          it { should respond_with 404 }
        end
      end
    end
{: lang="ruby"}

# Presence shared example in action

    describe CommentsController do
      let(:current_user) { users(:claude) }
      let(:blog_post) { blog_posts(:skinny_controller_fat_wallet) }

      before { sign_in :user, current_user }

      describe "#show" do
        subject { -> { get :show, id: comment, format: format } }
        let(:comment) { blog_post.comments.first }

        it_should_behave_like "an action that requires", :comment
      end
{: lang="ruby"}

# Response
{::comment}
TODO: fil this out -- maybe
{:/comment}
{: lang="ruby"}

# Response shared example

    shared_examples_for "an action that returns" do |*acceptable_formats|
      acceptable_formats.each do |acceptable_format|
        context "expecting a response in #{acceptable_format} format" do
          let(:format) { acceptable_format }
          it { should_not respond_with_status(:not_acceptable) }
        end
      end


      (%i(html js json xml csv) - acceptable_formats.collect(&:to_sym)).each do |unacceptable_format|
        context "expecting a response in #{unacceptable_format} format" do
          let(:format) { unacceptable_format }
          it { should respond_with_status(:not_acceptable) }
        end
      end
    end
{: lang="ruby"}

# Response shared example in action

    describe CommentsController do
      let(:current_user) { users(:claude) }
      let(:blog_post) { blog_posts(:skinny_controller_fat_wallet) }
      let(:format) { :html }

      before { sign_in :user, current_user }

      describe "#show" do
        subject { -> { get :show, id: comment, format: format } }
        let(:comment) { blog_post.comments.first }

        it_should_behave_like "an action that returns", :html
      end

      describe "#create" do
        subject { -> { post :create, blog_post_id: blog_post, comment: params, format: format } }
        let(:params) { { body: "What a great post. I loved the part about shared examples." } }

        it_should_behave_like "an action that returns", :html, :json
      end
    end
{: lang="ruby"}

# Your test is like a check list
{::comment}
"build to spec" like in the actual meaning of the term
{:/comment}

# But Johnny, what about...

- Likes/Bookmarks/Ratings
- Bulk creates
- Merging records
- Actions that touch several models

# "Skinny controller, fat model"

- Ever since I began Rails work people have been saying this

# 5/6 projects suffer from bloated controllers

{::comment}
- The reason is that the controller is a lazy place to stash stuff
- Laziness is a programmer virtue
- Take my advice and set yourself up for long term laziness
{/:comment}

{::comment}
TODO: let's talk about motherfucking resources now.
TODO: a better transition here would be great
{/:comment}

# ActiveModel

# There is no resource too small

# Models are cheap, especially ones not tied to the DB

# Use it!

# An illustrative example

# Password Reset

Client wanted to overhaul a legacy password reset workflow

# Suspend your dis-belief, they are not using Devise yet

# Too simple to break out into a model?

# Requirements always change

"Ah but wait, we want to tell users if they put in their e-mail wrong."

# Of course requirements change again

"If the user is locked out of their account, we shouldn't send a password reset."

# Suddenly, a fat controller

# ActiveModel makes it simple

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

# The rewards are great!
{::comment}
  after a little footwork your controllers are uniform
  keeping them slim and uniform is easy
{:/comment}

# Easier to test

# Drives good design

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

