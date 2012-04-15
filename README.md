Overview
========
**Frank on Heroku** is simply a [Frank](https://github.com/blahed/frank) project template that makes 
it easy to host a Frank-based static site on Heroku.

[Frank](https://github.com/blahed/frank) is useful Ruby gem that lets you build static sites using 
your favorite libs. Frank has a built in development server for previewing work as you develop, an 
"export" command for compiling and saving your work out to static html and css, and a publish command 
for copying your exported pages to a server.

[Heroku's Cedar Stack](https://devcenter.heroku.com/articles/cedar) makes it easy to host a Rack app 
by adding a `config.ru` file to the root of the project. Frank on Heroku includes a `config.ru` file
that tells Heroku how to serve up files from the `/exported` folder in a Frank project.

### Frank on Heroku features
* Prebuilt `config.ru` file that includes Ryan Tomayko's [TryStatic](https://github.com/rack/rack-contrib/blob/master/lib/rack/contrib/try_static.rb) 
class which will try and match requests to a static file in the `/exported` folder (the default export 
folder for Frank).

        http://www.example.com
            # => returns /exported/index.html
        http://www.example.com/about
            # => returns /exported/about.html
        http://www.example.com/tour/request-a-demo
            # => returns /exported/tour/request/request-a-demo.html
        
* Includes `/heartbeat` request handler that serves a simple `200` response. Great for **uptime 
monitoring**.
* `404` ("File not found") error handling automatically serves `/exported/404.html` to the user.

Prerequisites
-------------
1. Make sure [frank](https://rubygems.org/gems/frank) gem is installed. Install the gem if not:

        $ gem install frank
    
2. Clone frank-on-heroku files into `~/.frank_templates`

        $ cd ~/.frank_templates/
        $ git clone git://github.com/rarestep/frank-on-heroku.git
        
Creating a new website
----------------------
1. `cd` to the directory where your website code will live (e.g. `cd ~/Dev` )
2. Create the new Frank project using the **frank-on-heroku template**:

        $ frank new rarestep-www

        Frank is...
         - Creating your project 'rarestep-www'
        Which template would you like to use? 
         1. default
         2. frank-on-heroku
        > 2
         - Copying frank-on-heroku Frank template
         Congratulations, 'rarestep-www' is ready to go!
 
        $ cd rarestep-www/

3. Install `Gemfile` gems using [Bundler](http://gembundler.com/) (it's a good idea to to create a 
new `gemset` using [RVM](http://beginrescueend.com/) for your site):

        
        $ rvm gemset create rarestep-www
        $ rvm gemset use rarestep-www
        $ bundle install
        
4. Run the Frank server locally and make sure you can pull up the stie (open http://localhost:3601/ 
in a web browser to view):

        $ bundle exec frank server

        -----------------------
         Frank's takin' care of business...
         0.0.0.0:3601 
        >> Thin web server (v1.3.1 codename Triple Espresso)
        >> Maximum connections set to 1024
        >> Listening on 0.0.0.0:3601, CTRL+C to stop
        
5. Create a new app on Heroku on the [Cedar Stack](https://devcenter.heroku.com/articles/cedar):

        
        $ heroku create --stack cedar

        Creating severe-sky-5191... done, stack is cedar
        http://severe-sky-5191.herokuapp.com/ | git@heroku.com:severe-sky-5191.git
        Git remote heroku added
        
6. Export your site using Frank's `export` command, `commit` to git and `push` to Heroku:

        
        $ bundle exec frank export

        A folder named `exported' already exists, overwrite it? [y/n] y

        Frank is...
         - Creating 'exported'
         - Creating 'exported/css/frank.css'
         - Creating 'exported/index.html'
         - Creating 'exported/css/frank.css'
         - Copying static content
         Congratulations, project dumped to 'exported' successfully!

        $ git commit -a -m "Exported site"
        $ git push heroku master
        $ heroku open
        
Updating a website
------------------
*Super easy!* Pushing updates is as simple as [using Frank as normal to build out your site](https://github.com/blahed/frank/blob/master/README.md), 
then repeating **Step #6** above when you're ready to push to Heroku.