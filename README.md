# Awesome Semantic Nullability

A list of tools and frameworks in the GraphQL ecosystem that have some level of support for the [Semantic Nullability](https://github.com/graphql/graphql.github.io/blob/nullability-post/src/pages/blog/2024-08-14-exploring-true-nullability.mdx#our-latest-proposal) proposal.

At this phase in the proposals life-cycle (Sept. 2024), the community has coalesced around using `@semanticNonNull` as a stand-in for new syntax which we would eventually like to add to add as syntax in the GraphQL language for expressing "fields which can be null only on error". This shared convention will allow tools in the ecosystem to work together to explore how these ideas work in practice so that we can validate them before proposing them for inclusion in the GraphQL language itself.

## GraphQL Clients

For a client to take advantage of the Semantic Nullability proposal, it must have some mechanism for handling GraphQL field errors (fields that are null due to error) client side before the data is passed to product code. It must also have some mechanism to allow it's generated types/code to understand the `@semanticNonNull` schema directive.

- [Relay](https://relay.dev/) - As of [version 18](https://github.com/facebook/relay/releases/tag/v18.0.0), Relay has support for the Semantic Nullability proposal enabled by default.
  - [@catch](https://relay.dev/docs/next/guides/catch-directive/) lets you convert field errors into result types.
  - [@throwOnFieldError](https://relay.dev/docs/next/guides/throw-on-field-error-directive/) handle field errors by throwing an error which you expect to catch in a React error boundary.
  - [Semantic Nullability](https://relay.dev/docs/next/guides/semantic-nullability/) overall documentation for Semantic Nullability in Relay.
- [Apollo Kotlin](https://www.apollographql.com/docs/kotlin/) - Supports `@catch` and understands the `@semanticNonNull` schema directive.
  - [@catch](https://www.apollographql.com/docs/kotlin/advanced/nullability/#catch) lets you convert field errors into result types, or opt into explicitly throwing field errors with `@catch(to: THROW)`.
  - [@semanticNonNull](https://www.apollographql.com/docs/kotlin/advanced/nullability/#semanticnonnull) is understood by the Apollo Kotlin code generator.
  - [Nullability](https://www.apollographql.com/docs/kotlin/advanced/nullability/) overall documentation for nullability in Apollo Kotlin.

## GraphQL Servers

For a GraphQL servers to support Semantic Nullability it must provide a mechanism to add `@semanticNonNull` to fields which it knows will only be null in the case of error.

- [Grats](https://grats.capt.dev/) - Grats, an implementation-first GraphQL server, has opt-in support for adding automatically `@semanticNonNull` to fields whose TypeScript types are non-nullable.
  - [Strict Semantic Nullability](https://grats.capt.dev/docs/guides/strict-semantic-nullability/) overall documentation for Semantic Nullability in Grats.
- [Caliban Server](https://ghostdogpr.github.io/caliban) - Caliban, a GraphQL server for Scala, has opt-in support for auto-matically adding `@semanticNonNull` to fields whose Scala types fields that don't get resolved to nullable types .
  - [@semanticNonNull](https://ghostdogpr.github.io/caliban/docs/schema.html#semanticnonnull-support)

## Standalone Tools

In some cases clients or servers that don't have built-in support for Semantic Nullability can be combined with standalone tools that add support.

- [GraphQL TOE](https://github.com/graphile/graphql-toe) - Converts a GraphQL response containing data+errors into a single data tree where fields that are in an error state will throw when accessed.
  - For very simple GraphQL clients (e.g. `fetch()`), this will allow you to use the semantic non-null types while still having granular error handling, since you can test React error boundaries or simply nest try/catch blocks around the data access.

## Works in Progress

Here we collect in-progress pull requests or issues that are working towards adding support for the Semantic Nullability proposal to different projects.

- [graphql-js](https://github.com/graphql/graphql-js) - The reference implementation of GraphQL in JavaScript maintained by the GraphQL working group.
  - [Experimental support for semantic-non-null #4192](https://github.com/graphql/graphql-js/pull/4192) pull request to add experimental support for semantic-non-null to graphql-js.
