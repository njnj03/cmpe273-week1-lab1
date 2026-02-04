# Go HTTP Track

## How to Run Locally

### Step 1: Initialize Go Modules

**Service A:**
```bash
cd service-a
go mod init service-a
```

**Service B:**
```bash
cd service-b
go mod init service-b
```

### Step 2: Run Service A (Terminal 1)
```bash
cd service-a
go run .
```
Expected output: `service=A listening on :8080`

### Step 3: Run Service B (Terminal 2 - new terminal)
```bash
cd service-b
go run .
```
Expected output: `service=B listening on :8081`

## Test Results

### Success Case (Both Services Running)

**Command:**
```bash
curl "http://127.0.0.1:8081/call-echo?msg=hello"
```

**Output:**
```json
{"service_a":{"echo":"hello"},"service_b":"ok"}
```

**Service Logs:**
- Service A: `service=A endpoint=/echo status=ok latency_ms=X`
- Service B: `service=B endpoint=/call-echo status=ok latency_ms=X`

### Failure Case (Service A Stopped)

**Command:**
```bash
curl "http://127.0.0.1:8081/call-echo?msg=hello"
```

**Output:**
```json
{"error":"Get \"http://127.0.0.1:8080/echo?msg=hello\": dial tcp 127.0.0.1:8080: connect: connection refused","service_a":"unavailable","service_b":"ok"}
```

**Service B Error Log:**
```
service=B endpoint=/call-echo status=error error="Get \"http://127.0.0.1:8080/echo?msg=hello\": dial tcp 127.0.0.1:8080: connect: connection refused" latency_ms=X
```

**HTTP Status:** 503 Service Unavailable

## Summary

✅ Two independent services communicating over HTTP  
✅ Service A (Echo API) on port 8080  
✅ Service B (Client) on port 8081  
✅ Request logging with service name, endpoint, status, and latency  
✅ 1-second timeout configured in Service B  
✅ Proper error handling with 503 status when Service A is unavailable
