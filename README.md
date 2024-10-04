# Advanced URL Shortener with Analytics

This project is a backend API for a URL shortener service, enhanced with advanced analytics functionalities. It allows users to shorten URLs, track visits, and gather comprehensive analytics such as total visits, unique visitors, visits by device type, and more. It is built using Node.js and MongoDB for optimal performance and scalability.


## Features

- URL Shortening: Generate short URLs with an optional custom code.
- Redirection with Tracking: Track every visit, capturing details like IP, user agent, device type, and timestamp.
- Analytics: Retrieve detailed analytics for each shortened URL, including:
    - Total visits and unique visitors.
    - Visits breakdown by device type (mobile, desktop, etc.).
    - Time series data of visits (hourly/daily counts).
- Custom Short Codes: Support for user-defined custom short codes.
- Expiration: Optionally set expiration dates for shortened URLs.
- API Rate Limiting: Limits on API requests to prevent abuse.
- Background Jobs: Asynchronous processing of analytics data using task queues.

## Project Structure


```bash
  shorturl/
│
├── controllers/         # API Controllers
├── models/              # MongoDB Schema models
├── routes/              # API routes
├── services/            # Core business logic for shortening and analytics
├── config/              # Configuration files for the app
├── tests/               # Unit and integration tests
├── utils/               # Helper functions
│
└── README.md            # Project documentation
```


## API Reference

#### Shorten URL
- Endpoint: POST /shorten
- Request:

```JSON
  {
  "originalUrl": "https://example.com",
  "customCode": "myCustomCode"  // optional
  }
```
- Response:

```JSON
  {
  "shortUrl": "https://short.ly/myCustomCode",
  "expiresAt": "optional"
  }
```

### Redirect and Track Visit
- Endpoint: GET /:shortCode
- Response: 302 Found, redirects to the original URL.
- Tracking: Captures visit details such as timestamp, IP address, user agent, and device type.

### Get URL Analytics
- Endpoint: GET /analytics/:shortCode
- Response:
```JSON
{
  "originalUrl": "https://example.com",
  "totalVisits": 123,
  "uniqueVisitors": 100,
  "visitsByDevice": {
    "desktop": 70,
    "mobile": 50,
    "tablet": 3
  },
  "topReferrers": ["https://google.com", "https://facebook.com"],
  "timeSeriesData": {
    "2024-10-04": 30,
    "2024-10-05": 20
  }
}
```

### Custom Short Code Validation
- Endpoint: POST /validate-code
- Request:
```JSON
{
  "customCode": "myCustomCode"
}

```
- Response:
```JSON

{
  "isAvailable": true
}
```

## Database Schema
The database uses MongoDB with an optimized schema for tracking analytics efficiently

- URL Schema:
```JSON
{
  "originalUrl": "https://example.com",
  "shortCode": "abc123",
  "createdAt": "2024-10-04T12:34:56Z",
  "expiresAt": "optional",
  "visits": [
    {
      "timestamp": "2024-10-04T12:34:56Z",
      "ipAddress": "192.168.1.1",
      "userAgent": "Mozilla/5.0",
      "deviceType": "desktop"
    }
  ],
  "visitCount": {
    "total": 123,
    "unique": 100,
    "desktop": 70,
    "mobile": 50
  }
}
```


## Installation

- Clone the repository:

```bash
  git clone https://github.com/Ansh-20/shorturl.git
  cd shorturl
```
- Install dependencies:
```bash
npm install
```
- Set up environment variables in index.js file:
```bash
PORT=8001
MONGO_URI=mongodb://localhost:27017/shorturl
```
- Start the server:
```bash
npm start
```

## Testing
- Run unit and integration tests using:
```bash
npm test
```
## Security Measures
- Rate Limiting: Prevents excessive requests using IP-based rate limiting.
- Data Sanitization: Sanitizes incoming requests to avoid malicious inputs.
- Helmet: Provides basic protection by setting HTTP headers appropriately.

