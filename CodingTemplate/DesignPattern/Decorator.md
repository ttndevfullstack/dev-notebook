
# Decorator Pattern in a Real-World E-commerce OrderService

## Overview

This example demonstrates the **Decorator Pattern** applied to an e-commerce order processing service. Using decorators, you can add logging and notification features to order service without modifying the core order logic.

---

## What is the Decorator Pattern?

The **Decorator Pattern** is a structural design pattern that lets you dynamically add new responsibilities to objects by "wrapping" them in decorator classes. This approach keeps your code modular, maintainable, and open for extension.

---

## Why Use the Decorator Pattern?

- **No Core Modifications:** Add features such as logging, notification, loyalty, or fraud checks without changing the core order process.
- **Separation of Concerns:** Each decorator handles one feature, resulting in clean, understandable, and testable code.
- **Flexible & Stackable:** Easily combine multiple features by wrapping decorators as needed.
- **Reusability:** Use the same decorators in other services if necessary.

---

## When to Use?

- When you need to add new behaviors to old object **without touching core logic** and separate concepts.

---

## How Does It Work?

### Structure

- **OrderService Interface:** Declares `placeOrder()` method.
- **BasicOrderService:** Implements the actual order process (e.g., updating stock, saving the order).
- **OrderServiceDecorator:** Base decorator that delegates calls to the wrapped `OrderService`.
- **LoggingOrderService:** Adds logging before and after placing the order.
- **NotificationOrderService:** Sends email notifications after order placement.
- **LoyaltyOrderService:** Adds loyalty points after order placement.

### Code Example

```php
<?php

interface IOrderService
{
    public function placeOrder(array $order): array;
}

class OrderService implements IOrderService
{
    public function placeOrder(array $order): array
    {
        return $order;
    }
}

class OrderServiceDecorator implements IOrderService
{
    private OrderService $orderService;

    public function __construct(OrderService $orderService)
    {
        $this->orderService = $orderService;
    }

    public function placeOrder(array $order): array
    {
        return $this->orderService->placeOrder($order);
    }
}

class LoggingOrderService extends OrderServiceDecorator
{    
    public function placeOrder(array $order): array
    {
        echo "[INFO]: A Order is placed successfully\n";
        return parent::placeOrder($order);
    }
}

class NotificationOrderService extends OrderServiceDecorator
{
    public function placeOrder(array $order): array
    {
        echo "[EVENT]: Sent a email order completed notification...\n";
        return parent::placeOrder($order);
    }
}

$orderService = new OrderService();
$loggingOrderService = new LoggingOrderService($orderService);
$notificationOrderService = new NotificationOrderService($orderService);

$order = ['product_name' => 'Macbook Air', 'price' => 20000000, 'quantity' => 1];
$orderService->placeOrder($order);
$loggingOrderService->placeOrder($order);
$notificationOrderService->placeOrder($order)
```

---

## Benefits Recap

- **Single Responsibility:** Each decorator adds one feature (logging, notification, loyalty, etc.).
- **No Core Changes:** Main order logic stays untouched and safe.
- **Flexible:** Easily add, remove, or reorder features via decorator stacking.
- **Testability:** Each decorator can be tested independently.

