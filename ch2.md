



TLDR => () { React };
Chapter 2 





Oskar Niburski
Software Engineer 
Hockeystick.co



York University
212644944


Table of Contents

Abstract ……………..……………..……………..……………..…………….……………..………….2
List of Figures ……..……………..……………..……………..……………..……………..………….3
List of Tables .……………..……………..………………..……………..……………....………….....3
List of Acronyms Used..……………..……………..……………..……………..……………..……..4
Introduction.……………..……………..……………..……………..……………..……………..…….5
Background……………..……………..……………..……………..……………..………………...….6
    A Not so Long History…..……………..………………..……………..………………..…....1
    The Comparison…..……………..………………..……………..………………..……………8
        React Code…..……………..………………..……………..………………..…………9
        Angular Code…..……………..………………..……………..………………..………9
        Raw Code…..……………..………………..……………..………………..…………..9
    Figure This…..……………..………………..……………..………………..……………..…...9
Discussion…..……………..………………..……………..………………..……………..……………9
    What Makes React Work?…..……………..………………..……………..………………...9
    Virtual DOM…..……………..………………..……………..………………..………………..9
    React Negatives…..……………..………………..……………..………………..……………9
Conclusion…..……………..………………..……………..………………..……………..………..10
References…..……………..………………..……………..………………..……………..………10
Appendix…..……………..………………..……………..………………..……………..………11
    





Abstract 

This is an excerpt from a larger book concerning the React framework, by Oskar Niburski. The aim of chapter two is to understand React’s history, how it compares to other frameworks, and some of its benefits as well as concerns and problems the framework has. Ultimately it hopes to give the developer a firm understanding of React to start developing with the framework. It does so by exploring the Virtual DOM, the tree like component interrelations, as well as a component’s lifecycle. It also does comparisons to show React’s speed and efficiency. It is important to note that because this excerpt comes from a larger book, it represents a small microcosm to the larger work. For the entire book, please contact Oskar Niburski at oskarniburski@gmail.com.
























List of Figures

2.1 Performance Comparison for React and Angular - page
2.2 React vs Angular and Raw - Chrome
2.3 React vs Angular and Raw - Safari
2.4 React vs Angular and Raw - Firefox
2.5 Component Lifecycle 
2.6 Virtual DOM
2.7 React Interplay

List of Tables

Appendix A.1 - Response Times for Chrome
Appendix A.2 - Response Times for Safari
Appendix A.3 - Response Times for Firefox

Acronyms Used

DOM - Document Object Model
HTML - Hyper Text Markup Language
JSX - JavaScript XML
UI - User Interface





CHAPTER 2
An Introduction



“For in that sleep of death what dreams may come
When we have shuffled off this mortal coil,
Must give us pause.” - Hamlet, Shakespeare





React has come a long way.

I don’t mean that React itself has changed immensely over time, but rather, it finally has come to the forefront of JavaScript frameworks. The mortal coil has been shuffled off, and now we must pause at how far JavaScript, and ultimately React has come.

You can built mobile native applications with in, either in React Native or ReApp, you can implement functional like frameworks like Redux, and you can build a responsive web application in a few hours. It is this versatility, with similar core concepts running through all of them, that makes React fairly easy to be understood.React was engineered to be easily understood by even the most beginning developer. And that especially means you.

This ease of development is why likely React is so popular. Despite some new ideas and terms that that haven’t been popular in the Javascript community, React opens up a new way to develop. These words and concepts are for example isomorphism, flux, redux, and jsx. But before we explain these terms, let’s look at the history of React a little bit more.
Background  - A Not So Long History

Javascript itself has gone through immense changes. From the ES7 protocols to being coded by Brendan Eich’s in ten days, it now powers the web.

Yet one main problem with the current state of the union for JavaScript is that the language itself isn’t the most elegant, and that the immense amount of frameworks around it cause huge developer paralysis. Should you learn Elm or Angular? Batman.js or Meteor? The list goes on and on.

Like any great framework, React had its part in developer fatigue and paralysis.. Back in 2013, the Facebook engineers had a problem of building large applications wherein the data would change constantly in the UI level. This is understandable as Facebook  has around 500K active users every minute. In one day, this amounts to nearly 700 million people online. This figure, interestingly, is also called the active last minute user count, and is a good metric for growth and infrastructure support for any development team. Nevertheless, because of Facebook’s unprecedented demands on their own systems they realized the general PHP application originally built would not work. Their UI was too slow, the users were not getting updated, and as such cute gifs and updates about your cousin’s failing marriage were lost. Thus the Facebook engineers had a unique problem, how do we update the user's UI (or the document object model, DOM) of the user effectively and at scale. Better yet, how do we make this process fast?

