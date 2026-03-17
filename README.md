
# quick_payment

**Flutter SDK for accepting Bangladeshi manual mobile banking payments** like bKash, Nagad, Rocket, and Upay.

Easily integrate mobile payment options into your Flutter app with ready-to-use classes, assets, and UI components.

---

## Features

- Supports multiple payment methods: **bKash, Nagad, Rocket, Upay**
- Simple `PaymentMethod` class for easy integration
- Includes payment logos in assets
- Lightweight and well-documented
- Works with Flutter **>=3.10.0**
- Provides `PaymentData` callback on submission for DB storage or manual approval

---

## Installation

Add the package to your `pubspec.yaml`:

```yaml
dependencies:
  quick_payment: ^0.0.4
```

Then run:

```bash
flutter pub get
```

---

## Integration Guide

Follow these steps to integrate `quick_payment` into your Flutter app:

### 1️⃣ Import the package

```dart
import 'package:quick_payment/quick_payment.dart';
```

### 2️⃣ Prepare Customer Details

Use `CustomerDetails` to provide customer name and email.  
This will be included in `PaymentData` for your DB storage.

```dart
final customer = CustomerDetails(
  fullName: 'Khalid LikhOn',
  email: 'hello@khalidlikhon.me',
);
```

### 3️⃣ Setup Payment Credentials

Use `QuickPayCredentials` to configure mobile banking numbers and optional fee percentage.

```dart
final credentials = QuickPayCredentials(
  feePercentage: 1.8, // optional
  methods: [
    PaymentMethod.bkash("016xxxxxxxx"),
    PaymentMethod.nagad("017xxxxxxxx"),
    PaymentMethod.rocket("018xxxxxxxx"),
    // PaymentMethod.upay("019xxxxxxxx"), // optional
  ],
);
```

Replace the numbers with your personal mobile banking numbers to receive payments.

### 4️⃣ Trigger Payment Flow

Call `QuickPay.createPayment()` on button press or action.

```dart
QuickPay.createPayment(
  context: context,
  amount: 200, // Payment amount
  customer: customer,
  credentials: credentials,
  onPaymentSubmitted: (data) {
    // PaymentData returned after user submits
    // Save to your database manually
    print(data.toMap());
  },
);
```

`onPaymentSubmitted` returns a `PaymentData` object with:

| Field         | Description                                        |
|---------------|----------------------------------------------------|
| customer      | `CustomerDetails` object (name & email)           |
| amount        | Payment amount                                     |
| method        | Payment method type (e.g., bKash)                 |
| transactionId | User-entered transaction ID                        |
| time          | DateTime when payment submitted                    |
| status        | Default "pending" – approve manually in DB        |

⚠️ Payment status is `"pending"` by default. You must manually verify and approve in your system.

---

## Usage Example

This example demonstrates how to use `QuickPayment` with multiple payment methods in your Flutter app. Clicking the **Pay Now** button opens the payment dialog and handles submission callbacks.

```dart
import 'package:flutter/material.dart';
import 'package:quick_payment/quick_payment.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: const HomePage(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Quick Payment Example")),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Customer Details
            final customer = CustomerDetails(
              fullName: 'Khalid LikhOn',
              email: 'hello@khalidlikhon.me',
            );

            // Payment Credentials & Methods
            final credentials = QuickPayCredentials(
              feePercentage: 1.8,
              methods: [
                PaymentMethod.bkash("016xxxxxxxx"),
                PaymentMethod.nagad("017xxxxxxxx"),
                PaymentMethod.rocket("018xxxxxxxx"),
                // PaymentMethod.upay("019xxxxxxxx"), // optional
              ],
            );

            // Trigger Payment Flow
            QuickPay.createPayment(
              context: context,
              amount: 200,
              customer: customer,
              credentials: credentials,
              onPaymentSubmitted: (data) {
                // Returned PaymentData
                print(data.toMap());
                // Save to DB manually, status = "pending"
              },
            );
          },
          child: const Text("Pay Now"),
        ),
      ),
    );
  }
}
```

---

## License

This project is licensed under the  **MIT License** – see the [LICENSE](LICENSE) file for details.

---

## Links

- Homepage: [GitHub Repository](https://github.com/khalidlikhOn/quick_payment)
- Issue Tracker: [Report Issues](https://github.com/khalidlikhOn/quick_payment/issues)
