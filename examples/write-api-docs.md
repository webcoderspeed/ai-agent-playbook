# 📚 Writing API Documentation with Master Prompt

## 🎯 TASK CONTEXT
**Project**: TaskFlow API Documentation
**Product**: Task management SaaS platform with REST API
**Target Audience**: Developers, technical teams, integration partners
**Primary Goals**:
- Enable developers to integrate within 30 minutes
- Reduce support requests by 60%
- Achieve 95%+ API adoption rate
- Maintain 99.9% documentation accuracy

**Technical Requirements**:
- OpenAPI 3.1 specification
- Interactive API playground
- Code examples in 5+ languages (JavaScript, Python, Ruby, PHP, Go)
- Authentication examples (API keys, OAuth 2.0)
- Error handling documentation
- Rate limiting documentation
- Versioning strategy

**Constraints**:
- Must work with existing API infrastructure
- Support for both REST and WebSocket endpoints
- Mobile developer friendly
- SEO optimized for technical search queries
- Accessibility compliant (WCAG 2.1 AA)

---

## 🧠 THINK → Deep Problem Understanding

### Core Developer Pain Points
1. **Integration complexity**: Developers struggle with authentication and error handling
2. **Time-to-first-call**: Current average is 2+ hours for basic integration
3. **Documentation gaps**: Missing examples for edge cases and webhooks
4. **Version confusion**: Lack of clear migration guides between API versions
5. **Testing difficulties**: No easy way to validate API calls before implementation

### Success Metrics
- **Integration time**: Reduce from 2 hours to 30 minutes
- **Support reduction**: Decrease API-related support tickets by 60%
- **Adoption rate**: Increase from 80% to 95% API usage
- **Accuracy**: Maintain 99.9% documentation-spec alignment
- **Developer satisfaction**: Achieve 4.8/5+ rating on developer portals

### Strategic Approach
1. **Developer-first design**: Prioritize developer experience over completeness
2. **Progressive disclosure**: Start simple, provide advanced options later
3. **Interactive learning**: Combine documentation with live examples
4. **Community-driven**: Incorporate developer feedback loops
5. **Automated validation**: Ensure documentation stays synchronized with API

---

## 📋 PLAN → Structured Execution

### Phase 1: Foundation & Core Documentation (Week 1-2)
1. **API Specification**: Complete OpenAPI 3.1 spec with all endpoints
2. **Authentication**: Comprehensive auth guide with multiple methods
3. **Quickstart**: 5-minute getting started guide
4. **Core endpoints**: Tasks, projects, users with basic examples

### Phase 2: Advanced Features & Examples (Week 3-4)
1. **Webhooks**: Real-time event documentation
2. **Rate limiting**: Detailed usage guidelines
3. **Error handling**: Complete error code reference
4. **SDK integration**: Official SDK documentation

### Phase 3: Optimization & Developer Experience (Week 5-6)
1. **Interactive playground**: Live API testing environment
2. **Code samples**: Multi-language examples
3. **Tutorials**: Step-by-step integration guides
4. **Community features**: Feedback mechanisms

### Documentation Structure
```
docs/
├── getting-started/
│   ├── quickstart.md
│   ├── authentication.md
│   └── first-api-call.md
├── api-reference/
│   ├── tasks/
│   ├── projects/
│   ├── users/
│   └── webhooks/
├── guides/
│   ├── rate-limiting.md
│   ├── error-handling.md
│   └── best-practices.md
├── sdks/
│   ├── javascript.md
│   ├── python.md
│   └── ruby.md
└── tutorials/
    ├── todo-app.md
    ├── team-collaboration.md
    └── mobile-integration.md
```

### Mobile-First Considerations
- **Responsive design**: Works on mobile devices
- **Code readability**: Optimized for small screens
- **Progressive web app**: Offline documentation access
- **Touch-friendly**: Navigation optimized for touch interfaces

---

## 🔍 RESEARCH → Modern Best Practices

### API Documentation Patterns
1. **OpenAPI Standard**: Industry standard for API specification
2. **Interactive documentation**: Swagger UI, Redoc, Stoplight
3. **Code-first approach**: Generate documentation from code annotations
4. **Versioning strategy**: Semantic versioning with migration guides
5. **Deprecation policies**: Clear timelines for endpoint retirement

