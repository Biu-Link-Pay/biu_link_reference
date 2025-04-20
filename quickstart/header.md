---
icon: bullseye-arrow
---

# Header

## BiuLinkPay API Request Header Specification Document

#### 1. Document Overview

This document details the mandatory security verification header fields required for BiuLinkPay payment interface requests, including definitions, format requirements, and usage specifications for each field.

#### 2. Header Field List

All requests must include the following HTTP header fields:

| Header Field Name    | Required | Description            |
| -------------------- | -------- | ---------------------- |
| BiuLinkPay-AppId     | Yes      | Application Identifier |
| BiuLinkPay-TimeStamp | Yes      | Request Timestamp      |
| BiuLinkPay-Nonce     | Yes      | Random String          |
| BiuLinkPay-Signature | Yes      | Request Signature      |



