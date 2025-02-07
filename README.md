# weather-monitor-service-publisher
Create a service that runs using a scheduler (every 30 minutes) and fetches data for 5 cities weather conditions and publishes updates to message queues.   Filter when the precipitation is above 30 or temperature is below 5 and send those events to the queues

**Pattern - Publisher : **

In this service, the publish-subscribe (pub-sub) messaging pattern is implemented through a single publisher—the Weather Monitor Service. This service runs on a scheduled basis (every 30 minutes), fetching weather data for five cities and processing it according to defined thresholds.

Publisher Role: The Weather Monitor Service is the publisher that collects weather data (temperature and precipitation) from the OpenMeteo API. It then filters the data based on specific conditions (e.g., temperature below 5°C or precipitation above 30mm). Once filtered, the service publishes relevant weather alerts to specific message queues (topics).

Message Queues: The alerts are pushed to two topics:

weather.alerts.temperature

weather.alerts.precipitation

**Solution Design:**

Here we describe the high-level architecture and components of the weather alert system, highlighting how the different parts of the system fit together and interact.

1. System Overview
The weather alert system is designed to monitor temperature and precipitation data for multiple cities and send alerts if certain conditions are met. It leverages OpenMeteo's weather API to retrieve real-time data and sends the alerts to an ActiveMQ messaging system.

2. Components Involved
Scheduler: This component triggers the system at regular intervals (every 30 minutes) to check for weather changes in specified cities.

Weather Data Fetching: The system fetches weather data (temperature and precipitation) for each city using OpenMeteo's API.

Message Queuing: Alerts generated for each city are published to separate queues in ActiveMQ (weather.alerts.temperature and weather.alerts.precipitation).

Error Handler: A global error handler is used to ensure that errors are caught and managed effectively, ensuring the system is resilient and can retry when necessary.

3. System Flow
Periodic Trigger: The system fetches weather data every 30 minutes using a scheduled task.

City Iteration: The system supports multiple cities, iterating over each city to fetch weather data based on its latitude and longitude.

Data Transformation: For each city, the system extracts the relevant weather data (temperature or precipitation), compares it against predefined thresholds (like 5°C for temperature), and generates an alert if necessary.

Alert Publication: Alerts are sent to dedicated JMS queues based on the type of alert (temperature or precipitation).
