Note: This is an excerpt from an upcoming ebook, Flux, Redux, and a React Must: React the Native Book, by Oskar Niburski.
For in that sleep of death may dreams come, when we have shuffled off this mortal coil must give us pause.
Chapter 2

React has come a long way.React is a doo0zy.

I don’t mean that React itself has changed immensely overtime, but rather, it finally has come to the forefront of JavaScript frameworks. You can built mobile native applications with in, either in React Native or Reapp, you can implement functional like frameworks like Redux, and you can build a responsive web application in a few hours. It is this versatility, with similiar core concepts running through all of them, that makes React fairly easy to be understood. the ideas around React are difficult. In fact, quite the opposite. React was engineered to be easily understood by even the most beginning developer. And that especially means you.

This ease of development is why likely React is so popular. Despite some new ideas and terms that React introduces a few ideas however that haven’t been popular in the Javascript community, React opens up a new way to develop until only recently. These Such words and concepts are for example are isomorphism, flux, redux, and jsx. But bBefore we explain these terms, let’s look at the history of React a little bit more.

A not so long history

Javascript itself has gone through immense changes. From the ES7 protocols to being coded in Brenden Eich’s house over a weekend, it now powers the web.

Yet one main problem with the current state of the union for JavaScript is that the language itself isn’t the most elegant, and that the immense amount of frameworks around it cause huge developer paralysis. Should you learn Elm or Angular? Batman.js or Meteor? The list goes on and on.

