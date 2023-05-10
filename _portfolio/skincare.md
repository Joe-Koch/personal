---
title: "Skincare Ingredients"
excerpt: "How cultural differences affect approaches to skincare"
layout: single
header:
  overlay_image: /assets/images/skincare/skinhead.png
sidebar:
  - text: "<a href='https://github.com/Joe-Koch/skincare-ingredients/blob/master/skincare_visualization.ipynb'>View the project's code</a>"
author_profile: true
toc: true
tags:
  - data visualization
  - network graph
  - data scraping
  - Python
---

# Comparing Eastern and Western Skincare Ingredients


[See the visualization here.](https://Joe-Koch.github.io/assets/gexf-js-master/)


## Background

Globalization of beauty industries has provided opportunities for new insights into prevalent trends and recent breakthroughs in cosmetological products across the world. Recently, South Korean beauty (also known as K-beauty) trends have gained popularity in US markets. Sheet masks, mass-market oil cleansers, and BB creams are all products that originated in the South Korean beauty industry and become popular in the United States. This research compares the use of ingredients in American and Asian skincare products to identify key differences in western and eastern approaches to skincare.

## Data

**tl;dr:** I scraped skincare products from the websites of the largest drugstore chains in the US and Asia, 1,536 products from Walgreens and 1406 products from Watsons. Each product’s ingredient list was then looked up in cosdna.com, resulting in 1,821 ingredients. 
{: .notice}

Identifying a product as “American” or “Asian” is more complicated than it is on its face.  One methodology would be to compare products by brand by identifying the largest companies by revenue and analyzing their products. However, globalization of the skincare industry rendered this strategy unusable. For example, Nivea, Lancome, and Dove are all some of the highest-revenue global skincare brands, but are based in Germany, France, and Britain, respectively. Their products would be uncategorizable as “American” or “Asian”. Bioré is also highly popular in the United States, but is owned by Kao Corporation, headquartered in Nihonbashi-Kayabacho, Chūō, Tokyo, Japan. How would their products be categorized?

Another option would be to retrieve top selling-products from the most popular purveyors of skincare in each geographical region. To choose which stores to analyze, it is important to note the differences between average consumers and highly affluent, skin-conscious consumers. For example, South Korean beauty regimens are often publicized in the US as 10- or 15- step beauty routines that must be rigorously adhered to. However, the average Korean does not go to such great lengths in their daily lives. Taking data from high-end stores such as Sephora or Aritraum, where the most popular moisturizer sells for $30 for 6 ounces, would not be representative of an average consumer. One would hope a website like amazon.com would be both representative of average consumer habits and equally strong in eastern and western markets, but Amazon’s skincare section is poorly sorted and misrepresentative of skincare products’ popularity. Amazon’s most popular skincare products included products sold in huge quantities such as 10-packs of cleansers and 30-packs of sheet masks, men’s body washes mislabeled as “facial cleansers,” and products tthat have been recommended by other online communities (i.e. CeraVe moisturizers, Thayer’s witch hazel, and Aztec Secret Indian Healing Clay, all highly recommended and linked to on [reddit.com/r/skincareaddiction](https://www.reddit.com/r/SkincareAddiction)). 

Therefore, I chose to scrape data from the most popular drugstore of each region. Walgreens is the largest drugstore chain in the United States, and Watsons Personal Care Stores is the largest health care and beauty care chain store in Asia, operating over 700 stores in Hong Kong, Macau, Mainland China, Taiwan, Singapore, Thailand, Malaysia, the Philippines, Indonesia, Japan, South Korea, Turkey, Ukraine, Lithuania and Latvia. The Watsons website offers 12 countries and languages to choose from. I chose the Malaysian version of watsons.com since it is in English and would therefore minimize mistranslation errors when pulling and cleaning the data. The websites showed both in-store and online options, suggesting that the products shown online capture the habits of consumers who buy their products in person or virtually. Fortuitously, pricing is extremely similar between Walgreens and Watsons products. The cheapest cleanser for each company was Jean Pierre Cosmetics Exfoliating Dual Sided Facial Cleansing Pads with Gentle Microbeads for 7.4 cents/sheet, versus A.C. care cleansing tissue for 8.5 cents/sheet (10 won/sheet) for Walgreens and Watsons, respectively. The most expensive products were Aveda Green Science Firming Eye Creme at $56.19 / oz, compared to Vichy litactiv serum at $48.91/oz (57600 won/30mL). Retrieving data from online drugstores also allows for categorization of each product by type, i.e., cleanser, moisturizer, treatment, sun care, etc. It is important to note that eastern skincare products have already infiltrated western markets, and vice-versa. So although a product like Hada Labo's Gokujyun Alpha Lotion is clearly a Korean product, since it is sold in Walgreens, it is categorized as an American product. I contend that is an acceptable grouping, since its presence in an American store evidences that it’s being bought and used by Americans. However, this method of categorization does blur the lines between cultures.

Finally, to ensure ingredients like “aloe” and “aloe leaf extract” were recognized as the same ingredient, extraneous words like “seed” and “protein” were removed from the ingredient lists.

## Methods

The measure of “Americanness” is based on how much more frequently an ingredient appears in an American product than a Chinese one, given by the proportion of American products the ingredient is in, minus the proportion of Asian prodcuts the ingredient is in. The strength of a connection between two ingredients is a count of how many products contain both those ingredients. The size of each node, which represents how common an ingredient is, is the count of how many times it appears in any product. Since the most common ingredient, water, shows up hundreds of times more often than more obscure ingredients, if the total counts were left unnormalized, the graph would basically just look like two big nodes of water and glycerin. Counts were therefore adjusted using sklearn’s minmax scaler. 

The “all products” graph has also been simplified for better visuals by extracting a backbone network of average degree 4 using Olaf Sporns’s matlab function wu_backbone,[^1] based on the work of Hidalgo et al. (2007) Science 317, 482.

The network graphs were made using the networkX Python package to create the gexf files. These were then uploaded into Gephi and manipulated with their "forceAtlas2" tool to rearrange the nodes based on the strength of their connections. The visualizations were displayed on this website using [gexf-js](https://github.com/raphv/gexf-js). 

## Results

### Notes on the ingredients:

Notice that foods that are more common to each region show up as more regional skincare ingredients. The most “Asian” ingredients tend to be foods that are more common to the region, like sesame seed, rice, and ginger, while apple (appearing as pyrus malus, apple seed extract, or apple extract) is one of the most “American”. 

{% include figure image_path="/assets/images/skincare/watsons-ingredients.png" alt="" %}

{% include figure image_path="/assets/images/skincare/walgreens-ingredients.png" alt="" %}

Other ingredients highlight differences in approaches to skincare. The most “American” ingredient, sodium lauroyl sarcosinate, is a harsh surfactant. It also increases penetration of other ingredients, which can be irritating to the skin. Japan has concentration restrictions on SLS for use in cosmetics. However, the intense foaming and “squeaky clean” feeling after cleansing with it appeals to many American consumers. Indeed, it’s even what’s added to toothpaste to make it foam (resulting in the disgusting taste when you drink OJ post-brush).[^2]

Retinyl acetate, another “American” ingredient, is a natural form of vitamin A, the acetate ester of retinol. It has been shown to reduce the appearance of wrinkles, but also increases photosensitivity to sunlight, which may make it less appealing to Asian markets. Conversely, vitamin C is an extremely popular skincare ingredient that reduces the appearance of current sun damage and helps prevent new sun damage. Unfortunately, once in solution, it’s very susceptible to oxidative degeneration. One of the most “Asian ingredients”, zinc sulfate, is used in high-strength vitamin C treatments to stabilize the vitamin C.[^3] In general, Asian cosmetics are more geared towards preventing sun damage.

Sun damage is taken more seriously in Asia. While the US uses an “SPF” rating system that only accounts for UVB protection and is counterintuitive (for example, SPF 30 sunscreen only gives you 4% more protection than SPF 15 sunscreen), Asia uses a “PA” ranking system that refers to UVA protection, and essentially has 4 classes of protection (indicated by the number of plus signs, e.g., PA+++). Furthermore, there are ingredients like tinosorb that have been safely used in Asia and the EU for decades, but are still illegal in US products due to FDA regulatory backlog.[^4] There are also cultural differences, far beyond the scope of this research, that culminate in different beauty standards with respect to skin tone. 

### Similarities:

There were many similarities across the Watsons and Walgreens skincare products. Both markets saw similar rates of scientific words like “hydrolyzed,” and nature-based words like “leaf,” “root,” “peel.” Both stores sorted their skincare products into similar groups like cleansers, moisturizers, etc., and had similar proportions of products across these groups, though Watsons offers proportionally nearly twice as many masks and scrubs. We also see extremely similar numbers of ingredients per product, with the median of 28 for both groups. 

{% include figure image_path="/assets/images/skincare/producttypes.png" %}
{% include figure image_path="/assets/images/skincare/numberingredients.jpg" %}

Rank-size distributions often follow a power law distribution,[^5] and indeed we see similar behavior in skincare ingredients, plotted on a log-log scale in the graph below. Interpreting this a bit, the 1st-ranked ingredient, showing up in very nearly every product, was water. It was present in hundreds of times more products than the least common ingredients, which might be in only a single product. This pattern may emerge as ingredients that are well known and available become more commonly used in skincare products, which then causes them to become even more well known and available. 

{% include figure image_path="/assets/images/skincare/bestrankfreq.jpg" alt="" %}

### Differences:

Further highlighting the differences in Eastern and Western approaches to sun protection, nearly five times as many Watsons products were labeled as "sun care." Oil-based moisturizers are trickling into the US market, but are still not as popular, with roughly 8 times as many moisturizers sold by Watsons being categorized as oils. 

{% include figure image_path="/assets/images/skincare/suncaretrue.png" %}
{% include figure image_path="/assets/images/skincare/trueoils.png" %}

## Conclusion

The popularity of one skincare ingredient over another in a particular market is influenced by both availability of the ingredient (e.g. rice derivatives in Eastern products) and cultural, market driving forces. Harsh detergents that leave a "squeaky clean" feeling appeared more frequently in American products, while Asian cleansers were more likely to contain oils that gently cleanse without stripping the skin. Products sold at Watsons were also more sun-focused, with more sun care products offered, less popularity in ingredients that increase photosensitivity, and more emphasis on ingredients that prevent and repair sun damage. 

## References:

[^1]: Olaf Sporns. (2007/2008/2010/2012). <https://github.com/fieldtrip/fieldtrip/blob/master/external/bct/backbone_wu.m>

[^2]: EWG’s Skindeep Cosmetic Database: <https://www.ewg.org/skindeep/ingredient/706102/SODIUM_LAUROYL_SARCOSINATE/>

[^3]: Paula’s Choice Skincare: <https://www.paulaschoice.com/ingredient-dictionary/sensitizing/zinc-sulfate.html>

[^4]: New York Times, Explaining Sunscreen and the New Rules, Jane E. Brody, June 20, 2011 <https://www.nytimes.com/2011/06/21/health/21brody.html>

[^5]: Wikipedia, Rank-size distribution: https://en.wikipedia.org/wiki/Rank-size_distribution



