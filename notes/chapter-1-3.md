# Hypermedia Systems, Chapter 1.3:<br>_A Web 1.0 Application_

### Common approach to search results pages in Web 1.0 applications
> This is a common approach in web 1.0 style applications: the same URL that displays all instances of some resource also serves as the search results page for those resources. Taking this approach makes it easy to reuse the list display that is common to both types of request.

In the case of `contact.app`, the `/contacts` path is used to view all contacts, and the same path with a search query, e.g. in `/contacts?q=joe` is used to view all contacts that match the query.

### Query strings
Part of the URL specification, a query string is a key-value pair after the `?` in a URL, e.g. `https://example.com/contacts?q=joe`.

In this URL, the *query string* is `q=joe`, the *key* or *parameter name* is `q`, and the *value* or *parameter value* is `joe`.

URLs are not limited to containing only one parameter - they can contain multiple.

### Thinking with hypermedia
> Notice that our template, when rendered, provides all the functionality necessary to see all the contacts and search them, and also provides links to edit them, view details of them or even create a new one.
> 
> And it does all this **without the client (that is, the browser) knowing a thing about what contacts are or how to work with them.** Everything is encoded *in* the hypermedia. A web browser accessing this application just knows how to issue HTTP requests and then render HTML, **nothing more about the specifics of our application's end points or underlying domain model.**

### Web development patterns: contacts as resources
> This is a common pattern in web development: contacts are treated as resources and the URLs around these resources are organized in a coherent manner.
> 
> - If you wish to view all contacts, you issue a GET to /contacts.
> - If you wish want a hypermedia representation allowing you to create a new contact, you issue a GET to contacts/new.
> - If you wish to view a specific contact (with, say, and id of 42), you issue a GET to /contacts/42.

The text advises that you should pick a URL design scheme that is **resource-oriented.**

### Factoring code: client vs server-side
> Note that factoring on the server-side tends to be coarser-grained than on the client-side: you tend to split out common *sections* rather than create lots of individual components. This has benefits (it tends to be simple) as well as drawbacks (it is not nearly as isolated as client-side components).

(Not really sure what this means yet, but seemed worth writing down)
