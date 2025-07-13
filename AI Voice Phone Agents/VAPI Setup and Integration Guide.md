# **VAPI Setup and Integration Guide for AI Voice Agents**

## **Overview**

VAPI is a specialized platform for building real-time voice AI agents that can make and receive phone calls, integrate with web applications, and handle complex workflows. Unlike traditional telephony providers, VAPI focuses exclusively on AI voice agent infrastructure with sub-500ms latency and extensive SIP trunk compatibility.

### **Why VAPI for AI Voice Agents?**

- **Ultra-Low Latency**: Sub-500ms response times for natural conversations
- **Carrier Agnostic**: Bring Your Own SIP trunk support
- **AI-First Design**: Built specifically for voice AI applications
- **Global Compatibility**: Works with any SIP provider worldwide
- **Advanced Features**: Call transfers, real-time transcription, voice synthesis

---

## **VAPI Platform Architecture**

### **Core Components**

1. **Voice Assistants**: AI agents with configurable LLMs, voices, and transcription
2. **SIP Trunks**: BYO carrier integration via credentials
3. **Phone Numbers**: Virtual numbers linked to SIP trunks
4. **Call Control API**: Real-time call management and routing
5. **Webhooks**: Event-driven integration with external systems

### **Network Requirements**

**IP Allowlist** (Required for SIP traffic):
```
44.229.228.186/32
44.238.177.138/32
172.31.9.106/32
```

**VAPI SIP Endpoint**:
```
sip.vapi.ai
```

---

## **Account Setup and Authentication**

### **API Key Configuration**

Your VAPI API key should be stored securely in environment variables:

**Fish Shell** (`~/.config/fish/conf.d/private.fish`):
```fish
set -gx VAPI_API_KEY "d8933e55-9ef8-4bee-8377-5f7e68cf2a99"
```

**Environment Usage**:
```bash
curl -H "Authorization: Bearer $VAPI_API_KEY" https://api.vapi.ai/...
```

### **Base API Endpoint**
```
https://api.vapi.ai
```

---

## **SIP Trunk Integration**

### **Supported SIP Providers**

| **Provider** | **Authentication** | **Gateway** | **Use Case** |
|---|---|---|---|
| **Telnyx** | IP + Credentials | `sip.telnyx.com` | Global coverage, competitive pricing |
| **Twilio** | IP-based | Custom gateway ID | Managed service integration |
| **Plivo** | IP-based | Unique SIP domain | Developer-friendly APIs |
| **Zadarma** | Username/Password | `sip.zadarma.com` | European market focus |
| **Custom BYO** | Provider-dependent | Any SIP server | Maximum flexibility |

### **Create SIP Trunk Credential**

**Telnyx Integration** (Recommended for South Africa):
```bash
curl -X POST https://api.vapi.ai/credential \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "provider": "byo-sip-trunk",
    "name": "Telnyx SA Trunk",
    "gateways": [{"ip": "sip.telnyx.com"}],
    "outboundAuthenticationPlan": {
      "authUsername": "YOUR_TELNYX_USERNAME",
      "authPassword": "YOUR_TELNYX_PASSWORD",
      "sipRegisterPlan": {"realm": "sip.telnyx.com"}
    }
  }'
```

**Generic SIP Provider**:
```bash
curl -X POST https://api.vapi.ai/credential \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "provider": "byo-sip-trunk",
    "name": "Custom SIP Trunk",
    "gateways": [{"ip": "YOUR_SIP_GATEWAY"}],
    "outboundLeadingPlusEnabled": true,
    "outboundAuthenticationPlan": {
      "authUsername": "YOUR_SIP_USERNAME",
      "authPassword": "YOUR_SIP_PASSWORD"
    }
  }'
```

**Response includes credential ID**:
```json
{
  "id": "credential_id_for_next_steps",
  "provider": "byo-sip-trunk",
  "name": "Telnyx SA Trunk",
  "gateways": [{"ip": "sip.telnyx.com"}]
}
```

---

## **Phone Number Management**

### **Associate Phone Number with SIP Trunk**

```bash
curl -X POST https://api.vapi.ai/phone-number \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "provider": "byo-phone-number",
    "name": "SA Voice Agent Number",
    "number": "+27871234567",
    "numberE164CheckEnabled": false,
    "credentialId": "YOUR_CREDENTIAL_ID"
  }'
```

### **Create VAPI SIP Phone Number**

For internal SIP routing:
```bash
curl -X POST https://api.vapi.ai/phone-number \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "provider": "vapi",
    "sipUri": "sip:agent001@sip.vapi.ai",
    "assistantId": "YOUR_ASSISTANT_ID",
    "name": "Internal SIP Agent"
  }'
```

### **Phone Number with Authentication**

For secure SIP endpoints:
```bash
curl -X POST https://api.vapi.ai/phone-number \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "provider": "vapi",
    "sipUri": "sip:secure_agent@sip.vapi.ai",
    "assistantId": "YOUR_ASSISTANT_ID",
    "authentication": {
      "realm": "sip.vapi.ai",
      "username": "secure_user",
      "password": "secure_password"
    }
  }'
```

