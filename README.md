# ![LOGO](logo.png) fileee API **flow**ground Connector

## Description

A generated **flow**ground connector for the fileee API API (version 2.0.0-SNAPSHOT).

Generated from: https://api.workeee.de/v2/swagger.json<br/>
Generated at: 2019-06-20T11:15:51+03:00

## API Description

The fileee API contains methods for accessing documents and other data in a fileee user account. API built on 2019-06-18T22:01:06Z.

- Authorization follows the [OAuth 2.0](https://tools.ietf.org/html/rfc6749) standard, specifically the [Authorization Code Grant](https://tools.ietf.org/html/rfc6749#section-4.1) type.
  - Most API calls require the access token in the `Authorization` HTTP request header as a string     of the form `Bearer 12345xy` (where 12345xy needs to be replaced with the actual access token).
  - The _authorization endpoint_ is available at an URL of the form `https://my.workeee.de/oauth/authorize?response_type=code`     with the following additional request parameters: `client_id=` *your unique client ID* (REQUIRED),     `redirect_uri=` *the URI where the client should be redirected after authorization* (REQUIRED),     `scope=` *the space separated list of requested scopes* (OPTIONAL), `state=` *will be passed back to      the redirect endpoint unmodified* (RECOMMENDED).
- All unique IDs are strings in the format of [BSON ObjectIds](https://docs.mongodb.com/manual/reference/bson-types/#objectid).

## Authorization

Supported authorization schemes:
- OAuth2

For OAuth 2.0 you need to specify OAuth Client credentials as environment variables in the connector repository:
* `OAUTH_CLIENT_ID` - your OAuth client id
* `OAUTH_CLIENT_SECRET` - your OAuth client secret

## Actions

### API to create a special short-time login token.

> This API can be called with a valid access token to generate a new specialized token (not following any standard OAuth process) that can be used for a short time to login to the same account on a different device.

*Tags:* `Authorization`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### API to revoke and invalidate an OAuth token.

> Revoke or invalidate a token that is not needed any longer. Returns an empty response. After calling this method, the token (both access and refresh token, regardless of which one you passed) should not be used any longer.

*Tags:* `Authorization`

### API to get and exchange OAuth tokens.

> Exchange an authorization code into access and refresh tokens or (depending on the grant type) get a new access token using the refresh token.<br/>
> - Depending on what you want to do, exactly one of `code` or `refresh_token` is required.<br/>
> - For the `authorization_code` grant type (i.e. exchanging a code into tokens), you need to specify the code in the `code` parameter, for the `refresh_token` grant type (i.e. refreshing an access token) you need to give the refresh token in the `refresh_token`.<br/>
> - The `password` grant type is only available for special clients

*Tags:* `Authorization`

### Get own user-information

> Can be used to get information (like the participant ID) about the user associated with the supplied token (which needs to be valid).

*Tags:* `Authorization`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### Lists all available conversation-templates for a company

*Tags:* `ConversationTemplates`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### Create a new conversation.

> Only very specific fields in the conversation can be initially set. The title is the only mandatory field you have to specify (and it must be between 2 and 200 characters in length).<br/>
> - The messages and participants fields can optionally be used to directly add initial messages and participants to a conversation.<br/>
> - A participants ID is either a company ID in the fileee system (for `USER` and `COMPANY`) or an email address (for `EXTERNAL` participants).<br/>
> - See [the add message documentation](#/Conversations/addMessageToConversation) and [the add participant documentation](#/Conversations/addParticipant) below for details on the messages and participant fields supported.<br/>
> - A conversation has a system of roles and permissions controlling what participants can do in the context of this conversation, e.g. if they're   allowed to share documents or to invite other users. Each participant has a role that controls the basic permissions they have. In addition, the conversation also has attributes to give everyone (or just specific participants) some additional permissions on top of those from their role. The following fields control the roles and permissions:<br/>
>   - `roles`: A map from participant ID to the role of that participant.<br/>
>   - `defaultAdditionalPermissions`: A set of permissions everyone gets in addition to those from their role.<br/>
>   - `additionalPermissions`: A map from participant ID to a set of permissions that this specific participant gets in addition to those from their role.<br/>
> - For more details about the permission system, see [this document](https://docs.google.com/document/d/1gdL7cIvvIBdlJoVQEF_EAw-XVe_RX60y_HXz2Vwcvwc/edit?usp=sharing)

*Tags:* `Conversations`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### Create a new conversation from an existing template.

*Tags:* `Conversations`

#### Input Parameters
* `templateId` - _required_
* `Authorization` - _required_ - OAuth access token

### Create a new conversation from an existing template.

*Tags:* `Conversations`

#### Input Parameters
* `templateId` - _required_
* `extraFields` - _optional_
* `Authorization` - _required_ - OAuth access token

### Produces sample of the request body for the call 'createFromTemplate'.

> The sample request body will contain all variable keys with sample values from all modules of the given template and a sample participant.If any module contains any tasks, there will also be a sample override for the task identifier, for the tags and for the result of each task.

*Tags:* `Conversations`

#### Input Parameters
* `templateId` - _required_
* `Authorization` - _required_ - OAuth access token

### Get information about an invitation to a conversation.

> Get all the information available about an invitation to a conversation (e.g. what is the conversation about, who invited me, do we need password to accept?).

*Tags:* `Conversations`

#### Input Parameters
* `conversationToken` - _required_ - External token of an invitation

### Accepts the invitation to a conversation.

> Can be used to accept an invitation to a conversation, thereby importing the full conversation into the users fileee account.

*Tags:* `Conversations`

#### Input Parameters
* `conversationToken` - _required_ - External token of an invitation
* `Authorization` - _required_ - OAuth access token

### Rejects the invitation to a conversation.

> Can be used to reject an invitation to a conversation, thereby also making the token unusable in the future and removing the invitation object from users fileee account.

*Tags:* `Conversations`

#### Input Parameters
* `conversationToken` - _required_ - External token of an invitation
* `Authorization` - _required_ - OAuth access token

### Get existing conversation.

> Get the current state of a conversation, including all messages and participants.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - ID of an existing conversation
* `Authorization` - _required_ - OAuth access token

### Delete a conversation.

> Can be used to completely remove a conversation for all participants. Please note that you need the right permissions to perform this action.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - The ID of an existing conversation
* `Authorization` - _required_ - OAuth access token

### Get the document state of a conversation.

> Get the current state of requested and shared documents in a conversation. The `requestedDocuments` contain all the shared documents that could be matched to requests with their identifiers and information e.g. about the request state. The `unassignedDocuments` contain the document IDs of all documents shared in this conversation that were not assigned to one of the requests.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - ID of an existing conversation
* `Authorization` - _required_ - OAuth access token

### Share a document in a conversation

> Allows uploading and directly sharing a document in an existing conversation.<br/>
> <br/>
> Please note that this API method uses a new and simplified document JSON model.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - ID of an existing conversation
* `Authorization` - _required_ - OAuth access token

### Leave a conversation.

> Can be used to leave a conversation.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - The ID of an existing conversation
* `Authorization` - _required_ - OAuth access token

### Add a message to a conversation.

> This method allows adding a message to an existing conversation.<br/>
> <br/>
> - The `conversationId` is required, the conversation with that ID has to exist beforehand and the user from the given access token has to be a participant in it.<br/>
> - Only set the `id` of the message if you are sure you can reliably generate valid ObjectIDs on the client side. If you leave it blank, an ID will be generated for you.<br/>
> - If the `timestamp` field is left blank, a timestamp will be generated on the server.<br/>
> - The `senderId` field refers to the ID of the participant. If you leave this blank, the user from the given access token is assumed to be the sender.<br/>
> - The `onlyVisibleFor` field can optionally contain a set of participant IDs if you want to limit visibility of the message to specific participants.<br/>
> - Some of the messages (e.g. `DOCUMENT`) actually trigger the respective action (e.g. sharing a document), so be careful to only add them if you actually want that to happen.<br/>
> <br/>
> The following list describes the message types supported and their respective fields. All of this also applies to the messages that can be directly added using the create conversation API method.<br/>
> <br/>
> ---<br/>
> `CHAT`: Normal textual chat message.<br/>
> ```<br/>
> {<br/>
>   "type": "CHAT",<br/>
>   "senderId": "5798835568c0e021ebc6eb48",<br/>
>   "message": "Hello there"<br/>
> }<br/>
> ```<br/>
> ---<br/>
> `DOCUMENT`: Share an existing document in this conversation. The document has to be already uploaded to the sending users fileee account. The `identifier` can optionally be set to match this document to one requested by the `REQUEST_DOCUMENTS` message described below. The `remove` flag can optionally be set to `true` to remove an existing document with the given `documentId` from the conversation (if the `identifier` is also given, that identifier is removed from the shared document and the document is only completely removed if that was the only identifier). A document can only be removed by the user that originally shared it.<br/>
> ```<br/>
> {<br/>
>   "type": "DOCUMENT",<br/>
>   "documentId": "5798835568c0e021ebc6eb3e",<br/>
>   "identifier": "PAY_STATEMENT1",<br/>
>   "remove": false<br/>
> }<br/>
> ```<br/>
> ---<br/>
> `STATUS`: Changes the conversation status (e.g. if it is OPEN new messages can be added, if its CLOSED no new messages can be added by the user).<br/>
> ```<br/>
> {<br/>
>   "type": "STATUS",<br/>
>   "senderId": "5798835568c0e021ebc6eb48",<br/>
>   "status": "CLOSED"<br/>
> }<br/>
> ```<br/>
> ---<br/>
> `META_INFORMATION`: Can be used to exchange programmatic meta-information between all participants in a conversations in a generic map with string keys and string values. This message will never be directly shown to the user. The keys and values used are application-dependent and should be agreed upon for each specific scenario. For compatibility-reasons the keys should not contain dots.<br/>
> ```<br/>
> {<br/>
>   "type": "META_INFORMATION",<br/>
>   "information": {<br/>
>     "action": "NEW_APPLICATION",<br/>
>     "applicationType": "BUILDING_LOAN",<br/>
>     "applicationId": "FOO-BAR-23"<br/>
>     }<br/>
> }<br/>
> ```<br/>
> ---<br/>
> `REQUEST_DOCUMENTS`: Requests specific documents to be shared in this conversations (or updates their state). Multiple documents can be requested or updated with a single message. All requested documents from all previous messages are accumulated (and newer requests with the same `identifier` replace older requests for the same document) over time, unless `clearPrevious` is specified (after which all previous requests not contained in the current message in this conversation are forgotten).<br/>
> <br/>
> - For the individual documents the `identifier` is a technical ID unique at least for this conversation,<br/>
> - the `status` can be used to designate a process-specific review state of a document (can be empty/not set for documents not yet submitted, `submitted` for documents that are submitted but not yet reviewed, `accepted` or `rejected` for reviewed documents),<br/>
> - the `type` can optionally be specified to narrow down the type of document requested,<br/>
> - the `tags` can optionally contain a set of user-visible textual tags used for grouping multiple requested documents logically,<br/>
> - the `name` is a short name shown in the list of requested documents and the `description` is a longer plaintext description shown to the user in detail views.<br/>
> <br/>
> To update the status of a requested document, another request message has to be sent where the `status` field of the document with the right `identifier` is set to the new value.<br/>
> <br/>
> ```<br/>
> {<br/>
>   "type": "REQUEST_DOCUMENTS",<br/>
>   "message": "Please share the following documents with us",<br/>
>   "clearPrevious": true,<br/>
>   "documents": [<br/>
>       { "identifier": "PAY_STATEMENT1", "type": "payslip", "status": "submitted", "name": "Pay statement",  "description": "Your pay statement for 2015", "tags": ["Loan", "Brookhaven"] },<br/>
>       { "identifier": "CERT1", "status": "accepted", "name": "Insurance certificate", "description": "Your insurance certificate for the house", "tags": ["Loan", "Jonestown"] },<br/>
>       { "identifier": "CERT2", "name": "Other certificate", "description": "Your insurance certificate for the other house", "tags": ["Loan", "Brookhaven"] }<br/>
>     ]<br/>
> }<br/>
> ```<br/>
> ---<br/>
> `REQUEST_ACTION`: Requests the user to provide information or perform other specific actions. The task described by this message consist of one or more parts or sub-tasks (the individual actions), each resulting in one specific piece of information. This can be used to get the user to e.g. fill out a form, submit an application, cash in a voucher or participate in another business process that requires the users interaction. The overall task and message can have the following properties:<br/>
> <br/>
> - The `identifier` is a technical ID unique at least for this conversation (newer requests with the same `identifier` replace older tasks),<br/>
> - the `title` is the title for the overall task,<br/>
> - the `description` is a longer plaintext description shown to the user when completing the task,<br/>
> - the `status` can optionally be set to update the status of an existing request with the given identifier,<br/>
> - the `tags` can optionally contain a set of user-visible textual tags used for grouping multiple requests logically,<br/>
> - the `thankYouMessage` can optionally be set to specify a message that is shown to the user directly after providing the requested information,<br/>
> - and finally the `actions` are a list of the actual individual steps or pieces of information required, they must not be empty.<br/>
> <br/>
> The individual actions can have the following fields:<br/>
> <br/>
> - the `key` identifies this piece of information (e.g. "shippingAddress"). Required and should be unique among the different actions in one request,<br/>
> - the `actionType` should be one of `VALUE` (request to provide a value) or `DOCUMENT` (request to provide a document as part of a larger process), other types may be added in the future or supported for fileee-internal purposes,<br/>
> - the `template` is used to specify a template for a requested `VALUE` action, currently supported templates include `text`, `integer`, `double`, `boolean`, `date`, `date_time`, `amount`, `bank_account` and `address`,<br/>
> - the `templateOptions` can be used to set template-specific options like the maximum value or other constraints,<br/>
> - the `title` is a title for this specific action,<br/>
> - the `description` is a description for this specific action,<br/>
> - the `fieldDescription` is an optional field description that can be shown in or next to the actual field the user has to fill to provide the value,<br/>
> - the `optional` flag can be set to `true` if this action is not required and providing it can be skipped by the user.<br/>
> <br/>
> Some examples follow, but a more detailed explanation about using `REQUEST_ACTION` messages to add tasks for the user can be found [in this document](https://docs.google.com/document/d/1LzavoArlwTpbChWFB3QF9DgfnOmkVLGxPRUSS__s3RM/edit?usp=sharing).<br/>
> <br/>
> This example shows one task where three pieces of information are requested, a document, a textual value and an amount (with a maximum value):<br/>
> ```<br/>
> {<br/>
>   "type": "REQUEST_ACTION",<br/>
>   "identifier": "request-payout",<br/>
>   "message": "Hi Jane, your line of credit has been granted. You can now request a payout.",<br/>
>   "title": "Request payout",<br/>
>   "description": "You can request a payout here",<br/>
>   "actions": [<br/>
>     {<br/>
>       "actionType": "DOCUMENT",<br/>
>       "title": "Utility bill",<br/>
>       "description": "The bill you want reimbursed",<br/>
>       "key": "billDocument"<br/>
>     },<br/>
>     {<br/>
>       "actionType": "VALUE",<br/>
>       "template": "text",<br/>
>       "title": "Usage",<br/>
>       "description": "For what do you need the money?",<br/>
>       "key": "usage"<br/>
>     },<br/>
>     {<br/>
>       "actionType": "VALUE",<br/>
>       "template": "amount",<br/>
>       "templateOptions": {<br/>
>         "max": 5000<br/>
>       },<br/>
>       "title": "Payout Amount",<br/>
>       "description": "How much money should be disbursed?",<br/>
>       "fieldDescription": "Amount",<br/>
>       "key": "payoutAmount"<br/>
>     }<br/>
>   ]<br/>
> }<br/>
> ```<br/>
> This is an example how to completely remove an existing 'REQUEST_ACTION' task from the list of current tasks:<br/>
> ```<br/>
> {<br/>
>   "type": "REQUEST_ACTION",<br/>
>   "identifier": "request-payout",<br/>
>   "status": "removed"<br/>
> }<br/>
> ```<br/>
> <br/>
> ---<br/>
> `ACTION_RESULT`: Describes the result returned in response to a `REQUEST_ACTION` message. Should never be added by external partners, but will be generated by the fileee apps and is document here for consistency.<br/>
> ```<br/>
> {<br/>
>   "type": "ACTION_RESULT",<br/>
>   "id": "58de7f90d57e3ba4125526e7",<br/>
>   "timestamp": "2017-03-31T16:10:56.921Z",<br/>
>   "identifier": "request-payout",<br/>
>   "stepResults": {<br/>
>     "type": "COMPOSED",<br/>
>     "data": {<br/>
>       "billDocument": {<br/>
>         "value": "57fa2f91d573fcb641352a24",<br/>
>         "type": "TEXT"<br/>
>       },<br/>
>       "usage": {<br/>
>         "value": "I need a reimbursement for this utility bill",<br/>
>         "type": "TEXT"<br/>
>       },<br/>
>       "payoutAmount": {<br/>
>         "type": "COMPOSED",<br/>
>         "data": {<br/>
>           "value": {<br/>
>             "value": 23.5,<br/>
>             "type": "DOUBLE"<br/>
>           },<br/>
>           "currency": {<br/>
>             "value": "EURO",<br/>
>             "type": "ENUMERATION"<br/>
>           }<br/>
>         }<br/>
>       }<br/>
>     }<br/>
>   }<br/>
> }<br/>
> ```<br/>
> ---<br/>
> `CONTACT_INFORMATION`: Updates the contact information provided to the user. Only the last message of this type is taken into account for display. This gives the user some additional points of contact (e.g. phone numbers) to contact regarding this conversation. The `identifier` is a technical ID unique at least for this conversation.<br/>
> ```<br/>
> {<br/>
>   "type": "CONTACT_INFORMATION",<br/>
>   "contacts": [<br/>
>       { "identifier": "54321", "name": "fileee GmbH", "phoneNumber": "+4925132368309", "description": "Your fileee support contact" }<br/>
>     ]<br/>
> }<br/>
> ```<br/>
> ---<br/>
> `CHANGE_IDENTIFIERS`: Can be used when one or more of the identifiers used to identify requested / shared documents needs to be changed. Both document requests and shared documents will use the provided mapping. Please note that instead of using this, we would strongly advise to use non-changing identifiers throughout one conversation / business process!<br/>
> ```<br/>
> {<br/>
>   "type": "CHANGE_IDENTIFIERS",<br/>
>   "changedIdentifiers": {<br/>
>     "OLD_IDENTIFIER1": "NEW_IDENTIFIER1",<br/>
>     "OLD_IDENTIFIER2": "NEW_IDENTIFIER2"<br/>
>     }<br/>
> }<br/>
> ```

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - The ID of the conversation
* `Authorization` - _required_ - OAuth access token

### Get a specific message from a conversation.

> Can be used if you just need one specific message and already know its ID from other means.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - ID of an existing conversation
* `messageId` - _required_ - ID of an existing message in that conversation
* `Authorization` - _required_ - OAuth access token

### Adds a module to an existing conversation.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_
* `messageBundleId` - _required_
* `Authorization` - _required_ - OAuth access token

### Produces sample of the request body for the call 'createFromTemplate'.

> The sample request body will contain all variable keys with sample values from this module.If the module contains any tasks, there will also be a sample override for the task identifier, for its tags and for the result of each task.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_
* `messageBundleId` - _required_
* `Authorization` - _required_ - OAuth access token

### Add a participant to a conversation.

> Can be used to add/invite another participant to a conversation. Please note that adding other users will require special permissions.<br/>
> <br/>
> When adding `EXTERNAL` participants, the `id` needs to a valid email address. An invitation link to the conversation will be sent to that address. You can also set a {{phoneNumber}} for external participants, in which case the user is sent a password via SMS in addition to the link in the email, as a second security factor for accessing this conversation.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - The ID of an existing conversation
* `role` - _optional_ - The role of the new participant.
    Possible values: VIEWER, EDITOR, ADMIN.
* `resendInvitationEmail` - _optional_ - If the invitation mail should be resend when adding this participant and if it already exists in the conversation.
* `Authorization` - _required_ - OAuth access token

### Remove a participant from a conversation.

> Can be used to remove one or more participants from a conversation. Please note that you need the right permissions to perform this action.

*Tags:* `Conversations`

#### Input Parameters
* `conversationId` - _required_ - The ID of an existing conversation
* `Authorization` - _required_ - OAuth access token

### Upload a new document

> Uploading a document works through a `multipart/form-data` request with several parts. One of these parts contains the actual binary file to upload, the others can contain additional meta-data.<br/>
> - You can upload one document consisting of one PDF (with one or more pages) or multiple page images (i.e. JPG, PNG or GIF) at a time.<br/>
>   - If you are uploading multiple page images (e.g. scanned), please use the same file type for all images in one request.<br/>
>   - The order of the parts in the request determines the page order of the document.<br/>
> - After this API call returned, the document will be analyzed, thumbnails images will be generated and OCR will be done   (all done asynchronously in the background). Check the `status` field of the document meta-data and wait until   it is `CLASSIFIED` if you want to get the final version of the analyzed document.<br/>
> - All binary file parts will be used as described above, regardless of their parameter name. Nevertheless, we recommend naming the parameter for a single uploaded document `file` and for multiple page images `page0`, `page1` etc.<br/>
> - The `document` parameter uses the general document JSON that is also used for the response. You can use this to provide e.g.   an initial title or document type for the document or the ID of the fileeeBox to which this document should directly be added like this:  <br/>
> ```  <br/>
>     {  <br/>
>       "id": "5798835568c0e021ebc6eb3e",  <br/>
>       "documentTypeId": "bill",  <br/>
>       "documentInformation": {  <br/>
>         "title": "My awesome document",  <br/>
>         "fileeeBoxId": "5727934568c2f012eac3ca5f"  <br/>
>       }  <br/>
>     }  <br/>
> ```<br/>
>   - The currently supported values for documentTypeId are: `bill`, `receipt`, `ticket`, `payslip`, `certificate`, `information`, `letter`, `other`, `contract`, `ad`, `bankstatement`<br/>
> - The `meta` parameter may contain additional meta information for this upload that is not part of the actual document JSON.   You can use this to cause some specific actions to be done with the uploaded document. One example is directly sharing the  uploaded document in an existing conversation (optionally with an identifier described in the 'Add message' documentation) like this:  <br/>
> ```<br/>
>     {  <br/>
>       "conversationInformation": {  <br/>
>         "conversationId": "5798835568c0e021ebc6eb3f",  <br/>
>         "identifier": "PAY_STATEMENT1"  <br/>
>       }  <br/>
>     }  <br/>
> ```<br/>
> - Please note that the 'Try it out'-feature of the docs website will not work for this API call because of limitations in   swagger. You have to use other tools like Postman or your own code to try it.

*Tags:* `Documents`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### Get metadata for a document.

> The meta-data contains information about the type of document and other attributes and properties either found through our automatic analysis or set by the user.

*Tags:* `Documents`

#### Input Parameters
* `documentId` - _required_ - ID of the document
* `Authorization` - _required_ - OAuth access token

### Download a document as a PDF.

> Always produces a PDF, regardless of the original document format.

*Tags:* `Documents`

#### Input Parameters
* `documentId` - _required_ - ID of the document
* `mode` - _optional_ - Determines the content-disposition returned
    Possible values: download, print.
* `pages` - _optional_ - An optional comma-separated list of page IDs to include, defaults to all
* `max-size` - _optional_ - An optional int value for maximum size of the pdf in bytes. The resolution of the single pages will be reduced if original is too big. Decreases download speed.
* `Authorization` - _required_ - OAuth access token

### Connect a service to a customer with given identifier.

> Can optionally be connected with verification, in this case the service will not appear in the users account until he or she verified him- or herself.

*Tags:* `Enterprise`

#### Input Parameters
* `customerId` - _required_ - Identifier of an existing customer.
* `Authorization` - _required_ - OAuth access token

### Update a service for a customer with given identifier.

> Verification can e.g be removed by updating the verification type to NONE.

*Tags:* `Enterprise`

#### Input Parameters
* `customerId` - _required_
* `serviceInstanceId` - _required_ - Identifier of an existing service instance.
* `Authorization` - _required_ - OAuth access token

### Disconnect a service from a customer with given identifier.

> If a disconnect template was configured when connecting this service, the user will receive a conversation instance from that template.

*Tags:* `Enterprise`

#### Input Parameters
* `customerId` - _required_ - Identifier of an existing customer.
* `serviceInstanceId` - _required_ - Identifier of an existing service instance.
* `Authorization` - _required_ - OAuth access token

### getBoxList

*Tags:* `Fileeebox`

### createBox

*Tags:* `Fileeebox`

### checkORCode

*Tags:* `Fileeebox`

#### Input Parameters
* `qrCode` - _required_

### deleteBox

*Tags:* `Fileeebox`

### Activate a fileeeBox for an user.

*Tags:* `Fileeebox`

#### Input Parameters
* `qrCode` - _required_
* `Authorization` - _required_ - OAuth access token

### Get the user information for a public accessible fileeeBox.

*Tags:* `Fileeebox`

#### Input Parameters
* `qrCode` - _required_
* `Authorization` - _required_ - OAuth access token

### getBox

*Tags:* `Fileeebox`

#### Input Parameters
* `boxId` - _required_

### addDocumentToBox

*Tags:* `Fileeebox`

#### Input Parameters
* `boxId` - _required_
* `documentId` - _required_

### removedDocumentFromBox

*Tags:* `Fileeebox`

#### Input Parameters
* `boxId` - _required_
* `documentId` - _required_

### Lists all available modules for a company

*Tags:* `Modules`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### Gets a sample ActionResultMessage for a RequestActionMessage.

*Tags:* `Modules`

#### Input Parameters
* `moduleId` - _required_ - ID of an existing message bundle in that conversation template
* `messageId` - _required_ - ID of an existing message in that message bundle
* `mode` - _optional_ - Determines the content-disposition of the response
    Possible values: download, inline.
* `Authorization` - _required_ - OAuth access token

### Get all persons.

> Either all or a limited amount of persons is returned.

*Tags:* `Persons`

#### Input Parameters
* `limit` - _optional_ - An optional number of maximum elements that should be returned.
* `offSet` - _optional_ - Starting index in the list of existing persons.
* `modifiedAfter` - _optional_ - An optional date in milliseconds to get only persons which were modified after that date.
* `Authorization` - _required_ - OAuth access token

### Create a new person.

> The person from the request body is created. Addresses, contact data, logo and branding information are stored, if existent.

*Tags:* `Persons`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### Checks if the logo can be processed.

*Tags:* `Persons`

### Get a person with specified identifier.

> The returned person object includes existing contact data and addresses as well as branding information.

*Tags:* `Persons`

#### Input Parameters
* `id` - _required_ - Identifier of an existing person.
* `Authorization` - _required_ - OAuth access token

### Update an existing person with specified identifier.

> If the person with the given ID exists, will be updated with person values from request body.

*Tags:* `Persons`

#### Input Parameters
* `id` - _required_ - Identifier of the person that should be updated.
* `Authorization` - _required_ - OAuth access token

### Delete the person with given identifier.

*Tags:* `Persons`

#### Input Parameters
* `id` - _required_ - Identifier of the person that should be deleted.
* `Authorization` - _required_ - OAuth access token

### Get the logo of the person with specified identifier.

> Returns the image data in format PNG.

*Tags:* `Persons`

#### Input Parameters
* `id` - _required_ - ID of an existing Person.
* `Authorization` - _required_ - OAuth access token

### Create a new logo for a Person with given identifier.

*Tags:* `Persons`

#### Input Parameters
* `id` - _required_ - ID of an existing Person.
* `Authorization` - _required_ - OAuth access token

### Create a PostBox for a user.

> Creates a PostBox into which documents can be uploaded for a user. The user does not have to have an existing fileee account.

*Tags:* `PostBox`

#### Input Parameters
* `testPhone` - _optional_
* `Authorization` - _required_ - OAuth access token

### Get the status of a PostBox.

*Tags:* `PostBox`

#### Input Parameters
* `customerId` - _required_
* `Authorization` - _required_ - OAuth access token

### Send a document to a user

> Allows companies to send documents via **fileee** to the **postbox** of a user. This virtual postbox can take several different forms, e.g. either one folder or _shared space_ that receives all the documents from one company for a user or several individual _conversations_ for specific documents. The user does _not_ have to have an existing fileee account, they will be notified via email when you first want to share a document with them.<br/>
> <br/>
> - Uploading a document works through a **`multipart/form-data`** request with several parts. One of these parts contains the actual binary file to upload, another contains required information about the target user, the document and how it should be presented to the user.<br/>
> - The `file` form-part should contain the document file to send to the user. We strongly recommend **PDF** as a format, although some image types are also supported.<br/>
> - The `request` form-part must contain all the required information to deliver the document to a target user and can contain additionalmeta-data properties of the document, like a title or issue date that will be shown to the user. The content-type must be `application/json`.<br/>
> - **Please note:** The 'Try it out'-feature of the docs website will not work for this API call because of limitations in   swagger. You have to use other tools like Postman or your own code to try it.<br/>
> <br/>
> ---<br/>
> The `request` form-part describes the details of the request:<br/>
> <br/>
> - The `externalId` can optionally be set to an arbitrary value that helps you identify the request in your own systems, it is not used by fileee.<br/>
> - The `recipient` describes which user should receive this document. This field is required.<br/>
> - The `configuration` describes how the document should be delivered and presented to the user. This field is required.<br/>
> - The `document` describes the document-related meta-data, most of which is directly shown to the user in fileee. This field (but not all parts of it) is required.<br/>
> <br/>
> A very minimal request could look like this:<br/>
> <br/>
> ```<br/>
> {<br/>
>   "recipient": {<br/>
>     "email": "some.user@example.com",<br/>
>   },<br/>
>   "configuration": {<br/>
>     "type": "TEMPLATE",<br/>
>     "templateName": "our-template"<br/>
>   },<br/>
>   "document": {<br/>
>     "title": "Our awesome document"<br/>
>   }<br/>
> }<br/>
> ```<br/>
> <br/>
> ---<br/>
> The `recipient` field in the request identifies the target user that should receive the document. The minimum we need here is an email address, but a name should be given if possible to personalize the conversation. You can optionally also specify a phone number which can be used when sending out security codes to access protected content.<br/>
> <br/>
> ```<br/>
> {<br/>
>   "email": "some.user@example.com",<br/>
>   "name": "Some User",<br/>
>   "phoneNumber": "+49123456789",<br/>
> }<br/>
> ```<br/>
> <br/>
> ---<br/>
> The `configuration` field in the request controls how the new document is delivered and presented to the user. The most important parameter is the **type**, which also controls which other fields in the configuration need to be set. For the `TEMPLATE` type, you just need to specify the name of a template which in turn defines the attributes, messages etc. of the conversation or shared space for the document:<br/>
> <br/>
> ```<br/>
> {<br/>
>   "type": "TEMPLATE",<br/>
>   "templateName": "awesome-invoice-template23"<br/>
> }<br/>
> ```<br/>
> For the `CUSTOM` type, you can specify all the details of the created conversation yourself:<br/>
> <br/>
> ```<br/>
> {<br/>
>   "type": "CUSTOM",<br/>
>   "conversation": {<br/>
>     "kind": "new_loan_application",<br/>
>     "title": "Your new loan application was received",<br/>
>     "companyId": "5798835568c0e021ebc6eb44",<br/>
>     // ...<br/>
>   }<br/>
> }<br/>
> ```<br/>
> You should always only set the parameters relevant for the specific type, so e.g. only set the `templateName` if the `type` is `TEMPLATE` and only set the `conversation` when the `type` is `CUSTOM`.<br/>
> <br/>
> ---<br/>
> The `document` field in the request should contain attributes of the document that is created. This controls the title of the document that is shown to the user in fileee, but can also contain other attributes like an issue date or the sender of the document:<br/>
> <br/>
> ```<br/>
> {<br/>
>   "title": "Your invoice for August 2017",<br/>
>   "issueDate": "2017-08-23",<br/>
>   "sender": {<br/>
>     "contactId": "50ac222b0cf2f924c9ccb919"<br/>
>   },<br/>
>   "receiver": {<br/>
>     "name": "Awesome Fruit GmbH",<br/>
>     "street": "Superstr. 23",<br/>
>     "postalCode": "48143",<br/>
>     "city": "Munster"<br/>
>   }<br/>
> }<br/>
> ```<br/>
> A contact (sender or receiver) should _either_ contain a `contactId` _or_ the postal address fields, never both. If the receiver is not specified, we assume the document to be addressed to the fileee user in question.

*Tags:* `PostBox`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### Activates the postbox for a user

> This can be used to activate a postbox for a user. <br/>
> One reason can be that he handed in the required information personally so that he is no longer required to finish the task. <br/>
>  Limitations here are: <br/>
> - The invited user must be registered.<br/>
> - Documents send to the postbox before the activation are deleted and WON'T be available to the user

*Tags:* `PostBox`

#### Input Parameters
* `customerId` - _required_
* `Authorization` - _required_ - OAuth access token

### Connect ThinkOwl Desk

> Connect ThinkOwl Desk with fileee account by providing credentials for accessing desk

*Tags:* `ThinkOwl`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### Deprecated: Get own user-information

> **(Deprecated, see `/authorization/user-info` instead)** Can be used to get own participantId

*Tags:* `Users`

#### Input Parameters
* `Authorization` - _required_ - OAuth access token

### resendActivationMail

*Tags:* `Users`

### resendConversationSms

*Tags:* `Users`

#### Input Parameters
* `externalToken` - _required_

## License

**flow**ground :- Telekom iPaaS / fileee-api-connector<br/>
Copyright © 2019, [Deutsche Telekom AG](https://www.telekom.de)<br/>
contact: flowground@telekom.de

All files of this connector are licensed under the Apache 2.0 License. For details
see the file LICENSE on the toplevel directory.
