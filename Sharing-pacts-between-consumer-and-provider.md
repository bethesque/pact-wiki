You want to ensure that the provider has access to the latest version of the pact at all times. There are a few ways you can do this, depending on your requirements.

#### 1. Consumer CI build commits pact to provider codebase

Pretty self explanatory.

#### 2. Publish pacts as CI build artefacts

Work out the URL to the pact created by the most recent successful build, and configure the `pact:verify` task to point to this URL.

#### 3. Use Github/Bitbucket URL

This only works for repositories that don't require authentication to read. Make sure that you always regenerate the pacts before committing if you make any changes to the pact specs.

#### 4. Use a Pact Broker

The [Pact Broker][pact-broker] is a repository for sharing pacts between projects. It also provides:
* Auto-generated documentation
* Dynamically generated network diagrams
* The ability to tag a pact (ie. "prod") so a provider can verify itself against a fixed version of a pact to ensure backwards compatibility.
* Webhooks to trigger provider builds when a pact is published.

If you want to decouple the release cycle of your consumer and provider, and be able to cross test the head/prod versions of each, then the Pact Broker is the best solution for you.

[pact-broker]: https://github.com/bethesque/pact_broker
