socially-shoppable
    Most up to date working of website:  

http://192.241.203.207:4060/#!/ 

 
 

 
 

    revised estimates for the 1st iteration (affiliate setup). We scaled it down by eliminating the store/shop subscription functions and the SS mail & notification functions that have to do with the subscription feature (Promotions/Events, Coupon mails) from the 1st iteration. In addition, we also want to move all Collective functions to 2nd iteration or later. The remaining features in the revised estimates are necessary to go live. 

https://www.dropbox.com/s/fv85vtfeyw8u7va/SS%20Estimated%20Timeline%201st%20Iteration%20minimum%20features_with%20revised%20vs%20old%201st%20iteration.xls?dl=0 
<<SS Estimated Timeline 1st Iteration minimum features_with revised vs old 1st iteration.xls>>

 
 

 
 

    All pages with instructions and updates 

https://www.dropbox.com/sh/gzmi6w0a8gojxoo/AABDX1Vx7fYvSo50EtxsMObVa?dl=0 

 
 

    4. Smartsheet  https://app.smartsheet.com/sheets/wRwJhfpPmQmW9CWwRvvgFMGGh3V2cqpvcmv8fg91 

5 . Below this line is complied notes over time of the  processing progress I found while searching for the diagram on how the sort of “hybrid” backend would work but I couldn’t find it after several hours of searching through emails notes notebooks etc. So I figured as I was combing through all of that I could select some notes on how things progressed after trying thing that did or did not work, therefore no need for you to try things that may not work:) Hope this helps. 

 
 

 
 

 
 

_____________ 

 
 

Feb 21, 2019 

To-Dos / In Progress 

 
 

1. SCRAPER 

        The scraper has to be modified in order to extract color data on products with different nesting of required values. The nesting differs between shops, so the code must be made universal/adaptable to changing orders of field names. For example: 

         

            Forever 21's 

                -> ['color', 'size'] 

            Brooks Brothers 

                -> ["fit", "color", "neck size", "sleeve length", "quantity"] 

                -> ["fit", "color", "chest", "length", "quantity"] 

                -> plus more (depends on the product type) 

         

        Also, some shops have compounded field names such as 'size/color', 'color and size', etc. with random formats for the values ('BLACK - XL', 'Pink, XS', 'XXL ~ BEIGE', etc.) so the scraper must be updated to handle such exotic formats. 

         

        In addition, some products have their catalog keys changed within Two Tap. This is very problematic since these keys aren't supposed to change, and we are currently using these keys as ids within our web application. Both the scraper and the internal scripts for querying products must be updated to adapt to this breaking issue. Each product should now contain a list of catalog keys (from oldest to newest) which will be compared against product-specific metadata such as shares, cart, wishlist, etc. This will affect the scraper's performance since multiple checks have to be done before inserting, updating, and removing products in the database. 

         

        The whole overhaul process is tedious and needs a considerable amount of time to implement. 

         

2. PRODUCT VARIATION(S) 

        In order to make all variation options visible to the user, the quick view, shopping bag page, and single product page scripts must be updated to adopt to different nesting of required field names (similar to the scraper). This will also include updating the current script responsible for passing the selected required values to the Two Tap estimates endpoint to make the calculations more accurate. Currently, the script only has fixed/hard-coded list of required fields (quantity, size, color, etc.), which affects the estimate values since not all fields are passed to the endpoint. 

 
 

3. Reimplement the display of (attributes/variation) color and size dropdown options.  

         Reimplement the display to accommodate all formats; create a universal/generic codebase. 

 
 

4. Selected product size and color are not showing on Shopping bag page (reimplementation) 

        The current code(fix) only accommodates certain formats so it has to be modified. 

 
 

5. Incorrect product price displayed on the selected variation (reimplementation) 

The current code(fix) only accommodates certain formats so it has to be modified. 

 
 

6. Refactor the passing of the product's selected required field values to the estimates end point (Added to Bag modal and Shopping bag page) 

Pass the correct required fields/variations selected by the user to the estimates endpoint to fix the error in price estimates. 

 
 

April 1st 2019 

