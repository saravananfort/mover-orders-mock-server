---

# 🧩 my_server

A lightweight Dart Frog backend server for handling **Orders** and **Order Details** APIs used in your Flutter app.

---

## 🚀 Getting Started

### 1. Run the Local Server

Start the development server with:

```bash
dart_frog dev
```

By default, the server will run on:

```
http://localhost:8080
```

---

## 🧠 Flutter App Integration

Update your API URLs in
`lib/data/remote/orders/orders_repo_impl.dart`:

```dart
@override
Future<Either<ApiException, List<Orders>>> getOrders({
  int page = 0,
  int count = 10,
}) async {
  try {
    final url = "http://localhost:8080/orders";
    if (kDebugMode) {
      debugPrint("myorders url : $url");
    }
    final response = await _apiClient.get(url);
    Logger.write(response.toString());
    final data = response.data?["data"] ?? [];
    return Right([for (final order in data) Orders.fromJson(order)]);
  } catch (e) {
    Logger.write(e.toString());
    return Left(ApiException(e.toString()));
  }
}

@override
Future<Either<ApiException, Orders>> getOrderDetails(String id) async {
  try {
    final url = "http://localhost:8080/order-details/$id";
    if (kDebugMode) {
      debugPrint("order details url : $url");
    }
    final response = await _apiClient.get(url);
    Logger.write(response.toString());
    final data = response.data;
    return Right(Orders.fromJson(data ?? {}));
  } catch (e) {
    Logger.write(e.toString());
    return Left(ApiException(e.toString().isEmpty ? "Error" : e.toString()));
  }
}
```

---

## 📡 API Endpoints

| Endpoint             | Method | Description                        |
| -------------------- | ------ | ---------------------------------- |
| `/orders`            | `GET`  | Fetch all orders                   |
| `/order-details/:id` | `GET`  | Fetch details for a specific order |

---

## ⚙️ Notes

* Ensure the Dart Frog server is running **before** testing the Flutter app.
* The base URL (`http://localhost:8080`) is for **local development only**.
  Update it for staging or production environments when deploying.

---