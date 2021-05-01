# HotelBooking

This repo contains the production rest service of rooms booking for a hotel, with calculation of its overdue rate per hour based on check out time by customers.

## Setting Up

- Download the application
  - The Web Rest Service (zipped file)
  - The Database (.bak file)
  - The postman collection for making calls the service

- Ensure Sql Server 2014 or higher is installed on windows and restore the database with name: HotelBooking
- Unzip the Web Rest Service and Add as an application on Default Website under IIS with name HotelBooking
- Check the web config of the rest service and ensure connection string is correct with your sql server
- On the Root folder of the web service right click on it and click on properties and then click the security tab, grant either (IIS_IUSRS or Everyone) full control of the root folder. (There will be error 500 response if this is not done)


## API End Points

1. **Get the list of all Bookings in the Hotel**

After Set up your Base URL should look like: http://localhost/hotelbooking/

GET {BaseUrl}/api/booking

The above GET call fetches all the bookings in the hotel

2. **Insert Hotel Booking**

POST {BaseUrl}/api/booking

Post Body (Payload):
{
    "Entity" : {
        "RoomType": "palatial",
        "CustomerID": "12335",
        "Status": "paid",
        "ExpectedCheckInTime": "2021-03-22 14:00",
        "ExpectedCheckOutTime": "2021-03-22 16:00"
    }
}

The above url and post body will insert a booking for a customer with date time format for check in and check out being: yyyy-MM-DD hh:mm


3. **Get Booking By ReservationID**

GET {BaseUrl}/api/booking/{ReservationID}

A particular booking can be fetched using its ReservationID (This can be retrieved by getting the list of bookings)


4. **Update Booking By ReservationID**

PUT {BaseUrl}/api/booking/{ReservationID}

PUT Body (Payload):
{
    "Patch" : {
        "Status": "paid",
        "ExpectedCheckInTime": "2021-05-04 16:00",
        "ExpectedCheckOutTime": "2021-05-04 17:00"
    }
}

Fields Status, ExpectedCheckInTime and ExpectedCheckOutTime of a booking in the hotel can be updated. Any of the fields can be updated, only a single field too can be updated from the PUT Body as desired


5. **Calculate Overdue Amount**

POST {BaseURL}/api/booking/overdue/{ReservationID}

POST Body (Payload):

{"CheckOutTime": "2021-05-04 18:51"}

The time the customer left is the only field needed here to calculate the overdue amount to be paid by the customer. 

Below is a sample response:

{
    "InitialAmount": 230000.0,
    "OverDueAmount": 19550.0,
    "TotalAmount": 249550.0,
    "WeekendRate": false
}


Import the Postman Collection to see all these calls and run the test.