Like any great framework, React it had its part in developer fatigue and paralysis. had auspicious beginnings. Back in 2013, (wow how fast the world of Javascript is), the Facebook engineers had a problem of building large applications wherein the data would change constantly in the UI level. This is understandable as Facebook  has around 500K (https://www.quora.com/What-is-the-average-number-of-concurrent-users-on-Facebook) active users every minute. In one day, this amounts to nearly 700 million people online. This figure, interestingly, is also called the active last minute user count, and is a good metric fror growth and infrastructure support for any development team. Nevertheless, because of Facebook’s unprecedented demands on their own systems they realized the general PHP application originally built would not work. Their UI was too slow, the users were not getting updated, and as such cute gifs and updates about your cousin’s failing marriage were lost. Thus the Facebook engineers had a unique problem, how do we update the user's UI (or the document object model, DOM) of the user effectively and at scale. And Bbetter yet, how do we make this process fast?

At that time though there were only a few solutions around. Knockout.js, Angular, Meteor, and even Ember were beginning and Angular by far and large was the greatest contender for Facebook. One problem with Angular, according to Facebook, was that Google made it. The second problem was that Angular still relied on the DOM. Instead of updating the code in some precompile step, it instead inserts its code right into the HTML elements. At that time though there were only a few solutions around. Knockout.js, Angular, Meteor, and even Ember were beginning and Angular by far and large was the greatest contender for Facebook. One problem with Angular, acAngular, a great framework in its own right, however still relied heavily on the DOM. Instead of updating the code in some precompile step, it instead inserts its code right into the HTML elements. 

As such, the engineers sought to build their own framework outside of the DOM, and React was ultimately born. Since then, React (https://github.com/facebook/react) has quickly grown to outpace even Angular, with over 36,456 stars on Github, it has an avid opensource community (which I myself am part of!) React has offshooted major development advances from React Native to Redux, as well as React Router for handling routes. Clearly React is here to stay and thus worthy of exploration., alongside it’s great native development project, React Native.

A Comparison

Before we actually go into the why of React, I think it is best to go into the why not others when compared to React. In order to prove the efficiency of React, let’s use some sample React and Angular code to compare these frameworks. (Inspired by https://www.codementor.io/reactjs/tutorial/reactjs-vs-angular-js-performance-comparison-knockout)
The test itself will be a matter of implementing many UI elements into one page and then rerendering them. We will also explore the rerendering process in terms of structured elements. via a TODO app. The code for each todo app will be seen below.

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

If we were to run each one of these buttons once, we would see something like this:This gives us, after running the code something like this:


But of course, one time running isn’t indicative of anything. And what’s more, we have to test on many browsers!But is just after one time. So wWhat if we ran this over many times, over many browsers?. What would we get? The answer, like hopefully many things, is below in the ebook.

Figures
Using our test code, we saw after one time, the speedan initial bump favoreding React, but would this stand true given multiple tests. Looking at the graph for google chrome gives us a telling image:

Below you see graphs for other browsers, as well as a comparison of the various size of the repos for Angular and React. The comparison of the two rep

It is clear that React is a head and shoulders above the others in terms of benchmarking, and although usually is only a few ms off the raw implementation, it sometimes beats it.

//More graphs

Comparing the two in terms of download size, despite React’s speed, it is a much larger file size as a whole (~1.8MB). Angular is only 50KB. (https://docs.angularjs.org/misc/faq). However, if we only use the View layer in React, and not the entire application, it’s size is much more reasonable at 26KB. (https://blog.liip.ch/archive/2014/09/16/angularjs-vs-reactjs-for-large-web-applications.html)

What about real time rendering of UI elements, is React faster there? Well, using our examples we can see how React performs. Adding a preformatted tag into the code, at the very top of the html elements, allows us to test each run’s example of how they actually load the individual and changing components. With Angular for each run example, ie: `<pre> test </pre>`, you will seeee the Angular code flutter this pre tag on each implementation. The React code however seems aware almost of this formatted tag, and instead works around it. It updates everything else except that tag!
// images

So the question then becomesis how is React doing what it is doing?why and how?

What’s so good about React:
For in that sleep of death may dreams come, when we have shuffled off this mortal coil must give us pause was uttered by Hamlet, and I think the React engineers took that to heart when building the framework.

React has created and simultaneously endedshuffled off from Javascript fatigue. It has made palsied developers into frenzied advocates. And the reason was seen above, it is fast and efficient.

But why is it so fast. Let’s go into some of its features then.

React has great production experience: Unlike other frameworks like Elm, Meteor, or Ember, React is used by billions of people every year by Facebook. It is tested and has a huge community behind it, constantly optomizing code. Like Postgresql did to mysql, I imagine React will do to other frameworks because of its sheer use by other developers.Here is a comprehensive breakdown of some of React’s speed explanations:
React is Pluggable: Basically React can be an MVC or MVVM framework, but it often times is only a view layer. Therefore it can be integrated into any app by just targetting this View layer for React. Such a feature is advantageous, that because React is developer agnostic, it is able to be used in any part of an application!
Composable: React uses a tree like structure (refer to the figure below) to refer to its individual components. Each component has a reference to another component, and thus creates a hierarchical structure with many leafs and branches. This interlocking of components allows React to reuse its own components, and thus makes for a modular and dynamic developer environment. Furthermore, this structure allows it to build out a virtual DOM that represents each of these components
Non Trashing UIs: React builds out the virtual DOM in such a way it knows exactly what to build and where. This allows for smooth UI change, without any odd bubble effects or unforeseen consequences like those seen in Angular or Ember.
JSX: Angular and Ember load their JavaScript into the DOM itself. This can be seen in my example code for testing the speed of the framework. This works great perhaps for small apps, but leads to many errors. Because HTML is not a compiled language, and itself is just markdown, it becomes difficult to track errors. There is no compile process to see if your Angular code is itself accurate, and if there are bugs in it. JSX was introduced by the engineering team to compile JavaScript into HTML elements, and thus track errors in your code (for example, orphaned HTML tags, in correct returns, improper property types for elements). This is a great advantage for React, as it allows for a more controlled developer environment and allows JavaScript to do what it does best, which is dynamic control, and the HTML serve only as document structure afterwards.changes afterwards
- it has a great track record: Facebook, one of the largest sites, uses it
- pluggable: it is literally just a view layer so it can be integrated in any old app nicely. Although in most realities you want React to act more as a View-Controller layer, for the most part, it acts as a View only changing the UI elements once underlying data changes
- fast: It only changes exactly what needs to be, and the changes are not linked up unnecessarily like in Angular
- composable: it, like many hot off the press JS frameworks, believes and rewards reusable components
- non thrashing UIs: because of its intimate virtual DOM, React knows exactly what to change and where, and thus provides for a smooth UI change without any great complexities like difficult bubbling errors in Angular or Ember.

Further Explanation: How does react know what to update?


Looking at the above image is telling. React’s virtual DOM updating looks very similar to how Git determines differences in code. Some have made the comparison of React’s virtual DOM to the legendary SVN. Nevertheless, an explanation is necessary. 

Put simply, React sets up key attributes to each element in the virtual DOM. It tracks these IDs and keysideas. Once a change is made to one of these elements (it’s components are rendered in a tree like structure), the state of that component is active, or ‘dirty’ using web terminology. React notices this, because it has constructedconstructued a virtual DOM that has a diff tracker for all the DOM changes. Once one is fired off, it rerendersrerenderes those elements that are affected.

This re-rendering process explains React’s speed. Because it only renders what is necessary, it doesn’t have to re-render the entire DOM unlike AnNgular. Because Angular isn’t the smartest at tracking what changed in the UI element, it must change everythingchanges everything, and thus causes a slow down for larger applications..

This further explains the flickering of the `pre` tag example. Because Angular renders everything, the pre flag flickers as it is rendered once again during a change by the DOM. However, in the case of React you see that the virtual DOM never changes that element. Instead, it renders the elements around it, as the pre tag is not part of the React change in the virtual DOM. Once the DOM is rerendered, it spits out only what is necessary.

React Negatives
Yet React is not without its negatives. Here are a few notable criticisms:
Too much boilerplate: Looking at the Angular code and the pure React code, you see the Angular JS is much smaller than React’s. This is true. However, if you take into account Angular’s own need to rely on the DOM (and thus setting ng-attributes everywhere), they are much more comparable in size. Building in React though comes with many different boilerplating problems, especially with React Native or Flux. However, many libraries reduce this repetitive code. Lastly, in terms of boilerplate, once it is truly set up, development is incredibly quick!
No explicit two way binding: React is unidirectional, and only allows for one way to update the DOM. Referring to the image below, you see that the application notifies in one way, and the virtual DOM, as well as the DOM also updates in one way. In Angular, there is two way binding, that a change in the App immediately changes the DOM. However React is opinionated that unidirectional binding allows for more control of your application, and removes unintended consequences and cascades from your application’s UI.
JSX: Now I argued above of JSX’s strengths, but it does come with many weaknesses. The separation of concerns, JS in HTML vs HTML in JS, is immediately throw out the window when developing with JSX. However I submit that when using JSX, debugging becomes a breeze because you know where the error occurs. With JS in HTML, because there is no explicit interface and HTML is not strictly parsed like JS, it becomes a whole lot messier to debug.

- too much boilerplate: This is true. However, you can use clever design patterns to get around this!
- no two way binding: This is true, however there are good reasons for this. Angular or other two way binded frameworks often are unpredictable, in that one model change triggers a cascading effect for these model changes, this causes some debugging
- JSX: Now I would argue compiling HTML to Javascript makes it easier to read and write. However, unlike Angular, where you throw Javascript into the DOM (ie: ng-repeat), JSX makes you throw the HTML into a JS file which is then compiled into regular vanilla JS. This to me is often more readable and offers more strength in development. The separation of concerns, JS in HTML vs HTML in JS, can be easily answered by observing when using JSX, debugging becomes a breeze because you know where the error occurs. With JS in HTML, because there is no explicit interface and HTML is not strictly parsed like JS, it becomes a whole lot messier to debug.

Yet these negatives I often believe are not necessarily negatives.

Ultimately, React is a choice like any JS framework. Although I think it has a lot of strengths, it ultimately depends on your own comfort as a developer and your want to build with it. Any engineering problem is for the most part possible, React in my opinion just makes it a whole lot easier. 

But what about Redux, Flux!

These terms are just buzzwords, and not actually React. They are implementations of React, but quickly, flux is a unidirectional organizational pattern for using React in applications. Redux, inspired by frameworks like Elm, is another functional organizational framework, based on state-like programs.

With a little bit of React's watercooler moments under our belt, let's go to actually coding something. Chapter three and onwards!


