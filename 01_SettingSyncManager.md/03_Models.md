# Models

## SettingsSyncError

enum case              | Description                
-----------------------| ---------------------
notConsented           | The user's token does not contain the DataShare scope.
authenticationRequired | AuthManager reported that the user needs to re-authenticate.
networkError           | Some other error was returned by SecureNetworkingAPI.


## SyncableRecord (interface/protocol)


Field                | Type                  | Description
-------------------- | --------------------- | ------------------
| syncName	         | String	               | A computed property used at the name/key to store a value on the server.
| lastUpdateTime     | CalculatedTime        | Reset each time the record is saved.
| lastSyncTime       | CalculatedTime        | Reset each time the record is successfully posted to the server.


## Settings Persistence Model

SettingsSyncManager subscribes to publishers on AlertProfileRepository, QuietModesRecordRepository, and SyncableKeyValueRepository to know when changes have been made to AlertProfileRecord, AlertScheduleRecord, AlertSettingsRecord, QuietModesRecord, or SyncableKeyValueRecord. In SDK 2.2, QuietModesRecords are only synced from iOS to watchOS.

For iOS, SettingsSyncManager additionally subscribes to a publisher on FeatureFlagRepository to be notified of changes to FeatureFlagRecord. This is done in order to send feature flags one-way from the phone to the companion app on the watch. The watchOS app does not rely on GCS directly for the Unit of Measure feature flag since client apps in some regions will allow users to change their preferred Unit of Measure. When this flag changes on the phone, it will be synced to the watch. FeatureFlagRecord does not implement SyncableRecord since there is no need to keep track of a lastSyncTime (used for server sync retries) or a lastUpdateTime (used for server sync retries and two-way companion app syncing).




![](../../../../images/DexAppKit/settings_sync_manager/jLZTZzCu47zk_WgpJnohd7GIkeS8xGSKT1LmB7JX7axtu2RJrbhNoTbE8QpsVyTEdCIExHHEd5Rg9ZE_iUTxfhqI4dDP7sXoDJo9Ey8GBmKaoPuKWE1GoGOTuB01BfAamlaTtW5QmfVhgdfDvAytYaw419O3kayG8am2tsBrjcvOZZSKFa1Uyg9b8dGk5FLFV80ChIKdR8VkqGuaUkYu50lviIgmX5kYCIympMkgNWlzFcBq.png)




## AlertProfileServerModel & AlertSettingServerModel

These models represent the alert settings storage format used on the server to which both platforms must conform. SettingsSyncManager will translate between the server models and the local persistence models. These dedicated server models mitigate the possibility that future persistence changes on either platform break cross-platform compatibility.




![](../../../../../CgmCoreSDKSDS/images/DexAppKit/settings_sync_manager/AlertProfileServer.png)


The server models encoded as JSON will use snake_case for property names. Example:


...
{
\"is_enabled\":false,
\"alert_profile_type\":1,
\"mobile_platform\":\"Apple\",
\"days_enabled\":96,
\"is_schedule_enabled\":true,
\"start_time_seconds\":84600,
\"end_time_seconds\":113400,
\"sdk_version\":\"2.1.0\",
\"profile_name\":\"Night Time\",
\"settings\":[
{\"sound_intensity\":1,\"is_enabled\":true,\"level\":56,\"alert_identifier\":\"AA-01\",\"snooze_length_seconds\":1800,\"sound\":\"polarisUrgentLowMedium\",\"sound_preference\":0,\"snooze_enabled\":true,\"lifetime_alert_acount\":0},
{\"sound_intensity\":1,\"rate\":0,\"is_enabled\":true,\"level\":56,\"alert_identifier\":\"AA-02\",\"snooze_length_seconds\":1800,\"sound\":\"polarisUrgentLowSoonMedium\",\"sound_preference\":0,\"snooze_enabled\":true,\"lifetime_alert_acount\":0},
{\"sound_intensity\":1,\"is_enabled\":true,\"level\":70,\"alert_identifier\":\"AA-03\",\"snooze_length_seconds\":3600,\"sound\":\"polarisLowMedium\",\"sound_preference\":0,\"snooze_enabled\":true,\"lifetime_alert_acount\":0},
{\"delay_enabled\":false,\"sound_intensity\":1,\"is_enabled\":true,\"delay_length_seconds\":3600,\"level\":252,\"alert_identifier\":\"AA-04\",\"snooze_length_seconds\":3600,\"sound\":\"polarisHighMedium\",\"sound_preference\":0,\"snooze_enabled\":false,\"lifetime_alert_acount\":0},
{\"sound_intensity\":1,\"rate\":2,\"is_enabled\":false,\"level\":148,\"alert_identifier\":\"AA-05\",\"sound\":\"polarisRiseRateMedium\",\"sound_preference\":0,\"lifetime_alert_acount\":0},
{\"sound_intensity\":1,\"rate\":2,\"is_enabled\":false,\"level\":247,\"alert_identifier\":\"AA-06\",\"sound\":\"polarisFallRateMedium\",\"sound_preference\":0,\"lifetime_alert_acount\":0},
{\"delay_enabled\":true,\"sound_intensity\":1,\"delay_length_seconds\":1200,\"is_enabled\":true,\"alert_identifier\":\"AA-07\",\"sound\":\"polarisSignalLossMedium\",\"sound_preference\":0,\"lifetime_alert_acount\":0},
{\"delay_enabled\":true,\"sound_intensity\":1,\"delay_length_seconds\":1200,\"is_enabled\":true,\"alert_identifier\":\"AA-08\",\"sound\":\"polarisBriefSensorIssueMedium\",\"sound_preference\":0,\"lifetime_alert_acount\":0},
{\"sound\":\"polarisTechnicalAlertMedium\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-11\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-12\",\"is_enabled\":true},
{\"sound\":\"silence1sec\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-15\",\"is_enabled\":true},
{\"sound\":\"silence1sec\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-17\",\"is_enabled\":true},
{\"sound\":\"silence1sec\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-19\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-27\",\"is_enabled\":true},
{\"sound\":\"silence1sec\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-28\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-29\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":2,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-30\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-31-A\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-31-B\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":2,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-34\",\"is_enabled\":true},
{\"sound\":\"silence1sec\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-35\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-36\",\"is_enabled\":true},
{\"sound\":\"silence1sec\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-39\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-40\",\"is_enabled\":true},
{\"sound\":\"polarisTechnicalAlertMedium\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-41\",\"is_enabled\":true},
{\"sound\":\"silence1sec\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-42\",\"is_enabled\":true},
{\"sound\":\"polarisTechnicalAlertMedium\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-43\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":2,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-44\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-51\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AA-52\",\"is_enabled\":true},
{\"sound\":\"polarisSystemAlert\",\"sound_intensity\":1,\"sound_preference\":0,\"lifetime_alert_acount\":0,\"alert_identifier\":\"AD-01\",\"is_enabled\":true}
]
}
...
