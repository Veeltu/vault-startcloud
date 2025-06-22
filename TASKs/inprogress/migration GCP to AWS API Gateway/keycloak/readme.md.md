

## README

### Overview

This Python script is a Lambda function designed to authorize API requests to an API Gateway using Keycloak as the identity provider. It leverages Keycloak to authenticate clients, retrieves user permissions from a JWT (JSON Web Token), and generates an IAM (Identity and Access Management) policy that allows or denies access to specific API resources. This script enables authorization based on user roles and permissions defined in Keycloak.

### Functionality

The script performs the following primary functions:

1. **Authentication:** Authenticates clients against a Keycloak server using client credentials (client ID and secret).
2. **Token Retrieval:** Obtains an access token (JWT) from Keycloak upon successful authentication.
3. **Token Decoding:** Decodes the JWT to extract user-specific information, such as allowed API paths, usage plan keys, and custom attributes.
4. **Policy Generation:** Generates an IAM policy that allows or denies access to API resources based on the information extracted from the JWT. This policy is then used by API Gateway to authorize the request.
5. **Context Provision:** It also passes additional context data (client ID, secret, allowed paths, quota) to the backend API, allowing further request handling customization.

### Step-by-Step Explanation

Here's a breakdown of what the script does, step-by-step:

1. **Imports:** Imports necessary libraries:
    * `requests`: For making HTTP requests to Keycloak.
    * `jwt`: For decoding JWT tokens.
    * `json`: For handling JSON data in context.
2. **`generate_policy` Function:**
    * **Purpose:** Creates an IAM policy that either allows or denies access to API resources.
    * **Input:**
        * `principal_id`: Identifier of the user/client.
        * `effect`: "Allow" or "Deny".
        * `resources`: List of API resources (ARNs) this policy applies to.
        * `context`: Additional data passed to the API Gateway integration.
        * `usage_key`: Usage plan key if API Gateway usage plans are used.
    * **Process:** Constructs a policy document that specifies the allowed action (`execute-api:Invoke`) and resources, including additional context data, and returns the policy.
3. **`get_access_token` Function:**
    * **Purpose:** Obtains an access token from Keycloak using client credentials.
    * **Input:**
        * `keycloak_url`: The URL of the Keycloak server.
        * `realm`: The Keycloak realm.
        * `client_id`: The client ID.
        * `client_secret`: The client secret.
        * `grant_type`:  'client\_credentials' (typically used for machine-to-machine authentication).
    * **Process:** Sends a POST request to the Keycloak token endpoint with the client credentials and returns the `requests` response object.  Handles potential request exceptions (e.g., network errors).
4. **`decode_jwt_token` Function:**
    * **Purpose:** Decodes a JWT token to extract its claims (data).
    * **Input:**
        * `access_token`: The JWT token string.
    * **Process:** Decodes the JWT without verifying the signature (for speed, assuming the source is trusted). **Important**: In production, always verify the signature using the Keycloak public key. Handles JWT decoding errors.
5. **`generate_additional_context` Function:**
    * **Purpose:** Creates a dictionary of additional context data to be passed to the API Gateway integration.
    * **Input:**
        * `client_id`: Client identifier.
        * `client_secret`: Client secret.
        * `client_key`: Client key.
        * `custom_attributes`: Dictionary of custom data.
        * `allowed_paths`: List of allowed API paths.
        * `quota`: Request quota limit.
    * **Process:** Constructs a dictionary containing the input parameters, converts appropriate values to strings, and returns the dictionary.
6. **`construct_arn_resources` Function:**
    * **Purpose:** Constructs a list of ARN resources based on a base ARN and allowed paths.
    * **Input:**
        * `base_resource_arn`: The base ARN of the API Gateway resource.
        * `allowed_paths`: A list of allowed API paths.
    * **Process:** Splits the base ARN and combines the allowed paths with the base ARN to construct a list of specific ARN resources.
7. **`lambda_handler` Function:**
    * **Purpose:** Main entry point for the Lambda function.
    * **Input:**
        * `event`: Event data passed by API Gateway.
        * `context`: Lambda context object.
    * **Process:**

8. Extracts headers (including client ID, secret, Keycloak URL, realm, and grant type) and the method ARN from the event.
9. Calls `get_access_token` to obtain an access token from Keycloak. If this fails, denies access.
10. Extracts the access token and calls `decode_jwt_token` to decode it. If this fails, denies access.
11. Extracts information from the decoded token, such as allowed paths, usage plan key, custom attributes and quota.
12. Calls `generate_additional_context` to create additional context data.
13. Calls `construct_arn_resources` to generate a list of allowed ARN resources.
14. Calls `generate_policy` to generate an IAM policy that allows access to the specified resources with the provided context and usage plan key.
15. Handles any exceptions and denies access in case of an error.

### Usage

1. **Deployment:** Deploy this script as a Lambda function in your AWS environment.
2. **API Gateway Integration:** Configure API Gateway to use this Lambda function as a custom authorizer.
3. **Configuration:** Set up the necessary headers in the API Gateway integration to pass client credentials, Keycloak URL, realm, and grant type to the Lambda function.
4. **Keycloak Setup:** Ensure Keycloak is configured with the necessary clients, realms, and user roles to support the required authorization logic.
5. **Permissions:** Provide the Lambda function with the necessary IAM permissions to execute and access AWS resources.

### Notes

Headers have to include:

- **"client_id"**
- **"client_secret"**
- **"grant_type"**
- **"realm"**
- **"keycloak_url"**



<div style="text-align: center">⁂</div>

