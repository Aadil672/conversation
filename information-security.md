---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Information security
{: #information-security}

IBM is committed to providing our clients and partners with innovative data privacy, security and governance solutions.
{: shortdesc}

**Notice:**
Clients are responsible for ensuring their own compliance with various laws and regulations, including the European Union General Data Protection Regulation. Clients are solely responsible for obtaining advice of competent legal counsel as to the identification and interpretation of any relevant laws and regulations that may affect the clients’ business and any actions the clients may need to take to comply with such laws and regulations.

The products, services, and other capabilities described herein are not suitable for all client situations and may have restricted availability. IBM does not provide legal, accounting or auditing advice or represent or warrant that its services or products will ensure that clients are in compliance with any law or regulation.

## European Union General Data Protection Regulation (GDPR)
{: #gdpr}

IBM is committed to providing our clients and partners with innovative data privacy, security and governance solutions to assist them on their journey to GDPR compliance.

Learn more about IBM's own GDPR readiness journey and our GDPR capabilities and offerings to support your compliance journey [here ![External link icon](../../icons/launch-glyph.svg "External link icon")](../../icons/launch-glyph.svg "External link icon")](http://www.ibm.com/gdpr){: new_window}.

## Labeling and deleting data in {{site.data.keyword.conversationshort}}
{: #gdpr-wa}

Do not add personal data to the training data (entities and intents, including user examples) that you create.

**Note:** Experimental and beta features are not intended for use with a production environment and therefore are not guaranteed to function as expected when labeling and deleting data. Experimental and beta features should not be used when implementing a solution that requires the labeling and deletion of data.

If you need to remove a customer's message data from a {{site.data.keyword.conversationshort}} instance, you can do so based on the customer ID of the client, as long as you associate the message with a customer ID when the message is sent to the service.

**Note:** The automatic Slack and Facebook integration features do not support the labeling and therefore deletion of data based on customer ID. These features should not be used in a solution that requires the ability to delete based on customer ID.

### Before you begin
To be able to delete message data associated with a specific user, you must first associate all messages with a unique **customer ID** for each user. To specify the **customer ID** for any messages sent using the `/message` API, include the `X-Watson-Metadata: customer_id` property in your header. For example:

```
curl -X POST
 --user {username}:{password}
 --header
   'Content-Type: application/json'
   'Accept: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data '{"input":{"text":"hello"}}' 'https://gateway.watson.net/conversation/api/v1/workspaces/{workspaceID}/message?version=2017-05-26'
```
{: codeblock}

**Note**: The `customer_id` string cannot include the semicolon (`;`) or equal sign (`=`) characters. You are responsible for ensuring that each `customer ID` property is unique across your customers.

You can pass multiple **customer ID** values with semicolon-separated `customer_id={value}` pairs. For example: `'X-Watson-Metadata: customer_id=abc;customer_id=xyz'`

### Deleting data
To delete any message log data associated with a specific user that the service might have stored, use the `DELETE /user_data` API endpoint. Specify the customer ID of the user by passing a `customer_id` parameter with the request.

Only data that was added by using the `POST /message` API endpoint with an associated customer ID can be deleted using this delete method. Data that was added by other methods cannot be deleted based on customer ID. For example, entities and intents that were added from customer conversations, cannot be deleted in this way. Personal Data is not supported for those methods.

**IMPORTANT**: Specifying a `customer_id` will delete *all* messages with that `customer_id` that were received before the delete request, across your entire {{site.data.keyword.conversationshort}} instance, not just within one workspace.

As an example, to delete any message data associated with a user that has the customer ID `abc` from your {{site.data.keyword.conversationshort}} instance, send the following cURL command:

```
curl -X DELETE
 --user {username}:{password}
 'https://gateway.watson.net/conversation/api/v1/user_data?customer_id=abc&version=2017-05-26'
```
{: codeblock}

An empty JSON object `{}` is returned.

For more information, see the [API reference](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#delete-user-data).

**Note:** Delete requests are processed in batches and may take up to 24 hours to complete.
