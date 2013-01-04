This is an update to https://github.com/phonegap/phonegap-plugins/tree/master/Android/LocalNotification. This version supports 2.x+, where the previous version was &lt; 2.x.

1. Copy the LocalNotification.js file to your 'www' folder and include it in your index.html
2. Create a package com.phonegap.plugin.localnotification
3. Copy the .java files into this package
4. Fix the import in AlarmReceiver.java around line 67 where R.drawable.ic_launcher is referenced so it matches an icon in your project. This will likely require no more than an import of your.package.R;
5. Update your res/xml/config.xml ( this was previously res/xml/plugins.xml ) file with the following line:

        <plugin name="LocalNotification" value="com.phonegap.plugin.localnotification.LocalNotification" />

6. Add the following fragment in the AndroidManifest.xml inside the &lt;application&gt; tag:

        <receiver android:name="com.phonegap.plugin.localnotification.AlarmReceiver" >
        </receiver>
		
        <receiver android:name="com.phonegap.plugin.localnotification.AlarmRestoreOnBoot" >
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
            </intent-filter>
        </receiver>
    
    The first part tells Android to launch the AlarmReceiver class when the alarm is be triggered. This will also work when the application is not running.
	The second part restores all added alarms upon device reboot (because Android 'forgets' all alarms after a restart).
	
7. The following piece of code is a minimal example in which you can test the notification:

      	<script type="text/javascript">

                function appReady() {
                	window.plugins = {
			        	LocalNotificationPlugin: cordova.require( 'cordova/plugin/localNotification' )
			        };
                        
                    console.log("Device ready");
                    
                    window.plugins.LocalNotificationPlugin.add({
                            date : new Date(),
                            message : "Phonegap - Local Notification\r\nSubtitle comes after linebreak",
                            ticker : "This is a sample ticker text",
                            repeatDaily : false,
                            id : 4
                    });
                      
                }

                document.addEventListener("deviceready", appReady, false);
        </script>
		
8. You can use the following commands:

	- window.plugins.LocalNotificationPlugin.add({ date: new Date(), message: 'This is an Android alarm using the statusbar', id: 123 });
	- window.plugins.LocalNotificationPlugin.cancel(123); 
	- window.plugins.LocalNotificationPlugin.cancelAll();
		
9. Enjoy. Matthew
