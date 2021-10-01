# Authorization #
In order to send requests to the TBM API, you have to retrieve a token and include it in the header of all API requests.

Token can be retrieved by sending POST request to the following endpoint:
https://{API_URL}/Logon

with the following body:

   ```json
   {
     "tenant": "TBM",
     "userName": "Admin",
     "userPassword": "Your_Password"
   }
   ```

If specified credentials are valid, a server will return you a token:

   ```json
   {
   "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1lIjoiQWRtaW4iLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL3JvbGUiOiJUQk0iLCJuYmYiOjE2MzMwODExNjMsImV4cCI6MTYzNTY3MzE2MywiaXNzIjoiTXlBdXRoU2VydmVyIiwiYXVkIjoiTXlBdXRoQ2xpZW50In0.kLWE3QxZCDsuhHCf8JJq1CzZ1E867GYNFz1pwaenapg",
   "username": "Admin"
   }
   ```

You can save this token and add it to the **Authorization** header in the form of "Bearer" word followed by your token after space, like this:
   >Bearer *your_token*

Take a note that when you login with some user and password, it will affect on results of all operation you perform via API. This means that if you login with a user with limited access rights, then API won't allow you to read or write beyond allowed boundaries. To test this, you can create few users via TBM UI, limit access for second user, and then try to send login requests via API for these users. After that, try to send GET requests to retrieve list of entities where you have set limited access - you will notice that one user receives more data than the other.

# Code examples #

Here you will find few code examples written in C# demonstrating how to authorize in API.

## Sending Logon request

   ```cs
   var client = new RestClient("https://localhost:44334/Logon");
   var request = new RestRequest(Method.POST);
   var body = JsonConverter.SerializeObject(logonModel);
   request.AddParameter("application/json", body,  ParameterType.RequestBody);
   IRestResponse response = client.Execute(request);
   Console.WriteLine(response.Content);
   ```

## Sending GET request with Bearer token

   ````cs
   var client = new RestClient("https://localhost:44334/Unit");
   var request = new RestRequest(Method.GET);
   request.AddHeader("Authorization", "Bearer your_token");
   IRestResponse response = client.Execute(request);
   Console.WriteLine(response.Content);
   ````