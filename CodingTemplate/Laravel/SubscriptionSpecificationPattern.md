# 🚀 Getting Started

**Check Rules Of User Specification Buy Subscription Using Strategy Design Pattern**

## Step 1: Create ISpecification

```bash

interface ISpecification
{
    public function isSatisfiedBy(User $user): bool;
}

```

## Step 2: Create Rules ActiveSubscriptionSpecification & NoOverduePaymentsSpecification implementation IPaymentInterface

```bash

class ActiveSubscriptionSpecification implements ISpecification
{
    public function isSatisfiedBy(User $user): bool
    {
        return $user->hasActiveSubscription();
    }
}

class NoOverduePaymentsSpecification implements ISpecification
{
    public function isSatisfiedBy(User $user): bool
    {
        return !$user->hasOverduePayments();
    }
}

```

## Step 3: Create UserOrderEligibilityService

```bash

class UserOrderEligibilityService
{
    protected $specifications;

    public function __construct(array $specifications)
    {
        $this->specifications = $specifications;
    }

    public function canUserPlaceOrder(User $user): bool
    {
        foreach ($this->specifications as $specification) {
            if (!$specification->isSatisfiedBy($user)) {
                return false;
            }
        }

        return true;
    }
}

```

## Step 4: Use in SubscriptionController

```bash

class SubscriptionController
{
    public function store()
    {
        $subscription = [
            'id' => 1,
            'name' => 'ProMax Plan',
            'price' => 199,
        ];

        $specifications = [
            new AgeSpecification(),
            new ActiveSubscriptionSpecification(),
            new NoOverduePaymentsSpecification(),
        ];

        $user = new User(18, true, false);

        $eligibilityService  = new UserOrderEligibilityService($specifications);
        if ($eligibilityService->canUserPlaceOrder($user)) {
            echo "User can place an order.";
        } else {
            echo "User cannot place an order.";
        }
    }
}

$controller = new SubscriptionController();
$controller->store();

```

## Full Code

```bash

<?php

class User
{
    public $age;
    public $activeSubscription;
    public $overduePayments;

    public function __construct($age, $activeSubscription, $overduePayments)
    {
        $this->age = $age;
        $this->activeSubscription = $activeSubscription;
        $this->overduePayments = $overduePayments;
    }

    public function hasActiveSubscription(): bool
    {
        return $this->activeSubscription;
    }

    public function hasOverduePayments(): bool
    {
        return $this->overduePayments;
    }
}

interface ISpecification
{
    public function isSatisfiedBy(User $user): bool;
}

class AgeSpecification implements ISpecification
{
    public function isSatisfiedBy(User $user): bool
    {
        return $user->age >= 18;
    }
}

class ActiveSubscriptionSpecification implements ISpecification
{
    public function isSatisfiedBy(User $user): bool
    {
        return $user->hasActiveSubscription();
    }
}

class NoOverduePaymentsSpecification implements ISpecification
{
    public function isSatisfiedBy(User $user): bool
    {
        return !$user->hasOverduePayments();
    }
}

class UserOrderEligibilityService
{
    protected $specifications;

    public function __construct(array $specifications)
    {
        $this->specifications = $specifications;
    }

    public function canUserPlaceOrder(User $user): bool
    {
        foreach ($this->specifications as $specification) {
            if (!$specification->isSatisfiedBy($user)) {
                return false;
            }
        }

        return true;
    }
}

class SubscriptionController
{
    public function store()
    {
        $subscription = [
            'id' => 1,
            'name' => 'ProMax Plan',
            'price' => 199,
        ];

        $specifications = [
            new AgeSpecification(),
            new ActiveSubscriptionSpecification(),
            new NoOverduePaymentsSpecification(),
        ];

        $user = new User(18, true, false);

        $eligibilityService  = new UserOrderEligibilityService($specifications);
        if ($eligibilityService->canUserPlaceOrder($user)) {
            echo "User can place an order.";
        } else {
            echo "User cannot place an order.";
        }
    }
}

$controller = new SubscriptionController();
$controller->store();


```