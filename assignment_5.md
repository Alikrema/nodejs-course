---
layout: page
title: Session 7
parent: Assignments
---

# MongoDB Aggregation Practice Questions - Practical Queries (Part 1)

Assume a `products` collection with the following structure:
```
{
  _id: ObjectId,
  name: String,
  category: String,
  price: Number,
  stock: Number,
  supplier: {
    name: String,
    country: String
  },
  ratings: [
    { user: String, score: Number }
  ]
}
```

Write MongoDB aggregation pipelines to answer the following questions:

1. Find the average price of products in each category.

2. List the top 5 most expensive products.

3. Count the number of products in each category that have a stock quantity greater than 20.

4. Calculate the total value (price * stock) of all products for each supplier.

5. Find the products with an average rating score higher than 4.5.

6. Group products by their supplier's country and calculate the average price for each country.

7. Find the category with the highest total stock.

8. List all products that have more than 3 ratings, along with their average rating score.

9. Calculate the percentage of products in each category compared to the total number of products.

10. Find the supplier with the most diverse product range (i.e., supplies products in the most categories).

# MongoDB Aggregation Practice Questions - Practical Queries (Part 2)

In addition to the `products` collection from Part 1, we now have an `orders` collection with the following structure:
```
{
  _id: ObjectId,
  orderId: String,
  customerId: String,
  orderDate: Date,
  items: [
    {
      productId: ObjectId,
      quantity: Number,
      price: Number
    }
  ],
  status: String
}
```

Continue writing MongoDB aggregation pipelines to answer the following questions:

11. Find the top 3 customers who have spent the most money across all their orders.

12. Calculate the total revenue for each product (using the `orders` collection) and combine it with the product details from the `products` collection.

13. List all products that have never been ordered.

14. Find the average order value for each month in the year 2023.

15. Identify the category with the highest total revenue.

16. For each supplier, calculate the percentage of their products that have been ordered at least once.

17. Find the customers who have ordered products from all available categories.

18. Calculate the running total of order values for each customer, sorted by order date.

19. Identify the products that have been ordered more times than they currently have in stock.

20. For each country (of suppliers), find the category that generates the most revenue from orders.

# Solution Manual

## Part 1 Solutions

1. Find the average price of products in each category.
```javascript
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      averagePrice: { $avg: "$price" }
    }
  }
])
```
Explanation: This pipeline groups products by category and calculates the average price for each group.

2. List the top 5 most expensive products.
```javascript
db.products.aggregate([
  { $sort: { price: -1 } },
  { $limit: 5 },
  { $project: { name: 1, price: 1 } }
])
```
Explanation: This pipeline sorts products by price in descending order, limits the result to 5 documents, and projects only the name and price fields.

3. Count the number of products in each category that have a stock quantity greater than 20.
```javascript
db.products.aggregate([
  { $match: { stock: { $gt: 20 } } },
  {
    $group: {
      _id: "$category",
      count: { $sum: 1 }
    }
  }
])
```
Explanation: This pipeline first filters products with stock greater than 20, then groups them by category and counts the documents in each group.

4. Calculate the total value (price * stock) of all products for each supplier.
```javascript
db.products.aggregate([
  {
    $group: {
      _id: "$supplier.name",
      totalValue: { $sum: { $multiply: ["$price", "$stock"] } }
    }
  }
])
```
Explanation: This pipeline groups products by supplier name and calculates the sum of price multiplied by stock for each group.

5. Find the products with an average rating score higher than 4.5.
```javascript
db.products.aggregate([
  {
    $project: {
      name: 1,
      avgRating: { $avg: "$ratings.score" }
    }
  },
  { $match: { avgRating: { $gt: 4.5 } } }
])
```
Explanation: This pipeline calculates the average rating for each product, then filters for products with an average rating greater than 4.5.

6. Group products by their supplier's country and calculate the average price for each country.
```javascript
db.products.aggregate([
  {
    $group: {
      _id: "$supplier.country",
      avgPrice: { $avg: "$price" }
    }
  }
])
```
Explanation: This pipeline groups products by the supplier's country and calculates the average price for each country.

7. Find the category with the highest total stock.
```javascript
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      totalStock: { $sum: "$stock" }
    }
  },
  { $sort: { totalStock: -1 } },
  { $limit: 1 }
])
```
Explanation: This pipeline groups products by category and sums the stock, then sorts by total stock in descending order and limits to the top result.

8. List all products that have more than 3 ratings, along with their average rating score.
```javascript
db.products.aggregate([
  {
    $project: {
      name: 1,
      ratingCount: { $size: "$ratings" },
      avgRating: { $avg: "$ratings.score" }
    }
  },
  { $match: { ratingCount: { $gt: 3 } } }
])
```
Explanation: This pipeline calculates the number of ratings and average rating score for each product, then filters for products with more than 3 ratings.

