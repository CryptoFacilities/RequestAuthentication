Crypto Facilities REST API v3
==================================
This is a help readme for [Crypto Facilities Ltd](https://www.cryptofacilities.com/), to demonstrate
the message signing in  REST v3 API and WS v1 API requests and subscriptions.

NodeJs
------

```
var nonce = 1;

// Generate nonce
function createNonce() {
	
	var timestamp = (new Date()).getTime();
	var n = timestamp + ("0000" + nonce++).slice(-5);
	return n;
}

// Signs a message
function signMessage(endpoint, nonce, postData) {
	const utf8 = require("utf8");
	const crypto = require('crypto');
	
	// step 1: concatenate postData, nonce + endpoint   
	var message = postData + nonce + endpoint;

	// Step 2: hash the result of step 1 with SHA256
	var hash = crypto.createHash('sha256').update(utf8.encode(message)).digest();

	// step 3: base64 decode apiPrivateKey
	var secretDecoded = Buffer.from("aBLTMBs2+GK6ttSHvMhzVMHpIZDTFpOCFpHlMc8Wiiw4jU6Sc5YqMGYxw080o/RVnzDdh6jSc08Y3M9VXCPzbEhJ",'base64');

	// step 4: use result of step 3 to hash the resultof step 2 with
	var hash2 = crypto.createHmac('sha512',secretDecoded).update(hash).digest();
	
	// step 5: base64 encode the result of step 4 and return
	var signed = Buffer.from(hash2).toString('base64')

	return signed;
}
```

