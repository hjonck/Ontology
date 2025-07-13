# **Telnyx SIP Integration for South African AI Voice Agents**

## **Overview**

Telnyx is a cloud communications platform that provides global SIP trunking services on a private Tier-1 network, offering an excellent alternative to Twilio for AI voice agent deployments in South Africa. Unlike traditional carriers, Telnyx specializes in programmable communications with robust APIs, competitive pricing, and global coverage including South African market support.

### **Why Telnyx for South African AI Voice Agents?**

- **Private Tier-1 Network**: Low-latency, jitter-free calling with no packet loss
- **Competitive Pricing**: Up to 70% cost savings compared to Twilio
- **South African Coverage**: Local DID numbers and optimized routing
- **Developer-First**: Comprehensive APIs and documentation
- **AI Voice Platform Integration**: Native support for VAPI and other voice AI platforms

---

## **Telnyx Platform Architecture**

### **Core Components**

1. **SIP Connections**: Configure inbound traffic and authentication
2. **Outbound Voice Profiles**: Manage outbound call routing and caller ID
3. **Phone Numbers**: Local, national, mobile, and toll-free South African DIDs
4. **Voice API**: Programmable voice control with Call Control API
5. **Elastic SIP Trunking**: Instantly scalable voice connections

### **Key Features**

- **Real-time Voice Control**: WebSocket-based call control
- **Global Routing**: Optimized call paths with regional presence
- **Advanced Authentication**: Multiple security methods for SIP trunks
- **Number Porting**: Migrate existing South African numbers
- **Analytics & Monitoring**: Real-time call metrics and reporting

---

## **South African Market Support**

### **Available Number Types**

| **Number Type** | **Format** | **Use Case** |
|---|---|---|
| **Local Numbers** | +27-XX-XXX-XXXX | Geographic presence in specific cities |
| **National Numbers** | +27-87-XXX-XXXX | Non-geographic business presence |
| **Mobile Numbers** | +27-8X-XXX-XXXX | Mobile-specific routing |
| **Toll-Free** | +27-800-XXX-XXX | Free calling for customers |

### **DID Requirements for South Africa**

**Personal Identity Verification:**
- Full name and contact phone number
- Passport or South African ID copy

**Business Identity Verification:**
- Name of authorized representative
- Company name and registration certificate
- Representative's passport/ID

**Address Verification:**
- Address matching the number's area code
- Proof of address (within 3 months)

**Processing Time:** ~72 hours for validation and activation

### **Compliance & Regulatory**

- **ICASA Compliance**: Meets South African telecommunications regulations
- **Number Porting**: Support for existing South African number migration
- **Local Presence**: Enhanced answer rates with local caller ID

---

## **SIP Trunk Configuration**

### **Basic SIP Connection Setup**

```bash
# Create SIP Connection via API
curl -X POST https://api.telnyx.com/v2/sip_connections \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "connection_name": "SA-AI-Voice-Trunk",
    "transport_protocol": "UDP",
    "default_on_hold_comfort_noise_enabled": false,
    "dtmf_type": "RFC 2833",
    "encode_contact_header_enabled": false,
    "encrypted_media": "SRTP",
    "on_hold_enabled": true,
    "rtp_timeout_secs": 3600,
    "t38_passthrough_enabled": false,
    "webhook_event_url": "https://your-webhook-endpoint.com/telnyx",
    "webhook_event_failover_url": "https://your-backup-webhook.com/telnyx",
    "webhook_timeout_secs": 25,
    "rtcp_settings": {
      "port": "rtp+1"
    }
  }'
```

### **Authentication Methods**

**1. IP Authentication** (Recommended for AI platforms)
```json
{
  "sip_uri_calling_preference": "enabled",
  "tech_prefix_enabled": false,
  "generated_sip_username": "auto-generated",
  "inbound": {
    "ani_number_format": "E164",
    "dnis_number_format": "E164",
    "codecs": ["G722", "PCMU", "PCMA"]
  },
  "outbound": {
    "ani_override": "+27871234567",
    "ani_override_type": "always",
    "call_parking_enabled": false,
    "channel_limit": 100,
    "generate_ringback_tone": true,
    "localization": "ZA"
  }
}
```

