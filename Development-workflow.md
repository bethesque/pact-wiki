# Development Workflow

Using consumer driven contracts enables you to reverse the "normal" order of development, allowing you to build your consumer, in its entirety if need be, before you build your provider.

The development process will be different for every organisation, but this is one that has worked for the pact authors.

## Initial development
1. Write consumer tests with pact
1. Implement consumer 
1. Add `pact:publish` task to consumer build or publish pact as CI artifact
1. Create provider project
1. Configure pact:verify task to point to latest published pact
1. Implement provider until `pact:verify` passes

## New features

1. Consumer - Add new consumer feature, with pact specs, on a feature branch.
1. Consumer - Talk to the provider team about the new functionality that is required, and give them access to the pact with the new features.
1. Provider - In the provider project, use `rake pact:verify:at[/path/to/pact/on/branch]` to verify the new pact.
1. Provider - Develop and release new provider feature.
1. Consumer - Merge feature branch into main development branch, and publish new pact.

This may seem complex, but it is actually surfacing the underlying reality, that 1. you should not be committing functionality to the master branch of the consumer before it can be supported by the provider and 2. that the functionality of the provider should still be driven by the needs of the consumer.