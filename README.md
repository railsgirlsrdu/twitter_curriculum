# Rails Girls Twitter

**Details**  
The Rails Girls Twitter client is a simplified twitter-esque application mean to duplicate the very basics behind the [twitter](https://twitter.com/) social media platform. This application will meet the following set of requirements:  

* Users can create accounts
* Users can log into their accounts
* Users can post tweets
* Users can view all tweets
* When a users visits her own profile she sees her own tweets.
* When a user visits another user's profile she sees that user's tweets
* Tweets are no longer than 256 characters

##Basics
**Ruby**  
Ruby is the language you will be working in. It looks like this!  

```ruby
class Person
  attr_accessor :name
  
  def greet
    puts 'Hello!'
  end
  
  def say(statement)
    puts statement
  end
end
```
How cool is that! Well, it may look a bit like gibberish to you now. Let me try to explain.

Above is code that defines a person who has a name, can say things, and greet you with a howdy "Hello!" It's not important to understand how this code works now, just familiarize yourself with what code looks like!

You should all be running the latest version of ruby 2.0 on your machine, check that you are

**Rails**  
Rails is the framework you will be using. It is written in Ruby. If you ever get the two confused just remember "Ruby is the Language, Rails in the framework." We can have Ruby without Rails but we cannot have Rails without Ruby.

We use Rails to take care of all the heavy lifting of talking to our users via the browsers and showing them the web pages they want filled with the content they asked for. Rails is all about helping you write awesome websites in Ruby!

If you do not have rails installed on your machine, install it:


**Gems**  
To achieve the desired functionality we will use a series of [gems](http://rubygems.org). Gems are packets of code that can extend the functionality of your application. We call them gems because they are written in ruby!

> Protip: Rails is also considered a gem but the applications you build with rails are not considered gems!

##Lets Get Started!

###Know your tools

When you first start developing with Rails, your primary tools are the web browser, the text editor, and the terminal. But it isn't always clear which one you need when you start out. We've given each one an icon to help you along the way.

:pencil: Whenever you see this icon, it means you'll be editing a file in the text editor. We put the name of the file to edit or create next to each icon.  
:computer: This one indicates you'll be entering a command in the terminal, or viewing terminal output.  
:earth_americas: We'll use this to signify stuff you'll do in the browser. It's the *world wide web* right?  

###Your very first Rails app
So, we received the requirements under 'Details' (above) from our client and we are ready to get started!
Our first step is to check to make sure we have the latest and greatest version of ruby!

:computer:
```
ruby -v
```

you should see something like `ruby 2.0.0p0 (2013-02-24 revision 39474) [x86_64-darwin12.2.1]`

Our next step is to create a new rails application

:computer:
```
rails new twitter
```

If the above command doesn't work then make sure you have rails installed on your machine by running:

:computer:
```
gem install rails
```

**Voila!** You've created your first rails application!

###Start rails for the first time
Sadly, rails still need a little help from us to get started. Step into your new rails directory:

:computer:
```
cd twitter
```

Inside here you can type `ls` (Mac OS and Linux) or `dir` (Windows) to see a list of all the files and folders that `rails new` created. It should look something like this

```
Gemfile
README.md
app
config
db
log
test
vendor
Gemfile.lock
Rakefile
bin
config.ru
lib
public
tmp
```

From here we can startup rails running this simple command:

:computer:
```
rails server
```

Your output should look something like this

```
=> Booting WEBrick
=> Rails 4.0.0 application starting in development on http://0.0.0.0:3000
=> Run `rails server -h` for more startup options
=> Ctrl-C to shutdown server
[2013-09-05 12:02:48] INFO  WEBrick 1.3.1
[2013-09-05 12:02:48] INFO  ruby 2.0.0 (2013-02-24) [x86_64-darwin12.2.1]
[2013-09-05 12:02:48] INFO  WEBrick::HTTPServer#start: pid=27949 port=3000
```

After that, type :earth_americas:`localhost:3000` into your browser.

You should see this page:  
![](http://i.imgur.com/SwKp4rB.jpg)

###User Authentication

For user authentication we are going to use a gem called [devise](http://rubygems.org/gems/devise) which will make this a snap.

First, we need to tell Rails we want to use devise. We can do that with our
application's `Gemfile`. Each application has a file called simply `Gemfile`
which contains a list of Ruby gems we want to use in our application. To add a
new one, we open the `Gemfile` in our text editor and add the line

:pencil: `Gemfile`
```ruby
gem 'devise'
```

anywhere in the file. Save the file. Then, go to your terminal or command prompt and type the following

:computer:
```
bundle install
```

Next, we are going to follow the steps for ['Getting Started'](https://github.com/plataformatec/devise#getting-started) in the devise readme to generate some User modules, controllers, and views.

:computer:
```
rails generate devise:install
rails generate devise User
```

The following code generated some models and migrations for us. In order to update our database to coincide with our new code we need to run the following.

:computer:
```
rake db:migrate
rails server
```

Just like that we've added authentication to our application. Go to the url <http://localhost:3000/users/sign_up> to see the sign up page and create a new user! We'll need a user to create tweets later so make sure you create one now. You will wind up back on the index page when you're done.

>PROTIP: Never use one of your own passwords to create a user when developing an application! Use some junk password like 'securepassword'

###Tweets
So, our first two requirements are complete (Users can create accounts and Users can log into their accounts). Next up we have to deal with tweets.

We are going to use rails generators to save some time. This will generate the code we need to create Tweets in our application.

:computer:
```
rails generate model Tweet body:string user:references
```

This command creates a `Tweet` model and a migration to create a `tweets` table where we can store the body of a tweet and the user who made the tweet.

>PROTIP: `body:string` and `user:references` details the attributes of our tweets. `references` pertains to an ID of the owning record. In our case, Tweets reference users who make them. You can add any attributes you want to a record in this manner when using generators.

####Model

One of the files we generated was a Tweet model. We will use this Model to create, update, and destroy Tweets with commands like `Tweet.create`, `Tweet.update_attributes`, and `Tweet.destroy`. Take a look in your `app/models` directory. You should see a newly created file called `tweet.rb` with the following contents

:pencil: `app/models/tweet.rb`
```ruby
class Tweet < ActiveRecord::Base
  belongs_to :user
end
```

This is an [ActiveRecord](http://guides.rubyonrails.org/active_record_basics.html) Model. It is small now but it will expand as we add new features to our tweets.


####Migration

The next thing generated was a [migration](http://guides.rubyonrails.org/migrations.html). Look inside your `db/migrations` directory. You will se a file that looks something like `20130908173312_create_tweets.rb`. Your file's name may not be the same but the contents will be.

:pencil: `db/migrate/20130927025933_create_tweets.rb` (Your huge number will be different and that's normal.)
```ruby
class CreateTweets < ActiveRecord::Migration
  def change
    create_table :tweets do |t|
      t.string :body
      t.references :user, index: true

      t.timestamps
    end
  end
end
```

This code will modify your database and create a table called 'tweets'. Lets run it to create that table. Type the following into your command prompt.

:computer:
```
rake db:migrate
```
The output should look like this

```
==  CreateTweets: migrating ===================================================
-- create_table(:tweets)
   -> 0.0037s
==  CreateTweets: migrated (0.0038s) ==========================================
```

Now that your table has been created, lets take a look at our views and controllers.

> PROTIP: If you've run a migration that is incorrect in some way, run rake db:rollback to reset the migration an THEN go back to your migration code and fix it.

####Controller/Views
Next we want to display our tweets. In order to do that we are going to need to create a Tweet controller and some views. Controllers pull data out of the database like tweet content and serve that content up to views which render the HTML that is displayed to the users.

In order to start, we'll define some routes. To do so, open your `config/routes.rb` file and add the line below `devise_for :users`.

:pencil: `config/routes.rb`
```ruby
root 'tweets#index'
resources :tweets, only: [:new, :index, :create]
```

To create a controller, we can use a Rails generator

:computer:
```
rails generate controller tweets
```

This will create file `app/controllers/tweets_controller.rb` as well as helpers, test helpers, stylesheets, and javascript files for us to use. But we're going to focus on the controller file.

Now lets get started on our actions.

**index**

Index will be a list of every tweet that every user makes. It will be the job of our controller to collect all the tweets and the view will display them. Add the following code to your `tweets_controller.rb`

:pencil: `app/controllers/tweets_controller.rb`
```ruby
def index
  @tweets = Tweet.all.order(:created_at => :desc)
end
```

Next we will make our view. Create a file at `app/views/tweets` called `index.html.erb`. If the tweets directory doesn't exist, make it.

>PROTIP: On Linux and Mac, you can use the command `mkdir app/views/tweets` to make the tweets views directory.

Once you've created that `index.html.erb` file, add the following to it. This will display all our tweets and the user that made them.

:pencil: `app/views/tweets/index.html.erb`
```erb
<h1>Tweets</h1>
<%=link_to "New Tweet", new_tweet_path %>
<% @tweets.each do |tweet| %>
  <p>
    <%= tweet.user.email %><br/>
    <%= tweet.body %>
  </p>
<% end %>
```

The index action will automatically look for a view at `app/views/tweets` called index. Go ahead and start your rails server.

:computer:
```
rails s
```

Finally, open localhost:3000 in your browser and you should see an empty page with 'Tweets' heading at the top.

**new/create**

Now, lets add a form so that our users can post tweets. However, this is a bit tricky because we only want authenticated users to post new tweets! Well, it may seem tricky but with rails and devise it doesn't have to be.

Update your TweetsController.

:pencil: `app/controllers/tweets_controller.rb`
```ruby
class TweetsController < ApplicationController
  before_filter :authenticate_user!, only: [:create, :new]

  def new
    @tweet = Tweet.new
  end

  def create
    if @tweet = Tweet.create(body: params[:tweet][:body], user_id: current_user.id )
      redirect_to tweets_path
    else
      render :new
    end
  end

  def index
    @tweets = Tweet.all.order('created_at DESC')
  end
end
```

Take a minute to really understand what each line of code is doing here. We are creating a [before_filter](http://guides.rubyonrails.org/action_controller_overview.html#filters) that will be ensuring our visitor is an authenticated user before hitting our new and create actions. This single line of code will handle everything from blocking anonymous users from posting a tweet to redirecting those anonymous users to sign in or sign up for a new account when they try to post a tweet.

We also added new a create actions which will be used to populate the proper objects for our views. Speaking of which, lets finish things up with a new tweet form! Add a `new.html.erb` file to your `app/views/tweets` directory and add the following to it.

:pencil: `app/views/tweets/new.html.erb`
```erb
<h1>New Tweet</h1>

<%= form_for @tweet do |f| %>
  <%= f.text_area :body %>
  <%= f.submit %>
<% end %>
```

Now, the "New Tweet" link on your tweets index page should instruct you to login. From there, click "Sign up" to create a new user account (link under login form). Once you have a new user created, log that user. Craft your tweet and post it! Just like that we have the basics down for sign up, login, and tweet posting!

###Users

One thing we are missing is the ability to view tweets a user makes. First, we will need to add the ability for us to collect all the tweets a user makes. Add the following to your `app/views/layouts/application.html.erb` directly underneath the `<body>` tag.

:pencil: `app/views/layout/application.html.erb`
```erb
<% if user_signed_in? %>
  <%= link_to('Logout', destroy_user_session_path, :method => :delete) %>
<% else %>
  <%= link_to('Login', new_user_session_path)  %>
<% end %>
```

Open your `user.rb` model and add `has_many :tweets`.

:pencil: `app/models/user.rb`
```ruby
class User < ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
  has_many :tweets
end
```

After that we need to update our routes, controller, and views so that we have a user show page that gives us a list of all tweets a user has made.

Add the following addition to your `config/routes.rb` below our `resources :tweets` line from earlier.

:pencil: `config/routes.rb`
```ruby
get 'users/:id' => 'users#show', as: 'user'
```

Create a Users controller using the rails generator.

:computer:
```
rails generate controller users
```

Open the UsersController at `app/controllers/users_controller.rb`.

:pencil: `app/controllers/users_controller.rb`
```ruby
class UsersController < ApplicationController
  def show
    @user = User.find(params[:id])
  end
end
```

Create a view at `app/views/users/show.html.erb`

:pencil: `app/views/users/show.html.erb`
```erb
<h1><%= @user.email %></h1>

<% @user.tweets.each do |tweet| %>
  <p>
    <%= link_to tweet.user.email, user_path(tweet.user) %><br/>
    <%= tweet.body %>
  </p>
<% end %>
```

Next, add a link to your users show page by adding `link_to tweet.user.email, user_path(tweet.user)` to your `app/views/tweets/index.html.erb`, replacing `tweet.user.email`:

:pencil: `app/views/tweets/index.html.erb`
```erb
<h1>Tweets</h1>
<%=link_to "New Tweet", new_tweet_path %>
<% @tweets.each do |tweet| %>
  <p>
  	<%= link_to tweet.user.email, user_path(tweet.user) %><br />
    <%= tweet.body %>
  </p>
<% end %>
```

Now we can click on the name of the user that makes a tweets to see all tweets made by that user. See this in action by going to the app and viewing all tweets and clicking on one of the user names: http://localhost:3000/

### Let the world see your app

Up until now, your Twitter app has been running on your laptop, only accessible to you. When you're ready for the world to see your work, or just to share your app with coworkers, you need to deploy it somewhere that's publicly accessible. [Heroku](http://heroku.com) is an application deployment platform that makes it easy to deploy apps. To deploy your app to Heroku, follow these steps:

#### Create your Heroku account

1. Sign up at https://id.heroku.com/signup/www-header (and confirm account email etc...)
2. Install the Heroku Toolbelt: https://toolbelt.heroku.com/
3. In *your terminal* run the following command to login into Heroku

```console
heroku login
Enter your Heroku credentials.
Email: adam@example.com
Password: 
Could not find an existing public key.
Would you like to generate one? [Yn] 
Generating new SSH public key.
Uploading ssh public key /Users/adam/.ssh/id_rsa.pub
```

#### Update `Gemfile`

Heroku requires that you use a different database than what you've been using in development. This database is called Postgresql. To configure your app to use Postgresql when on Heroku, open up the `Gemfile` in your text editor. Change this line:

```ruby
gem 'sqlite3'
```
to this:
```ruby
group :development do
  gem 'sqlite3'
end
```
And add a new section:
```ruby
group :production do
  gem 'pg'
  gem 'rails_12factor'
end
```

Update your dependencies by running this command in your terminal:

```console
bundle --without production
```

#### Deploy your app

Heroku uses a version-control system called Git to deploy apps from your laptop to its servers. To setup `git` for your project run these commands in the terminal:

```console
git init
echo "public/uploads" >> .gitignore
echo "tmp" >> .gitignore
echo "logs" >> .gitignore
git add .
git commit -m "initial commit"
```

Create your app on Heroku:

```console
heroku create
```

Deploy your app from your laptop to Heroku with this command:

```console
git push heroku master
```

And make sure your database on Heroku is configured for your app:

```console
heroku run rake db:migrate
```

`rake db:migrate` is the same command you've run on your laptop to configure your database, this command just runs it on your environment on Heroku.

If you get an error that looks like this:

```console
$ heroku run rake db:migrate
Running `rake db:migrate` attached to terminal... up, run.1600
 !    
 !    Error connecting to process
```

You're most probalby on a WIFI network that is blocking access on a port Heroku needs access to. (For RailsGirlsRDU, use WIFI `AmericanTobacco` and *not* `Visitor`)

Once your database migrations have run, open your app on Heroku with this command:

```console
$ heroku open
```

You should see your Tweet app running at a URL like: `http://peaceful-mountain-9178.herokuapp.com/`. Since it's running against a fresh database, you will need to register a new user again to create Tweets.
