# Product-Categorization-
Predicting primary category of a product using product description.

## About the task
- Clean and visualize the data.
- Separate all categories from the product category tree.
- Figure out the primary category from all the categories.
- Build a model to predict the primary category keeping description as the main feature.
- Measure the accuracy of the model.

## Initial cleaning of the dataset
- The task required just 2 columns from the whole dataset, so I first removed the rest of the columns.
- There were 2 rows which had NULL value for “description”. So, I removed these 2 rows. 
 
## Separating the categories from the product category tree
- To achieve this, I converted the item present in the product_category_tree column into a list by splitting the elements using " >> " as our separator. 
- I found out the maximum and minimum number of levels to check how many columns are required to be created to accommodate all the values of the product_category_tree.
- I created lists to store the values of categories at respective levels and added it to the dataset.
 
## Visualizing the dataset and further cleaning it.
First, I visualized the percentage of items having different numbers of levels in the product_category_tree.
I also printed some random data of rows having just 1 level in their product category tree.
From this, I observed that the data items which contain level 1 only are very specific in nature, thus they cannot be the primary category.

Therefore, l removed those items which only had a single level of categorization.
They formed just 1.2% of the whole dataset, thus removing them didn’t affect the dataset much.
 
## Cleaning the text in description column
I performed the following operations to clean the data:
- Removed the numerical values and special characters using Regular Expression Tokenizer.
- Converted all the data into lowercase.
- Removed all the stopwords (such as the, he, have).
- Stemming all the words i.e. converting the words into their root form. 

## Figuring out the primary category
 
### First Approach 
Use the whole product_category_tree as the primary category.
 This approach is <b>very naive and not meant to be used</b> in real-life scenarios as in reality, the end product is always different and thus no two items will ever have the same product_category_tree.
Therefore, automatic classification according to the primary category would be impossible.
 
### Second Approach
Use the level 1 categorization as the primary category for each product. Although this approach reduces the number of unique categories, and can be used to train a classification model, it is <b>not good enough</b> as it still does not give the exact primary category for a particular item.
 
Let us consider an example to justify this,
 
Clothing >> Women's Clothing >> Lingerie, Sleep & Swimwear >> Shorts >> Alisha Shorts >> Alisha Solid Women's Cycling Shorts
 
The second approach will give <b>Clothing</b> as the primary category. But here, Clothing is the <b>Product Group</b>. A product group is basically like an umbrella for related primary categories.
 
For this item, the primary category should be <b>Shorts</b> as the item Alisha Solid Women's Cycling Shorts is a specific kind of shorts.
 
Thus, we cannot use the second approach to find the primary category.

### Third Approach
I visualized some data and made the following observations:
- The product groups formed a huge part of the dataset while primary categories didn’t have that broad range of items.
- When the number of items in a particular category started decreasing in number, we started moving from product groups towards the primary key.
 
I used these observations to figure out an algorithm to find the primary category. I observed that while moving in the product category tree, when the number of items in a particular category became approximately 30% equal to the total items at that level, that specific level was our primary category. But if we reached the last level before that, we have to consider the second last level as the primary category.
 
Also I observed that after level 5, the categories became very specific, thus we didn't have to go beyond that level. Therefore, I used this algorithm only till level 5.
#### Algorithm:
1. Set a threshold percentage value.
2. Start traversing through all the products.
3. Start traversing through the product category tree.
4. Check if we have reached the last element in product_category_tree.
5. If yes, set the previous level as the primary category.
6. If no, go further in the tree to check if the number of items (n) are more than the threshold percentage of total items (th).
7. n < th : set the primary category to current level.
8. n > th : go further in the product_category_tree.

Then, I used the third approach to find the primary category of each of the products and store them in the dataset.

## Preprocessing the data
- Used LabelEncoder() to label the primary category for training.
- Used CountVectorizer() to convert collection of text descriptions to a vector of token counts.
- Divided the data into training and testing sets.

## Building a model and making predictions
I trained three different types of models on the training set : Naive Bayes Classifier, Support Vector Machine (SVM) and Random Forest Classifier.
I optimized the hyperparameters for all the models, trained them and made predictions for the testing set.

## Results
Achieved highest accuracy of <b>96%</b> using <b>SVM Classifier</b> on testing data for prediction of Primary Category from Product Description.



