# Hypermedia Systems, Chapter 1.1:<br>_Hypermedia: A Reintroduction_

### Hypermedia controls
> A **hypermedia control** is an element in a hypermedia that describes (or controls) some sort of interaction, often with a remote server, by encoding information about that interaction directly and completely within itself.

The only two hypermedia controls available in base HTML are the anchor element `<a>` and the `<form>` element. We consider these "hypermedia controls" because they contain information about the action they should perform encoded within themselves.

### Hypermedia controls vs JSON APIs
Consider the use of a typical JSON API in a web application. We request some information from the API, and it returns that information to us as text containing a set of key-value pairs.

But nowhere within this text are we told what we should *do* with the information it provides. Instead, we are relying on some sort of "side channel" - usually client-sided JavaScript, in order to figure out what to do with it. Without the JS, it's just dead text.

Now consider a hypermedia counterexample, where we make a request to the backend and receive some HTML containing an `<a>` anchor element in response. No client-sided script is needed in order to understand what to do with this response. Instead, the action is encoded within the HTML itself - clicking on the link will send us to another location. No script, just hypermedia.

### Hypermedia's resilience to API changes
In a typical Single Page Application, it is often the case that making an API change on the backend will necessitate changes to the frontend too. Changes to the backend can and often do break the frontend.

However, with a hypermedia approach, changing the API will simultaneously change the frontend, too. Think about it: our backend serves hypermedia, so by modifying the hypermedia that is served, we are simultaneously changing the backend and frontend. Pages may still break, but in general, will be far more resilient to API changes than in the SPA approach.

### Complexity budget
No matter a team's size or skill level, they will inevitably reach a point where the level of complexity becomes too great to manage.

One way to think of complexity is as a currency that can be "spent" on certain aspects of a project and "saved" on others. You simply cannot say "yes" to everything - you need to learn to say "no" in order to keep your project simple enough to be manageable.

Remember:
- Complexity tends to grow exponentially.
- Developers use abstractions and frameworks as tools to manage complexity. Ironically, this can and often does result in a project becoming far more complex.

> **intractable**, *adjective*\
> hard to control or deal with