---

## **Voice Assistant Configuration**

### **Create AI Voice Assistant**

```bash
curl -X POST https://api.vapi.ai/assistant \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "name": "SA Sales Agent",
    "transcriber": {
      "provider": "deepgram",
      "model": "nova",
      "language": "en-ZA"
    },
    "model": {
      "provider": "openai",
      "model": "gpt-4o-mini",
      "stream": true
    },
    "voice": {
      "provider": "aws",
      "voiceId": "Ayanda",
      "engine": "neural"
    },
    "firstMessage": "Hello, I am calling from AgileWorks. How can I help you today?",
    "firstMessageMode": "assistant-speaks-first"
  }'
```

### **South African Voice Configuration Options**

| **Provider** | **Voice ID** | **Quality** | **Latency** |
|---|---|---|---|
| **AWS Polly** | `Ayanda` | High | Low |
| **ElevenLabs** | Custom SA voice | Ultra-high | Medium |
| **Azure** | SA English voices | High | Low |
| **Google** | `en-ZA` variants | High | Low |

---

## **Call Management API**

### **Initiate Outbound Call**

```bash
curl -X POST https://api.vapi.ai/call/phone \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "assistantId": "YOUR_ASSISTANT_ID",
    "customer": {
      "number": "+27831234567",
      "numberE164CheckEnabled": false
    },
    "phoneNumberId": "YOUR_PHONE_NUMBER_ID"
  }'
```

### **Scheduled Outbound Call**

```bash
curl -X POST https://api.vapi.ai/call/phone \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "assistantId": "YOUR_ASSISTANT_ID",
    "customer": {"number": "+27831234567"},
    "phoneNumberId": "YOUR_PHONE_NUMBER_ID",
    "schedulePlan": {
      "earliestAt": "2024-12-31T09:00:00Z"
    }
  }'
```

### **Batch Outbound Calls**

```bash
curl -X POST https://api.vapi.ai/call/phone \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "assistantId": "YOUR_ASSISTANT_ID",
    "phoneNumberId": "YOUR_PHONE_NUMBER_ID",
    "customers": [
      {"number": "+27831234567"},
      {"number": "+27832345678"},
      {"number": "+27833456789"}
    ]
  }'
```

---

## **Advanced Call Features**

### **Call Transfers to SIP**

**Transfer to SIP URI**:
```json
{
  "type": "transferCall",
  "destinations": [{
    "type": "sip",
    "sipUri": "sip:support@company.com",
    "message": "Transferring you to our support team."
  }]
}
```

**Transfer with Custom Headers**:
```json
{
  "destination": {
    "type": "sip",
    "sipUri": "sip:sales@company.com",
    "sipHeaders": {
      "X-Customer-ID": "12345",
      "X-Call-Priority": "high"
    }
  }
}
```

### **Phone Number Hooks**

**Auto-transfer on ringing**:
```bash
curl -X PATCH https://api.vapi.ai/phone-number/YOUR_PHONE_ID \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "hooks": [{
      "on": "call.ringing",
      "do": [{
        "type": "transfer",
        "destination": {
          "type": "sip",
          "sipUri": "sip:reception@company.com"
        }
      }]
    }]
  }'
```

**Custom message on ringing**:
```bash
curl -X PATCH https://api.vapi.ai/phone-number/YOUR_PHONE_ID \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "hooks": [{
      "on": "call.ringing", 
      "do": [{
        "type": "say",
        "exact": "Please hold while we connect you to an agent."
      }]
    }]
  }'
```

### **Dynamic Variables in Calls**

```bash
curl -X POST https://api.vapi.ai/call/phone \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "assistantId": "YOUR_ASSISTANT_ID",
    "assistantOverrides": {
      "variableValues": {
        "customerName": "John Smith",
        "accountNumber": "ACC123456",
        "lastOrderDate": "2024-11-15"
      }
    },
    "customer": {"number": "+27831234567"},
    "phoneNumberId": "YOUR_PHONE_NUMBER_ID"
  }'
```

---

## **Webhook Configuration**

### **Event Types**

- `call.initiated` - Call started
- `call.ringing` - Call is ringing
- `call.answered` - Call was answered
- `call.ended` - Call completed
- `call.failed` - Call failed to connect

### **Webhook Handler Example**

```javascript
app.post('/vapi-webhook', (req, res) => {
  const event = req.body.data;
  
  switch(event.event_type) {
    case 'call.initiated':
      console.log('Call started:', event.payload.call_id);
      break;
      
    case 'call.answered':
      console.log('Call answered by:', event.payload.customer.number);
      // Update CRM, start recording, etc.
      break;
      
    case 'call.ended':
      console.log('Call ended:', event.payload.endedReason);
      // Process call summary, update lead status
      break;
      
    case 'call.failed':
      console.log('Call failed:', event.payload.failureReason);
      // Retry logic, fallback actions
      break;
  }
  
  res.status(200).send('OK');
});
```

