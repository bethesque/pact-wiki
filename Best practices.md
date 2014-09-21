# Pact best practices

## In your consumer project

#### Make the latest pact available to the provider via a URL

See [Sharing pacts between consumer and provider](Sharing-pacts-between-consumer-and-provider) for options to implement this.

#### Ensure all calls to the provider go through methods that have been tested with Pact

Do not hand create any HTTP requests directly in your consumer app. Testing through a client class gives you the assurance that your consumer app will be creating exactly the HTTP requests that you think it should.

#### Ensure the models you stub with are valid

Sure, you've checked that your client deserialises the HTTP response into the object you expect, but then you need to make sure in your other tests where you stub your client that you're stubbing it with a valid object (eg. is `time` a Time or a DateTime?). One way to do this is to use factories or fixtures to create the models for all your tests. See this [gist](https://gist.github.com/bethesque/69ae590e8312523e5337) for a more detailed explanation.

## In your provider project

#### Ensure that the latest pact is being verified

Use a URL that you know the latest pact will be made available at. Do not rely on manual intervention (eg. someone copying a file across to the provider project) because this process will _inevitably_ break down, and your verification task will give you a false positive. Do not try to "protect" your build from being broken by instigating a manual pact update process. `pact:verify` is the canary of your integration - manual updates would be like giving your canary a gas mask.

#### Ensure that pact:verify runs as part of your CI build

It should run with all your other tests.

#### Only stub layers beneath where contents of the request body are extracted

If you don't _have_ to stub anything in the provider when running pact:verify, then don't. If you do need to stub something, make sure that you only stub the code that gets executed _after_ the contents of the request body have been extracted and/or validated. Otherwise, you could send any old garbage in a `POST` or `PUT` body, and your provider wouldn't even notice.

#### Stub calls to downstream systems

Consider making a separate pact with the downstream system and using shared fixtures.
