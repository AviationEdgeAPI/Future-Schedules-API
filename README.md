# Future-Airport-Schedules-API
Aviation Edge [Future Airport Schedules API](https://aviation-edge.com/future-flight-schedules-and-timetables-of-airports-api/) provides flight schedule data of a given date in the future by using an algorithm to create the schedule based on historical data. It has global coverage with the exception of military and private airfields and airports with really little traffic. When implemented, your users can choose a flight off the list after picking a date and an airport. Details for each flight in the response include the flight number, flight time, airline, departure and arrival locations, aircraft details, the most used terminal and gate for the flight, and more. The API is particularly useful for online travel agencies and flight booking services.

### Documentation
You may find input parameters, output examples with explanations for each item, filter list, and more in the [documentation](https://aviation-edge.com/developers/).

### Example Fields of Use
- Travel agency and flight booking websites where users are expected to submit a date and view available flights
- Websites, tools or apps to  display scheduled flights on a given date
- Flight schedule analysis based on region or dates

### Request 
For the departure schedule of a certain airport on a certain future date.

**GET** `https://aviation-edge.com/v2/public/flightsFuture?key=[API_KEY]&type=departure&iataCode=BER&date=YYYY-MM-DD`

For the arrival schedule of a certain airport on a certain future date.

**GET** `https://aviation-edge.com/v2/public/flightsFuture?key=[API_KEY]&type=arrival&iataCode=BER&date=YYYY-MM-DD`

For the flights that are scheduled to arrive at a certain airport on a certain date (out of a departure schedule).

**GET** `https://aviation-edge.com/v2/public/flightsFuture?key=[API_KEY]&type=departure&iataCode=BER&arr_iataCode=ORY&date=YYYY-MM-DD`

For the flights that are scheduled to depart from a certain airport on a certain date (out of an arrival schedule).

**GET** `https://aviation-edge.com/v2/public/flightsFuture?key=[API_KEY]&type=arrival&iataCode=BER&dep_iataCode=ory&date=YYYY-MM-DD`

### Filters
```
&iata_code=     (obligatory) Departure or arrival airport IATA code depending on the "&type=" value
&type=          (obligatory) Either "departure" or "arrival" as both within the same query is not possible
&date=          (obligatory) Future date in YYYY-MM-DD format

&dep_iataCode=  filter of departure airport if "arrival" for "&type=" was chosen, based on the airport IATA code
&dep_icaoCode=  filter of departure airport if "arrival" for "&type=" was chosen, based on the airport ICAO code
&arr_iataCode=  filter of arrival airport if "departure" for "&type=" was chosen, based on the airport IATA code
&arr_icaoCode=  filter of arrival airport if "departure" for "&type=" was chosen, based on the airport ICAO code
&airline_iata=  option to filter airline based on airline IATA code
&airline=icao=  option to filter airline based on airline ICAO code
&flight_num=    option to filter a specific flight based on its flight number
```

### Useful Code Examples

1.	Fetching the Data (axios library used)

```
const axios = require('axios');

const API_ENDPOINT = "https://aviation-edge.com/v2/public/flightsFuture?key=api-key&type=departure&iataCode=BER&date=2023-12-17";

async function fetchAirportSchedule() {
    try {
        const response = await axios.get(API_ENDPOINT);
        return response.data;
    } catch (error) {
        console.error("Error fetching data:", error);
        return null;
    }
}

// Test fetching the data
fetchAirportSchedule().then(data => {
    if(data) {
        console.log("Received Data:", data);
    }
});
```

2.	Display a certain flight’s details

```
function displayFlightDetails(flight) {
    console.log("Flight Number:", flight.flight.iataNumber);
    console.log("Aircraft:", flight.aircraft.modelText);
    console.log("Departure Time:", flight.departure.scheduledTime);
    console.log("Arrival Time:", flight.arrival.scheduledTime);
    console.log("Departure Gate:", flight.departure.gate);
    console.log("Arrival Gate:", flight.arrival.gate);
    console.log("Codeshare Information:", flight.codeshared ? flight.codeshared.airline.name : "No codeshare information available.");
}

// Test display function with the first flight from fetched data
fetchAirportSchedule().then(data => {
    if(data && data.length > 0) {
        displayFlightDetails(data[0]);
    }
});
```

3.	Filter all flights based on a criteria (example is flights that are to depart after a certain time)

```
function filterFlightsBefore(data, time) {
    return data.filter(flight => flight.departure.scheduledTime < time);
}

// Test filter function with the fetched data
fetchAirportSchedule().then(data => {
    if(data) {
        const filteredFlights = filterFlightsBefore(data, "07:00");
        console.log("Filtered Flights:", filteredFlights);
    }
});
```

### Response
```
[
{"weekday": "1",
"departure": 
{
"iataCode": "mty",
"icaoCode": "mmmy",
"terminal": "c",
"gate": "f2",
"scheduledTime": "20:35"
}, 
"arrival": 
{"iataCode": "iah", 
"icaoCode": "kiah",
"terminal": "d", 
"gate": "d12", 
"scheduledTime": "22:00"
}, 
"aircraft": 
{"modelCode": "a320", 
"modelText": "airbus a320-232"
},
"airline": 
{"name": "vivaaerobus",
"iataCode": "vb", 
"icaoCode": "viv"}, 
"flight": 
{"number": "616", 
"iataNumber": "vb616", 
"icaoNumber": "viv616"}
}
]
```

### Access & Support
[Contact us](https://aviation-edge.com/contact/) via email for any questions or support requests.

[Get your API key](https://aviation-edge.com/premium-api/) in a minute and start testing the data right away. The API is provided through API subscriptions. All plans grant access to the Future Schedules API and other APIs with a difference of the monthly API call limit. Choose the best plan for you and upgrade, downgrade or cancel your plan anytime without  commitments.

### License
The use of the API is subject to Aviation Edge [Terms and Conditions](https://aviation-edge.com/api-terms-of-service/).
