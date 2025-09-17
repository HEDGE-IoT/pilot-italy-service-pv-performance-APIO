## Services Functional Specifications

---

### General Information and Purpose
- **Service Name/Title**: Performances Service  
- **Description and Purpose**: Compute PV plant performance KPIs  
- **Owner/Contact Information**: Mattia Alfieri (Apio) <m.alfieri@apio.cc>  

---

### Functional Requirements
1. Real-Time KPIs computation: The system should be able to compute the Performance KPIs in real-time  
2. Historical Data processing: The system should be able to pre-process, normalize and filter data  
3. User Authentication and Authorization: The system should be able to protect resources and functionalities with authorization and authentication  
4. Audit Logging: The system should be able to track configuration updates  

---

### Non-Functional Requirements
- **Performance**:  
  - Realtime KPI computations should be quick  
- **Reliability and Availability**:  
  - The system should provide high availability (targeting 99%) and failover mechanisms  
- **Security (authentication, authorisation, data encryption, data privacy)**:  
  - Authentication: Use Token based auth for both interactive and non-interactive auth methods  
  - Authorization: Use RBAC  
  - Encryption: both data at rest and in motion should be encrypted  
- **Privacy**: The system should be compliant with data privacy regulations  

---

### Service Interfaces
Resources are modelled after a REST paradigm. Below are the main endpoints.  
(A complete OpenAPI is available with parameters, status codes, payloads, and responses.)  

| Method | Path | Description |
|--------|------|-------------|
| GET    | `/projects/{projectId}/plants/{uuid}/production` | Obtain Plant performance KPIs in a specific time range delimited by timeFrom and timeTo params |
| GET    | `/projects/{projectId}/plants/{uuid}/analytics` | Obtain inverter string analysis and linear regressions on production |

---

### Data Model
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Plant Production Performance Response",
  "type": "object",
  "properties": {
    "peakPower": { "type": "number" },
    "totalProducedEnergy": { "type": "number" },
    "totalExportedEnergy": { "type": "number" },
    "totalExpectedEnergy": { "type": "number" },
    "expectedEnergy": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "label": { "type": "string" },
          "value": { "type": "number" }
        },
        "required": ["label", "value"]
      }
    },
    "producedEnergy": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "label": { "type": "string" },
          "value": { "type": "number" }
        },
        "required": ["label", "value"]
      }
    },
    "exportedEnergy": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "label": { "type": "string" },
          "value": { "type": "number" }
        },
        "required": ["label", "value"]
      }
    },
    "irradiation": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "label": { "type": "string" },
          "value": { "type": "number" }
        },
        "required": ["label", "value"]
      }
    },
    "baselinePr": { "type": "number" },
    "realPr": { "type": "number" },
    "baselineProduction": { "type": "number" },
    "realProduction": { "type": "number" },
    "messages": { "type": "array", "items": {} },
    "power": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "label": { "type": "string" },
          "value": { "type": "number" }
        },
        "required": ["label", "value"]
      }
    },
    "punctualIrradiation": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "label": { "type": "string" },
          "value": { "type": "number" }
        },
        "required": ["label", "value"]
      }
    },
    "totalRadiation": { "type": "number" }
  },
  "required": [
    "peakPower",
    "totalProducedEnergy",
    "totalExportedEnergy",
    "totalExpectedEnergy",
    "expectedEnergy",
    "producedEnergy",
    "exportedEnergy",
    "irradiation",
    "baselinePr",
    "realPr",
    "baselineProduction",
    "realProduction",
    "messages",
    "power",
    "punctualIrradiation",
    "totalRadiation"
  ]
}
```

---

### Integration and Dependencies
- **External dependencies**:  
- **System dependencies**:  
  - IoT Platform: Dependent on Device Data and Device Catalogue  
  - Databases, libraries, frameworks and other internal tools  
- **Third-party integrations**:  
  - Solar irradiation API (for plants without irradiation devices)  
- **HEDGE-IOT**: Edge Devices/interfaces and cloud services integration  
- **Integration with the App Store**: API-related integration constraints  
- **Integration with the data space**: TBD  

---

### Security and Privacy
- **Data Sensitivity**: TBD  
- **Access Control**: Usage of the APIs or of the Frontend requires authentication. Authenticated users may have access to a subset of features depending on the role and permissions.  
- **Audit Logs**: User actions that modify the system trigger audit log collection.  

---

### Performance Considerations
- **Stress testing**: Stress tests are performed periodically  
- **Response times**: Response times are monitored, see 1.9  

---

### Deployment and Environment
- **Configuration**: Environment-specific settings  
- **CI/CD**: Deployed through a Continuous Integration / Continuous Deployment pipeline with stages:  
  - **Test Stage**: Audit (vulnerability check), Lint, Test suite  
  - **Build Stage**: Build container image, publish to registry  
- **Monitoring and Logging**:  
  - Instrumented with Prometheus Metrics (resource consumption, application-specific metrics)  
  - JSON logger  
  - Open-telemetry compatible traces  

---

### Testing and Validation
- All commits are checked against a test suite  
