# **DNS Resolver - Iterative and Recursive Lookup**

## Group Members

- **Suvrat Pal (211089)**
- **Devesh Pandita (210330)**
- **Om Kothawade (210682)**

---

## **1. Introduction**
This script implements a **DNS resolver** that can perform **iterative** and **recursive** lookups to resolve domain names to IP addresses.  

- **Iterative Mode:** Starts at the root servers and queries down the hierarchy (TLD, authoritative) until an answer is found.  
- **Recursive Mode:** Uses the system’s default DNS resolver (e.g., Google DNS, ISP resolver) to fetch the result.  

This script is written in Python using the **dnspython** library.  

---

## **2. How to Run the Code**
### **Prerequisites**
Ensure Python is installed (Python 3 recommended). Install the required library:  
```bash
pip install dnspython
```

### **Running the Script**
The script accepts two command-line arguments:  
1. **Mode:** `iterative` or `recursive`  
2. **Domain name** to resolve  

#### **Example Usage**
- **Iterative Lookup:**  
  ```bash
  python3 dns_resolver.py iterative example.com
  ```
- **Recursive Lookup:**  
  ```bash
  python3 dns_resolver.py recursive example.com
  ```

---

## **3. How the Code Works**
### **Iterative DNS Resolution**
1. Starts with a **list of root servers**.  
2. Sends a **DNS query** to a root server.  
3. If no direct answer is received, extracts **NS (nameserver) records** from the response.  
4. Resolves those NS records to IP addresses.  
5. Queries the resolved NS servers (TLD, authoritative) until it finds an **A record (IP address)**.  

#### **Key Functions**
- `send_dns_query(server, domain)`: Sends a query to the specified DNS server.  
- `extract_next_nameservers(response)`: Extracts and resolves NS records to find the next servers.  
- `iterative_dns_lookup(domain)`: Implements the full iterative process.  

---

### **Recursive DNS Resolution**
1. Uses `dns.resolver.resolve()` to query the system’s **default recursive resolver**.  
2. Directly retrieves the **A record** for the given domain.  
3. Prints the resolved IP address.  

#### **Key Function**
- `recursive_dns_lookup(domain)`: Uses the system's recursive resolver to fetch the IP address.  

---

## **4. Error Handling**
The script includes **multiple levels of error handling** to ensure robustness:  

| **Error Type**          | **Handling Mechanism** |
|-------------------------|-----------------------|
| Timeout or Unreachable Server | Moves to the next available nameserver. |
| Invalid Domain Name | Prints an error and exits. |
| No Response from DNS Server | Stops resolution and displays an error. |
| Recursive Lookup Failure | Catches the exception and prints an error message. |

---

## **5. Example Outputs**
### **Iterative Lookup (`google.com`)**
```plaintext
[Iterative DNS Lookup] Resolving google.com
[DEBUG] Querying ROOT server (198.41.0.4) - SUCCESS
Extracted NS hostname: l.gtld-servers.net.
Resolved l.gtld-servers.net. to 192.41.162.30
[DEBUG] Querying TLD server (192.41.162.30) - SUCCESS
Extracted NS hostname: ns1.google.com.
Resolved ns1.google.com. to 216.239.32.10
[DEBUG] Querying AUTH server (216.239.32.10) - SUCCESS
[SUCCESS] google.com -> 142.250.194.78
Time taken: 0.597 seconds
```

### **Recursive Lookup (`google.com`)**
```plaintext
[Recursive DNS Lookup] Resolving google.com
[SUCCESS] google.com -> ns4.google.com.
[SUCCESS] google.com -> 172.217.167.206
Time taken: 0.014 seconds
```
