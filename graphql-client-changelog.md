# One GraphQL client tool to rule them all

I want a single tool that generates GraphQL clients with up-to-date and correctly typed interfaces. The tool should work across all (most) of our products. This should be achieved using the exposed GraphQL schemas of the internal and external GraphQL servers we use at Magenta.

The developer workflow doesn't have to be fighting a scary, blackbox-y res.json()-blob that needs to be logged to your terminal, for you to have an idea of what's going on. The tooling is out there*!

**In some form, and it might require a ton of work from us - who knows ¯\\_(ツ)_/¯*

The Problem: # Forklar kort til folk der ikke normalt bruger GraphQL

Wishlist:
- Popular with a low chance of getting abandoned
- One solution that works for at least both Python and JS/TS (and Go?)
- No/low amount of boilerplate
- Satisfying end result that won't inspire hacky modifications

## Our experiences with GraphQL

Datascanner:
1 GraphQL integration (OS2mo) in Python just using the `requests`-library. A so-called "because we had to"-implementation.

Greenland: 
???

BorgerPC: 
Doesn't use it in any form.

Obvius:
Yeah, no.

OS2mo:
The only(?) project with a GraphQL server.
Multiple GraphQL integrations and a different way of doing it every time. Some of the tools used consists of the following:
- https://github.com/quicktype/quicktype
  Used in OS2monaldo and some smaller OS2mo projects to generate interfaces of queries in both TypeScript and Python. Works decently, but requires manual work, or for us to build our own tooling around it.
- https://strawberry.rocks/docs/codegen/query-codegen 
  This seems like a decent idea since it is what OS2mo's GraphQL server is built with, but it couldn't keep up with OS2mo the last time we tried to use it.
- Probably more...

## A solution that solves half the problem

https://git.magenta.dk/apb/sveltekit-graphql-codegen

My attempt at a nice developer experience in JS/TS using [The Guild's GraphQL codegen tool](https://the-guild.dev/graphql/codegen) and [a minimal GraphQL client](https://github.com/jasonkuhrt/graphql-request) (the specific client isn't important in this case)

1. Put your query in a `.graphql`-file
2. `yarn run generate` (or `npm`, `pnpm`, whatever)
3. Use the generated Document with your GraphQL client
4. 🪄✨ You now have auto-complete and types
![](https://i.imgur.com/e4EAqmp.png)


Pros:
- JS framework agnostic - will work with any project as long as Node is involved
- Easy to turn into a pre-commit/CI step that forces the codebase to match the interfaces and stay up-to-date
- Very low boilerplate

Cons:
- Doesn't support Python... Yet (someone™ please fix).
- Might not be useful for Django projects that won't touch JavaScript


## More (untested) tool that might be nice, but doesn't tick off all of our wishes:
- https://github.com/profusion/sgqlc#code-generator