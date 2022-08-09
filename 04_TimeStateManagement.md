# Time State Management

## Purpose

The purpose of Time State Management is to publish the time state of the system.

## Sub-component Design

Time is a state of the application where absolute time is not certain. This state is orthogonal to the rest of the system and is useful to broadcast to other components/sub-components of the system so certain features can react. This management feature depends on the TimeProvider component.



![](../../../images/DexAppKit/settings_sync_manager/TSM_SubcomponentDesign.png)



### Workflows

#### Initialization


![](../../../images/DexAppKit/settings_sync_manager/TSM_Initialization.png)



#### Time State Change


![](../../../images/DexAppKit/settings_sync_manager/TSM_TimeStateChange.png)