**2. Credential Authentication** (Higher security)
```json
{
  "user_name": "your_sip_username",
  "password": "secure_sip_password",
  "sip_uri_calling_preference": "enabled"
}
```

### **Outbound Voice Profile**

```bash
# Create Outbound Profile for South African calls
curl -X POST https://api.telnyx.com/v2/outbound_voice_profiles \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "SA-Voice-Agent-Profile",
    "traffic_type": "conversational",
    "service_plan": "global",
    "concurrent_call_limit": 50,
    "enabled": true,
    "tags": ["ai-voice", "south-africa"],
    "usage_payment_method": "rate-deck",
    "whitelisted_destinations": ["ZA"],
    "max_destination_rate": 0.10,
    "daily_spend_limit": "100.00",
    "daily_spend_limit_enabled": true
  }'
```

---

## **Integration with AI Voice Platforms**

### **VAPI Integration**

Telnyx provides native integration with VAPI for AI voice agents. VAPI requires specific IP addresses to be allowlisted for SIP traffic:

**Required IP Allowlist**:
```
44.229.228.186/32
44.238.177.138/32
172.31.9.106/32
```

**VAPI SIP Endpoint**: `sip.vapi.ai`

**Create Telnyx SIP Trunk in VAPI**:
```bash
# Add Telnyx SIP Trunk to VAPI
curl -X POST https://api.vapi.ai/credential \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
     "provider": "byo-sip-trunk",
     "name": "Telnyx-SA",
     "gateways": [{"ip": "sip.telnyx.com"}],
     "outboundAuthenticationPlan": {
        "authUsername": "YOUR_TELNYX_USERNAME",
        "authPassword": "YOUR_TELNYX_PASSWORD",
        "sipRegisterPlan": {"realm": "sip.telnyx.com"}
     }
  }'
```

**Associate South African Phone Number**:
```bash
curl -X POST https://api.vapi.ai/phone-number \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VAPI_API_KEY" \
  -d '{
    "provider": "byo-phone-number",
    "name": "Telnyx SA Number",
    "number": "+27871234567",
    "numberE164CheckEnabled": false,
    "credentialId": "YOUR_CREDENTIAL_ID"
  }'
```

> **For comprehensive VAPI setup instructions**, see our detailed [VAPI Setup and Integration Guide](VAPI%20Setup%20and%20Integration%20Guide.md).

### **Configuration for South African Voice Agents**

```json
{
  "assistantConfig": {
    "name": "SA-Voice-Agent",
    "transcription": {
      "provider": "deepgram",
      "model": "nova",
      "language": "en-ZA"
    },
    "voice": {
      "provider": "aws",
      "voice_id": "Ayanda",
      "engine": "neural"
    },
    "llm": {
      "provider": "openai",
      "model": "gpt-4o-mini",
      "stream": true
    }
  },
  "sipRouting": {
    "credentialId": "Telnyx-SA",
    "outboundNumber": "+27871234567"
  }
}
```

### **Webhook Configuration**

Set up webhooks to handle call events:

```javascript
// Node.js webhook handler example
app.post('/telnyx-webhook', (req, res) => {
  const event = req.body.data;
  
  switch(event.event_type) {
    case 'call.initiated':
      console.log('Call started:', event.payload.call_control_id);
      break;
      
    case 'call.answered':
      // Connect to VAPI or AI voice platform
      connectToVoiceAI(event.payload.call_control_id);
      break;
      
    case 'call.hangup':
      console.log('Call ended:', event.payload.hangup_cause);
      break;
  }
  
  res.status(200).send('OK');
});
```

---

## **Pricing and Cost Optimization**

### **South African Call Rates**

| **Destination** | **Rate (USD/min)** | **Description** |
|---|---|---|
| South Africa Local | $0.0180 | Geographic numbers |
| South Africa Mobile | $0.0450 | Cellular networks |
| South Africa National | $0.0270 | 087 numbers |
| South Africa Toll-Free | $0.0850 | 800 numbers |

### **Cost Comparison vs Twilio**

- **Telnyx SA Local**: ~$0.018/min
- **Twilio SA Local (default)**: ~$0.47/min
- **Twilio SA Local (ZA route)**: ~$0.027/min
- **Savings**: Up to 33% vs Twilio's best rates, 96% vs default

### **Additional Costs**

