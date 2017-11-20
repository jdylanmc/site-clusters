# Site Clusters

### What is a site cluster?
  - Multiple "websites" built upon the same content tree.
  - By this definition, each language can be considered it's own website.
  - Jocks to the Core:
    - http://jockstothecore.com/site-clusters-sitecore-part-1/
    - http://jockstothecore.com/site-clusters-part-2/
    - http://jockstothecore.com/site-clusters-part-3/
    - http://jockstothecore.com/site-clusters-part-4/

### Different Flavors:
  - Localized Languages
    - AKA: Simple Language Strategy
    - Allegis's EASi & Aerotek websites
    - Out of the box with Sitecore

![Localized Languages](/images/localized-languages.png)

  - Language Fallback
    - Out of the box with Sitecore

![Language Fallback](/images/language-fallback.png)

  - Shared Languages
    - Iron Mountain / Recall
    - LNRS?
    - Likely requires some customization
  - Unclustered Tenants
    - This is the opposite of a Site Clusters
    - Out of the box with Sitecore

![Unclustered Tenants](/images/unclustered-languages.png)

  - Read about strategies here: https://brainjocks.atlassian.net/wiki/spaces/ALGP/pages/214962331/Localization+Strategies

### Architectural Impact:
- URL Strategy: 
  - Impact on Site Configuration, how you resolve sites (and thus content tree), how you resolve languages (multilingual 404? 500?).
  - Each of these are possible in Sitecore, but to achieve each you must configure Sitecore differently.
    - mysite.com vs mysite.co.uk
    - mysite.com/en vs. mysite.com/fr
    - mysite.com/en-us vs mysite.com/fr-ca vs mysite.com/fr-fr
    - asia.mysite.com vs europe.mysite.com
    - mysite.com/application/en-us vs mysite.com/application/en-ca
- SEO/Navigation Strategy
  - How easy is it for you to support and manage featuresets like hreflang tags?
  - How are you resolving canonical tags?
  - Do you care about the difference between mysite.com/en-gb/contact-us vs mysite.co.uk/contact-us?
- Information Architecture strategy- major impact to Assembly team
  - What percentage of your content tree is available in all languages you support?
    - Does the business sell different products in different regions?
    - Are there country specific offerings.
      - Iron Mountain Example: Hipaa only applies to the US.
    - Is there a different marketing team per region with different goals or branding?
  - What is the demographic of your content authoring team?
    - Do your content authors speak the language they're maintaining?
      - Clay Tablet
      - Transperfect
    - How many languages is each content author responsible for?  
      - One person managing a 10 language website is different than 5 people managing a 5 language website.
    - Does your content authoring team communicate well?
      - When managing multiple languages out of the same tree, it's very easy to accidentally affect languages you're not meaning to negatively impact.  
      - e.g., making a change in Shared vs Final. 
    - Language Support:
      - If you want to maintain non .NET standard languages (e.g., en-jp), then you may run into problems:
        - Have to add language to registry for .NET runtime
        - Services like xDB Cloud do not support faux languages
        - This may be desirable to the business so that they can market to alternative markets where their content authors don't speak the native language.



### What is Unshared, Shared, Final?
- On templates, when adding fields, you can elect fields to be Shared vs Unshared
  - Shared fields are the same in all languages
  - Unshared fields can change per language
    - Only partly true for rendering parameters.
- When using Experience Editor, or editing Presentation Details, you have the option to edit in "Shared" or "Final" mode.
  - This is synonymous with Shared and Unshared fields.
  - Editing Shared in experience editor edits the shared presentation field (which is !unversioned!, and shared)
    - This means that changes in shared layout will affect all languages
    - This means that changes in shared layout won't go through workflow
  - Editing Final in experience editor edits the final presentation field (which is versioned, and unshared)
    - This means that changes in final layout will only apply to the language you are editing.
  - To determine the markup to render on the page, Sitecore calculates the shared layout and then applies a "diff" from final layout
    - Changes in final layout will always win over changes in shared layout
  - A note on rendering parameters:
    - Rendering Parameters are stored within the XML of the Presentation fields
      - You can't 'translate' rendering parameters in Shared layout mode because you're editing a Shared field
      - Changing rendering parameters in final layout mode is synonymous to 'translating' them, as you're editing an unshared field


###Impacts on changing Shared vs Final:
| Action | Affects Shared/Final Layout | Where should you do this? |
|---|---|---|
| Editing a field on the page (image, text, etc.) |	No | Doesn’t matter |
| Switching from the English Language to the Spanish Language | No | Doesn’t matter |
| Changing text in a field in the Spanish language | No | Doesn’t matter |
| Removing a component (rendering) for Spanish only | Yes | Final Presentation |
| Adding a component (rendering) for Spanish only | Yes | Final Presentation |
| Moving components on the page for all languages | Yes | Shared Presentation |
| Editing Component Properties (rendering parameters) for all languages | Yes | Shared Presentation |
| Editing Component Properties (rendering parameters) for the Spanish language only | Yes | Final Presentation |
| Changing a personalization rule for all languages | Yes | Shared Presentation |
| Changing a personalization rule for Spanish only | Yes | Final Presentation |

### Strategies:
- A few easy "Rule of Thumb" strategies for websites:
  - Option 1: Build your standard values in Shared layout, and do all content in the tree in Final layout
    - This keeps your std values clean, and allows you to easily make changes to std values without impacting content
    - Use powershell to "kickstart" the next language.
  - Option 2: "Assemble" website in Shared (e.g., the U.S. site), and lock everyone into final after you launch the first language (and rollout new languages periodically)
    - This makes it easier to assemble multiple languages quickly, as (theoretically) all you need to focus on is datasource & page template translation.
    - This also makes it very easy to start a new language, as you will have the page structure in place from the shared presentation