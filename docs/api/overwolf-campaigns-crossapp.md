---
id: overwolf-campaigns-crossapp
title: overwolf.campaigns.crossapp API
sidebar_label: overwolf.campaigns.crossapp
---

Use this API to allow crossapp-promotions.

One app can promote another app and then get an indication for a successful conversion.  
For example - an app can promote a video capture and sharing app and receive a notification as soon as the user shares a video from the promoted app.

**Note that the Achievement Rewards app will only work on the Overwolf Client version 0.156.0.16 and above**.

## Methods Reference

* [overwolf.campaigns.crossapp.getAvailableActions()](#getavailableactionscallback)
* [overwolf.campaigns.crossapp.set()](#setcampaign-callback)
* [overwolf.campaigns.crossapp.reportConversion()](#reportconversionconversioninfo-callback)
* [overwolf.campaigns.crossapp.consumeConversions()](#consumeconversionscallback)

## Events Reference

* [overwolf.campaigns.crossapp.onAvailableActionUpdated](#onavailableactionupdated)

## Types Reference

* [overwolf.campaigns.crossapp.CrossAppCampaign](#crossappcampaign-object) Object
* [overwolf.campaigns.crossapp.CrossAppCampaignConversion](#crossappcampaignconversion-object) Object
* [overwolf.campaigns.crossapp.GetCrossAppAvailableActionsResult](#getcrossappavailableactionsresult-object) Object
* [overwolf.campaigns.crossapp.GetCrossAppConversionsResult](#getcrossappconversionsresult-object) Object

## getAvailableActions(callback)
#### Version added: 0.158

> Receive all cross-app actions that target the currently running extension.

Parameter | Type                  | Description                                                             |
--------- | ----------------------| ----------------------------------------------------------------------- |
callback  | ([Result: GetCrossAppAvailableActionsResult](#getcrossappavailableactionsresult-object)) => void  | Returns an array of cross-app actions   |

## set(campaign, callback)
#### Version added: 0.158

> Initiate or modify a cross-app campaign action for this extension.

You may modify an existing action by using the same id parameter. See [CrossAppCampaign.id](#crossappcampaign-object).

Parameter  | Type                  | Description                                                             |
---------- | ----------------------| ----------------------------------------------------------------------- |
campaign   | [CrossAppCampaign](#crossappcampaign-object) object   |                                         |
callback   | (Result) => void      | Reports success or failure.                                             |

#### Callback argument: Success

Returns with the code

```json
{"success":true}

```

#### Usage example

```js
overwolf.campaigns.crossapp.set({
 id: '8mRuEC_bbbbbbDxk', // fake
 action: 'ar-invite',
 expiration: Date.now() + 1000 * 60 * 60 * 24 * 7,
 target_apps_uids: ['*'],
 data: {
   name: 'ar-aw-gr-2020', // fake
   iconUrl: 'https://content.overwolf.com/achievement-rewards/campaigns/8mRuEC_bbbbbbDxk/icon.svg', // will be a local file in the future
   title: "Gewinne 650 RP",
   text: "mit der Alienware Challenge!"
 }
}, console.log);
```

## reportConversion(conversionInfo, callback)
#### Version added: 0.158

> Submit new conversion for a cross-app campaign.

Parameter      | Type                  | Description                                                             |
-------------- | ----------------------| ----------------------------------------------------------------------- |
conversionInfo | [CrossAppCampaignConversion](#crossappcampaignconversion-object) object    |                    |
callback       | (Result) => void      | Reports success or failure.                                             |

#### Callback argument: Success

Returns with the code

```json
{"success":true}

```

## consumeConversions(callback)
#### Version added: 0.158

> Consume all pending conversions for this extension. Consumed conversions are deleted.

Parameter      | Type                  | Description                                                             |
-------------- | ----------------------| ----------------------------------------------------------------------- |
callback       |  [GetCrossAppConversionsResult](#getcrossappconversionsresult-object) object      |    |

## onAvailableActionUpdated

> Called when an available action has updated (or added), with the following structure: [CrossAppCampaign](#crossappcampaign-object) Object.

#### Version added: 0.158

> Fired when current extension campaign is updated.


## GetCrossAppAvailableActionsResult Object

Parameter          | Type                            | Description                                       |
-------------------| --------------------------------| ------------------------------------------------- |
*success*          | boolean                         | inherited from the "Result" Object                |
*error*            | string                          | inherited from the "Result" Object                |
actions            | [CrossAppCampaign](#crossappcampaign-object)[]  |                                   |   

#### Example data: Success

```json
{"success":true, "actions":[]}
```

## CrossAppCampaign Object

> Container that represent a shared data parameters.

Parameter          | Type                            | Description                                       |
-------------------| --------------------------------| ------------------------------------------------- |
id                 | string                          | An id to identify the campaign (action/conversion). See [notes](#id-notes). |   
action             | string                          | The type of action this cross-app campaign supports.This is a free-text string. |   
expiration         | number                          | Expiration date expressed in milliseconds since epoch (Unix Time, UTC). See [notes](#expiration-notes). |   
owner_app_uid      | string                          | The UID of the app that owns the targeted cross-app campaign. Optional |   
target_apps_uids   | string[]                        | An array of app UIDs this cross-app campaign targets. Optional |   
data               | object                          | Information about the cross-app campaign. See [notes](#data-notes). |   

#### Example object data

```json
{
    "id": "8mRuEC_bbbbbbDxk", // fake
    "action": "ar-invite",
    "expiration": Date.now() + 1000 * 60 * 60 * 24 * 7,
    "target_apps_uids": ["*"],
    "data": 
    {
        "name": "ar-aw-gr-2020", // fake
        "iconUrl": "https://content.overwolf.com/achievement-rewards/campaigns/8mRuEC_bbbbbbDxk/icon.svg", // will be a local file in the future
        "title": "Gewinne 650 RP",
        "text": "mit der Alienware Challenge!"
    }
 }
```

#### id notes

"id" should be unique per an extension (two different extensions can use the same id).

#### expiration notes

e.g. Date.now() or (new Date()).getTime().

#### data notes

This is a free-form json object that gives more instructions on the required action.

## CrossAppCampaignConversion Object

> Container that represent a cross app campaign conversions.

Parameter          | Type                            | Description                                       |
-------------------| --------------------------------| ------------------------------------------------- |
id                 | string                          | The ID of the cross-app campaign the conversion targets. |   
owner_app_uid      | string                          | The UID of the app that owns the targeted cross-app campaign. |   
data               | object                          | Conversion data for the specified action. |   
origin_app_uid     | object                       | The UID of the app that performed the conversion (the promoted app). Optional. See [notes](#origin_app_uid-notes). |   
timestamp          | number                          | When the conversion took place.  Optional. See [notes](#timestamp-notes).  |   

#### origin_app_uid notes

Set by the Overwolf client when calling |consumeConversions|.

#### timestamp notes

Set by the Overwolf client when calling |consumeConversions|.

## GetCrossAppConversionsResult Object

> Container that represent a cross app conversions.

Parameter          | Type                            | Description                                       |
-------------------| --------------------------------| ------------------------------------------------- |
*success*          | boolean                         | inherited from the "Result" Object                |
*error*            | string                          | inherited from the "Result" Object                |
conversions        | [CrossAppCampaignConversion](#crossappcampaignconversion-object)[]   |                 |

