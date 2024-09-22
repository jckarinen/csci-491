# Hypermedia Systems, Chapter 2.1:<br>_Extending HTML as Hypermedia_

## Limitations of HTML and opportunities for improvement

### The DOM
> The DOM is the internal model that a browser builds up when it processes HTML, forming a tree of "nodes" for the tags and other content in the HTML. The DOM provides a programmatic JavaScript API that allows you to update the nodes in a page directly, without the use of hypermedia. Using this API, JavaScript code can insert new content, or remove or update existing content, entirely outside the normal browser request mechanism.

### Common approach for building web apps with modern frameworks
A JavaScript model of the document is created - then, when changes happen to the model, the DOM itself is automatically updated to reflect the change. This is the approach used by frameworks like React and Vue.

### What does a link do?
```
<a href="https://hypermedia.systems/">
  Hypermedia Systems
</a>
```
1. It renders the text within the `<a>` tag on the screen in a way that indicates it's clickable.
2. The user clicks on it.
3. The browser then sends an HTTP GET request to the URL specified in the `href` attribute.
4. The browser receives the response and replaces the current document with the HTML body contained in the response.

### Review of the types of HTTP requests
- GET: Gets a resource.
- POST: Creates or changes a resource.
- PUT: Updates or replaces a resource.
- PATCH: Partially updates a resource, but does not replace it.
- DELETE: Deletes a resource.

It is probably obvious, but remember that all of the HTTP requests describe an interaction with some resource.

### Four opportunities to improve HTML
Four things we might do to address the "clunkiness" and "legacy" feel of Web 1.0 apps and leverage the full power of HTTP and hypermedia:
1. Let any element send requests, not just anchors and forms.
2. Let any event trigger a request, not just a click.
3. Utilize all of the HTTP request types (GET, POST, PUT, PATCH, DELETE) instead of just two.
4. Enable replacement of *part* of the document instead of needing to replace the entire thing whenever we load a new resource, aka **"transclusion"** - the inclusion of content into an existing document via a hypermedia reference.

Htmx is a JavaScript framework that aims to exploit these four opportunities, allowing us to leverage the full power of hypermedia without relying on JavaScript to build rich and modern interactive applications.

## Implementing the improvements: htmx

### Addressing opportunities #1 and #3
With htmx we use the following attributes to specify the request type we want to issue:
- hx-get
- hx-post
- hx-put
- hx-patch
- hx-delete

These attributes can be used with any HTML element.

### Addressing opportunity #4: Transclusion
We use the `hx-target` attribute to specify the element to which we wish to direct the resulting HTML from our HTTP request. For example,
```
<div id="main"> 

  <button hx-get="/contacts" hx-target="#main"> 
    Get The Contacts
  </button>

</div>
```
In this example,
1. `hx-get` is used to enable the button to send a `GET` request when the button is clicked.
2. When the response is received, the response's HTML body will replace the content of the element with the ID `main`, i.e. the `main` `div` here.

The observable result is that clicking the button will replace the button (and any other content within `#main`) with a list of contacts.

### Transclusion continued: the hx-swap attribute
`hx-swap` lets you direct a request's HTML response to *replace* an element instead of replacing its content. Many values can be used to specify exactly where the content should be placed:
- `innerHTML`: (default) replace inner HTML of target (the content within the tag)
- `outerHTML`: replace entire target 
- `beforebegin`: insert before target
- `afterbegin`: insert before first child of target (so "insert as new first child")
- `beforeend`: insert after last child of target (so "insert as new last child")
- `afterend`: insert after target
- `delete`: delete target regardless of response
- `none`: don't swap, do nothing

Note that while that attribute is called `hx-swap`, some values don't actually result in the target being swapped, but instead insert the content before or after the target, or as a child of the target.

Here is an example of hx-swap:
```
<div id="main">

  <button hx-get="/contacts" hx-target="#main" hx-swap="outerHTML"> <1>
    Get The Contacts
  </button>

</div>
```
In this example,
1. Clicking the button will initiate a GET request to the resource at `/contacts`.
2. When the response is received, it will *replace* the element with id `#main`.

