# Marketplace Database Design

A simple relational database design for a marketplace platform supporting **Thrift** and **Handmade** products.

## Overview

The database manages users, products, shopping carts, orders, payments, and delivery information.

The design uses separate tables for common product information and category-specific information for Thrift and Handmade products.

## Entities

| Entity            | Purpose                                  |
| ----------------- | ---------------------------------------- |
| `user_profile`    | Stores user account information          |
| `products`        | Stores common product information        |
| `thrift`          | Stores Thrift-specific product details   |
| `handmade`        | Stores Handmade-specific product details |
| `cart`            | Stores users' shopping carts             |
| `cart_items`      | Stores products added to carts           |
| `orders`          | Stores orders placed by users            |
| `order_item`      | Stores products included in an order     |
| `payments`        | Stores payment information               |
| `delivery_status` | Stores order delivery information        |

## Entity Relationships

```text
USER
 │
 ├──── CART
 │       │
 │       └──── CART_ITEMS ──── PRODUCT
 │
 └──── ORDERS
         │
         ├──── ORDER_ITEMS ──── PRODUCT
         │
         ├──── PAYMENTS
         │
         └──── DELIVERY_STATUS

PRODUCT
 │
 ├──── THRIFT
 │
 └──── HANDMADE
```

### Main Relationships

* `user_profile → cart`
* `user_profile → orders`
* `cart → cart_items`
* `products → cart_items`
* `orders → order_item`
* `products → order_item`
* `orders → payments`
* `orders → delivery_status`
* `products → thrift`
* `products → handmade`

## Order Flow

```text
User
 ↓
Add Product to Cart
 ↓
Cart Item
 ↓
Checkout
 ↓
Order
 ↓
Order Item
 ↓
Payment
 ↓
Delivery
```

## Product Types

### Thrift

Thrift products contain additional information such as:

* Product condition
* Thrift-specific features

### Handmade

Handmade products contain:

* Bulk order availability
* Minimum bulk order quantity

## Order History

The `orders` table itself acts as the user's order history.

A user's previous orders can be retrieved using:

```sql
SELECT *
FROM orders
WHERE author_id = ?;
```

## Files

```text
├── schema.er
├── er-diagram.png
└── README.md
```

* **`schema.er`** — Eraser ER diagram source code.
* **`er-diagram.png`** — Visual ER diagram.
* **`README.md`** — Documentation.

## Purpose

This database design provides a structured way to manage the complete marketplace flow, from adding products to a cart through checkout, payment, and delivery.