- **SIP Connection**: $0/month (free)
- **Phone Number Rental**: $1.50-$5.00/month depending on type
- **Webhook/API Calls**: Free up to reasonable limits
- **Number Porting**: One-time fee varies

---

## **Implementation Guide**

### **Step 1: Account Setup**

1. Create Telnyx account at [portal.telnyx.com](https://portal.telnyx.com)
2. Complete business verification
3. Add payment method
4. Enable Voice API access

### **Step 2: South African Phone Number**

```bash
# Search available numbers
curl -X GET "https://api.telnyx.com/v2/available_phone_numbers?filter[country_code]=ZA&filter[features][]=voice" \
  -H "Authorization: Bearer YOUR_API_KEY"

# Purchase number
curl -X POST https://api.telnyx.com/v2/number_orders \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "phone_numbers": ["+27871234567"],
    "connection_id": "your_connection_id"
  }'
```

### **Step 3: SIP Trunk Configuration**

```bash
# Configure for AI voice platform
curl -X PATCH https://api.telnyx.com/v2/sip_connections/YOUR_CONNECTION_ID \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "outbound": {
      "outbound_voice_profile_id": "your_profile_id",
      "localization": "ZA"
    },
    "inbound": {
      "codecs": ["G722", "PCMU"],
      "dtmf_type": "RFC 2833"
    }
  }'
```

### **Step 4: Test Configuration**

```bash
# Test outbound call via API
curl -X POST https://api.telnyx.com/v2/calls \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "connection_id": "YOUR_CONNECTION_ID",
    "to": "+27831234567",
    "from": "+27871234567",
    "webhook_url": "https://your-app.com/webhooks/calls"
  }'
```

---

## **Best Practices for South African Deployment**

### **Latency Optimization**

1. **Regional Routing**: Use Telnyx's South African PoPs
2. **Codec Selection**: Prefer G.722 for voice quality
3. **Direct Integration**: Connect AI platforms via SIP
4. **Webhook Proximity**: Host webhooks in African regions

### **Voice Quality Settings**

```json
{
  "audio_settings": {
    "codec_preference": ["G722", "PCMU", "PCMA"],
    "dtmf_type": "RFC 2833",
    "comfort_noise": false,
    "echo_cancellation": true
  },
  "ai_optimization": {
    "enable_barge_in": true,
    "speech_timeout": 2000,
    "silence_timeout": 3000
  }
}
```

### **Security Configuration**

- **TLS Encryption**: Enable for signaling
- **SRTP**: Use for media encryption
- **IP Whitelisting**: Restrict to known AI platform IPs
- **Webhook Validation**: Verify Telnyx signatures

---

## **Monitoring and Analytics**

### **Call Detail Records (CDRs)**

Access comprehensive call analytics via API:

```bash
curl -X GET "https://api.telnyx.com/v2/detail_records?filter[start_time][gte]=2024-01-01" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### **Real-time Monitoring**

- **Call Quality Metrics**: Jitter, latency, packet loss
- **Success Rates**: Connection success by destination
- **Cost Tracking**: Real-time spend monitoring
- **Performance Analytics**: Response times and call duration

---

## **Related Documentation**

- **[VAPI Setup and Integration Guide](VAPI%20Setup%20and%20Integration%20Guide.md)** - Complete VAPI configuration, API usage, and SIP trunk integration
- **[Research VAPI and Twilio](Research%20VAPI%20and%20Twilio.md)** - Platform comparison, latency analysis, and South African dialect support
- **[Setup Twilio And VAPI with Trunk](Setup%20Twilio%20And%20VAPI%20with%20Trunk.md)** - Step-by-step implementation recipes for both platforms

---

## **Conclusion**

Telnyx offers a compelling alternative to Twilio for South African AI voice agent deployments, providing:

- **Cost Efficiency**: Up to 96% savings on South African call rates
- **Superior Performance**: Private Tier-1 network with low latency
- **Local Compliance**: Full South African regulatory compliance
- **AI Platform Ready**: Native integrations with VAPI and other platforms
- **Developer Experience**: Comprehensive APIs and documentation

The platform's combination of competitive pricing, robust infrastructure, and developer-friendly approach makes it an excellent choice for businesses looking to deploy AI voice agents in the South African market while maintaining cost control and high voice quality.