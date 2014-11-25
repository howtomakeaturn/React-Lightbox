React-Lightbox
==============

React Lightbox is a React plugin for implementing lightbox effect easily with React.

# Features
* Turn on/off the lightbox (openLightbox, closeLightbox)
* Communicate variables between different elements in the lightbox (setLightboxState).

# Demo
[Basic usage](http://howtomakeaturn.github.io/react-lightbox/basic.html)

[Passing variable](http://howtomakeaturn.github.io/react-lightbox/pass-variable.html)

[Custom elements](http://howtomakeaturn.github.io/react-lightbox/custom-element.html)

[Communication between elements](http://howtomakeaturn.github.io/react-lightbox/communication-between-elements.html)

# Getting started

To use the lightbox plugin, include the react library, the JSX transformer, and the react-lightbox library inside the tag of your HTML document:

```html
<head>
    <script src='//cdnjs.cloudflare.com/ajax/libs/react/0.12.0/JSXTransformer.js'></script>
    <script src='//cdnjs.cloudflare.com/ajax/libs/react/0.12.0/react.js'></script>
    <script type="text/jsx" src='react-lightbox.jsx'></script>    
</head>
```

Start by telling react-lightbox to render the elements you want when the document is loaded.
Just put them inside the LightboxTrigger element and LightboxModal element:

```html
<body>
    <div id='react-canvas'></div>
    <script type="text/jsx">
        /** @jsx React.DOM */
        
        React.renderComponent(
            <Lightbox>
                <LightboxTrigger>
                    <a href='#'>Click me to open!</a>
                </LightboxTrigger>
                <LightboxModal>
                    <div>
                        <h1>This is the basic usage!</h1>
                        <p>Good luck :D</p>
                    </div>
                </LightboxModal>
            </Lightbox>,                
            document.getElementById('react-canvas')
        );
    </script>
</body>
```
We done! That’s all you need to do!
Click on the text and you’ll see the beautiful lightbox popup on your browser!
See the [Basic usage](http://howtomakeaturn.github.io/react-lightbox/basic.html).

*NOTE! react-lightbox will auto-bind openLightbox function to the onClick event of the element inside the LightboxTrigger. You don’t need to set the onClick event yourself.*

If you think simply use ‘a’ tag as trigger and ‘p’ tag as the content of lightbox is too simple, you can create custom elements for both LightboxTrigger and LightboxModal:

```html
<body>
    <div id='react-canvas'></div>
    <script type="text/jsx">
        /** @jsx React.DOM */
        var ToggleButton = React.createClass({
            render: function(){
                return(<button onClick={this.props.openLightbox}>Open Lightbox</button>);
            }
        });
        
        var MyPanel = React.createClass({            
            onClickSave: function(){
                alert('saved!');
                this.props.closeLightbox();
            },            
            render: function(){
                return (
                    <div>
                        <h3>My Panel</h3>
                        <hr />
                        <textarea placeholder='Type something here...'></textarea>
                        <hr />
                        <button onClick={this.onClickSave}>Save</button>
                    </div>                
                );
            }
        });                
        React.renderComponent(
            <Lightbox>
                <LightboxTrigger>
                    <ToggleButton />
                </LightboxTrigger>
                <LightboxModal>
                    <MyPanel />
                </LightboxModal>
            </Lightbox>,                
            document.getElementById('react-canvas')
        );
    </script>
</body>
```
Your custom elements inside LightboxTrigger and LightboxModal will receive ‘openLightbox’ and ‘closeLightbox’ functions automatically. Use them in whatever way you want.

The live example for above code is here: [Custom elements](http://howtomakeaturn.github.io/react-lightbox/custom-element.html).



*NOTE! If you use custom element as trigger, react-lightbox will NOT auto-bind openLightbox for you. Remember to bind it as the example shows.*

# Advanced

Sometimes you need to share variables between elements inside the lightbox. For instance, you want the change the text in the trigger according to the text user type into the textarea in modal. How can we deal with that?

React-lightbox provide ‘setLightboxState’ function to solve this.
First, pass the values you want to share between elements to the Lightbox element ‘data’ props:

```
<Lightbox data={{ content: ''}}>
```

Then you can use this values from the props:
```
<textarea ref='note' defaultValue={this.props.content} placeholder='Type something here...'></textarea>
```

To change the shared value, use the ‘setLightboxState’ function which all children elements will receive automatically.

What happened behind it is easy. Lightbox set the ‘data’ object as state. And then pass every value inside the state to all children as props! So every time you ‘setLightboxState’, all the elements will update automatically!

Here’s a more concrete example:

```
<body>
    <div id='react-canvas'></div>
    <script type="text/jsx">
        /** @jsx React.DOM */
        var ToggleText = React.createClass({                        
            render: function() {
                var text = this.props.content.trim() ? 'View Notes' : 'Add Notes';                
                if (this.props.content.trim()){
                    return ( <a style={{color: 'red'}} href='#' onClick={this.props.onClick}>{text}</a> );                                                            
                } else {
                    return ( <a href='#' onClick={this.props.openLightbox}>{text}</a>);                                                
                }                
            }
        });
 
        var NotePanel = React.createClass({
            
            clickSave: function(){
                var content = this.refs['note'].getDOMNode().value;
                this.props.setLightboxState({ content: content});
                // you may want to send ajax here
                //$.post('/ajax/document/update-note', data);
                this.props.closeLightbox();
            },
            
            render: function(){
                return (
                    <div>
                        <h3>Note</h3>
                        <hr />
                        <textarea ref='note' defaultValue={this.props.content} placeholder='Type something here...'></textarea>
                        <hr />
                        <button onClick={this.clickSave}>Save</button>        
                    </div>
                );
            }
        });
 
        React.renderComponent(
            <Lightbox data={{ content: ''}}>
                <LightboxTrigger>
                    <ToggleText />
                </LightboxTrigger>
                <LightboxModal>
                    <NotePanel />
                </LightboxModal>
            </Lightbox>,                
            document.getElementById('react-canvas')
        );
    </script>
</body>
```
A live demo for this can be found here:
[Communication between elements](http://howtomakeaturn.github.io/react-lightbox/communication-between-elements.html).


# Requirements

* React
* JSXTransformer


Or you can view the documentation on my blog:

http://blog.turn.tw/?page_id=1027
