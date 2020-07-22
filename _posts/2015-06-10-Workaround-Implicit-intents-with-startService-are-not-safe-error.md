---
layout: post
title: "Workaround for: Implicit intents with startService are not safe"
date: 2015-06-10
comments: true
---
In Android 5.0 and higher (API level >= 21) the system will throw an exception if you call *bindService()* with an implicit intent.
Here I will show you a simple workaround to change an implicit intent to an explicit intent.


Google has changed the usage of implicit intents to a start service. The reason is: 
> Using an implicit intent to start a service is a security hazard because you cannot be certain what service will respond to the intent, and the user cannot see which service starts.

Source: [http://developer.android.com/guide/components/intents-filters.html](http://developer.android.com/guide/components/intents-filters.html) 

If your target API level is smaller than 21 you will only recive a warning: *Implicit intents with startService are not safe*. But the warning is justified!
Malicious APPS could exploit an implicit intent and intercept messages.


#### Easy way to change an implict intent to an explicit intent:


- The actual java code which is binding the service we need to change:

	```
Intent intent = new Intent("action.name.of.a.service");
bindService(intent, serviceConnection, Service.BIND_AUTO_CREATE);
```
Change the intent to:

	```
Intent intent = new Intent("action.name.of.a.service");
intent.setPackage("the.package.of.the.service");
bindService(intent, serviceConnection, Service.BIND_AUTO_CREATE);
```
So we only need to now the package name of the class which implements the service.

- And the service from the manifest file. We don't need to change that:

	```
<service
	android:name=".service.ClassNameOfTheService"
	android:exported="true"
	android:process=":remote">
	<intent-filter>
		<action android:name="action.name.of.a.service"/>
	</intent-filter>
</service>
```


And now we have an explicit intent without knowing the class which will care about the call! Great if you call a service from another app or library and you can't name the class.