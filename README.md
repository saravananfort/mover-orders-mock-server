# my_server

Modify the url in lib/data/remote/orders/orders_repo_impl.dart

@override
  Future<Either<ApiException, List<Orders>>> getOrders(
      {int page = 0, int count = 10}) async {
    try {
      //final url = "${AppConstants.myorders}/$page/$count";
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
      //final url = "${AppConstants.orderDetails}/$id";
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


To run the server locally use dart_frog dev
macbookair@MacBooks-MacBook-Air my_server % dart_frog dev


