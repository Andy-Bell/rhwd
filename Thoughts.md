# Thoughts on Rhwd

## "be ergonomic"

So this is the most subjective point in my plan, so I thought I would type out
some thoughts I had on it.

One of my major issues I have had with rust frameworks that I have tried is that
I felt I was fighting the compiler for relatively basic web operations. I am
unsure if this was due to me not getting how rust works, me not getting how the
web operations I was used to from dyn langs have to be changed to fit the borrow
checker, or the creator of the framework had a clear way of how to use it and I
was coming at it from a right angle, so bashing my head against a wall.

With Rhwd, I want a lot of the complexity to be abstracted away from the
individual working on the application. This will likely involve playing with
DSLs & macros to get the syntax working how I see it in my head.

Two main things I want: to be able to declare middleware stacks cleanly
(assuming they all follow a specific trait impl), and to have a tidy way for
declaring "restful" route file to handle routing.

My ideal routing would be something like this:

```
/// resource creating the 5 standard routes:
/// index - show - create - update - delete
/// with show/update/delete being scoped to
/// an id, but index/create being scoped to
/// the resource as a whole
resource!('/users', UsersController)

/// resource would also allow a third argument
/// that would allow for nesting further routes
/// under that resource.
resource!(
    '/books',
    BooksController,
    resource!(
        '/reviews',
        ReviewController
    )
)

/// individual routes would be allowed via
/// specifying the exact action call, so
/// get! post! patch! delete! would all be
/// supported, these would have three arguments
/// the uri, controller to use, and function
/// within the controller module.
get!('/my_books', BooksController, my_books)
```

And I would like middleware to do something like this:
```
middleware_chain!(
    request_processor,
    cors,
    auth,
    db_handler,
    router,
    json_handler,
    response_processor,
)
```

But am less clear on this so far, and the values I have given are just
placeholders for what I think might be needed
