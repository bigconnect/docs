# Embedding introduction

You can embed Answers tables, charts, and dashboards—even Answers’s query builder—in your website or application.

## Different ways to embed

There are three ways to embed Answers in your app:

* Interactive embedding
* Static embedding
* Public links and embeds

## Interactive embedding

Interactive embedding is the only kind of embedding that integrates with SSO and data permissions to enable true self-service access to the underlying data.

**When to use interactive embedding**: when you want to offer multi-tenant, self-service analytics. With interactive embedding, people can create their own questions, dashboards, models, and more, all in their own data sandbox.

## Static embedding

Also known as signed embedding, static embedding is a secure way to embed charts and dashboards.

**When to use static embedding**: you don’t want to give people ad hoc query access to their data for whatever reason, or you want to present data that applies to all of your tenants at once. For example, say you want to showcase some benchmarking stats: if you just want to make those stats available exclusively to your customers, you could use a signed embed.

## Public links and embeds

If you’d like to share your data with the good people of the internet, you can create a public link or embed a question or dashboard directly in your website.

**When to use public links and embeds**: public links and embeds are good for one-off charts and dashboards. Use them when you just need to show someone a chart or dashboard without giving people access to your Answers. And you don’t care who sees the data; you want to make those stats available to everyone.

## Comparison of embedding types

| Action                                   | Interactive | Static | Public |
| ---------------------------------------- | ----------- | ------ | ------ |
| Display charts and dashboards            | ✅           | ✅      | ✅      |
| Display interactive filter widgets       | ✅           | ✅      | ✅      |
| Restrict data with locked filters        | ❌           | ✅      | ❌      |
| Restrict data with sandboxes             | ✅           | ❌      | ❌      |
| Use the drill-through menu               | ✅           | ❌      | ❌      |
| Self-serve via query builder             | ✅           | ❌      | ❌      |
| View usage of embeds with auditing tools | ✅           | ❌      | ❌      |

## Switching from static to interactive embedding

Interactive embedding requires authentication via single sign-on (SSO), so you’ll need to set that up both in your Answers app and in your application’s server. Check out our Interactive embedding quick start.