At that time though there were only a few solutions around. Knockout.js, Angular, Meteor, and even Ember were beginning and Angular by far and large was the greatest contender for Facebook. One problem with Angular, according to Facebook, was that Google made it. The second problem was that Angular still relied on the DOM. Instead of updating the code in some precompile step, it instead inserts its code right into the HTML elements.  

As such, the engineers sought to build their own framework outside of the DOM, and React was ultimately born. Since then, React has quickly grown to outpace even Angular, with over 36,456 stars on Github, it has an avid open source community (which I myself am part of). React has created major development advances from React Native to Redux, as well as React Router for handling routes in the application view layer solely. Clearly React is here to stay and thus worthy of exploration.

The Comparison

Before we actually go into the why of React, I think it is best to go into the why not others when compared to React. In order to prove the efficiency of React, let’s use some sample React and Angular code to compare these frameworks.

The test itself will be a matter of implementing many UI elements into one page and then re-rendering them. We will also explore the re-rendering process in terms of structured elements. 

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


That to set up binding in React, you have to force it via a this.select.bind(null, this…). This sets up the component to have quasi two-directional binding. The other parts of the code will be explained in detail later!

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

Fairly simple Angular code. Notice the $$postDigest, which has a good explanation by the guys at code mentor (refer to the footnote 6). Important to note is that Angular 2.0 is much more modular and composable than the above.

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

    $("#run-raw").text((new Date() - date) + " ms");});

This will act as a baseline to compare the rendering of the components. Unfortunately, the raw is not fully fledged in terms of interactivity, but does provide a great starting point at understanding the strengths of JS frameworks.

The comparison is rendered via a webpage, seen in Figure 2.1 below. If we were to run each one of these buttons once, we would see something like this:

Figure 2.1 Source: Oskar Niburski

But of course, one time running isn’t indicative of anything. And what’s more, we have to test on many browsers! So what if we ran this over many times, over many browsers? What would we get? The answer, like hopefully many things, is below in the ebook.

Figure This

Using our test code, we saw after one time, the speed favored React, but would this stand true given multiple tests. Looking at the graph for chrome gives us a telling image:

Figure 2.2 Source: Oskar Niburski

Looking at the comparison for other browsers you see a similar trend:

Figure 2.3 Source: Oskar Niburski               Figure 2.4 Source: Oskar Niburski

It is clear that React is a head and shoulders above the others in terms of benchmarking, and although usually is only a few ms off the raw implementation, it sometimes beats it.

Comparing the two in terms of download size, despite React’s speed, it is a much larger file size as a whole (~1.8MB). Angular is only 50KB. Yet if we only use the View layer in React, and not the entire application, it’s size is much more reasonable at 26KB.

What about real time rendering of UI elements, is React faster? Using our examples we can see how React performs. Adding a preformatted tag into the code, at the very top of the html elements, allows us to test each run’s example of how they actually load the individual and changing components. With Angular you will see flutter this pre tag on each implementation. The React code however seems aware almost of this formatted tag, and instead works around it. It updates everything else except that tag!

This can be explained perhaps in a diagram outlining the lifecycle of a React component.
Figure 2.5 Source: Oskar Niburski

In this diagram, you see that once a component is rendered, it is mounted to the DOM. This basically means it is placed in the DOM, with the Virtual DOM tracking the said mounted component. The render stage gives back HTML markup, and the cycle continues as the elements on the page changes. In Angular, there is no difference tracking explicitly, therefore, it must run the rerender for all components.

So the question then becomes how is React doing what it is doing?

Discussion - What Makes React Work?

React has created and simultaneously ended Javascript fatigue. It has made palsied developers into frenzied advocates. And the reason was seen above, it is fast and efficient.

But why is it so fast. Let’s go into some of its features then.

React has great production experience: Unlike other frameworks like Elm, Meteor, or Ember, React is used by billions of people every year by Facebook. It is tested and has a huge community behind it, constantly optimizing code. Like Postgresql did to mysql, I imagine React will do to other frameworks because of its sheer use by other developers.

React is Pluggable: Basically React can be an MVC or MVVM framework, but it often times is only a view layer. Therefore it can be integrated into any app by just targeting this View layer for React. Such a feature is advantageous, that because React is developer agnostic, it is able to be used in any part of an application!

