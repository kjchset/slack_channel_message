# Slack Message Posting Script

This script sends a formatted message to a Slack channel using the Slack API.
## Prerequisites

- **Bot Token**:Bot Token Used to authenticate with Slack.
- **Channel ID**: ID of the Slack channel where the message will be posted.

## Required libraries
- **request library**:This Python library used to make HTTP requests to slack API to post message in slack channel.
- **os library**:os module is used to interact with the operating system and manage environment variables
- **load_dotenv**:To load variables from .env file


## The message includes:
-  A header with an emoji and title :
-  Emoji value:**\U0001F4CA** 
-  **Title**:App Sign-Ups and Marketing Performance Analysis
-  **Key insights** about app sign-ups, cumulative sign-ups, and Instagram performance.
-  **Suggested next steps** for improving performance.
-  A context section with a data source link.
## The Code Explanation

import requests
import os
from dotenv import load_dotenv

## Load environment variables from .env file
load_dotenv()

## Retrieve the Slack API token and channel ID from environment variables
slack_token = os.getenv("SLACK_API_TOKEN")
channel_id = os.getenv("SLACK_CHANNEL_ID")

## The message payload
data_to_send = {
    "channel": channel_id,
    "blocks": [
        {
            "type": "header",
            "text": {
                "type": "plain_text",
                "text": "\U0001F4CA App Sign-Ups and Marketing Performance Analysis"
            }
        },
        {
            "type": "section",
            "text": {
                "type": "mrkdwn",
                "text": """*Date Range:* Jul-25-23 to Aug-01-23

**Key Insights:**
\U0001F4C8 *App sign-ups* have shown a slight increase overall, with a *total of 19 sign-ups* during the week.
\U0001F53C *Cumulative sign-ups* reached *373* by *August 1*, indicating steady growth compared to the previous week.
\U0001F4F1 *Instagram posts* related to specific events led to higher engagement, with notable spikes in impressions and reach on *July 26* and *July 28*.


**Next Steps:**
\U0001F680 Focus on creating more content for Instagram, especially around successful events like the Vegan Breakfast Recipe Post and Zoe Planet Organic Reel.
\U0001F3AF Analyze the lower sign-up days (July 29 and July 31) to identify potential factors affecting performance.
\U0001F50D Consider increasing promotional efforts on days with high impressions but low sign-ups to improve conversion rates."""
            }
        },
        {
            "type": "divider"
        },
        {
            "type": "context",
            "elements": [
                {
                    "type": "mrkdwn",
                    "text": "Data powered by [Your Service Endpoint](https://327a-2405-201-c036-a02d-a123-7c8a-4f1d-8de8.ngrok-free.app/query)"
                }
            ]
        }
    ]
}

## the POST request to Slack API
response = requests.post(
    "https://slack.com/api/chat.postMessage",
    headers={
        "Authorization": f"Bearer {slack_token}",
        "Content-Type": "application/json"
    },
    json=data_to_send
)

## The response
if response.status_code == 200 and response.json().get("ok"):
    print("Message sent successfully!")
else:
    print(f"Failed to send message: {response.status_code}, {response.json()}")
