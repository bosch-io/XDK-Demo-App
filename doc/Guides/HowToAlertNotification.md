
# How to use the Alert Notification Object in the XDK #

This chapter will show you how you can use the alert notification object to send custom events to thr LWM2M Server (and thus to the cloud). We will create a periodic notification message as an example.

 
1. First of all go to the header file `Lwm2mObject_AlertNotification.c` and have a look at the function: `void ObjectAlertNotification_setValue(const char* alert);` 

		void ObjectAlertNotification_setValue(const char* alert)
		{
			if (strncmp(alert_message, alert, sizeof(alert_message) - 1) != 0) {
				if (LWM2M_MUTEX_LOCK(mutex)) {
					strncpy(alert_message, alert, sizeof(alert_message) - 1);
					printf("alert: %s\r\n", alert_message);
					LWM2M_MUTEX_UNLOCK(mutex);
					if (started) Lwm2mReporting_resourceChanged(&alertUriPath);
				}
			}
		}


	In the LWM2M example firmware the above function is called whenever the Button-1 i called. See: `ButtonMan.c` line: 54 and 68. As you can see setting the alert message is as simple as passing the message to this function.

2. To create a periodic event, you can create your own timer. Let's do this by adding an additional user application timer inside the `Lwm2mInterface.c` after the already defined periodic timer initialization in *line 418*. Use the following timer example code:

        OS_timerHandle_tp userAppTimer_ptr = OS_timerCreate((const int8_t *) "userAppTimer_ptr", // text name, used for debugging.
                                             OS_getMsDelayTimeInSystemTicks(1 * 1000),            // The timer period in ticks.
                                             pdTRUE,         // The timers will auto-reload themselves when they expire.
                                             (void *) NULL,  // Assign each timer a unique id equal to its array index.
                                             userAppTimerCB  // On expiry of the timer this function will be called.
                                             );
        OS_timerStart(userAppTimer_ptr, 1000);

	where `userAppTimerCB` is the function to be provided.

3. Now use the following example code to set and send the alert message: 
    
        void userAppTimerCB(OS_timerHandle_tp pxTimer)
        {
            (void) pxTimer;
	        char buf[100];
	        sprintf(buf,"User Alert:%lu:Periodic Alert:LOW:USER", getUtcTime());
	        ObjectAlertNotification_setValue(buf);
        }

4. In order to receive the message on the server side, you need to make sure the LWM2M server has activated observation of it the `AlertNotification:0` object instance or of the respective `alert` resource. When using the Bosch IoT XDK Demo App observation is enabled from the configuration tab of the frontend.