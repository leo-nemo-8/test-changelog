---
title: overview
deprecated: false
hidden: false
metadata:
  robots: index
---
<ApiOverviewCards />

| Parameter | Data Type | Description |
| --- | --- | --- |
| `updateCurrencyExpiryDateResponse` | object | Object containing details of the points expiry update request. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `customerId` | integer (int64) | Unique identifier of the customer whose points expiry is updated. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `programId` | integer (int64) | Unique identifier of the loyalty program. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `currencyName` | string | Unique name of the currency whose expiry is updated. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `pointsExpiryUpdateLogs` | object | Object containing details of the points expiry update. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↳ `paType` | enum | Type of points awarded to the user. |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↳ `paId` | integer (int64) | Unique reference ID generated when points are awarded to a user. |

new

````
| Parameter | Data Type | Description |
|-----------|-----------|-------------|
| updateCurrencyExpiryDateResponse | object | Object containing details of the points expiry update request. |
| .customerId | integer (int64) | Unique identifier of the customer whose points expiry is updated. |
| .programId | integer (int64) | Unique identifier of the loyalty program. |
| .currencyName | string | Unique name of the currency whose expiry is updated. |
| .pointsExpiryUpdateLogs | object | Object containing details of the points expiry update. |
| ..paType | enum | Type of points awarded to the user. Possible values: POINT_AWARDED, POINT_AWARDED_BILL_PROMOTION, POINT_AWARDED_LINEITEM, POINT_AWARDED_LINEITEM_PROMOTION, POINT_AWARDED_CUSTOMER_PROMOTION. |
| ..paId | integer (int64) | Unique reference ID is generated when points are awarded to a user. |