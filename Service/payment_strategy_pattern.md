# 🚀 Getting Started

**Payment Service using Strategy Design Pattern**

## Step 1: Create IPaymentInterface

```bash

interface IPaymentStrategy
{
    public function process($order);
}

```

## Step 2: Create PaymentStrategy implementation IPaymentInterface

```bash

class CreditCardPaymentStrategy implements IPaymentStrategy
{
    public function process($order)
    {
        echo "Payment with CreditCard: {$order['id']}...";
    }
}

class PayPalPaymentStrategy implements IPaymentStrategy
{
    public function process($order)
    {
        echo "Payment with PayPal: {$order['id']}...";
    }
}

class StripePaymentStrategy implements IPaymentStrategy
{
    public function process($order)
    {
        echo "Payment with Stripe: {$order['id']}...";
    }
}

```

## Step 3: Create PaymentService

```bash

class PaymentService
{
    protected $strategy;

    public function __construct(IPaymentStrategy $strategy)
    {
        $this->strategy = $strategy;
    }

    public function processPayment($order)
    {
        return $this->strategy->process($order);
    }
}

```

## Step 4: Create PaymentStrategyFactory

```bash

class PaymentStrategyFactory
{
    public static function create($paymentMethod)
    {
        switch ($paymentMethod) {
            case 'CreditCard':
                return new CreditCardPaymentStrategy();
            case 'PayPal':
                return new PayPalPaymentStrategy();
            case 'Stripe':
                return new StripePaymentStrategy();
            default:
                throw new Exception("Unsupported payment method: $paymentMethod");
        }
    }
}

```

## Step 5: Use in OrderController

```bash

class OrderController
{
    public function store()
    {
        $order = [
            'id' => 123,
            'amount' => 100,
            'name' => 'John',
            'payment_method' => 'CreditCard'
        ];

        $paymentStrategy = PaymentStrategyFactory::create($order['payment_method']);
        $paymentService = new PaymentService($paymentStrategy);
        $paymentService->processPayment($order);
    }
}


$orderController = new OrderController();
$orderController->store();

```

## Full Code

```bash

<?php

interface IPaymentStrategy
{
    public function process($order);
}

class CreditCardPaymentStrategy implements IPaymentStrategy
{
    public function process($order)
    {
        echo "Payment with CreditCard: {$order['id']}...";
    }
}

class PayPalPaymentStrategy implements IPaymentStrategy
{
    public function process($order)
    {
        echo "Payment with PayPal: {$order['id']}...";
    }
}

class StripePaymentStrategy implements IPaymentStrategy
{
    public function process($order)
    {
        echo "Payment with Stripe: {$order['id']}...";
    }
}

class PaymentService
{
    protected $strategy;

    public function __construct(IPaymentStrategy $strategy)
    {
        $this->strategy = $strategy;
    }

    public function processPayment($order)
    {
        return $this->strategy->process($order);
    }
}

class PaymentStrategyFactory
{
    public static function create($paymentMethod)
    {
        switch ($paymentMethod) {
            case 'CreditCard':
                return new CreditCardPaymentStrategy();
            case 'PayPal':
                return new PayPalPaymentStrategy();
            case 'Stripe':
                return new StripePaymentStrategy();
            default:
                throw new Exception("Unsupported payment method: $paymentMethod");
        }
    }
}

class OrderController
{
    public function store()
    {
        $order = [
            'id' => 123,
            'amount' => 100,
            'name' => 'John',
            'payment_method' => 'CreditCard'
        ];

        $paymentStrategy = PaymentStrategyFactory::create($order['payment_method']);
        $paymentService = new PaymentService($paymentStrategy);
        $paymentService->processPayment($order);
    }
}


$orderController = new OrderController();
$orderController->store();

```