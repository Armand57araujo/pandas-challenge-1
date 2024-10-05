# pandas-challenge-1

## Technology Used 

| Technology Used | Resource URL | 
| ------------- |:-------------:| 
| Python | [https://www.w3schools.com/python/python_reference.asp](https://www.w3schools.com/python/python_reference.asp) | 
| Git | [https://git-scm.com/](https://git-scm.com/) | 
| Pandas | [https://www.w3schools.com/python/pandas/default.asp](https://www.w3schools.com/python/pandas/default.asp)

## Description 
In this project I created using a dataset from a fictional e-commerce company, I explored, transformed, and analyzed the data, I identifying top customers, popular product categories, calculating profits, and more. 

## Code Example 

<---- First we import the pandas library afor data manipulation and analysis and alias it as pd.
----->


      import pandas as pd

df = pd.read_csv('Resources/client_dataset.csv')
df.head()

<---- In this part we use the describe function to gather some basic statistics ----->


     df.describe()

<---- Next we use this space to do any additional research and familiarize yourself with the data. ----->


     df.head()

<------ Here we determine which three item categories had the most entries, and in conjunction with the category with the most entries, we determine the subcategory has the most entries then which five clients had the most entries in the data, after which we store the client ids of those top 5 clients in a list.------>


     top_categories = df['category'].value_counts().head(3)
     top_categories

     top_subcategory = df.loc[df['category'] == 'consumables','subcategory'].value_counts()
     top_subcategory

     top_clients = df['client_id'].value_counts().head(5)
     top_clients

     top_client_ids = top_clients.index.tolist()
     top_client_ids


<----- Then we 1st create a column that calculates the subtotal for each line using the unit_price and the qty, 2nd a column for shipping price, assuming a shipping price of $7 per pound for orders over 50 pounds and $10 per pound for items 50 pounds or under, 3rd a column for the total price using the subtotal and the shipping price along with a sales tax of 9.25%, 4th a column for the cost of each line using unit cost, qty, and
shipping price (assume the shipping cost is exactly what is charged to the client). Then 4th and finally a column for the profit of each line using line cost and line price------->

     top_client_ids = df.loc[df['client_id'] == 33615,'qty'].sum()
     top_client_ids

     df['subtotal'] = df['unit_price'] * df['qty']
     df.head()

     def shipping_calculator(weight):
         if weight > 50:
        return weight * 7
         return weight * 10

     df['total_weight'] = df['qty'] * df['unit_weight']
     df['shipping_price'] = df['total_weight'].apply(shipping_calculator)
     df[['unit_price', 'unit_weight', 'qty', 'total_weight', 'shipping_price']].head(3)

     df.head()



     df['total_price'] = df['subtotal'] + df['shipping_price']
     df['total_price'] * 1.0925

     df.head()

     df['line_cost'] = df['unit_cost'] * df['qty'] + df['shipping_price']
     df.head()

     df['profit'] = df['total_price'] - df['line_cost']

     df.head()    


        
<----- In this step we create a column for the total price using the subtotal and the shipping price along with a sales tax of 9.25%, then create a column for the cost of each line using unit cost, qty, and
shipping price (assume the shipping cost is exactly what is charged to the client). Create a column for the profit of each line using line cost and line price ------>


     order_ids = [2742071, 2173913, 6128929]
        tax_rate = 1.0925

      def find_order_price(order_id):
          order_rows_df = df.loc[df['order_id'] == order_id, 'total_price']
         total_price = order_rows_df.sum()
         return round(total_price * tax_rate, 2)

     for order_id in order_ids:
         print(f"Order {order_id} total with tax: ${find_order_price(order_id)}")



     top_clients = df['client_id'].loc[df['qty'].isin(df['qty'].value_counts().head(5).index)].unique()


     top_clients_spent = df[df['client_id'].isin(top_clients)].groupby('client_id').agg(
          total_spent=('total_price', 'sum'),
         total_qty=('qty', 'sum')
         ).reset_index()


     top_clients_spent_sorted = top_clients_spent.sort_values(by='total_qty', ascending=False)

     top_clients_spent_sorted.head()



<------ Finally, Format the data and rename the columns to names suitable for presentation then sort the updated data by "Total Profit (millions)" form highest to lowest and assign the sort to a new DatFrame. -------->

      money_cols = ['total_units_purchased', 'total_shipping_price', 'total_revenue', 'total_profit']


     def currency_format_millions(amount):
         return amount/1000000


     for col in money_cols:
          top_clients_summary_sorted[col] = top_clients_summary_sorted[col].apply(currency_format_millions)

 
     rename_dict = {
         'client_id': 'Client ID',
         'line_price': 'Total Revenue (millions)',
         'shipping_price': 'Shipping (millions)',
         'qty': 'Units',
         'line_cost': 'Total Cost (millions)',
         'line_profit': 'Total Profit (millions)'
     }

      summary_df = top_clients_summary_sorted.rename(columns = rename_dict)
      summary_df.head()


         summary_df_sorted = summary_df.sort_values(by='total_profit', ascending=False)
         summary_df_sorted.head()










## Challenges
This project was a massive learning curve, I'd never even heard of data frames much less new they could be explored, analyzed and transformed in such a fashion. The .agg and .apply were very interesting concepts to learn and apply to this project but difficult in some ways to understand in terms of application. I got the hang of it thanks to Geeks for Geeks, W3 and Chat, but it was all very new. Additionally, I kept messing up the calculation for the difference in weight determining the difference in price. That took a good chunk of time until I conferred with my classmates and a tutor to learn the way to go about creating the function.



## Learning Points 
I have a new best friend. Google: Best bud, w3 Schools: Lovely lady friend, ChatGPT: My BOY, and now GeeksForGeeks, .... there are no words... Friends... For Life....


## Author Info
Armand Araujo
Age: 29
Location: Las Vegas, NV

 
* [LinkedIn](https://www.linkedin.com/in/armand-araujo-a82ba2291/) 
* [Github](https://github.com/Armand57araujo) 


## Credits 

W3 Schools, GeeksforGeeks, chatgpt, MDN