We looked into CJ, Rakuten, and AWIN(affiliate windw). These three don't provide any unified checkout mechanism and product catalog API. They do provide downloadable catalog data(product feed per merchant/advertiser) which we can upload to our own database. We are currently experimenting on ways to handle each catalog data from these affiliates since they have their own method of data retrieval (FTP & HTTP) and the data comes in various formats (XML & CSV [gzipped]), none of which are directly compatible with our database (MongoDB, which uses JSON), so some format conversion has to be done. Also, each affiliate catalog data has its own field naming scheme, so we will have to come up with a unified set of field names that encompasses all pertinent product fields across all affiliates, which we will then integrate into the scraping/updating process before importing the formatted data into the database. 

 
 

We have already tested locally scraping the data from Rakuten and AWIN using various node.js modules. The combined product catalog sums up to 4.6 million. But the AWIN  products are not from approved programs. We're still applying for programs in AWIN for testing. The Rakuten products are from the 12 approved programs/merchants that support product feed.  As with CJ, they haven't responded yet on how we can access the exported product feed via CJ SFTP. We can't find the instruciton in the email notification they've sent after exporting data via CJ SFTP. The other option is downloading the feed thru email but it's not ideal for download automation. 

 
 

April 2 2019 

We looked into CJ, Rakuten, and AWIN(affiliate windw). These three don't provide any unified checkout mechanism and product catalog API. They do provide downloadable catalog data(product feed per merchant/advertiser) which we can upload to our own database. We are currently experimenting on ways to handle each catalog data from these affiliates since they have their own method of data retrieval (FTP & HTTP) and the data comes in various formats (XML & CSV [gzipped]), none of which are directly compatible with our database (MongoDB, which uses JSON), so some format conversion has to be done. Also, each affiliate catalog data has its own field naming scheme, so we will have to come up with a unified set of field names that encompasses all pertinent product fields across all affiliates, which we will then integrate into the scraping/updating process before importing the formatted data into the database. 

 
 

We have already tested locally scraping the data from Rakuten and AWIN using various node.js modules. The combined product catalog sums up to 4.6 million. But the AWIN  products are not from approved programs. We're still applying for programs in AWIN for testing(Update: As of today we have two approved merchants). Also, the Rakuten products are from the 12 approved programs/merchants that support product feed.   

 
 

Update on CJ, we're able to access the files via CJ SFTP. However, we had to use a different node.js module (ssh2) because they're using sftp, plus they're using normal zip files (zlib only handles gzip files) so we had to find another module that handles regular zip files. 

 
 

April 4 2019 

Hello Maam. For updates, we've already implemented the basic scraper for CJ. Our next step is to implement sequential scraping. In our previous test, we implemented basic scraping individually per affiliate network. Our next goal is to scrape the product feed of the three networks in sequence. 

April 5 2019 

Hi Maam. For progress update, we have implemented scraper endpoints for individual testing in dev site. It is  basic scraping but instead of doing it locally, we tested it on the server. This is part of the process for the implementation of sequential scraping, which we will be doing next week.  

 
 

April 9 2019 

I'll let you know some time this week about our progress. Yesterday, we have started implementing sequential triggering of affiliate scrapers and it is still in progress. Also we're examining and analyzing the field names across the three affiliate networks. 

 
 

April 11 2019 

Hi Maam. For progress updates, the implementation of the sequential triggering of affiliate scrapers is already done. We are now working on formatting the field names of the three affiliate networks and it is still in progress. After that, we will create a new database table/collection for the new setup. 

 
 

April 12 2019 

Just an update for today, we're done formatting the field names for AWIN. CJ and Rakuten are still in progress. 

 
 

April 14 2019 

Meeting discussion: 

 
 

We looked into CJ, Rakuten, and AWIN(affiliate windw). These three don't provide any unified checkout mechanism and product catalog API. They do provide downloadable catalog data(product feed per merchant/advertiser) which we can upload to our own database. We are currently experimenting on ways to handle each catalog data from these affiliates since they have their own method of data retrieval (FTP & HTTP) and the data comes in various formats (XML & CSV [gzipped]), none of which are directly compatible with our database (MongoDB, which uses JSON), so some format conversion has to be done. Also, each affiliate catalog data has its own field naming scheme, so we will have to come up with a unified set of field names that encompasses all pertinent product fields across all affiliates, which we will then integrate into the scraping/updating process before importing the formatted data into the database. 

 
 

