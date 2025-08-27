# SQL-Ecommerce-Dataset
## I. Introduction

This project contains an eCommerce dataset that I will explore using SQL on Google BigQuery. The dataset is based on the Google Analytics public dataset and contains data from an eCommerce website.

## II. Dataset

The eCommerce dataset is stored in a public Google BigQuery dataset. To access the dataset, follow these steps: 

- Log in to your Google Cloud Platform account and create a new project. - Navigate to the BigQuery console and select your newly created project.
- In the navigation panel, select "Add Data" and then "Search a project".
- Enter the project ID "bigquery-public-data.google_analytics_sample.ga_sessions" and click "Enter".
- Click on the "ga_sessions_" table to open it.

| Field Name | Data Type | Description |
| --- | --- | --- |
| fullVisitorId | STRING | The unique visitor ID. |
| date | STRING | The date of the session in YYYYMMDD format. |
| totals | RECORD | This section contains aggregate values across the session. |
| totals.bounces | INTEGER | Total bounces (for convenience). For a bounced session, the value is 1, otherwise it is null. |
| totals.hits | INTEGER | Total number of hits within the session. |
| totals.pageviews | INTEGER | Total number of pageviews within the session. |
| totals.visits | INTEGER | The number of sessions (for convenience). This value is 1 for sessions with interaction events. The value is null if there are no interaction events in the session. |
| totals.transactions | INTEGER | Total number of ecommerce transactions within the session. |
| trafficSource.source | STRING | The source of the traffic source. Could be the name of the search engine, the referring hostname, or a value of the utm_source URL parameter. |
| hits | RECORD | This row and nested fields are populated for any and all types of hits. |
| hits.eCommerceAction | RECORD | This section contains all of the ecommerce hits that occurred during the session. This is a repeated field and has an entry for each hit that was collected. |
| hits.eCommerceAction.action_type | STRING | The action type. Click through of product lists = 1, Product detail views = 2, Add product(s) to cart = 3, Remove product(s) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0. Usually this action type applies to all the products in a hit, with the following exception: when hits.product.isImpression = TRUE, the corresponding product is a product impression that is seen while the product action is taking place (i.e., a "product in list view"). Example query to calculate number of products in list views: SELECT COUNT(hits.product.v2ProductName) FROM [foo-160803:123456789.ga_sessions_20170101] WHERE hits.product.isImpression == TRUE Example query to calculate number of products in detailed view: SELECT COUNT(hits.product.v2ProductName), FROM [foo-160803:123456789.ga_sessions_20170101] WHERE hits.ecommerceaction.action_type = '2' AND ( BOOLEAN(hits.product.isImpression) IS NULL OR BOOLEAN(hits.product.isImpression) == FALSE ) |
| hits.product | RECORD | This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data. |
| hits.product.productQuantity | INTEGER | The quantity of the product purchased. |
| hits.product.productRevenue | INTEGER | The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 (e.g., 2.40 would be given as 2400000). |
| hits.product.productSKU | STRING | Product SKU. |
| hits.product.v2ProductName | STRING | Product Name. |

## III.  Explore the Dataset

In this project, I will write 08 query in Bigquery base on Google Analytics dataset

### Query 01: calculate total visit, pageview, transaction for Jan, Feb and March 2017 (order by month)

<img width="685" height="206" alt="image" src="https://github.com/user-attachments/assets/c80e0eb2-799e-4083-8286-c4fdde9ee929" />




| month | visits | pageviews | transactions |
| --- | --- | --- | --- |
| 201701 | 64694 | 257708 | 713 |
| 201702 | 62192 | 233373 | 733 |
| 201703 | 69931 | 259522 | 993 |

### Query 02: Bounce rate per traffic source in July 2017 (Bounce_rate = num_bounce/total_visit) (order by total_visit DESC)

<img width="709" height="186" alt="image" src="https://github.com/user-attachments/assets/b1b2226f-5db2-4b42-a88e-02a5a0a4afc2" />



