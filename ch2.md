ch 2

React is a doozy.

I don’t mean that the ideas around React are difficult. In fact, quite the opposite. React was engineered to be easily understood by even the most beginning developer. And that especially means you.

React introduces a few ideas however that haven’t been popular in the Javascript community until only recently. Such words are isomorphism, flux, redux, and jsx. Before we explain these terms, let’s look at the history of React a little bit more.

A not so long history

Like any great framework, it had auspicious beginnings. Back in 2013 (wow how fast the world of Javascript is), the Facebook engineers had a problem of building large applications wherein the data would change constantly in the UI level. This is understandable as Facebook  has around 500K (https://www.quora.com/What-is-the-average-number-of-concurrent-users-on-Facebook) active users every minute. In one day, this amounts to nearly 700 million people. This figure, interestingly, is also called the active last minute user count, and is a good metric fro growth and infrastructure support for any development. Nevertheless, because of Facebook’s unprecedented demands on their own systems they realized the general PHP application originally built would not work. Their UI was too slow, the users were not getting updated, and as such cute gifs were lost. Thus the Facebook engineers had a unique problem, how do we update the user's UI (or the document object model, DOM) of the user. And better yet, how do we make this fast?

At the time, a few solutions were around. Knockout.js, Angular, Meteor, and even Ember were beginning and Angular by far and large was the greatest contender for Facebook. Angular, a great framework in its own right, however still relied heavily on the DOM. Instead of updating the code in some precompile step, it instead inserts its code right into the HTML elements. 

As such, the engineers sought to build their own framework, and React was ultimately born. React (https://github.com/facebook/react) has quickly grown to outpace even Angular, with over 36,456 stars on Github, it has an avid opensource community (which I myself am part of!) Clearly React is here to stay, alongside it’s great native development project, React Native.

A Comparison

Before we actually go into the why of React, I think it is best to go into the why not others when compared to React. In order to prove the efficiency of React, let’s use some sample React and Angular code to compare these frameworks. (Inspired by https://www.codementor.io/reactjs/tutorial/reactjs-vs-angular-js-performance-comparison-knockout)
The test itself will be a matter of implementing many UI elements into one page via a TODO app. The code for each todo app will be seen below.

React Code:
var Class = React.createClass({
    select: function(data) {
        this.props.selected = data.id;
        this.forceUpdate();
    },

    render: function() {
        var items = [];
        for (var i = 0; i < this.props.data.length; i++) {
            items.push(
                React.createElement("div", { className: "row" },
                    React.createElement("div", { className: "col-md-12 test-data" },
                        React.createElement("span", { className: this.props.selected === this.props.data[i].id ? "selected" : "", onClick: this.select.bind(null, this.props.data[i]) }, this.props.data[i].label)
                    )
                )
            );
        }

        return React.createElement("div", null, items);
    }
});

$("#run-react").on("click", function() {
    var built = new Class({ data: data, selected: null });

    var data = _buildData(),
    date = new Date();

    React.render(built, $("#react")[0]);
    $("#run-react").text((new Date() - date) + " ms");
});

Angular
<div>
    <div class="row" ng-repeat="item in data">
        <div class="col-md-12 test-data">
            <span ng-class="{ selected: item.id === $parent.selected }" ng-click="select(item)">{{item.label}}</span>
        </div>
    </div>
</div>

angular.module("test", []).controller("controller", function($scope) {
    $scope.run = function() {
        var data = _buildData(),
        date = new Date();

        $scope.selected = null;
        $scope.$$postDigest(function() {
            $("#run-angular").text((new Date() - date) + " ms");
        });

        $scope.data = data;
    };

    $scope.select = function(item) {
        $scope.selected = item.id;
    };
});

Raw Dom
<script type="text/html" id="raw-template">
    <div class="row">
        <div class="col-md-12 test-data">
            <span class="{{className}}">{{label}}</span>
        </div>
    </div>
</script>

$("#run-raw").on("click", function() {
    document.getElementById("raw").innerHTML = "";
    var data = _buildData(),
        date = new Date(),
        template = $("#raw-template").html(),
        html = "";

    for (var i = 0; i < data.length; i++) {
        var render = template;
        render = render.replace("{{className}}", "");
        render = render.replace("{{label}}", data[i].label);
        html += render;
    }

    document.getElementById("raw").innerHTML = html;

    $("#raw").on("click", ".test-data span", function() {
        $("#raw .selected").removeClass("selected");
        $(this).addClass("selected");
    });

    $("#run-raw").text((new Date() - date) + " ms");
});

To get a comparison of the results, I created a standard page that runs each of the following pieces of code: 

Standard Html Page Code
<html ng-app="test" class="ng-scope" hola_ext_inject="disabled"><head><style type="text/css">@charset "UTF-8";[ng\:cloak],[ng-cloak],[data-ng-cloak],[x-ng-cloak],.ng-cloak,.x-ng-cloak,.ng-hide:not(.ng-hide-animate){display:none !important;}ng\:form{display:block;}</style>
        <title>Performance Comparison for Angular and React</title>
        <link href="//cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/3.3.1/css/bootstrap.css" rel="stylesheet">        

        <script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.3.3/angular.min.js"></script>
        <script type="text/javascript" src="//cdnjs.cloudflare.com/ajax/libs/react/0.12.1/react.js"></script>
        <script type="text/javascript">
            console.timeEnd("build");

            document.addEventListener("DOMContentLoaded", function() { 
                _react();
                _raw();
            });

            _angular();

            function _buildData(count) {
                count = count || 1000;

                var adjectives = ["pretty", "large", "big", "small", "tall", "short", "long", "handsome", "plain", "quaint", "clean", "elegant", "easy", "angry", "crazy", "helpful", "mushy", "odd", "unsightly", "adorable", "important", "inexpensive", "cheap", "expensive", "fancy"];
                var colours = ["red", "yellow", "blue", "green", "pink", "brown", "purple", "brown", "white", "black", "orange"];
                var nouns = ["table", "chair", "house", "bbq", "desk", "car", "pony", "cookie", "sandwich", "burger", "pizza", "mouse", "keyboard"];
                var data = [];
                for (var i = 0; i < count; i++)
                    data.push({id: i+1, label: adjectives[_random(adjectives.length)] + " " + colours[_random(colours.length)] + " " + nouns[_random(nouns.length)] });
                return data;
            }

            function _random(max) {
                return Math.round(Math.random()*1000)%max;
            }            

            function _angular(data) {
                
            }

            function _react() {
                
            }

            function _raw() {
                
            }

            ko.observableArray.fn.reset = function(values) {
                var array = this();
                this.valueWillMutate();
                ko.utils.arrayPushAll(array, values);
                this.valueHasMutated();
            };
        </script>
    </head>
    <body ng-controller="controller" class="ng-scope">
        <div class="container">
            <div class="row">
                <div class="col-md-12">
                    <h2>Performance Comparison for React, Angular</h2>
                </div>
            </div>

            <div class="col-md-3">
                <div class="row">
                    <div class="col-md-7">
                        <h3>React</h3>
                    </div>
                    <div class="col-md-5 text-right time" id="run-react">Run</div>
                </div>
                <div id="react"></div>
            </div>

            <div class="col-md-3">
                <div class="row">
                    <div class="col-md-7">
                        <h3>Angular</h3>
                    </div>
                    <div class="col-md-5 text-right time" id="run-angular" ng-click="run()">Run</div>
                </div>
                <div>
                    <!-- ngRepeat: item in data -->
                </div>
            </div>            

            <div class="col-md-3">
                <div class="row">
                    <div class="col-md-7">
                        <h3>Raw</h3>
                    </div>
                    <div class="col-md-5 text-right time" id="run-raw">Run</div>
                </div>
                <div id="raw"></div>
            </div>
        </div>

        <script type="text/html" id="raw-template">
            <div class="row">
                <div class="col-md-12 test-data">
                    <span class="{{className}}">{{label}}</span>
                </div>
            </div>
        </script>
    

</body></html>

This gives us, after running the code something like this:


But is just after one time. What if we ran this over many times, over many browsers. What would we get? The answer, like hopefully many things, is below in the ebook.

Figures
Using our test code, we saw an initial bump favoring React, but would this stand true given multiple tests. Looking at the graph for google chrome gives us a telling image:


It is clear that React is a head and shoulders above the others in terms of benchmarking, and although usually is only a few ms off the raw implementation, it sometimes beats it.

What about real time rendering of UI elements, is React faster there? Well, using our examples we can see how React performs. Adding a preformatted tag into the code for each run example, ie: `<pre> test </pre>`, you will see the Angular code flutter this pre tag on each implementation. The React code however seems aware almost of this formatted tag, and instead works around it. It updates everything else except that tag!

So the question then is why and how?

What’s so good about React:
For in that sleep of death may dreams come, when we have shuffled off this mortal coil must give us pause was uttered by Hamlet, and I think the React engineers took that to heart when building the framwork.

React has shuffled off from Javascript fatigue. It has made palsied developers into frenzied advocates. And the reason was seen above, it is fast and effiecien.

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


