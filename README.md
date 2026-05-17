# Compliance as a Service

The problem with most compliance programs is that they depend on someone remembering to run them.

A checklist someone reviews manually fails the moment it competes with a deadline. A rule embedded in an operational document fails when the document gets stale. The enforcement layer has to be structural — it runs whether or not anyone remembered to check.

I built compliance as an isolated service that every workflow calls. You can't publish content without passing the content check. You can't proceed without the operational audit passing. The compliance layer isn't coupled to any one workflow — it survives changes to the systems that call it.

Four domains, each enforced differently:

**Content:** FTC and platform ToS adherence, four severity tiers. Prohibited content is blocked structurally — publishing can't happen without passing.

**Operational:** A 60-point audit that verifies the system is actually behaving correctly, not just that the rules were written down. 10/10 executions passing.

**Legal:** Deadline surfacing and flag generation — AI surfaces, human executes. The AI flags a contract renewal 30 days out; a person still makes the call.

**Tax:** Every transaction categorized at point of entry. Human submits; AI never touches the actual filing.

The enforcement runs regardless of whether anyone remembered to check. That's the architecture.

## What's Here

- Isolated compliance enforcement service (decoupled from callers)
- Content compliance with 4 severity tiers and FTC/ToS mapping
- 60-point operational audit framework
- Legal deadline surfacing workflow
- Tax categorization at point of entry

## Built With

n8n · Claude (Anthropic) · Supabase · Model Context Protocol (MCP)

## Author

Jordan Waxman — [jordanwaxman.com](https://jordanwaxman.com) · [LinkedIn](https://www.linkedin.com/in/waxmanjordan/)
