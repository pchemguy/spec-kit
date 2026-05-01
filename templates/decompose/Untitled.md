https://chatgpt.com/c/69f4870e-de1c-83eb-8662-5174d477cdce

Well, I am not so much concerned about US5. Desktop packaging is a separate goal. It is not easy to frame it as user initiated functional flow, but it does need to be implemented, so even a weak US5 might be okay as a means to further use it in downstream flow. The basic test would probably be: given the app with US1-4 as a web app, would it make sense from user point of view, as inferred from target description, possibly any other relevant context, to ask an agent to to turn a web app into a desktop packaged app (basically, splitting the original target prompt at packaging boundary). Well, it does. So it deserves a story even if it does not quite fit "user initiated workflow" test.

I guess this is something that might be considered for additional US rule validity test. We convert each user story into corresponding coding agent request for incremental implementation of the associated functionality. A user story is fine than a feature, and may or may not present an MVP or a complete in some sense functional increment. But it should still be sensible from the user's point of view.

So "US5 — Use the Calculator in a Packaged Desktop Runtime" -> something like "Extend the current web app so that it could be used as both a web app and a standalone desktop app, keeping most of the code base shared between the two." (Hence, the idea of packaging, preserving the existing web app). Makes sense as a standalone user request / capability increment.

It seems that both US1 and US4 would also pass this test.

"US2 — Execute Binary Stack Operations"... Given this title I have to deduce something like "Extend the current app by implementing binary ops." Well, the user does give a heck about arity, so this would be total BS; likewise, US3.


---
---

How do I change the prompt to force LLM critically assess each functionality grouping considerations against the rules. Should I be asking first to articulate candidate grouping criteria and assess those against their priority? Should I also require that an LLM would derive from the described problem what would be the main/core needs/objectives/etc. of a target user of the specified app or functionality?