| source | total_visits | total_no_of_bounces | bounce_rate (%) |
| --- | --- | --- | --- |
| google | 38400 | 19798 | 51.55729167 |
| (direct) | 19891 | 8606 | 43.2657986 |
| [youtube.com](http://youtube.com/) | 6351 | 4238 | 66.72964887 |
| [analytics.google.com](http://analytics.google.com/) | 1972 | 1064 | 53.95537525 |
| Partners | 1788 | 936 | 52.34899329 |
| [m.facebook.com](http://m.facebook.com/) | 669 | 430 | 64.27503737 |
| [google.com](http://google.com/) | 368 | 183 | 49.72826087 |
| dfa | 302 | 124 | 41.05960265 |
| [sites.google.com](http://sites.google.com/) | 230 | 97 | 42.17391304 |
| [facebook.com](http://facebook.com/) | 191 | 102 | 53.40314136 |
| [reddit.com](http://reddit.com/) | 189 | 54 | 28.57142857 |
| [qiita.com](http://qiita.com/) | 146 | 72 | 49.31506849 |
| [quora.com](http://quora.com/) | 140 | 70 | 50 |
| baidu | 140 | 84 | 60 |
| bing | 111 | 54 | 48.64864865 |
| [mail.google.com](http://mail.google.com/) | 101 | 25 | 24.75247525 |
| yahoo | 100 | 41 | 41 |
| [blog.golang.org](http://blog.golang.org/) | 65 | 19 | 29.23076923 |
| [l.facebook.com](http://l.facebook.com/) | 51 | 45 | 88.23529412 |
| [groups.google.com](http://groups.google.com/) | 50 | 22 | 44 |
| [t.co](http://t.co/) | 38 | 27 | 71.05263158 |
| [google.co.jp](http://google.co.jp/) | 36 | 25 | 69.44444444 |
| [m.youtube.com](http://m.youtube.com/) | 34 | 22 | 64.70588235 |
| [dealspotr.com](http://dealspotr.com/) | 26 | 12 | 46.15384615 |
| [productforums.google.com](http://productforums.google.com/) | 25 | 21 | 84 |
| [support.google.com](http://support.google.com/) | 24 | 16 | 66.66666667 |
| ask | 24 | 16 | 66.66666667 |
| [int.search.tb.ask.com](http://int.search.tb.ask.com/) | 23 | 17 | 73.91304348 |
| [optimize.google.com](http://optimize.google.com/) | 21 | 10 | 47.61904762 |
| [docs.google.com](http://docs.google.com/) | 20 | 8 | 40 |
| [lm.facebook.com](http://lm.facebook.com/) | 18 | 9 | 50 |
| [l.messenger.com](http://l.messenger.com/) | 17 | 6 | 35.29411765 |
| [adwords.google.com](http://adwords.google.com/) | 16 | 7 | 43.75 |
| [duckduckgo.com](http://duckduckgo.com/) | 16 | 14 | 87.5 |
| [google.co.uk](http://google.co.uk/) | 15 | 7 | 46.66666667 |
| [sashihara.jp](http://sashihara.jp/) | 14 | 8 | 57.14285714 |
| [lunametrics.com](http://lunametrics.com/) | 13 | 8 | 61.53846154 |
| [search.mysearch.com](http://search.mysearch.com/) | 12 | 11 | 91.66666667 |
| [outlook.live.com](http://outlook.live.com/) | 10 | 7 | 70 |
| [tw.search.yahoo.com](http://tw.search.yahoo.com/) | 10 | 8 | 80 |
| [phandroid.com](http://phandroid.com/) | 9 | 7 | 77.77777778 |
| [plus.google.com](http://plus.google.com/) | 8 | 2 | 25 |
| [connect.googleforwork.com](http://connect.googleforwork.com/) | 8 | 5 | 62.5 |
| [m.yz.sm.cn](http://m.yz.sm.cn/) | 7 | 5 | 71.42857143 |
| [google.co.in](http://google.co.in/) | 6 | 3 | 50 |
| [search.xfinity.com](http://search.xfinity.com/) | 6 | 6 | 100 |


### Query 3: Revenue by traffic source by week, by month in June 2017

<img width="744" height="311" alt="image" src="https://github.com/user-attachments/assets/a984e021-3881-4865-b7c8-968d3144b550" />
<img width="743" height="402" alt="image" src="https://github.com/user-attachments/assets/17276e8f-bbbc-4585-a803-8971cb0e5df5" />


| time_type | month | source | revenue |
| --- | --- | --- | --- |
| Month | 201706 | dfa | 8862.229996 |
| Month | 201706 | youtube.com | 16.99 |
| Month | 201706 | chat.google.com | 74.03 |
| Month | 201706 | mail.google.com | 2563.13 |
| Month | 201706 | google.com | 23.99 |
| Month | 201706 | mail.aol.com | 64.849998 |
| Month | 201706 | (direct) | 97333.619695 |
| Month | 201706 | google | 18757.17992 |
| Month | 201706 | sites.google.com | 39.17 |
| Month | 201706 | phandroid.com | 52.95 |
| Month | 201706 | dealspotr.com | 72.95 |
| Month | 201706 | l.facebook.com | 12.48 |
| Month | 201706 | yahoo | 20.39 |
| Month | 201706 | groups.google.com | 101.96 |
| Month | 201706 | search.myway.com | 105.939998 |
| Month | 201706 | bing | 13.98 |
| Week | 201726 | google | 5330.569964 |
| Week | 201725 | google.com | 23.99 |
| Week | 201725 | groups.google.com | 38.59 |
| Week | 201725 | sites.google.com | 25.19 |
| Week | 201723 | search.myway.com | 105.939998 |
| Week | 201724 | dealspotr.com | 72.95 |
| Week | 201724 | dfa | 2341.56 |
| Week | 201725 | google | 1006.099991 |
| Week | 201726 | groups.google.com | 63.37 |
| Week | 201722 | (direct) | 6888.899975 |
| Week | 201722 | dfa | 1670.649998 |
| Week | 201723 | (direct) | 17325.679919 |
| Week | 201725 | phandroid.com | 52.95 |
| Week | 201725 | mail.aol.com | 64.849998 |
| Week | 201723 | google | 1083.949999 |
| Week | 201724 | (direct) | 30908.909927 |
| Week | 201724 | bing | 13.98 |
| Week | 201724 | mail.google.com | 2486.86 |
| Week | 201725 | (direct) | 27295.319924 |
| Week | 201726 | (direct) | 14914.80995 |
| Week | 201726 | yahoo | 20.39 |
| Week | 201726 | dfa | 3704.74 |
| Week | 201722 | google | 2119.38999 |
| Week | 201725 | mail.google.com | 76.27 |
| Week | 201723 | dfa | 1145.279998 |
| Week | 201723 | youtube.com | 16.99 |
| Week | 201722 | sites.google.com | 13.98 |
| Week | 201723 | chat.google.com | 74.03 |
| Week | 201724 | google | 9217.169976 |
| Week | 201724 | l.facebook.com | 12.48 |


### Query 04: Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017.

<img width="869" height="291" alt="image" src="https://github.com/user-attachments/assets/78b53c57-2692-49ff-88db-dc5c97cba7a9" />
<img width="866" height="420" alt="image" src="https://github.com/user-attachments/assets/e7c82f8f-c879-49ce-863b-3d11e12989eb" />
<img width="424" height="152" alt="image" src="https://github.com/user-attachments/assets/99f86503-4b5a-40e5-8a6e-8010f8c2e873" />




| month | avg_pageviews_purchase | avg_pageviews_non_purchase |
| --- | --- | --- |
| 201706 | 94.02050114 | 316.8655885 |
| 201707 | 124.2375519 | 334.0565598 |

### Query 05: Average number of transactions per user that made a purchase in July 2017

<img width="923" height="201" alt="image" src="https://github.com/user-attachments/assets/160ed1a7-1a96-41b5-a441-b45a6d1b9c61" />



| month | Avg_total_transactions_per_user |
| --- | --- |
| 201707 | 4.163900415 |

### Query 06: Average amount of money spent per session. Only include purchaser data in July 2017

<img width="984" height="203" alt="image" src="https://github.com/user-attachments/assets/05db1668-bf68-42d8-bc46-92f3a9f3b62b" />



| month | avg_revenue_by_user_per_visit |
| --- | --- |
| 201707 | 43.85659835 |

### Query 07: Other products purchased by customers who purchased product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered.

![7](https://github.com/user-attachments/assets/503cef2b-7df4-4def-9a73-28fe9c8e14b8)


| ProductName | quantity |
| --- | --- |
| Google Sunglasses | 20 |
| Google Women's Vintage Hero Tee Black | 7 |
| SPF-15 Slim & Slender Lip Balm | 6 |
| Google Women's Short Sleeve Hero Tee Red Heather | 4 |
| YouTube Men's Fleece Hoodie Black | 3 |
| Google Men's Short Sleeve Badge Tee Charcoal | 3 |
| Red Shine 15 oz Mug | 2 |
| YouTube Twill Cap | 2 |
| Google Doodle Decal | 2 |
| Crunch Noise Dog Toy | 2 |
| Android Men's Vintage Henley | 2 |
| 22 oz YouTube Bottle Infuser | 2 |
| Android Wool Heather Cap Heather/Black | 2 |
| Google Men's Short Sleeve Hero Tee Charcoal | 2 |
| Recycled Mouse Pad | 2 |
| Android Women's Fleece Hoodie | 2 |
| YouTube Women's Short Sleeve Tri-blend Badge Tee Charcoal | 1 |
| Google Slim Utility Travel Bag | 1 |
| Google Men's Performance Full Zip Jacket Black | 1 |
| YouTube Women's Short Sleeve Hero Tee Charcoal | 1 |
| Google Toddler Short Sleeve T-shirt Grey | 1 |
| YouTube Custom Decals | 1 |
| YouTube Men's Short Sleeve Hero Tee Black | 1 |
| Google Men's Vintage Badge Tee Black | 1 |
| Google Men's Bike Short Sleeve Tee Charcoal | 1 |
| Android Sticker Sheet Ultra Removable | 1 |
| Google 5-Panel Cap | 1 |
| Google Twill Cap | 1 |
| Google Men's Pullover Hoodie Grey | 1 |
| 26 oz Double Wall Insulated Bottle | 1 |
| YouTube Men's Short Sleeve Hero Tee White | 1 |
| Google Men's  Zip Hoodie | 1 |
| Google Men's Long & Lean Tee Charcoal | 1 |
| Google Men's Performance 1/4 Zip Pullover Heather/Black | 1 |
| 8 pc Android Sticker Sheet | 1 |
| Google Men's Long Sleeve Raglan Ocean Blue | 1 |
| Four Color Retractable Pen | 1 |
| Android Men's Pep Rally Short Sleeve Tee Navy | 1 |
| Google Men's Vintage Badge Tee White | 1 |
| Google Women's Long Sleeve Tee Lavender | 1 |
| Google Laptop and Cell Phone Stickers | 1 |
| Android Men's Short Sleeve Hero Tee White | 1 |
| Google Men's Airflow 1/4 Zip Pullover Black | 1 |
| Android Men's Vintage Tank | 1 |
| YouTube Men's Long & Lean Tee Charcoal | 1 |
| Android Men's Short Sleeve Hero Tee Heather | 1 |
| YouTube Hard Cover Journal | 1 |
| Android BTTF Moonshot Graphic Tee | 1 |
| Google Men's Long & Lean Tee Grey | 1 |
| Google Men's 100% Cotton Short Sleeve Hero Tee Red | 1 |

### Query 08: Calculate cohort map from product view to addtocart to purchase in Jan, Feb and March 2017. For example, 100% product view then 40% add_to_cart and 10% purchase. Add_to_cart_rate = number product add to cart/number product view. Purchase_rate = number product purchase/number product view. The output should be calculated in product level.

![8](https://github.com/user-attachments/assets/7f2b1c5e-1bc5-4103-87f2-4a2046374d39)


| month | num_product_view | num_addtocart | num_purchase | add_to_cart_rate | purchase_rate |
| --- | --- | --- | --- | --- | --- |
| 201701 | 25787 | 7342 | 2143 | 28.47171055 | 8.310388956 |
| 201702 | 21489 | 7360 | 2060 | 34.25008144 | 9.586299967 |
| 201703 | 23549 | 8782 | 2977 | 37.29245403 | 12.64172576 |

## IV. Conclusion

- By exploring eCommerce dataset, I have gained valuable information about total visits, pageview, transactions, bounce rate, and revenue per traffic source,.... which could inform future business decisions.
- To deep dive into the insights and key trends, the next step will visualize the data with some software like
Power Bi, Tableau,..
- Overall, this project has demonstrated the power of using SQL and big data tools like Google BigQuery to gain insights into large datasets.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          
