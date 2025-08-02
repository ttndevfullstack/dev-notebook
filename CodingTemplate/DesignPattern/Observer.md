# Observer Design Pattern Guide

## 1. What is Observer Design Pattern? (WWWH)

- **What:** Observer is a behavioral design pattern that defines a one-to-many dependency between objects, where a subject notifies multiple observers automatically about any state changes.
- **Why:** To decouple the subject from dependent objects, allowing flexible subscription and notification without hardcoded dependencies.
- **When:** When multiple parts of the system need to be updated automatically after a certain object changes its state.
- **How:** The subject maintains a list of observers and provides methods to add/remove observers. When the state changes, it calls `notify()` on all observers.

---

## 2. Real-World Problem in an E-commerce System

In an e-commerce platform, when a new order is placed:
- Customers should receive an **Email** confirmation.
- Administrators should receive an **SMS** notification.
- A **CRM system** may need to log the new order for analytics.

Hardcoding these actions inside the `OrderService` class tightly couples it to each notification channel. Adding new channels (e.g., Push Notification) forces modifying the service, breaking the **Open/Closed Principle**.

---

## 3. Solution using Observer Design Pattern

We can decouple the notification logic using Observer pattern:

- **Subject:** `OrderService` – Responsible for order placement and notifying observers.
- **Observers:**
  - `EmailNotificationObserver` – Sends order confirmation email to customer.
  - `SmsAdminNotificationObserver` – Sends SMS to admin for new order.
  - `CrmLoggerObserver` – Logs the new order to CRM.

When a new order is placed, `OrderService` triggers `notifyObservers()` without knowing details of observers.

---

### Example Code (PHP)

```php
<?php
// Observer.php

interface OrderObserver
{
    public function update(int $orderId, array $orderData): void;
}

class EmailNotificationObserver implements OrderObserver
{
    public function update(int $orderId, array $orderData): void
    {
        echo "📧 Gửi email tới {$orderData['customer_email']} xác nhận đơn hàng #$orderId<br>";
    }
}

class SmsAdminNotificationObserver implements OrderObserver
{
    public function update(int $orderId, array $orderData): void
    {
        echo "📱 Gửi SMS tới admin: Đơn hàng mới #$orderId từ {$orderData['customer_name']}<br>";
    }
}

class CrmLoggerObserver implements OrderObserver
{
    public function update(int $orderId, array $orderData): void
    {
        echo "📝 Ghi log vào CRM cho đơn hàng #$orderId<br>";
    }
}

class OrderService
{
    private array $observers = [];

    public function addObserver(OrderObserver $observer): void
    {
        $this->observers[] = $observer;
    }

    public function removeObserver(OrderObserver $observer): void
    {
        $this->observers = array_filter($this->observers, fn($obs) => $obs !== $observer);
    }

    private function notifyObservers(int $orderId, array $orderData): void
    {
        foreach ($this->observers as $observer) {
            $observer->update($orderId, $orderData);
        }
    }

    public function placeOrder(array $orderData): void
    {
        // 1️⃣ Save order to database (simulated)
        $orderId = rand(1000, 9999);
        echo "✅ Đơn hàng #$orderId đã được tạo.<br>";

        // 2️⃣ Notify all observers
        $this->notifyObservers($orderId, $orderData);
    }
}

// Usage
$orderService = new OrderService();
$orderService->addObserver(new EmailNotificationObserver());
$orderService->addObserver(new SmsAdminNotificationObserver());
$orderService->addObserver(new CrmLoggerObserver());

$orderService->placeOrder([
    'customer_name' => 'Nguyen Van A',
    'customer_email' => 'nguyenvana@example.com',
    'items' => ['iPhone 15', 'AirPods Pro']
]);
```

---

✅ **Benefits:**
- `OrderService` focuses only on order creation, not notification details.
- Adding/removing notification channels requires no changes to `OrderService`.
- Follows **Open/Closed Principle** and makes the system scalable.

