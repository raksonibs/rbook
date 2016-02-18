ch 2

React is a doozy.

I don’t mean that the ideas around React are difficult. In fact, quite the opposite. React was engineered to be easily understood by even the most beginning developer. And that especially means you.

React introduces a few ideas however that haven’t been popular in the Javascript community until only recently. Such words are isomorphism, flux, redux, and jsx. Before we explain these terms, let’s look at the history of React a little bit more.

A not so long history

Like any great framework, it had auspicious beginnings. Back in 2013 (wow how fast the world of Javascript is), the Facebook engineers had a problem of building large applications wherein the data would change constantly in the UI level. This is understandable as Facebook  has around 500K (https://www.quora.com/What-is-the-average-number-of-concurrent-users-on-Facebook) active users every minute. In one day, this amounts to nearly 700 million people. This figure, interestingly, is also called the active last minute user count, and is a good metric fro growth and infrastructure support for any development. Nevertheless, because of Facebook’s unprecedented demands on their own systems they realized the general PHP application originally built would not work. Their UI was too slow, the users were not getting updated, and as such cute gifs were lost. Thus the Facebook engineers had a unique problem, how do we update the user's UI (or the document object model, DOM) of the user. And better yet, how do we make this fast?

At the time, a few solutions were around. Knockout.js, Angular, Meteor, and even Ember were beginning and Angular by far and large was the greatest contender for Facebook. Angular, a great framework in its own right, however still relied heavily on the DOM. Instead of updating the code in some precompile step, it instead inserts its code right into the HTML elements. 

As such, the engineers sought to build their own framework, and React was ultimately born. React (https://github.com/facebook/react) has quickly grown to outpace even Angular, with over 36,456 stars on Github, it has an avid opensource community (which I myself am part of!) Clearly React is here to stay, alongside it’s great native development project, React Native.

A Comparison

Before we actually go into the why of React, I think it is best to go into the why not others when compared to React. In order to prove the efficiency of React, let’s use some sample React, Angular, and Ember code to compare these frameworks. 
The test itself will be a matter of implementing many UI elements into one page. Afterwards, we will do a comparison by live loading certain parts of a page via an application. 

Why is React so great? The answer is fairly easy, but here is a breakdown of a few of its talking points:
- it has a great track record: Facebook, one of the largest sites, uses it
- pluggable: it is literally just a view layer so it can be integrated in any old app nicely. Although in most realities you want React to act more as a View-Controller layer, for the most part, it acts as a View only changing the UI elements once underlying data changes
- fast: It only changes exactly what needs to be, and the changes are not linked up unneccessarily like in Angular
- composable: it, like many hot off the press JS frameworks, believes and rewards reusable components
- nonthrashing uis: because of its intimate virtual DOM, React knows exaclty what to change and where, and thus provides for a smooth UI change without any great complexities like difficult bubbling errors in Angular or Ember

Some complaints of React are:
- too much boilerplate: This is true. However, you can use clever design patterns to get around this!
- no two way binding: This is true, however there are good reasons for this. Angular or other two way binded frameworks often are unpredictable, in that one model change triggers a cascading effect for these model changes, this causes some debugging
- JSX: Now I would argue compiling HTML to Javascript makes it easier to read and write. However, unlike Angular, where you throw Javascript into the DOM (ie: ng-repeat), JSX makes you throw the HTML into a JS file which is then compiled into regular vanilla JS. This to me is often more readable and offers more strength in development. The separation of concerns, JS in HTML vs HTML in JS, can be easily answered by observing when using JSX, debugging becomes a breeze because you know where the error occurs. With JS in HTML, because there is no explicit interface and HTML is not strictly parsed like JS, it becomes a whole lot messier to debug.

With an understanding of some of React's strengths and weaknesses, I want to give a quick word about it's Virtual DOM. React creates a a virtual dom which tracks all the elements of the DOM based on its components. When you make a regular change to the DOM, you have to then blindly update the DOM (often refreshing the entire page and state) with the new elements. With the tracking virtual DOM, React compare's the two states from the current state to the desired state. As a result, React then optimizes the update and only updates the necessary components. This provides React with it's legendary speed.

Ultimately, React is a choice like any JS framework. Although I think it has a lot of strengths, it ultimately depends on your own comfort as a developer and your want to build with it. Any engineering problem is for the most part possible, React in my opinion just makes it a whole lot easier. 

With a little bit of React's watercooler moments under our belt, let's go to setting it up. The next two posts will be about setting up a simple backend for React as well as using Gulp or Webpack to get things rolling.

ch 3

So with the features of React in the back of our mind, let's make a backend suitable of working with React. The plan is to make a backend via express that has mongo as it's db and express as it's layout manager. Basically, lets make a MERN App:D

So start by running `express --ejs` in a directory you wish to create the project.

Once your directory structure is made (sorry if I am skipping the basics, I am assuming some understanding of MVC/MVVM and JS), let's run a bunch of node commands to set everything up.

Run: `sudo npm install --save gulp gulp-live-server jquery react reactify vinyl-source-stream guid connect-mongo browserify browser-sync babel`

These are a lot of packages, but I am going to likely show a few ways of setting up config with React, so bare with me.

After this, let's set up our express app server to take in a few routes for json. Here is what it look:
{% highlight javascript %}
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
var mongoose = require('mongoose');

var routes = require('./routes/index');
var users = require('./routes/users');


var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

// uncomment after placing your favicon in /public
//app.use(favicon(__dirname + '/public/favicon.ico'));
app.use(logger('dev'));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', routes);
app.use('/users', users);

require('./routes/things.js')(app);


if (app.get('env') === 'development') {
    console.log("connecting to local db");
    var db = "mongodb://localhost/myapp";
    mongoose.connect(db, function() {
        // console.log("dropping local db to renew");
        // mongoose.connection.db.dropDatabase();
    })   
} else {
    console.log("connecting to prod db");
    mongoose.connect(secrets.db);
}

mongoose.connection.on('error', function() {
  console.error('MongoDB Connection Error. Please make sure that MongoDB is running.');
});


module.exports = app;

{% endhighlight %}

Here is the routes for the things.js:
{% highlight javascript %}
var express = require('express');
var router = express.Router();
var Thing = require('../models/Thing');

module.exports = function(app) {

app.route('/api/things')
.get(function(req,res) {
    console.log('using this route')
    Thing.find({}, function(err, things) {
    if (err) return next(err)
    
    res.send(things);  
  });
  })
.post(function(req, res) {
  var thing = new Thing({
    name: req.body.name,
    loved: false
  })

  thing.save(function(err) {
    if (err) return next(err)

    res.status(300).send()
  })
})

app.route('/api/things/:id')
  .delete(function(req, res) {
    Thing.findOne({
      _id: req.params.id
    }).remove(function(x) {
      
    });
  })
  .patch(function(req, res) {
    Thing.findOne({
      _id: req.body._id
    }, function(error, thing) {
      for (var key in req.body) {
        thing[key] = req.body[key]
      }

      thing.save()
      res.status(200).send()
    })
  })

}
{% endhighlight %}

Here is the basic structure for our only model for now:
{% highlight javascript %}
var mongoose = require('mongoose');

var thingSchema = new mongoose.Schema({
  name: String,
  loved: String,
  id: String
});

module.exports = mongoose.model('Thing', thingSchema, "Things");

{% endhighlight %}

To make sure everything is okay, run a few curl requests with mongo running:

`curl localhost:3000/api/things
curl -d "name=silly" localhost:3000/api/things
curl localhost:3000/api/things`

After seeing what returns, make sure the server starts with a `node ./bin/www` or setup your own ports and start process (this will change shortly mind you once gulp comes in)!

Alright great, we have a JSON CRUD app suitable to build React upon. First though, let's get a packaged solution manager like gulp to help us out here!

