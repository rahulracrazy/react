# Creating a Status Page with Integration and API Data

In this guide, we'll explore how to create a status page with integration options, design templates, and the use of API data from Opsgenie and New Relic. A status page typically includes the following components:

## Basic Status Page

1. **Status Summary**:
   - A high-level summary of the overall service status, indicating operational, degraded performance, partial outage, or major outage.
   - Utilize visual indicators (e.g., green for operational, yellow for degraded, red for outage) for a quick status overview.

2. **Service Components**:
   - List individual service components or key features (e.g., website, API, database, email service) to report on.

3. **Status of Each Component**:
   - Display the current status of each service component (Operational, Degraded, Outage).
   - Include a timestamp for the last status update.

4. **Description of the Issue**:
   - If any component is not operational, provide a concise but informative description of the issue or reason for the degraded or outage status.

5. **Incident History**:
   - Show recent incidents, including date, time, status, and a brief description.
   - Provides context and transparency about past problems.

6. **Performance Metrics**:
   - Include key performance metrics, such as response times, error rates, or other critical data if relevant.

7. **User Notifications**:
   - Allow users to subscribe to notifications (email, SMS, webhooks) for updates on service status.

8. **Scheduled Maintenance**:
   - Share information about upcoming scheduled maintenance or downtime, including date, time, and expected duration.

9. **Contact Information**:
   - Provide contact details for your support team, including email addresses and links to your support portal.

10. **Links to More Information**:
    - Include links to detailed incident reports, status history, or a knowledge base with troubleshooting guides.

11. **Last Updated Timestamp**:
    - Display the last update time to inform users about the information's current status.

12. **Social Media Links**:
    - Links to official social media accounts for users to get updates and communicate with your team during incidents.

13. **Logo and Branding**:
    - Incorporate your company's logo and branding for recognition and trustworthiness.

Remember, simplicity and clarity are crucial for a basic status page. Users should quickly assess the situation and understand the impact on your services. For advanced features or more details, consider using third-party status page services with customization options.

## Integrating Opsgenie API Data

You can use Opsgenie's REST APIs to retrieve incident data. To fetch data from Opsgenie, follow these steps:

- Use Opsgenie's REST API and authenticate your requests with your Opsgenie API key.
- Fetch incident history updates using various API endpoints provided by Opsgenie.
- For real-time updates, consider using technologies like WebSockets or server-sent events (SSE) to push new incident history updates to your status page.
- Allow users to subscribe to notifications for incident history updates through a subscription mechanism like email alerts or RSS feeds.

## Integrating New Relic API Data

For integrating New Relic data, you can use the provided Lambda API snippet as an example:

```python
import json
import requests

# New Relic API Configuration
NEW_RELIC_API_KEY = 'YOUR_NEW_RELIC_API_KEY'
NEW_RELIC_ACCOUNT_ID = 'YOUR_NEW_RELIC_ACCOUNT_ID'
NEW_RELIC_API_BASE_URL = f'https://api.newrelic.com/v2/alerts_incidents/{NEW_RELIC_ACCOUNT_ID}/incidents.json'

# Opsgenie API Configuration
OPSGENIE_API_KEY = 'YOUR_OPSGENIE_API_KEY'
OPSGENIE_API_BASE_URL = 'https://api.opsgenie.com/v2/incidents'

def lambda_handler(event, context):
    service = event.get('service')

    if not service:
        return {
            "statusCode": 400,
            "body": json.dumps({"error": "Service parameter is required"})
        }

    try:
        # Fetch incident logs from New Relic
        new_relic_response = requests.get(
            NEW_RELIC_API_BASE_URL,
            headers={'X-Api-Key': NEW_REL_RELIC_API_KEY}
        )

        new_relic_incidents = new_relic_response.json()

        # Fetch incident logs from Opsgenie
        opsgenie_response = requests.get(
            OPSGENIE_API_BASE_URL,
            headers={'Authorization': f'GenieKey {OPSGENIE_API_KEY}'}
        )

        opsgenie_incidents = opsgenie_response.json()

        return {
            "statusCode": 200,
            "body": json.dumps({
                "NewRelicIncidents": new_relic_incidents,
                "OpsgenieIncidents": opsgenie_incidents
            })
        }

    except Exception as e:
        return {
            "statusCode": 500,
            "body": json.dumps({"error": str(e)})
        }
```

This Lambda function fetches incident data from New Relic and Opsgenie and can be used to integrate their data into your status page.

## Open Source Status Page Project

For a more open-source approach, you can explore the [statsig-io/statuspage](https://github.com/statsig-io/statuspage) project on GitHub, which can serve as a foundation for your custom status page.

By following these guidelines and utilizing integration options and APIs, you can create a robust and informative status page for your services.