---

## **Error Handling and Debugging**

### **Common SIP Error Codes**

| **Error Code** | **Description** | **Resolution** |
|---|---|---|
| `403-forbidden` | Authentication failure | Check SIP credentials |
| `407-proxy-authentication-required` | Proxy auth needed | Configure proxy settings |
| `480-temporarily-unavailable` | Destination unreachable | Check number format |
| `503-service-unavailable` | Provider issues | Retry or use backup trunk |

### **Call Transfer Compatibility**

| **From** | **To Phone** | **To SIP** | **Supported** |
|---|---|---|---|
| Phone Call | ✅ Yes | ❌ No | PSTN limitations |
| SIP Call | ✅ Yes | ✅ Yes | Full compatibility |
| Web Call | ❌ No | ❌ No | Not supported |

### **Network Diagnostics**

**Test SIP connectivity**:
```bash
# From your SIP provider to VAPI
sip-test sip.vapi.ai 5060

# Verify firewall rules allow VAPI IPs
telnet 44.229.228.186 5060
```

---

## **Monitoring and Analytics**

### **Call Detail Records (CDRs)**

```bash
curl -X GET "https://api.vapi.ai/calls?limit=100" \
  -H "Authorization: Bearer $VAPI_API_KEY"
```

### **Real-time Call Status**

```bash
curl -X GET https://api.vapi.ai/call/CALL_ID \
  -H "Authorization: Bearer $VAPI_API_KEY"
```

### **Call Artifacts and Recordings**

VAPI provides:
- **Call recordings** (audio files)
- **Transcripts** (real-time and final)
- **PCAP files** (network packet capture for debugging)
- **Call summaries** (AI-generated)

**Enable PCAP for debugging**:
```json
{
  "pcapEnabled": true,
  "pcapS3PathPrefix": "call-debug-logs/"
}
```

---

## **Best Practices for Production**

### **Performance Optimization**

1. **Use regional SIP providers** for lowest latency
2. **Configure appropriate codecs** (G.722 recommended)
3. **Enable streaming** for all AI components
4. **Set proper timeouts** for speech recognition

### **Security Configuration**

1. **IP whitelisting** for SIP traffic
2. **Strong SIP passwords** for authenticated trunks
3. **Webhook signature validation** for events
4. **SRTP encryption** for media streams

### **Scalability Planning**

1. **Multiple SIP trunks** for redundancy
2. **Load balancing** across providers
3. **Rate limiting** for outbound calls
4. **Monitoring dashboards** for call quality

---

## **Integration Examples**

### **TypeScript SDK Usage**

```typescript
import { VapiClient } from "@vapi-ai/server-sdk";

const vapi = new VapiClient({
  token: process.env.VAPI_API_KEY!
});

// Create outbound call
const call = await vapi.calls.create({
  phoneNumberId: "phone_number_id",
  customer: { number: "+27831234567" },
  assistantId: "assistant_id"
});

console.log(`Call created: ${call.id}`);
```

### **Python SDK Usage**

```python
from vapi import Vapi

client = Vapi(token=os.environ["VAPI_API_KEY"])

# List phone numbers
phone_numbers = client.phone_numbers.list()

# Create call
call = client.calls.create(
    assistant_id="assistant_id",
    phone_number_id="phone_number_id",
    customer={"number": "+27831234567"}
)
```

---

## **Cost Optimization**

### **Trunk Selection Strategy**

1. **Primary trunk**: Lowest-cost provider (e.g., Telnyx for SA)
2. **Backup trunk**: High-reliability provider (e.g., Twilio)
3. **Routing logic**: Cost-based with quality fallback

### **Usage Monitoring**

- **Per-minute costs** vary by provider and destination
- **VAPI platform fees** (if any) are separate from trunk costs
- **Volume discounts** available with most SIP providers

---

## **Related Documentation**

- **[Telnyx SIP Integration for South Africa](Telnyx%20SIP%20Integration%20for%20South%20Africa.md)** - Detailed Telnyx setup, South African DID requirements, pricing, and compliance
- **[Research VAPI and Twilio](Research%20VAPI%20and%20Twilio.md)** - Comprehensive platform comparison, latency analysis, and South African dialect support
- **[Setup Twilio And VAPI with Trunk](Setup%20Twilio%20And%20VAPI%20with%20Trunk.md)** - Step-by-step implementation recipes for both platforms

---

## **Conclusion**

VAPI provides a powerful, AI-first platform for building voice agents with exceptional performance and flexibility. Its SIP trunk compatibility makes it ideal for cost-optimized deployments in South Africa when combined with providers like Telnyx.

**Key advantages**:
- **Sub-500ms latency** for natural conversations
- **Global SIP provider support** including Telnyx
- **Advanced AI features** with real-time streaming
- **Comprehensive APIs** for integration and automation
- **Production-ready monitoring** and error handling

The platform's carrier-agnostic approach allows you to leverage the best combination of AI performance (VAPI) and cost-effective telephony (Telnyx) for South African voice agent deployments.