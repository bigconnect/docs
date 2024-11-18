---
description: >-
  Simply select the AI column, add a prompt, and let AI do the work for you -
  from analyzing customer feedback to triaging support tickets, drafting
  responses, or writing inventory descriptions.
---

# AI Column

### Setup

This feature is only available if you have activated or successfully connected an AI config. The new AI column is only available when using Mware Application Development Framework DB, but we do plan to add support for external databases such as Postgres and MySQL.

### Operations

The AI column can perform 7 specific operations on your data. In some cases, these are based on a single column in your table, like "Translate," and in other cases, it is possible to select multiple columns, like "Summarise Text." In all cases, the value returned will be a string.

/

#### Summarise Text

Select multiple columns to summarise. The response generated will be in plain paragraphs.

<figure><img src="https://files.readme.io/377045870bffe0d984d31536b3c3fc4ceb48153365a6beeca347af44766b8936-image.png" alt=""><figcaption></figcaption></figure>

In this example, the Summarise Text option is used to take multiple columns from the Employees table and provide a summary of the information they contain.

<figure><img src="https://files.readme.io/2b60a91ee5cf52d60e208a075ae064533944537074e00afc912dced8ec327d58-image.png" alt=""><figcaption></figcaption></figure>

\


#### Clean Data

Select a single column to clean. Fixes spelling and grammatical errors, as well as any data issues.

<figure><img src="https://files.readme.io/a3c65109f9913c89d6b5123a786492d926836d78a2c8fb3c1f53f20d2cbe6a80-image.png" alt=""><figcaption></figcaption></figure>

In this example, the input column is being cleaned of spelling mistakes.

<figure><img src="https://files.readme.io/15758d8b02058833c270db746967a91631dd56805ee14290ee6095b36d174671-image.png" alt=""><figcaption></figcaption></figure>

\


#### Translate

Select a column to translate, and specify a language. When the specified column is filled or updated a translation will be fetched.

<figure><img src="https://files.readme.io/2611a2334c4558e9d2daec8f12fde47052e9f5de712b4f4e931c8721518ba110-image.png" alt=""><figcaption></figcaption></figure>

In this example using the notes from the sample data Expenses table, we can translate the notes to Welsh - particularly useful when working across borders or in circumstances with bilingual or multi-lingual requirements.

<figure><img src="https://files.readme.io/d0d6b9bd6937a1c26213713196ea7c8d4c089524a0aa6a77ee00513782f41d58-image.png" alt=""><figcaption></figcaption></figure>

\


#### Categorise Text

Select multiple columns to categorise. Provide your desired categories separated by commas.

<figure><img src="https://files.readme.io/b18d7e2a14f7ea5965a290046f4b608d316feacd6853fd6fb00d69670e30d6ab-image.png" alt=""><figcaption></figcaption></figure>

In this example, we can set a list of comma-separated categories and use it to classify the values of the selected columns.

<figure><img src="https://files.readme.io/83b2692d88ff499518c29c8503eb3522f1ebbf761e941b34a9b7d68ba4addea7-image.png" alt=""><figcaption></figcaption></figure>

\


#### Sentiment Analysis

Select a single column to analyse. Useful for understanding and categorizing customer feedback at a glance.

<figure><img src="https://files.readme.io/2aa7f2c2782a0b273171c0583259ca8c7b8b700ac81a939c76019ad9e26c57c2-image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/91560d0b6d7464025ca0ec881c8fadf67b6b552f89b2104447196421f1ab0b13-image.png" alt=""><figcaption></figcaption></figure>

\


#### Prompt

<figure><img src="https://files.readme.io/5b079020c5c210f9372a0b548c124d4723bb15da154796de5eb6f8a18d1dbe41-image.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://files.readme.io/c3aa20d1195465966452989590bfd223a42056cecb6a2b7e52f18cf8490cf0b7-image.png" alt=""><figcaption></figcaption></figure>

\


#### Search Web

Takes multiple columns and returns AI search results. Not compatible with OpenAI configurations.