9. Calculate the percentage of products in each category compared to the total number of products.
```javascript
db.products.aggregate([
  {
    $group: {
      _id: null,
      total: { $sum: 1 },
      categories: { $push: "$category" }
    }
  },
  { $unwind: "$categories" },
  {
    $group: {
      _id: "$categories",
      count: { $sum: 1 },
      total: { $first: "$total" }
    }
  },
  {
    $project: {
      category: "$_id",
      percentage: {
        $multiply: [{ $divide: ["$count", "$total"] }, 100]
      }
    }
  }
])
```
Explanation: This pipeline first counts the total number of products and collects all categories. It then unwinds the categories, groups again to count products in each category, and finally calculates the percentage.

10. Find the supplier with the most diverse product range (i.e., supplies products in the most categories).
```javascript
db.products.aggregate([
  {
    $group: {
      _id: "$supplier.name",
      categories: { $addToSet: "$category" }
    }
  },
  {
    $project: {
      supplier: "$_id",
      categoryCount: { $size: "$categories" }
    }
  },
  { $sort: { categoryCount: -1 } },
  { $limit: 1 }
])
```
Explanation: This pipeline groups products by supplier and collects unique categories. It then counts the number of unique categories for each supplier, sorts by this count, and returns the top result.

## Part 2 Solutions

11. Find the top 3 customers who have spent the most money across all their orders.
```javascript
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $group: {
      _id: "$customerId",
      totalSpent: { $sum: { $multiply: ["$items.quantity", "$items.price"] } }
    }
  },
  { $sort: { totalSpent: -1 } },
  { $limit: 3 }
])
```
Explanation: This pipeline unwinds the items array, groups by customer to calculate total spent, sorts by total spent in descending order, and limits to the top 3 results.

12. Calculate the total revenue for each product (using the `orders` collection) and combine it with the product details from the `products` collection.
```javascript
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $group: {
      _id: "$items.productId",
      totalRevenue: { $sum: { $multiply: ["$items.quantity", "$items.price"] } }
    }
  },
  {
    $lookup: {
      from: "products",
      localField: "_id",
      foreignField: "_id",
      as: "productDetails"
    }
  },
  { $unwind: "$productDetails" },
  {
    $project: {
      productName: "$productDetails.name",
      category: "$productDetails.category",
      totalRevenue: 1
    }
  }
])
```
Explanation: This pipeline calculates total revenue for each product from the orders collection, then uses $lookup to join with the products collection for additional details.

13. List all products that have never been ordered.
```javascript
db.products.aggregate([
  {
    $lookup: {
      from: "orders",
      let: { productId: "$_id" },
      pipeline: [
        { $unwind: "$items" },
        { $match: { $expr: { $eq: ["$items.productId", "$$productId"] } } }
      ],
      as: "orders"
    }
  },
  { $match: { orders: { $size: 0 } } },
  { $project: { name: 1, category: 1 } }
])
```
Explanation: This pipeline uses $lookup to find matching orders for each product, then filters for products with no matching orders.

14. Find the average order value for each month in the year 2023.
```javascript
db.orders.aggregate([
  { $match: { orderDate: { $gte: new Date("2023-01-01"), $lt: new Date("2024-01-01") } } },
  { $unwind: "$items" },
  {
    $group: {
      _id: { $dateToString: { format: "%Y-%m", date: "$orderDate" } },
      totalValue: { $sum: { $multiply: ["$items.quantity", "$items.price"] } },
      count: { $sum: 1 }
    }
  },
  {
    $project: {
      month: "$_id",
      averageOrderValue: { $divide: ["$totalValue", "$count"] }
    }
  },
  { $sort: { month: 1 } }
])
```
Explanation: This pipeline filters orders from 2023, calculates the total value for each month, and then computes the average order value.

15. Identify the category with the highest total revenue.
```javascript
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $lookup: {
      from: "products",
      localField: "items.productId",
      foreignField: "_id",
      as: "product"
    }
  },
  { $unwind: "$product" },
  {
    $group: {
      _id: "$product.category",
      totalRevenue: { $sum: { $multiply: ["$items.quantity", "$items.price"] } }
    }
  },
  { $sort: { totalRevenue: -1 } },
  { $limit: 1 }
])
```
Explanation: This pipeline calculates total revenue for each category by joining orders with products, then finds the category with the highest total revenue.

16. For each supplier, calculate the percentage of their products that have been ordered at least once.
```javascript
db.products.aggregate([
  {
    $lookup: {
      from: "orders",
      let: { productId: "$_id" },
      pipeline: [
        { $unwind: "$items" },
        { $match: { $expr: { $eq: ["$items.productId", "$$productId"] } } }
      ],
      as: "orders"
    }
  },
  {
    $group: {
      _id: "$supplier.name",
      totalProducts: { $sum: 1 },
      orderedProducts: {
        $sum: { $cond: [{ $gt: [{ $size: "$orders" }, 0] }, 1, 0] }
      }
    }
  },
  {
    $project: {
      supplier: "$_id",
      percentageOrdered: {
        $multiply: [
          { $divide: ["$orderedProducts", "$totalProducts"] },
          100
        ]
      }
    }
  }
])
```
Explanation: This pipeline checks which products have been ordered, groups by supplier, and calculates the percentage of products that have been ordered for each supplier.

