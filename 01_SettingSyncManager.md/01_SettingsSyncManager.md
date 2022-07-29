# SettingsSyncManager

## Purpose

The purpose of this sub-component is to manage persistence of certain client settings in local storage on the device and also post them to the Data Platform, so that they may be preserved and downloaded if the user reinstalls the client app. Additionally, on iOS, this sub-component communicates settings changes between the phone app and the watch app.

## Configurability

Field                       | Type                  | Description
--------------------------- | --------------------- | ------------------
settingsSyncPartitionId     | UID String            | PartitionId is a required parameter of the Data Platform's Named Values endpoints, which SettingsSyncManager uses to save settings on the server.  "Name" & "PartitionId" are a composite key for values stored on the server. Named Values are associated with a particular user account and can be thought of as being "grouped" by partitionId, so that a user may store values with identical names in different partitions. understand the
g6SettingsSyncPartitionId   |	UUID String           | The G6 app's PartitionId is used to retrieve the G6 settings from Data Platform's Named Values endpoints of the user, if there are no G7 Settings.  
defaultAlertSettings        | [AlertSettingsRecord] | The defaultAlertSettings contains the default values of all the Alert Settings. These are used during mapping the G6 data to G7 Alert Settings. G6 settings do not have 1:1 mapping of each field from the server for Alerts. We use these Alert Settings with default values for properties that do not have mapping from G6.

## Dependencies

### NamedValuesManager

Handles storage of user settings as Name-Value pairs on the Data Platform servers.

### AuthManager

Provides authentication status and data consent status to determine whether settings may be posted to the Data Platform.

### AppLifecycleManager (iOS only)

Observes app lifecycle notifications and converts them to an enum published via Combine. Subscribers can react to the various lifecycle events from the sink closure. On iOS, SettingsSyncManager listens for appDidBecomeActive in order to request the latest user settings from the instance of the SDK running on the paired device. This is particularly important for the instance running on the watch since the watch app spends most of its time in an inactive state and will receive realtime settings updates from the phone infrequently.

### WatchCommunication (iOS only)

Facilitates synchronization of user settings between the phone app and the watch app.