We have already tested locally scraping the data from Rakuten and AWIN using various node.js modules. The combined product catalog sums up to 4.6 million. But the AWIN  products are not from approved programs. We're still applying for programs in AWIN for testing(Update: As of today we have two approved merchants). Also, the Rakuten products are from the 12 approved programs/merchants that support product feed.   

 
 

Update on CJ, we're able to access the files via CJ SFTP. However, we had to use a different node.js module (ssh2) because they're using sftp, plus they're using normal zip files (zlib only handles gzip files) so we had to find another module that handles regular zip files. 

 
 

 
 

April 15 2019 

Hi maam. We could meet tomorrow our morning for progress update. We're still working on fieldname formatting for Rakuten. Both CJ and AWIN are done. 

 
 

 
 

April 22 2019 

 
 

 
 

# Features 
	

Affiliate Networks (CJ Affiliate/Rakuten/AWIN) 

# Product catalog 
	

No endpoint; manual upload per merchant/advertiser 

# Search function 
	

Supported 

# Sidebar Filter 
	

No native endpoint provided but can be manually implemented 

 
 

"Search and Store Chosen page filters: 

- Store/Merchant 

- Brand 

- Category 

- Price Range 

- On Sale/Promotion 

- Color 

- Gender 

- Age Group 

# Sorting 
	

Can implement: 

- Most Popular (likes and shares only) 

- Low to High 

- High to Low 

- % Off Sale 

- Favourites 

- Most Shared (can be reimplemented - shares only)" 

- Newest First 

 
 

Cannot implement (require verified purchase): 

- Customer rating 

- Bestseller 

- Relevance (For clarification: not found in smartsheet; only in UI) 

# Scraping 
	

Automated download of product catalog 

# Merchants API (endpoint) 
	

Custom implementation 

Store Chosen Page 
	

Can be implemented as long as merchants are unique per affiliate network 

Single Product Page/ Product Details Page 
	

Can be implemented 

Display only; no selection of variation and quantity 

 
 

Collective tab -- requires verified purchase 

Details tab - supported 

Designer tab -supported 

Bonus offers 

Shipping info tab - supported 

Likes/Share/Review tab: 

--Likes and Share -supported 

--Reviews: requires verified purchase 

Cart/Shopping Bag Page 
	

Not supported 

Shopping Bag Expanded 
	

Not supported 

Saved List/Favorites Page 
	

Can be implemented 

Checkout 
	

not supported: Redirect to merchant/advertiser site 

Product Meta (Likes, Shares, Review, Rating, Views) 
	

Supported except Rating and Review (no verified purchase) 

Catalog update 
	

Automated daily update(once a day) 

Navigation (menu) 
	

Can be implemented 

Banner and Logo 
	

Manual implementation 

Top Search Filter 
	

Supported but not recommended 

 
 

MAY 7 2019 

 
 

Attached herewith is the revised estimates for the 1st iteration (affiliate setup). We scaled it down by eliminating the store/shop subscription functions and the SS mail & notification functions that have to do with the subscription feature (Promotions/Events, Coupon mails) from the 1st iteration. In addition, we also want to move all Collective functions to 2nd iteration or later. The remaining features in the revised estimates are necessary to go live. 

https://www.dropbox.com/s/fv85vtfeyw8u7va/SS%20Estimated%20Timeline%201st%20Iteration%20minimum%20features_with%20revised%20vs%20old%201st%20iteration.xls?dl=0 

 
 

 
 

MAY 9 2019 

Hello Maam. Currently, we're working on grouping related products in scraper and it's kind of a complicated process. We will be merging the same products that come in different sizes so we can minimize showing the same items in search results. We also continue fixing bugs that we've found on other areas of the site. 

 
 

 
 

 
 

 
 

 