17. Find the customers who have ordered products from all available categories.
```javascript
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $lookup: {
      from: "products",
      localField: "items.productId",
      foreignField: "_id",
      as: "product"
    }
  },
  { $unwind: "$product" },
  {
    $group: {
      _id: "$customerId",
      categories: { $addToSet: "$product.category" }
    }
  },
  {
    $lookup: {
      from: "products",
      pipeline: [
        { $group: { _id: null, allCategories: { $addToSet: "$category" } } }
      ],
      as: "allCategories"
    }
  },
  { $unwind: "$allCategories" },
  {
    $project: {
      customer: "$_id",
      hasAllCategories: {
        $eq: [
          { $size: "$categories" },
          { $size: "$allCategories.allCategories" }
        ]
      }
    }
  },
  { $match: { hasAllCategories: true } }
])
```
Explanation: This pipeline finds the categories ordered by each customer, compares it with all available categories, and filters for customers who have ordered from all categories.

18. Calculate the running total of order values for each customer, sorted by order date.
```javascript
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $group: {
      _id: { customerId: "$customerId", orderId: "$orderId", orderDate: "$orderDate" },
      orderValue: { $sum: { $multiply: ["$items.quantity", "$items.price"] } }
    }
  },
  { $sort: { "_id.customerId": 1, "_id.orderDate": 1 } },
  {
    $group: {
      _id: "$_id.customerId",
      orders: {
        $push: {
          orderId: "$_id.orderId",
          orderDate: "$_id.orderDate",
          orderValue: "$orderValue"
        }
      }
    }
  },
  {
    $project: {
      customer: "$_id",
      orders: {
        $map: {
          input: "$orders",
          as: "order",
          in: {
            orderId: "$$order.orderId",
            orderDate: "$$order.orderDate",
            orderValue: "$$order.orderValue",
            runningTotal: {
              $sum: {
                $map: {
                  input: {
                    $filter: {
                      input: "$orders",
                      as: "o",
                      cond: { $lte: ["$$o.orderDate", "$$order.orderDate"] }
                    }
                  },
                  as: "o",
                  in: "$$o.orderValue"
                }
              }
            }
          }
        }
      }
    }
  }
])
```
Explanation: This complex pipeline calculates order values, sorts them by date, and then computes a running total for each customer's orders.

19. Identify the products that have been ordered more times than they currently have in stock.
```javascript
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $group: {
      _id: "$items.productId",
      totalOrdered: { $sum: "$items.quantity" }
    }
  },
  {
    $lookup: {
      from: "products",
      localField: "_id",
      foreignField: "_id",
      as: "product"
    }
  },
  { $unwind: "$product" },
  {
    $project: {
      productName: "$product.name",
      totalOrdered: 1,
      stock: "$product.stock",
      exceedsStock: { $gt: ["$totalOrdered", "$product.stock"] }
    }
  },
  { $match: { exceedsStock: true } }
])
```
Explanation: This pipeline calculates total ordered quantity for each product, joins with the products collection to get current stock, and filters for products where ordered quantity exceeds current stock.

20. For each country (of suppliers), find the category that generates the most revenue from orders.
```javascript
db.orders.aggregate([
  { $unwind: "$items" },
  {
    $lookup: {
      from: "products",
      localField: "items.productId",
      foreignField: "_id",
      as: "product"
    }
  },
  { $unwind: "$product" },
  {
    $group: {
      _id: {
        country: "$product.supplier.country",
        category: "$product.category"
      },
      revenue: { $sum: { $multiply: ["$items.quantity", "$items.price"] } }
    }
  },
  {
    $sort: { "_id.country": 1, revenue: -1 }
  },
  {
    $group: {
      _id: "$_id.country",
      topCategory: { $first: "$_id.category" },
      maxRevenue: { $first: "$revenue" }
    }
  },
  {
    $project: {
      country: "$_id",
      topCategory: 1,
      maxRevenue: 1,
      _id: 0
    }
  },
  { $sort: { country: 1 } }
])
```

Explanation: 
1. First, we `$unwind` the `items` array in the orders collection to work with individual items.
2. We then use `$lookup` to join with the products collection to get product and supplier information.
3. We `$group` by both country and category, calculating the total revenue for each combination.
4. We `$sort` first by country (ascending) and then by revenue (descending) within each country.
5. We `$group` again, this time only by country. We use `$first` to get the top category and its revenue (since we sorted in descending order of revenue in the previous stage).
6. Finally, we `$project` to reshape our output and sort the results by country name.

This pipeline effectively finds the highest-grossing category for each supplier country, giving us insights into which product categories are most successful in different geographical areas.