```
import requests
import jwt
import json
import re

# Helper function to generate an access policy
def generate_policy(principal_id, effect, resource, context=None, usageKey=None):
    auth_response = {
        'principalId': principal_id,
        'policyDocument': {
            'Version': '2012-10-17',
            'Statement': [
                {
                    'Action': 'execute-api:Invoke',
                    'Effect': effect,
                    'Resource': resource
                }
            ]
        }
    }

    if context:
        auth_response['context'] = context
    if usageKey:
        auth_response['usageIdentifierKey'] = usageKey

    return auth_response

def lambda_handler(event, context):
    try:
        # Extract headers and resource from the event
        headers = event.get('headers', {})
        resource = event.get('methodArn', {})
        
        # Extract client credentials and other parameters
        client_id = headers.get('client_id', 'wrong-client')
        client_secret = headers.get('client_secret', 'password')
        realm = headers.get('realm', 'realm-01')
        keycloak_url = headers.get('keycloak_url', 'https://kc.aws.staritcloud.pl:8443')
        grant_type = headers.get('grant_type', 'client_credentials')
        client_path = event.get('path', "nopath")  # Moved up for clarity

        # Request an access token from Keycloak
        response = requests.post(
            f"{keycloak_url}/realms/{realm}/protocol/openid-connect/token",
            data={
                "client_id": client_id,
                "client_secret": client_secret,
                "grant_type": grant_type
            },
            headers={"Content-Type": "application/x-www-form-urlencoded"},
            verify=False  # Temporarily disable SSL verification
        )

        # Handle unsuccessful token requests
        if response.status_code != 200:
            return generate_policy('unknown', 'Deny', resource)

        # Extract the access token from the response
        token_response = response.json()
        access_token = token_response.get('access_token')
        
        try:
            # Decode the JWT token to extract client information
            decoded_token = jwt.decode(access_token, options={"verify_signature": False})
            allowed_paths = decoded_token.get('path', [])
            usage_plan_key = decoded_token.get('usage_plan_key')

            # Normalize allowed paths to a list if necessary
            if isinstance(allowed_paths, str):
                allowed_paths = [allowed_paths]

            # Function to check if the client path matches any allowed paths
            def path_matches(request_path, allowed_paths):
                for pattern in allowed_paths:
                    # Convert '*' to '[^/]*' for regex matching
                    regex = re.sub(r'\*', '[^/]*', pattern)
                    # Make the trailing slash optional and ensure full match
                    regex = f"^{regex}/?$"
                    if re.match(regex, request_path):
                        return True
                return False

            # Check if the client path is allowed
            if path_matches(client_path, allowed_paths):
                # Return an 'Allow' policy with usage plan key
                return generate_policy(
                    client_id,
                    'Allow',
                    resource,
                    {'quotaRequests': '5'},
                    usage_plan_key
                )
            # If path does not match, return a 'Deny' policy
            return generate_policy(client_id, 'Deny', resource)

        except jwt.PyJWTError as e:
            # Handle JWT decoding errors
            print(f"JWT error: {str(e)}")
            return generate_policy('unknown', 'Deny', resource)

    except Exception as e:
        # Handle any other exceptions
        print(f"Global error: {str(e)}")
        return generate_policy('unknown', 'Deny', resource)
```