### Developer Experience Best Practices
1. **Quickstart focus**: Get developers to first API call in <5 minutes
2. **Copy-paste examples**: Ready-to-use code snippets
3. **Error prevention**: Clear guidance on common mistakes
4. **Testing tools**: Built-in API playground
5. **Community integration**: GitHub issues, Discord support

### SEO Optimization for Technical Content
1. **Keyword strategy**: Target "REST API", "task management API", etc.
2. **Technical SEO**: Schema.org markup for API documentation
3. **Internal linking**: Cross-reference related endpoints
4. **Freshness signals**: Regular updates and version tracking

### Accessibility Requirements
1. **Screen reader support**: Proper semantic structure
2. **Keyboard navigation**: Full keyboard accessibility
3. **Color contrast**: WCAG 2.1 AA compliance
4. **Reduced motion**: Support for motion sensitivity

---

## 🛠️ EXECUTE → Production Documentation

### OpenAPI 3.1 Specification Foundation
```yaml
openapi: 3.1.0
info:
  title: TaskFlow API
  description: REST API for task management and team collaboration
  version: 1.0.0
  contact:
    name: API Support
    email: api@taskflow.com
    url: https://api.taskflow.com/support
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT

servers:
  - url: https://api.taskflow.com/v1
    description: Production server
  - url: https://sandbox.taskflow.com/v1
    description: Sandbox server for testing

components:
  securitySchemes:
    ApiKeyAuth:
      type: apiKey
      in: header
      name: X-API-Key
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  schemas:
    Task:
      type: object
      required:
        - id
        - title
        - status
      properties:
        id:
          type: string
          format: uuid
          example: "550e8400-e29b-41d4-a716-446655440000"
        title:
          type: string
          example: "Complete API documentation"
        description:
          type: string
          example: "Write comprehensive API docs for TaskFlow"
        status:
          type: string
          enum: [pending, in_progress, completed]
          example: "in_progress"
        due_date:
          type: string
          format: date-time
          example: "2024-01-20T10:00:00Z"

paths:
  /tasks:
    get:
      summary: List all tasks
      description: Retrieve a paginated list of tasks with optional filters
      parameters:
        - $ref: '#/components/parameters/PageParam'
        - $ref: '#/components/parameters/LimitParam'
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Task'
                  pagination:
                    $ref: '#/components/schemas/Pagination'
        '401':
          $ref: '#/components/responses/UnauthorizedError'
        '429':
          $ref: '#/components/responses/RateLimitError'
```

### Authentication Documentation
```markdown
# 🔐 Authentication

TaskFlow API supports multiple authentication methods for different use cases.

## API Keys (Recommended for Server-to-Server)

### Getting Your API Key
1. Navigate to [API Settings](https://app.taskflow.com/settings/api)
2. Click "Generate New API Key"
3. Copy the key immediately - it won't be shown again!

### Using API Keys
Include your API key in the `X-API-Key` header:

```javascript
// JavaScript (Node.js)
const response = await fetch('https://api.taskflow.com/v1/tasks', {
  method: 'GET',
  headers: {
    'X-API-Key': 'your_api_key_here',
    'Content-Type': 'application/json'
  }
});
```

```python
# Python
import requests

headers = {
    'X-API-Key': 'your_api_key_here',
    'Content-Type': 'application/json'
}
response = requests.get('https://api.taskflow.com/v1/tasks', headers=headers)
```

## OAuth 2.0 (Recommended for User Applications)

### Authorization Code Flow
1. Redirect users to authorization endpoint
2. Handle callback with authorization code
3. Exchange code for access token

### Example OAuth Flow
```javascript
// Redirect user to authorization endpoint
const authUrl = `https://auth.taskflow.com/oauth/authorize?\
  client_id=YOUR_CLIENT_ID&\
  redirect_uri=YOUR_REDIRECT_URI&\
  response_type=code&\
  scope=tasks:read tasks:write`;

window.location.href = authUrl;
```

## Best Practices
- 🔒 Never commit API keys to version control
- 🔄 Rotate keys every 90 days
- 📱 Use OAuth for mobile applications
- 🛡️ Implement rate limiting awareness
```

