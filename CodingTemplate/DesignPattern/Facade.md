# Facade Design Pattern Guide

## 1. What is Facade Design Pattern? (WWWH)

- **What:** Facade is a structural design pattern that provides a simplified, unified interface to a set of interfaces in a subsystem.
- **Why:** To hide the complexity of a system and make it easier for clients to interact with it.
- **When:** When you have a complex system with multiple subsystems and want to provide a single entry point to reduce dependencies and simplify usage.
- **How:** Create a Facade class that delegates client requests to appropriate subsystem classes, shielding clients from complex interactions.

---

## 2. Real-World Problem in an E-commerce System

When a new order is placed:
- An **Email** confirmation must be sent to the customer.
- An **SMS** notification must be sent to the admin.
- The order details must be logged in the **CRM system**.

If each component is called individually across the system:
- The code is duplicated.
- It's easy to forget one of the notifications.
- It tightly couples order handling code to every notification service.

---

## 3. Solution using Facade Design Pattern

We can centralize and simplify this process:
- Create a **NotificationFacade** class that internally calls EmailService, SmsService, and CrmService.
- Other parts of the system only need to call the facade method.

---

### Example Code (PHP)

```php
<?php

class EmailService
{
    public function send(string $to, string $subject, string $message): void
    {
        echo "📧 Email gửi tới $to: $subject - $message<br>";
    }
}

class SmsService
{
    public function send(string $phone, string $message): void
    {
        echo "📱 SMS gửi tới $phone: $message<br>";
    }
}

class CrmService
{
    public function logOrder(int $orderId, array $data): void
    {
        echo "📝 CRM log: Đơn hàng #$orderId được ghi với dữ liệu: " . json_encode($data) . "<br>";
    }
}

class NotificationFacade
{
    private EmailService $emailService;
    private SmsService $smsService;
    private CrmService $crmService;

    public function __construct(
        private EmailService $emailService,
        private SmsService $smsService,
        private CrmService $crmService,
    ) {}

    public function sendOrderNotifications(int $orderId, array $orderData): void
    {
        $this->emailService->send(
            $orderData['customer_email'],
            "Xác nhận đơn hàng #$orderId",
            "Cảm ơn {$orderData['customer_name']} đã đặt hàng!"
        );

        $this->smsService->send(
            "+84901234567",
            "Đơn hàng mới #$orderId từ {$orderData['customer_name']}"
        );

        $this->crmService->logOrder($orderId, $orderData);
    }
}

// Usage
$orderData = [
    'customer_name' => 'Nguyen Van A',
    'customer_email' => 'nguyenvana@example.com',
    'items' => ['iPhone 15', 'AirPods Pro']
];

$facade = new NotificationFacade();
$facade->sendOrderNotifications(12345, $orderData);
```

---

✅ **Benefits:**
- Simplifies client code by providing a single method call.
- Reduces duplication of notification logic.
- Shields clients from changes in subsystem implementations.

