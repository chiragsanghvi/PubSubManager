PubSubManager
=============

A library for subscribing to custom events and publishing them.

**Install**
```javascript
npm install pubsubmanager
```

This library is basically used in scenarios where custom and mutiple eventing is needed.

It follows a publisher-subscribers model. An event if fired, multiple subscribers who're listening to that event receive that event.

**Basic Scenario**

For example, consider a scenario where on a particular action you need to perform multiple actions irrespective of eachother.
```javascript
var setProfile = function(sender, args) {
	//custom code
};

var setUserActive = function(sender, args) {
	//Custom code	
};

PubSubManager.subscribe('user.login', setProfile);
PubSubManager.subscribe('user.login', setUserActive);

PubSubManager.fire('user.login', this, {id: 'XCVDF1DD233', name : 'John'});
```

**Publish**

To publish an event you need to provide three arguments viz. `eventname`, `sender` and optional `arguments` 
```javascript
PubSubManager.fire(
	"user.login", // event-name
	this, // a refernece to sender, this can be string or an object
	{ name:'John' } // an object of arguments 
)
```

**Subscribe**

To subscribe an event you need to provide three arguments viz. `eventname` and `callback` 
```javascript
var subId = PubSubManager.subscibe(
	"user.login", // event-name
	callback 	  //callback
)
```

**Unsubscribe**

Whenever you subscribe to an event , you're returned with an subscriptionId. You can save this for unsubscribing to the event in future
```javascript
PubSubManager.unsubscribe(subId);
```

**ClearAndSubscribe**

This is an enhaced version of subscribe, which makes sure that before you subscribe to that event, all other subscriptions for that event are cleared.
```javascript
PubSubManager.clearAndSubscribe("eventName", callback);
```
**ClearSubscriptions**

This method allows you to unsubscribe all other subscriptions for this event.
```javascript
PubSubManager.clearSubscriptions("eventName");
```
**Other Scenarios**

For example, consider you're writing a multipage app, and intend to reuse components/javascript files in these pages.
There're cases where you may include not all but only some of these files. Thus, unintentionally giving way for exceptions, for undefined functions, which're defined in other script which was not included. This is the scenario where PubSubManager has its real purpose.

a.js
```javascript
var showMessage = function(sender, args) {
	//Your code
};

//Subscribe to message show event
PubSubManager.subscribe('message.show', showMessage);
```

b.js
```javascript
var hideMessage = function(sender, args) {
	//Your code
};

//Subscribe to message hide event
PubSubManager.subscribe('message.hide', hideMessage);
```

c.js
```javascript
//Fire message show event
PubSubManager.fire('message.show', this, { msg: 'This is a message' });

//Fire message hide event
PubSubManager.fire('message.hide', this, {});

```
In above example, consider if you include a.js and c.js, it won't throw an exception, as there is no subscriber for `message.hide` function.

**License**

MIT License
