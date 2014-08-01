# Random ideas
* Just because it's called "consumer driven contracts" doesn't mean that the team writing the consumer gets to tell the team writing the provider what to do! The pact is the starting point of a collaborative effort.
* The way pact works, it's the pact verification task (on in the provider codebase) that fails when a consumer expects things that are different from what a provider responds with, even if the consumer itself is "wrong". This is a little unfortunate, but it's the nature of the beast.
* Running the pact verification task in a separate CI build from the rest of the tests for the provider is a good idea - if you have it in the same build, someone is going to get cranky about another team being able to break their build.

# How to develop new functionality while keeping all the builds green
See [Development workflow](Development-workflow)