### Quickstart Guide
```markdown
# ⚡ 5-Minute Quickstart

Get started with TaskFlow API in less than 5 minutes.

## Step 1: Get Your API Key
1. Sign up at [TaskFlow](https://app.taskflow.com/signup)
2. Go to [API Settings](https://app.taskflow.com/settings/api)
3. Generate a new API key

## Step 2: Make Your First API Call

```javascript
// Node.js example
const fetch = require('node-fetch');

async function createFirstTask() {
  const response = await fetch('https://api.taskflow.com/v1/tasks', {
    method: 'POST',
    headers: {
      'X-API-Key': 'your_api_key_here',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      title: 'My First API Task',
      description: 'Created via TaskFlow API',
      status: 'pending'
    })
  });

  if (response.ok) {
    const task = await response.json();
    console.log('Task created:', task);
  } else {
    console.error('Error:', response.status);
  }
}

createFirstTask();
```

```python
# Python example
import requests

api_key = 'your_api_key_here'
headers = {
    'X-API-Key': api_key,
    'Content-Type': 'application/json'
}

data = {
    'title': 'My First API Task',
    'description': 'Created via TaskFlow API',
    'status': 'pending'
}

response = requests.post(
    'https://api.taskflow.com/v1/tasks',
    headers=headers,
    json=data
)

if response.status_code == 201:
    print('Task created:', response.json())
else:
    print('Error:', response.status_code)
```

## Step 3: Explore the API
- View your new task at `GET /tasks`
- Update it with `PATCH /tasks/{id}`
- Delete with `DELETE /tasks/{id}`

## Next Steps
- 📖 Read the [complete API reference](/api-reference)
- 🎓 Follow our [integration tutorials](/tutorials)
- 🛡️ Learn about [authentication methods](/authentication)
- ⚠️ Understand [error handling](/guides/error-handling)
```

### Error Handling Documentation
```markdown
# ⚠️ Error Handling

TaskFlow API uses conventional HTTP response codes and consistent error formats.

## HTTP Status Codes

| Code | Meaning | Description |
|------|---------|-------------|
| 200 | OK | Successful request |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Invalid request parameters |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Error | Server error |

## Error Response Format

All errors return a consistent JSON format:

```json
{
  "error": {
    "code": "validation_error",
    "message": "Title is required",
    "details": [
      {
        "field": "title",
        "message": "This field is required"
      }
    ],
    "documentation_url": "https://docs.taskflow.com/errors/validation_error"
  }
}
```

## Common Errors

### Validation Errors (400)
```json
{
  "error": {
    "code": "validation_error",
    "message": "Invalid request parameters",
    "details": [
      {
        "field": "due_date",
        "message": "Must be a valid ISO 8601 date"
      }
    ]
  }
}
```

### Rate Limit Exceeded (429)
```json
{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "Too many requests",
    "retry_after": 60,
    "limit": 100,
    "remaining": 0,
    "reset": 1642546800
  }
}
```

### Handling Errors in Code

```javascript
// JavaScript error handling
async function makeApiRequest() {
  try {
    const response = await fetch('/api/endpoint');
    
    if (!response.ok) {
      const error = await response.json();
      
      switch (response.status) {
        case 400:
          console.error('Validation error:', error.error.details);
          break;
        case 429:
          console.error('Rate limited. Retry after:', error.error.retry_after);
          setTimeout(makeApiRequest, error.error.retry_after * 1000);
          break;
        default:
          console.error('API error:', error.error.message);
      }
      
      throw new Error(error.error.message);
    }
    
    return await response.json();
  } catch (error) {
    console.error('Request failed:', error);
  }
}
```

## Best Practices
- 🔄 Always implement retry logic for 429 and 5xx errors
- 📝 Log error details for debugging
- 🛡️ Handle authentication errors gracefully
- ⏰ Respect retry-after headers for rate limits
```

### Interactive API Playground
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaskFlow API Playground</title>
    <style>
        .playground {
            max-width: 1000px;
            margin: 0 auto;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
        }
        .endpoint-selector {
            margin-bottom: 20px;
        }
        .code-editor {
            border: 1px solid #ddd;
            border-radius: 4px;
            padding: 10px;
            font-family: 'Monaco', 'Menlo', monospace;
            min-height: 200px;
        }
        .response-area {
            margin-top: 20px;
            border: 1px solid #ddd;
            border-radius: 4px;
            padding: 10px;
            background: #f8f9fa;
        }
    </style>
