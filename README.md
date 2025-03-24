# MCP interacting with GitHub, X, and API specs.

Work towards making this work perfectly. In the end, instructions aren't even needed, as I can replace it programatically. The important thing is that the LLM gets guided properly in the responses and knows and can tell the user what's actually happening and tells the user how he done it, so people end up actually sharing the link containing the og:image.

After this works smoothly, #1 to figure out is auth, to track usage of this MCP. GitHub oauth is best!

Let's work towards this as an ultimate MCP!

The philosophy behind slop can be embedded as additional params in the openapi; `x-workflow-operations: string[]`.

This could ensure that it shows a summary of how to use these operation(s) using GET, such that it becomes a workflow.

My principles for making the LLM actually work well with tons of tools:

1. As the LLM knows popular websites, instruct it to simply use the web like normal.
2. Under water, ensure every input is somehow routed to the right substitute website(s).
3. Ensure every response is markdown and contains very few tokens, ideally less than 1000! This ensures we can do many steps.
4. Ensure the dead ends guide the LLM back on track.
5. Ensure every step in a multi-step process contains instructions about what to do next.
6. Ensure a sessionID of the conversation is sent, which we can use to estimate tokensize. When tokensize becomes excessively large, ensure instructions are added that summarize the obtained context so far and a link to start a new conversation.
7. Ensure the path the LLM visit is the same as the path the user or crawler visits. Respond well on accept header and other information to distinguish.

How should product builders of today become ready to allow for this?

1. Most APIs use POST, but GET is easier to be instructed about, as it can be done in markdown. Let's promote making APIs GET and promote super easy to understand URL structures with minimal token length.
2. Ensure to use OpenAPI to show the possible endpoints and routing. Your API should be the first-class citizen, not your website.
3. Ensure to make your openapi explorable by either putting it right on the root at `/openapi.json`, or by putting redirecting to it from `/.well-known/openapi` if that's not possible.
4. Ensure all your pages that are exposed as text/html also expose a non-html variant (preferably markdown, or yaml if structured data can also be useful) that is under 1000 tokens with the same/similar functionality.
5. Hitting errors in your API should always guide the agent back on track, just like we do with humans. Try buildling these UX pathways on the API level!

Is it somehow possible to provide this as a middleware to APIs? For sure! The one tool to rule them all is fetch, and it could be made safe in the following way:

- Ensure to route away from human-first websites to ai-optimised websites.
- Ensure to truncate the response to never be above a certain limit
- Ensure to prefer accepting markdown

^ These are the easiest ones. Let's do it!
