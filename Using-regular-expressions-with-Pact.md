Sometimes you will have keys in a request or response with values that are hard to know beforehand - timestamps and generated IDs are two examples. One way to handle this problem in the Consumer tests is to use Pact to unit test the client class (the one responsible for handling the HTTP conversation with the provider) only, and not use it for integration tests where times and IDs are hard to control.

However, this is not always practical, and really, there should be a way to say "I expect something matching this regular expression, but I don't care what the actual value is". If you are using the Ruby implementation of Pact for both the Consumer and the Provider, then you are in luck! \**

```ruby
animal_service.given("an alligator named Mary exists").
  upon_receiving("a request for an alligator").
  with(
    path: "/alligators/Mary", 
    headers: {"Accept" => "application/json"}).
  will_respond_with(
    status: 200,
    headers: {"Content-Type" => "application/json"},
    body: {
      name: "Mary",
      dateOfBirth: Pact::Term.new(
        generate: "02/11/2013", 
        matcher: /\d{2}\/\d{2}\/\d{4}/)
    })
```

Note the use of the `Pact::Term`. When you run the Consumer tests, the mock server will return the value that you specified to "generate", and when you verify the pact in the Provider codebase, it will ensure that the value matches the specified regular expression.

You can also use `Pact::Term` for request matching.

```ruby
animal_service.given("an alligator named Mary exists").
  upon_receiving("a request for an alligator").
  with(
    path: "/alligators/Mary", 
    query: Pact::Term.new(generate: "transactionId=1234", matcher: /transactionId=\d{4}/),
  will_respond_with(
    status: 200, ...)
```

The `matcher` will be used to ensure that the actual request query was in the right format, and the value specified in the `generate` field will be the one that is replayed against the provider as an example of what a real value would look like. This means that your provider states can still use known values to set up their data, but your Consumer tests can generate data on the fly.

You can use `Pact::Term` for request and response header values, the request query, and inside request and response bodies.

\** (Unfortunately, this technique involves serialising Ruby specific JSON, so it can't be used with any of the other Pact implementations. Hang around for v2 of the [Pact Specification](https://github.com/bethesque/pact-specification) for cross language regular expressions.)