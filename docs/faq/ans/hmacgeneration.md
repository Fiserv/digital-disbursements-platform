## What is the process for generating and agreeing about the secret key for the HMAC to secure the notifications?

Below are the way which we are using to generate the signature, you may also use the same.
var key = postman.getEnvironmentVariable('clientId');
var secret = postman.getEnvironmentVariable('clientSecret');
 
var time = new Date().getTime();
var method = request.method;
 
var rawSignature = key + ":" + time;
var requestBody = request.data;
 
if (method != 'GET' && method != 'DELETE') {
    var payload_digest = CryptoJS.SHA256(requestBody);
    var b64BodyContent = CryptoJS.enc.Base64.stringify(payload_digest);
    rawSignature = rawSignature + ":" + b64BodyContent;
}
 
var signature = CryptoJS.HmacSHA256(rawSignature, secret);
postman.setEnvironmentVariable('time', time);
postman.setEnvironmentVariable('signature', CryptoJS.enc.Base64.stringify(signature));
 
