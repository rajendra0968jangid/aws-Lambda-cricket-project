import json
import urllib.request

def lambda_handler(event, context):
    # Target external API URL
    url = "https://api-v1.com/v1/sV3.php?key=10ZC"
    
    try:
        # Fetch the data from the external API
        with urllib.request.urlopen(url, timeout=5) as response:
            # Read and parse the raw string response
            raw_data = response.read().decode('utf-8')
            api_response = json.loads(raw_data)
            
        # Return the exact data structure received from the API
        return {
            "statusCode": 200,
            "headers": {
                "Content-Type": "application/json",
                "Access-Control-Allow-Origin": "*"  # Prevents CORS errors
            },
            "body": json.dumps(api_response)  # Safely serialized string
        }
        
    except Exception as e:
        # Gracefully handle connection or timeout failures
        return {
            "statusCode": 502,
            "headers": {
                "Content-Type": "application/json"
            },
            "body": json.dumps({
                "error": "Failed to fetch data from external API",
                "details": str(e)
            })
        }
