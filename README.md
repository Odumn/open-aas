# <span style="color:#ee4c2f;">Open as a Service</span>

Open as a Service is an open-source, community-driven repository created with the goal of making information about B2B products, product tiers, and product pricing freely available to developers or anyone looking to make informed product adoption decisions.


### <span style="color:#487ce5;">Data Model</span>
Prior to a 1.0 release, the data model may undergo breaking changes. For discussion of the data model or to propose an alternative, see [here](https://github.com/Odumn/open-aas/issues/1).

The current data model consists of `Products`, `Providers`, and `Tiers`, along with the enums,  <span style="color:#548da6;">categories</span>, and <span style="color:#548da6;">functionalities</span>. 

A `Product` models a single product offered by a `Provider`, when ignoring potential service tier differences. For example, Github Copilot is a `Product` offered by the `Provider`, Github, with an individual `Tier` and business `Tier`. Github Copilot falls under the <span style="color:#548da6;">categories</span>, *Developer Tools* and *Productivity Tools*, and offers the <span style="color:#548da6;">functionalities</span>, *AI Coding Assistant*.

In the inline comments below `[a, b]` should be read as "a and b", and `{a, b}` should be read as "a or b". Parenthesis refer to ordered groupings of fixed length.

```javascript
Product:                             // uniquely identified by the ordered pair, (Product.name, Product.provider)
    name: string                     // e.g. "Copilot"
    provider: string                 // e.g. "Github"
    url: string                      // e.g. "https://github.com/features/copilot"
    signup_url: string               // e.g. "https://github.com/features/copilot#pricing"
    desc: string                     // e.g. "Trained on billions of lines of code, GitHub Copilot turns natural language prompts into coding suggestions across dozens of languages." (from url)
    categories: [category]           // e.g. ["Dev Tools", "Productivity Tools"]
    functionalities: [functionality] // e.g. ["AI Code Assistant"]
    category_deps: [category]        // e.g. ["Code Editor"]
    product_deps: [{Product}]        // e.g. [{("Microsoft", "Visual Studio"), ("Microsoft", "Visual Studio Code"), ("Neovim", "Neovim"), ...}]
    integrations: [Product]          // e.g. [] <-- these are integrations in addition to the product_deps, which we assume integrate with the product
    tiers: [Tier]                    // see Tier schema below for an example
```

```javascript
Provider:             // uniquely identified by Provider.name
    name: string      // e.g. "Github"
    url: string       // e.g. "github.com"
    desc:  string     // e.g. "GitHub, Inc. is an Internet hosting service for software and version control." (from wikipedia)
    wiki: string      // e.g. "https://en.wikipedia.org/wiki/GitHub" ("" if no entry)
```

```javascript
Tier:                                  // uniquely identified by (Tier.name, Tier.product)
    product: Product                   // e.g. ("Github", "Copilot")
    name: string                       // e.g. "Business" (Github Copilot Business Tier)
    price_numerator: float             // e.g. 19.00 (USD)
    price_denoms: [string]             // e.g. ["user", "month"] -- the price is $4.99 per user per month
    functionalities: [functionality]   // e.g. [] -- these are functionalities in addition to the functionalities common to all Tiers
```

We don't yet have a rule which generates every <span style="color:#548da6;">category</span> and <span style="color:#548da6;">functionality</span>. We only offer guidelines which will change as this repository grows and as it becomes clear what the users of this repo are using it for.

A <span style="color:#548da6;">category</span> is a high level umbrella which a products may belong. A <span style="color:#548da6;">functionality</span>  is a specific feature or ability provided by a product. 

For example, a CRM software product would fall under the <span style="color:#548da6;">categories</span> of *Sales Tools* and *Customer Relations* and would offer <span style="color:#548da6;">functionalities</span>  such as *Customer Contract Management*, *Account Management*, and *Case Management*.
