# Using Markdown in Documentation

Markdown is a text-to-HTML conversion tool for web writers. Markdown allows you to write using an easy-to-read, easy-to-write plain text format, then convert it to structurally valid XHTML (or HTML).

For example, this entire page was created using Markdown!

Below is a quick reference of all the Markdown syntax that is supported by Stoplight.

## Headers

```no-highlight
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

# H1

## H2

### H3

#### H4

##### H5

###### H6

## Emphasis

*Italics*

**BOLD**

***Combined Empasis***

 ~~Scratch this.~~

```no-highlight
Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~
```

## Lists

>In this example, leading and trailing spaces are shown with dots: ⋅⋅⋅

1. First ordered list item
2. Another item
   - Unordered sub-list
3. Actual numbers don't matter, just that it's a number
   1. Ordered sub-list
4. And another item

```no-highlight
1. First ordered list item
2. Another item
   - Unordered sub-list
3. Actual numbers don't matter, just that it's a number
   1. Ordered sub-list
4. And another item

...You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).
```

## Links

Different ways to create links.

1. To link Inline-style
[I'm an inline-style link](https://www.google.com)

2. To link Reference-style
[I'm a reference-style link][https://www.google.com "Google's Homepage"]

3. To link to API explorer from documentation pages
[Create Account API](../api?type=post&path=/v1/recipients/{recipientId}/accounts)

4. To link/reference to another document/markdown
[FAQ](?path=docs/faq/faq.md)

5. To create anchor link within the page. You can place anchor by declaring <a name = "portal"></a>. Now you can reference this link anywhere within the page by declaring link such as [Dev Portal](#portal)

```no-highlight
1. To link Inline-style
[I'm an inline-style link](https://www.google.com)

2. To link Reference-style
[I'm a reference-style link][https://www.google.com "Google's Homepage"]

3. To link to API explorer from documentation pages
[Create Account API](../api?type=post&path=/v1/recipients/{recipientId}/accounts)

4. To link/reference to another document/markdown
[FAQ](?path=docs/faq/faq.md)

5. To create anchor link within the page. You can place anchor by declaring <a name = "portal"></a>. Now you can reference this link anywhere within the page by declaring link such as [Dev Portal](#portal)

```

## Images

Here's our logo ( hover to see the title text ):

![Fiserv logo](../../assets/images/Fiserv_Logo.jpg "Fiserv Logo" "Width:200px")

## Code and Syntax Highlighting

Inline `code` has back-ticks (`) around it.

```no-highlight
Inline `code` has back-ticks (`) around it.
```

> Here is the example for javascript code.

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

>Use language tags to change the syntax highlighting.

```json
{
  "JSON": "Syntax Highlighting"
}
```

## Tables

Tables aren't part of the core Markdown spec, but they are part of GFM and *Markdown Here* supports them. They are an easy way of adding tables to your email -- a task that would otherwise require copy-pasting from another application.

| Tables        |      Are      |  Cool |
|---------------|:-------------:|------:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      |   centered    |   $12 |
| zebra stripes |   are neat    |    $1 |

```no-highlight
Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

```

The outer pipes (|) are optional, and you don't need to make the raw Markdown line up prettily. You can also use inline Markdown.

| Markdown | Less      | Pretty     |
|----------|-----------|------------|
| *Still*  | `renders` | **nicely** |
| 1        | 2         | 3          |

```no-highlight
Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3
```

## Blockquotes

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.

```no-highlight
> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote. 
```

## Horizontal Rule

---

```json

Three or more...

---

Hyphens

***

Asterisks

___

Underscores
```

## Extended markdown syntax

Developer Studio Markdown extends standard Markdown with inline comment annotations.
The value inside of the annotations is one or more key-value pairs, and the annotation affects the Markdown block right after it in the document.

### Image centering

To center an image annotate `align` wrapped in a comment:

<!-- align: center -->
![Image centering](../assets/images/md/align_image.png "Image centering")

### Callouts, themed blockquotes

Callouts are MD blockquotes with optional annotation to indicate intent.
There are four blockquote themes: the default (no annotation required), success, warning, and danger.

![Themes](../assets/images/md/theme_syntax.png "Blockquote Themes")

> Blockquotes in default color.
<!-- theme: success -->
> Blockquotes in success theme.
<!-- theme: warning -->
> Blockquotes in warning theme.
<!-- theme: danger -->
> Blockquotes in danger theme.

### Cards & rows of cards

A `card` has a border and optional values for a title, a description, and a link.
To have a number of cards in a row, use `row` & `row-end` annotation before and after card(s) to be in a row.

![Row of Cards](../assets/images/md/card_syntax.png "Row of cards")

<!-- type: row -->

<!-- type: card
title: Card One
description: About...
link: ?path=docs/about-developer-studio.md
-->

<!-- type: card
title: Second Card
description: About...
link: ?path=docs/about-developer-studio.md
-->

<!-- type: row-end -->

### Tab sets

To organize content in tabs, indicate `titles` within the first `tab` annotation, followed by the tab content, and closed by a final `tab-end` annotation.
> Users of Stoplight Flavored Markdown, need to include additional `title` annotation for each tab.

![Tab sets](../assets/images/md/tab_syntax.png "Tab sets")

<!-- theme: danger -->
> Tab containers cannot be nested.

#### Example

<!--
type: tab
titles: First Tab, Second Tab
-->

First tab content...

<!--
type: tab
-->

Second tab content...

<!-- type: tab-end -->

---

#### Additional tabset example

Bellow example of a tabset contains a table and a code block.

<!--
type: tab
titles: additionalDataCommon, JSON Example
-->

The below table identifies the parameters in the `additionalDataCommon` object.

 Variable | Type | Maximum Length | Description |
 -------- | -- | ------------ | ------------------ |
 `additionalData` | *object* | N/A | Used to identify specific data based on transaction requirements. |
 `installments` | *object* | N/A | Used in [installment bill payments](?path=docs/Resources/Guides/Bill-Payments/Installment-Payment.md) |
 `recurring` | *object* | N/A | Used in [recurring bill payments](?path=docs/Resources/Guides/Bill-Payments/Recurring-Payment.md) |
 `amountComponents` | *object* | N/A | Used in transactions where additional [amount](?path=docs/Resources/Master-Data/Amount-Components.md) fields such as tax, surcharge, fees are required as part of the request. |
 `billPaymentType` | *string* | 12 | Indicates the type of [bill payment](#bill-payment-indicator). Required for Charges, Cancel and Capture transactions where a bill payment is being processed. |
 `deferredPayments` | *object* | N/A | Used in [defferred bill payments](?path=docs/Resources/Guides/Bill-Payments/Deferred-Payment.md) |
 `directedRouting` | *object* | N/A | Required in Directed Routing transactions. |
 `subMerchant` | *object* | N/A | Required in transaction initiated by a [Payment Facilitator](?path=docs/Resources/Guides/Industry-Verticals/Payment-Faciliator.md) to identify the sub-merchant information. |
 `privateLabel` | *object* | N/A | Used to process [Private Label](?path=docs/Resources/Guides/Payment-Sources/Private-Label.md) payment cards. |
 `customFields` | *array* | N/A | Used to submit merchant custom fields used in terminal processing such as Key Value Pair. |

---

<!-- type: tab -->

JSON string format for `additionalDataCommon`:

```json
{
   "additionalDataCommon":{
      "additionalData":{
         "baiFlag": "PERSON_TO_PERSON",
         "ecomURL":"https://www.somedomain.com",
         "requestedTestResponseCode":"NO_CONNECTION_AVAILABLE",
         "emvParameterDownloadIndicator":true
      }
   }
}
```
