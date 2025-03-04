# Assignment-1.Real-time Application with REST, GraphQL, and WebSockets
**By Satyam Panseriya & Keval Trivedi**

**Use Case: Real-time Stock Market Monitoring App**

---

### **Section 1: REST and GraphQL for Data Requests and Updates**

#### **REST Implementation**
By specifying organized endpoints for various activities, REST APIs are frequently used to manage resources in web applications.  The following endpoints can be used to construct RESTful APIs in a real-time stock market monitoring app:

- `GET /stocks` - Retrieves a list of all stocks along with their latest prices. This allows users to access up-to-date market data.
- `GET /stocks/{symbol}` - Fetches detailed information about a specific stock, including price, volume, and company details.
- `POST /stocks` - Adds a new stock to the monitoring system, ensuring that newly listed stocks are available for tracking.
- `PUT /stocks/{symbol}` - Updates stock details such as the company name or industry classification, keeping the database accurate.
- `DELETE /stocks/{symbol}` - Removes a stock from the monitoring system when it is no longer relevant.

REST has constraints when processing real-time data, despite being simple to grasp and implement.  Regular updates are wasteful since every request creates a new connection to the server.  WebSockets or polling techniques would be needed for real-time functionality in order to get around this.

#### **GraphQL Implementation**
GraphQL provides a flexible query language to fetch and manipulate data in a single request. For this use case, the following GraphQL operations could be used:

- **Query:**
  ```graphql
  query {
    stocks {
      symbol
      price
      companyName
    }
  }
  ```
- **Mutation:**
  ```graphql
  mutation {
    updateStock(symbol: "AAPL", price: 180.25) {
      symbol
      price
    }
  }
  ```
GraphQL allows clients to request only the data they need, reducing unnecessary payloads and improving efficiency compared to REST.

#### **Comparison: REST vs. GraphQL**
| Feature | REST | GraphQL |
|---------|------|---------|
| Data Fetching | Fixed endpoints with predefined responses | Custom queries fetching only required data |
| Real-time Capabilities | Requires polling or WebSockets | Supports subscriptions for real-time updates |
| Performance | Over-fetching and multiple requests | Optimized queries, reducing data transfer |
| Complexity | Simpler to implement | More flexible but requires schema management |

---

### **Section 2: WebSockets for Real-time Communication**

WebSockets are perfect for real-time stock price updates because they allow full-duplex communication between clients and the server.  WebSockets keep the client-server connection active, enabling immediate bidirectional data transfer, in contrast to REST and GraphQL, which depend on request-response cycles.

- Maintain a persistent connection to push real-time stock price changes to clients.
- Reduce latency by eliminating the need for continuous polling.
- Handle multiple client connections efficiently.

**How WebSockets Differ from REST and GraphQL:**
- **REST:** Requires clients to continuously poll for updates, increasing server load and response delays.
- **GraphQL:** Supports real-time updates via subscriptions, but still requires a WebSocket-based transport layer.
- **WebSockets:** Pushes updates instantly to connected clients, making them optimal for high-frequency updates like stock price changes.

Example WebSocket implementation for real-time updates:
```javascript
const socket = new WebSocket("wss://example.com/stocks");

socket.onmessage = (event) => {
    const stockUpdate = JSON.parse(event.data);
    console.log(`New price for ${stockUpdate.symbol}: $${stockUpdate.price}`);
};
```

---

### **Section 3: Technology Recommendation and Justification**

#### **Recommended Approach: Hybrid (GraphQL + WebSockets)**
A combination of WebSockets and GraphQL offers the best real-time capabilities, flexibility, and efficiency for a stock market monitoring app.

1. **GraphQL for Data Fetching and Updates**
   - Enhanced Retrieval of Data GraphQL enables customers to query only the required stock data, in contrast to REST, which provides clients with preset replies from specified endpoints.  By doing this, 
     bandwidth consumption is reduced and unnecessary information is not overfetched.

   - Effective Updates Using Mutations in GraphQL Structured data updates are made possible by GraphQL mutations, which enable accurate reflection of changes in metadata or stock prices.  This facilitates 
     effective stock data management without making needless API requests.


2. **WebSockets for Real-time Price Updates**
   - Push-based, low-latency communication When stock values fluctuate, the server can rapidly push updates thanks to WebSockets' persistent connection.  For financial applications, where even a small delay 
     might affect decision-making, this is essential.

   - Decreased Server Traffic  In contrast to REST Polling, REST-based polling necessitates constant client requests to monitor price changes, which increases server load and network traffic.  By keeping a 
     single open connection and only providing updates when required, WebSockets remove this problem.


#### **Justification**
| Factor | REST | GraphQL | WebSockets |
|--------|------|---------|------------|
| Real-time Support | Requires polling | Supports subscriptions | Best for real-time updates |
| Efficiency | Over-fetching | Optimized queries | Persistent connection reduces requests |
| Scalability | High with caching | Moderate with optimized queries | Requires handling concurrent connections |
| Ease of Use | Simple but limited | Flexible with schema | Requires WebSocket server |


---

**Conclusion:**
The best method for a real-time stock market monitoring app is a hybrid one that uses WebSockets for real-time updates and GraphQL for data management.  This combination streamlines data fetching, reduces latency, and scales effectively to accommodate numerous concurrent users.


