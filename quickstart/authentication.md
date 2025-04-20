---
icon: globe-pointer
---

# Authentication

## BiuLinkPay Signature Verification Technical Specification

#### Signature Generation Mechanism

The system employs HMAC-SHA512 algorithm to generate request signatures, ensuring the integrity and authenticity of transaction requests. The signature process strictly adheres to the following technical specifications:

**Core Parameters**

* **Encryption Algorithm**: HMAC-SHA512
* **Character Encoding**: UTF-8
* **Output Format**: Uppercase hexadecimal string

**Signature Generation Process**

1. **Data Preparation Phase**:
   * Convert the content to be signed into UTF-8 encoded byte array
   * Obtain merchant security key (secretKey)
2. **Encryption Processing Phase**:
   * Initialize HMAC-SHA512 encryption instance
   * Generate encryption key using security key
   * Perform encryption operation on raw data
3. **Format Conversion Phase**:
   * Convert encrypted result byte array to hexadecimal string
   * Output is uniformly converted to uppercase

#### Key Technical Implementation Points

1. **Encoding Standards**:
   * Strictly use UTF-8 character encoding for string processing
   * Apply 0xFF mask during byte conversion to ensure unsigned conversion
2. **Security Features**:
   * Adopts 512-bit strong hash algorithm
   * Separate processing of keys and data
   * Tamper-proof design: Any parameter changes will invalidate the signature
3. **Performance Optimization**:
   * Uses static character array to improve hexadecimal conversion efficiency
   * Predefined encryption algorithm instances avoid repeated initialization

#### Important Notes

1. **Key Management**:
   * Security keys must remain strictly confidential
   * Regular key rotation recommended
   * Hardcoding keys in client-side code is prohibited
2. **Signature Verification**:
   * Server must use same algorithm and parameters to verify signatures
   * Recommended timestamp validity window: ±5 minutes
   * Random strings (nonce) must ensure uniqueness
3. **Exception Handling**:
   * Must handle invalid encoding exceptions
   * Clear exceptions should be thrown when encryption algorithm is unavailable
   * Null inputs should be properly intercepted

#### Sign Example For Java

```
public static void main(String[] args) throws Exception {
        String nonce = UUID.randomUUID().toString().replace("-", "");

        Map<String, Object> jsonMap = new HashMap<>();
        jsonMap.put("tradeNo", "7788991111711116");

        Gson gson = new Gson();

        HttpUtil httpUtil = new HttpUtil();

        String currentTime = String.valueOf(System.currentTimeMillis());

        String sendJson = gson.toJson(jsonMap);
        String payload = currentTime + "\n" + nonce + "\n" + sendJson + "\n";

        Map<String, String> headers = new HashMap<String, String>();

        headers.put("BiuLinkPay-AppId", "FEA21349995B45929AA26EC1AB6C1DA0");
        headers.put("BiuLinkPay-TimeStamp", currentTime);
        headers.put("BiuLinkPay-Nonce", nonce);
        headers.put("BiuLinkPay-Signature", signature("Eh1+2XmwSWZ8HJ8a98okMIux6GqWHZRPhe1vAwb8ziI=", payload));

        String result = httpUtil.post("https://api.biulink.online/api/pay/merchant/paymentQuery", sendJson, headers).getBody();
        System.out.println(result);
}
    
private static String signature(String secretKey, String payload) throws Exception {
    byte[] bytes = payload.getBytes(StandardCharsets.UTF_8);
    byte[] hmacBytes = HmacSha512(bytes, secretKey);
    return byteArrayToHexStr(hmacBytes).toUpperCase();
}

private static byte[] HmacSha512(byte[] binaryData, String key) throws Exception {
    Mac mac = Mac.getInstance("HmacSHA512");
    SecretKeySpec secretKey = new SecretKeySpec(key.getBytes(), "HmacSHA512");
    mac.init(secretKey);
    byte[] HmacSha1Digest = mac.doFinal(binaryData);
    return HmacSha1Digest;
}

private static String byteArrayToHexStr(byte[] byteArray) {
    if (byteArray == null) {
        return null;
    }
    char[] hexArray = "0123456789ABCDEF".toCharArray();
    char[] hexChars = new char[byteArray.length * 2];
    for (int j = 0; j < byteArray.length; j++) {
        int v = byteArray[j] & 0xFF;
        hexChars[j * 2] = hexArray[v >>> 4];
        hexChars[j * 2 + 1] = hexArray[v & 0x0F];
    }
    return new String(hexChars);
}
```