The difference between the `hx-target` example and this example is that, in the `hx-target` example, the *content* of `#main` is replace (due to the default `hx-swap` value of `innerHTML`), but in this example, the *entire element* `#main` is replaced.

### Reiterating the aim of htmx
With htmx, the intent is that you can deliver your entire application through hypermedia. This means that you shouldn't need to write any client-sided logic for your application to function on the client side - everything is rendered by your server and delivered via HTTP requests.

### Addressing opportunity #2: Allow any event to trigger requests
`hx-trigger` allows us to specify which events should trigger an element to fire its request. Some elements have default `hx-trigger` behaviors, like we saw earlier with a click triggering a button's `GET` request.

`hx-trigger` is evidently more elaborate than any of the other htmx attributes - this is because events are complicated.

Here is an example of `hx-trigger` in action:
```
<div id="main">

<button hx-get="/contacts" hx-target="#main" hx-swap="outerHTML"
hx-trigger="click, keyup[ctrlKey && key == 'l'] from:body"> <1>
Get The Contacts
</button>

</div>
```
In this example:
- Like the last example, the button will send a `GET` request when triggered that replaces `#main` with the response HTML content.
- The difference is that we now have multiple possible triggers for the request: `click`, or `keyup[ctrlKey && key == 'l']`. So not only will clicking the button trigger the `GET` request; so will pressing the keyboard shortcut `CTRL+L`.

### Breaking down the "keyboard shortcut" event
```
keyup[ctrlKey && key == 'l']
```
- `keyup`: the base event we're talking about
- `[ctrlKey && key == 'l']`: an "event filter" - a JavaScript expression that's evaluated when the event fires.
  - The event will be triggered if the expression within the square brackets returns true.

### But wait, it's still not perfect
Due to the way that events work in JavaScript, there is a glaring flaw with the "keyboard shortcut" portion of our `hx-trigger`: it will only work when the shortcut is typed "within" the element, i.e. when the user is tabbed onto the button.

Thankfully, htmx enables the ability to listen to other elements for events using the `from` modifier. The working result is as follows:
```
<div id="main">

  <button hx-get="/contacts" hx-target="#main" hx-swap="outerHTML"
    hx-trigger="click, keyup[ctrlKey && key == 'l'] from:body"> <1>
    Get The Contacts
  </button>

</div>
```
The only difference here is the `from:body` added at the end of the `keyup` event declaration. This will enable the event to occur when the user activates the keyboard shortcut in the `body`, so basically from anywhere in the document.

### Observation: Custom shortcuts
What if we wanted to provide the user with the ability to modify the keyboard shortcut?

One interesting way to address this problem would be to tweak the `hx-trigger` attribute of the `button` by serving modified hypertext to the user based on their established preferences. So instead of delivering a `button` with `keyup[ctrlKey && key == 'l']`, we could deliver a slightly modified button with `keyup[ctrlKey && key == 'm']`. The intriguing part is that the user's preferences are stored in the version of the document that we served them - a document we probably rendered using a combination of templating and data from our database.

### opportunities and the htmx aspects that address them
1. any element can make requests: hx-get, hx-post, hx-put, hx-patch, hx-delete
2. any event can trigger http requests: hx-trigger
3. can use all of the http actions in the spec: hx-put, hx-patch, hx-delete
4. dont need to reload entire page, can replace and modify parts: hx-target, hx-swap

### Opportunities: addressed
Here is a list of each "opportunity for improvement" htmx aims to address, and each attribute that addresses it:
1. Any element can make requests: `hx-get`, `hx-post`, `hx-put`, `hx-patch`, `hx-delete`
2. Any event can trigger requests: `hx-trigger`
3. All HTTP actions are available to use: `hx-put`, `hx-patch`, `hx-delete`
4. Transclusion: `hx-target`, `hx-swap`

Eight attributes in total.




































