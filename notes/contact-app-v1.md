# Hypermedia Systems, Chapter 1

## Notes for `contact.app`, Web 1.0 Version

### Flask: making variables available within a template
```
return render_template("index.html", contacts=contacts_set)
```
We can pass variables in and use them to render our templates. In this example, we can pass in data about our contacts so that the template can display information about them, e.g. their first name, last name, phone number.

Note that within the template, we can also iterate through values. Here, instead of passing a single `Contact` to the template, we pass a list of them, and then use the templating language's iteration syntax to do something for each `Contact` in the list:

```
{% for contact in contacts %}
<tr>
    <td>{{ contact.first }}</td>
    <td>{{ contact.last }}</td>
    <td>{{ contact.phone }}</td>
    <td>{{ contact.email }}</td>
    <td><a href="/contacts/{{ contact.id }}/edit">Edit</a>
        <a href="/contacts/{{ contact.id }}">View</a></td>
</tr>
{% endfor %}
```

### Using templates intelligently
Here is `layout.html`, the basic template that all of the templates extend:
```
<html lang="en">
<head>
    <title>Contact App</title>
    <link rel="stylesheet" href="/static/site.css">
    <link rel="stylesheet" href="https://the.missing.style/v0.2.0/missing.min.css">
</head>
<body>
<main>
    <header>
        <h1>
            <all-caps>contact.app</all-caps>
            <sub-title>A Demo Contacts Application</sub-title>
        </h1>
    </header>
    {% block content %} {% endblock %}
</main>
</body>
</html>
```
Note that this template contains the basic elements that will be common to all or most pages: the header, the `h1` page heading, and the stylesheets that should be used on all pages.

Also note how the `content` block is defined. When we want to extend this `layout`, all we need to do is provide some `content`. To do so is simple:
```
{% extends 'layout.html' %}

{% block content %}

<form action="/contacts" method="get" class="tool-bar">
    <label for="search">Search Term</label>

<!-- (... the rest of the content we want to include ...) -->

{% endblock %}
```
Good templating reduces the amount of times you need to repeat yourself when building your pages.

### A button is all you need
Apparently, all that is needed to submit a `form` is a plain button.\
Just one of these at the end of your `form` will do:
```
<button>Save</button>
```
### Interesting using of semantic HTML elements
The example templates in `contact.app` use some elements I've never (ever) seen before, particularly `fieldset` and `legend`:
```
    <form action="/contacts/new" method="post">
        <fieldset>
            <legend>Contact Values</legend>
            <p>
                <label for="email">Email</label>
                <input name="email" id="email" type="email" placeholder="Email" value="{{ contact.email or '' }}">
                <span class="error">{{  contact.errors['email'] }}</span>
            </p>
            <p>
                <label for="first_name">First Name</label>
                <input name="first_name" id="first_name" type="text" placeholder="First Name" value="{{ contact.first or '' }}">
                <span class="error" {{ contact.errors['first'] }}></span>
            </p>
            <!-- (...) -->
```
Also notable is the "unconventional" usage of the `p` element. I've only ever seen this element used to contain text. However, looking at the documentation, `p` can actually be used to contain "any structural grouping of related content, such as images or form fields," as it is used here.

### What is a "flash"?
> A "flash" is a common feature in web frameworks that allows you to store a message that will be available on the *next* request, typically in a cookie or session store.
