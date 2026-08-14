## Logs 
- Discrete records of revents 
1. Capture the raw details of descrete events 
2. More expensive to store and index than metrics 
3. Human-readable cs machine-readable logs 
4. Multiple levels: `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL`

### Log levels 
In production typically we normally enable `INFO` and higher level
- DEBUG: Detailed information for debugging. "Called function X with parameters Y."
- INFO: Normal operations. "User logged in." "Order processed." Helpful for understanding application flow.
- WARN: Something unexpected but not an error. "Rate limit approaching." "Cache miss." Potential issues to investigate.
- ERROR: Something failed. "Payment gateway timeout." "Database connection refused." Requires attention.
- FATAL: Critical failure, application can't continue. "Out of memory." "Config file missing."