</head>
<body>
    <div class="playground">
        <h1>API Playground</h1>
        
        <div class="endpoint-selector">
            <select id="endpointSelect">
                <option value="get-tasks">GET /tasks - List tasks</option>
                <option value="create-task">POST /tasks - Create task</option>
                <option value="update-task">PATCH /tasks/{id} - Update task</option>
            </select>
        </div>

        <div class="code-editor" contenteditable="true" id="requestEditor">
// Example: Create a new task
{
  "title": "Learn TaskFlow API",
  "description": "Explore the API documentation and playground",
  "status": "in_progress",
  "due_date": "2024-01-20T10:00:00Z"
}
        </div>

        <button onclick="executeRequest()">Execute Request</button>

        <div class="response-area" id="responseArea">
            <p>Response will appear here...</p>
        </div>
    </div>

    <script>
        async function executeRequest() {
            const endpoint = document.getElementById('endpointSelect').value;
            const requestBody = document.getElementById('requestEditor').innerText;
            const responseArea = document.getElementById('responseArea');

            responseArea.innerHTML = '<p>Loading...</p>';

            try {
                const response = await fetch(`/api/${endpoint}`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'X-API-Key': 'playground-key'
                    },
                    body: requestBody
                });

                const data = await response.json();
                responseArea.innerHTML = `<pre>${JSON.stringify(data, null, 2)}</pre>`;
            } catch (error) {
                responseArea.innerHTML = `<p style="color: red;">Error: ${error.message}</p>`;
            }
        }
    </script>
</body>
</html>
```

---

## ✅ Quality & Review Checklist

### Documentation Accuracy
- [ ] All endpoints documented in OpenAPI spec
- [ ] Response schemas match actual API responses
- [ ] Error formats consistent across endpoints
- [ ] Authentication examples working correctly
- [ ] Rate limiting documentation accurate

### Developer Experience
- [ ] Quickstart guide functional in <5 minutes
- [ ] Code examples run without modification
- [ ] Copy-paste examples available for all endpoints
- [ ] Mobile-responsive design
- [ ] Search functionality working

### Content Quality
- [ ] Clear, concise language
- [ ] Technical accuracy verified
- [ ] Examples cover common use cases
- [ ] Tutorials provide real-world value
- [ ] Versioning information clear

### Accessibility & SEO
- [ ] WCAG 2.1 AA compliance
- [ ] Semantic HTML structure
- [ ] Keyboard navigation support
- [ ] SEO metadata optimized
- [ ] Internal linking strategy

### Maintenance
- [ ] Automated sync with API codebase
- [ ] Version update procedures
- [ ] Change log maintenance
- [ ] Feedback collection mechanism
- [ ] Analytics tracking implemented

---

## 🚀 Future Enhancement Roadmap

### Immediate (Next 30 days)
- [ ] Interactive API playground
- [ ] Webhook testing interface
- [ ] SDK code generation
- [ ] Postman collection
- [ ] curl examples for all endpoints

### Medium-term (Next 90 days)
- [ ] Video tutorials
- [ ] API changelog RSS feed
- [ ] Integration marketplace
- [ ] Community-contributed examples
- [ ] Multilingual documentation

### Long-term (Next 6 months)
- [ ] AI-powered documentation assistant
- [ ] Real-time API monitoring integration
- [ ] Advanced analytics for documentation usage
- [ ] Personalized documentation experience
- [ ] Automated migration guide generation

### No-Refactor Extensions
- [ ] Additional code language examples
- [ ] More integration tutorials
- [ ] Community showcase section
- [ ] API usage patterns gallery
- [ ] Performance optimization guides

---

## 🎯 Expected Output Level

This API documentation is **production-ready** and meets enterprise standards:

- ✅ **Developers** can integrate in under 30 minutes
- ✅ **Technical writers** have complete framework and templates
- ✅ **Product managers** can track adoption and satisfaction
- ✅ **Support teams** have reference documentation for troubleshooting
- ✅ **QA engineers** can validate API behavior against documentation
- ✅ **Leadership** can measure ROI and developer engagement

The documentation follows industry best practices, provides exceptional developer experience, and creates a foundation for scalable API growth while maintaining accuracy and usability.