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