Composable: React uses a tree like structure (refer to the figure below) to refer to its individual components. Each component has a reference to another component, and thus creates a hierarchical structure with many leafs and branches. This interlocking of components allows React to reuse its own components, and thus makes for a modular and dynamic developer environment. Furthermore, this structure allows it to build out a virtual DOM that represents each of these components

Non Trashing UIs: React builds out the virtual DOM in such a way it knows exactly what to build and where. This allows for smooth UI change, without any odd bubble effects or unforeseen consequences like those seen in Angular or Ember.

JSX: Angular and Ember load their JavaScript into the DOM itself. This can be seen in my example code for testing the speed of the framework. This works great perhaps for small apps, but leads to many errors. Because HTML is not a compiled language, and itself is just markdown, it becomes difficult to track errors. There is no compile process to see if your Angular code is itself accurate, and if there are bugs in it. JSX was introduced by the engineering team to compile JavaScript into HTML elements, and thus track errors in your code (for example, orphaned HTML tags, in correct returns, improper property types for elements). This is a great advantage for React, as it allows for a more controlled developer environment and allows JavaScript to do what it does best, which is dynamic control, and the HTML serve only as document structure afterwards.

Overall, React has many positives that many frameworks do not have. Let’s look with more detail how it implements these features.
Virtual DOM


Figure 2.6 Source:  http://techblog.constantcontact.com/software-development/reactive-component-based-uis/

Looking at the above image is telling. React’s virtual DOM updating looks very similar to how Git determines differences in code. Some have made the comparison of React’s virtual DOM to the legendary SVN. Nevertheless, an explanation is necessary. 

Put simply, React sets up key attributes to each element in the virtual DOM. It tracks these IDs and keys. Once a change is made to one of these elements (it’s components are rendered in a tree like structure), the state of that component is active, or ‘dirty’ using web terminology. React notices this, because it has constructed a virtual DOM that has a diff tracker for all the DOM changes. Once one is fired off, it rerenders those elements that are affected.

This re-rendering process explains React’s speed. Because it only renders what is necessary, it doesn’t have to re-render the entire DOM unlike Angular. Because Angular isn’t the smartest at tracking what changed in the UI element, it must change everything, and thus causes a slow down for larger applications.

This further explains the flickering of the `pre` tag example. Because Angular renders everything, the pre flag flickers as it is rendered during a change by the DOM. However, in the case of React you see that the virtual DOM never changes that element. Instead, it renders the elements around it, as the pre tag is not part of the React change in the virtual DOM. Once the DOM is rerendered, it spits out only what is necessary.

React Negatives

Yet React is not without its negatives. Here are a few notable criticisms:

Too much boilerplate: Looking at the Angular code and the pure React code, you see the Angularjs is much smaller than React’s. This is true. However, if you take into account Angular’s own need to rely on the DOM (and thus setting ng-attributes everywhere), they are much more comparable in size. Building in React though comes with many different boilerplating problems, especially with React Native or Flux. However, many libraries reduce this repetitive code. Lastly, in terms of boilerplate, once it is truly set up, development is incredibly quick!

No explicit two way binding: React is unidirectional, and only allows for one way to update the DOM. Referring to the image below, you see that the application notifies in one way, and the virtual DOM, as well as the DOM also updates in one way. In Angular, there is two way binding, that a change in the App immediately changes the DOM. However React is opinionated that unidirectional binding allows for more control of your application, and removes unintended consequences and cascades from your application’s UI.Figure 2.7

JSX: Now I argued above of JSX’s strengths, but it does come with many weaknesses. JSX cares more about the separation of concerns rather than the separation of technologies. All your styles, markup, and dynamic code fit into one file and is served into a structure. This was surprising and rejected by many developers at first. It becomes a battle of JS in HTML vs HTML in JS. However I submit that when using JSX, debugging becomes a breeze because you know where the error occurs. With JS in HTML, because there is no explicit interface and HTML is not strictly parsed like JS, it becomes a whole lot messier to debug.
Conclusions - A Framework like a Picture

Ultimately, React is a choice like any JS framework. Although I think it has a lot of strengths, it ultimately depends on your own comfort as a developer and your want to build with it. Any engineering problem is for the most part possible, React in my opinion just makes it a whole lot easier. 

With a little bit of React's watercooler moments under our belt, let's go to actually coding something. Chapter three and onwards!

References 

Appendices
