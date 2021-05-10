# Fall 2021 Data Science Intern Challenge

## TL;DR 
### Question 1) Short Explaination

Q1(a) What is being done?

- Directly summing up order_amount is being divided by total number of orders. Basically taking the direct mean of order amounts.

Q1(a) What is wrong here?

- There are some outlier orders where the order_amount is large, and total_items could be because of a bulk order/wholsesale buying. These outliers negatively impact our calculation of the should be excuded in calculating the AOV
- The lower item price outlier orders are possibly because of "sale" in particular stores, which could skew data, but cannot be ignored here since they represent actual customers buying sneakers at those prices.
- The higher item price outlier orders are possibly because of the sale of luxury sneakers, which also skew the calculation towards the more expensive side.

Q1(a) Better way to evaluate this dataset?

- Ignoring the bulk orders
- Giving lesser weightage/importance to rarely occuring luxury/discounted goods

Q1(b) What metric would you report for this dataset?

- Weighted AOV based on Relative Frequency of Average Item Price per order

Q1(c) What is its value?

- $218.29

## Question 1) Detailed Explaination

From the given calculated AOV we can see that a simple mean of the order amounts has been taken. Which would be a correct way to calculate AOV given a cleaner dataset. But on closer inspection of the data, we can see the distribution of the _order_amount_ column (given below) isn't uniform and it contains few large outliers which skew our calculation negatively.


![Img of graph plotting various Order Amounts](./plots/AmountvID.png?raw=true "Order Amounts for each Order ID")

It is evident that there are a few orders with amounts in the magnitude of ~700,000$ and the items in the order being 2000, which shows that these order were bulk orders. It is very unusual for a single person to buy 2000 pairs of sneakers at once, which means that we should avoid these bulk orders from our calculation of AOV to get a more meaningful result.

We can safely assume that any single order with more than 50 items could be considered a bulk order, and using that as a filter, we can now clean up our dataset and recalculate the AOV for the data. After this clean up, we end up with an AOV of $754.09 which is more realistic than our original calculation. The new Order Amount distribution looks like:

![Img of graph plotting various Order Amounts after Trimming](./plots/AmountvID2.png?raw=true "Order Amounts for each Order ID after Trimming")

Analyzing the data once more we see that the max order amount is still ~150,000 which is very unrealistic for regular pairs of sneakers. From the shops average price per shoe data it is evident that store 78 is selling shoes with an average item price of ~$25000 which shows that these sneakers might be luxury goods. The minimum value of the order_amount is 90$ which is lower than the usual average sneaker price, which shows that some stores are selling sneakers on a discount possibly on a sale. Looking at the graph of Order Amount v Shop ID this is evident:

![Img of graph plotting various Order Amounts v ShopID](./plots/AmountvShop.png?raw=true "Order Amounts for each Order ID v ShopID")

Excluding extreme values solely due to their extremeness can distort the results by removing information about the variability inherent in the study area. We would be forcing the subject area to appear less variable than it is in reality. Hence why we cannot exclude these Luxury and Discounted orders from our average order value since they represent good data points where a customer has purchased these items regardless of their price.


But since these points are skewing our data, we would need to change our metric accordingly to make sure they only have the intended effect and not more. 
For this reason, I have deicded to use a weighted average of order value as the new metric which would assign weights based on the distribution of the relative average item price. This would lead to the very realistic AOV value that could be used for further statistical analysis.
Looking at the relative frequency plots (given below) we can see the outlier points which show the expensive sneakers:

![Img of graph plotting Relative Frequency of the Average Item Price per order](./plots/RelFreq.png?raw=true "Relative Frequency of the Average Item Price per order")

Looking at a more zoomed in plot we can see a more distributed frequency plot:

![Img of graph plotting Zoomed Relative Frequency of the Average Item Price per order](./plots/ZoomRelFreq.png?raw=true "Zoomed Relative Frequency of the Average Item Price per order")

Using this distribution to the weight the order amount and then calculating the AOV value would provide us with the most realistic average.
That value is $218.29, which lines up with the average sneaker prices which is a relatively affordable item.


