# Building Your Framework

> **Stackpole Consulting AI Agent Best Practices**  
> Extend this foundation with your own innovations and domain-specific patterns

## Philosophy

This framework is **intentionally incomplete**. It provides a foundation, but your team will discover patterns, protocols, and best practices unique to your domain, technology, and culture.

**This guide teaches you how to:**
- Identify patterns worth formalizing
- Create custom protocols and templates
- Document tribal knowledge systematically
- Contribute back to the community

## Recognizing Patterns Worth Capturing

### **Signs You've Found a Pattern**

```markdown
✅ You've solved the same problem 3+ times
✅ New team members ask "how do we handle X?"
✅ Code reviews repeatedly catch the same issue
✅ You have a "we always do it this way" conversation
✅ An AI agent keeps making the same mistake

Example Discoveries:
- "We always validate API keys in the same way"
- "Every microservice needs the same health check structure"
- "Our error messages follow a specific format for the mobile app"
- "We have a standard way to handle rate limiting"
```

### **Anti-Pattern: Documentation Theater**
```markdown
❌ Don't Document These:
- One-off solutions that won't repeat
- Temporary workarounds
- Personal preferences without team consensus
- Technology-specific details that change frequently

✅ Do Document These:
- Patterns used in 3+ places
- Architecture decisions with lasting impact
- Team-agreed standards
- Hard-won lessons from production incidents
```

## Creating Custom Protocols

### **Protocol Template**
```markdown
# [Protocol Name]

> **[Your Organization] AI Agent Best Practices**  
> [One-line description of what this protocol solves]

## The Problem

[Describe the specific problem this protocol addresses]

**Real-World Impact:**
[Quantify the problem with data or examples]

## The Solution

[Describe your protocol/approach]

### **Decision Framework**
[Provide step-by-step guidance]

### **Implementation Guide**
[Show practical examples]

## AI Agent Integration

[How should AI agents apply this protocol?]

### **Example Prompts**
```

### **Example: Custom Rate Limiting Protocol**

```markdown
# API Rate Limiting Protocol

> **Acme Corp AI Agent Best Practices**  
> Consistent rate limiting across all microservices with graceful degradation

## The Problem

Our microservices ecosystem has 20+ APIs, each implementing rate limiting differently. This causes:
- Inconsistent client experiences
- Difficult troubleshooting
- Poor error messages
- No coordinated backoff strategy

**Real-World Impact:**
```
Q1 2025 Incident Analysis:
- 12 customer escalations due to unclear rate limit errors
- 3 production incidents from inconsistent retry logic
- 8 hours average developer time to implement rate limiting correctly
- 40% of new services initially deployed without rate limiting
```

## The Solution

**Standardized Rate Limiting Middleware** with three tiers:

### **Tier 1: Anonymous Users**
- 100 requests per hour
- Keyed by IP address
- Return 429 with Retry-After header

### **Tier 2: Authenticated Users**
- 1,000 requests per hour
- Keyed by user ID
- Return 429 with detailed error message

### **Tier 3: Premium/Partner APIs**
- 10,000 requests per hour
- Keyed by API key
- Return 429 with usage dashboard link

### **Implementation Pattern**

```typescript
// middleware/rateLimiter.ts
import { RateLimiterRedis } from 'rate-limiter-flexible';
import { Request, Response, NextFunction } from 'express';

interface RateLimitConfig {
  tier: 'anonymous' | 'authenticated' | 'premium';
  points: number;
  duration: number; // seconds
}

const TIER_CONFIGS: Record<string, RateLimitConfig> = {
  anonymous: { tier: 'anonymous', points: 100, duration: 3600 },
  authenticated: { tier: 'authenticated', points: 1000, duration: 3600 },
  premium: { tier: 'premium', points: 10000, duration: 3600 }
};

export class RateLimiter {
  private limiters: Map<string, RateLimiterRedis>;
  
  constructor(private redis: Redis) {
    this.limiters = new Map();
    
    // Initialize rate limiters for each tier
    Object.entries(TIER_CONFIGS).forEach(([tier, config]) => {
      this.limiters.set(tier, new RateLimiterRedis({
        storeClient: redis,
        keyPrefix: `rate_limit:${tier}`,
        points: config.points,
        duration: config.duration,
        blockDuration: 0 // Don't block, just return 429
      }));
    });
  }
  
  middleware(tier: keyof typeof TIER_CONFIGS = 'anonymous') {
    return async (req: Request, res: Response, next: NextFunction) => {
      const limiter = this.limiters.get(tier);
      if (!limiter) {
        return next();
      }
      
      // Determine rate limit key
      const key = this.getRateLimitKey(req, tier);
      
      try {
        const rateLimitResult = await limiter.consume(key, 1);
        
        // Add rate limit headers to response
        res.setHeader('X-RateLimit-Limit', TIER_CONFIGS[tier].points);
        res.setHeader('X-RateLimit-Remaining', rateLimitResult.remainingPoints);
        res.setHeader('X-RateLimit-Reset', new Date(Date.now() + rateLimitResult.msBeforeNext).toISOString());
        
        next();
      } catch (rejRes) {
        // Rate limit exceeded
        const retryAfter = Math.ceil(rejRes.msBeforeNext / 1000);
        
        res.setHeader('Retry-After', retryAfter);
        res.setHeader('X-RateLimit-Limit', TIER_CONFIGS[tier].points);
        res.setHeader('X-RateLimit-Remaining', 0);
        res.setHeader('X-RateLimit-Reset', new Date(Date.now() + rejRes.msBeforeNext).toISOString());
        
        res.status(429).json({
          error: 'rate_limit_exceeded',
          message: `Too many requests. Please try again in ${retryAfter} seconds.`,
          tier: tier,
          limit: TIER_CONFIGS[tier].points,
          retryAfter: retryAfter,
          documentation: 'https://docs.acme.com/rate-limits'
        });
      }
    };
  }
  
  private getRateLimitKey(req: Request, tier: string): string {
    switch (tier) {
      case 'anonymous':
        return req.ip || 'unknown';
      case 'authenticated':
        return req.user?.id || req.ip || 'unknown';
      case 'premium':
        return req.headers['x-api-key'] as string || req.user?.id || 'unknown';
      default:
        return 'unknown';
    }
  }
}

// Usage in routes
import express from 'express';
const app = express();
const rateLimiter = new RateLimiter(redisClient);

// Apply to all routes
app.use(rateLimiter.middleware('anonymous'));

// Override for authenticated routes
app.use('/api/v1/users', 
  authenticateUser, 
  rateLimiter.middleware('authenticated')
);

// Premium tier for partner APIs
app.use('/api/v1/partner',
  validateApiKey,
  rateLimiter.middleware('premium')
);
```

## AI Agent Integration

### **When to Apply This Protocol**
AI agents should apply this protocol when:
- Creating new API endpoints
- Reviewing existing endpoints without rate limiting
- Refactoring authentication middleware
- Implementing public-facing APIs

### **Example Prompts**

```markdown
"Add rate limiting to the /api/users endpoint following our rate limiting protocol.
This endpoint will be used by authenticated users, so use the authenticated tier."

"Review all endpoints in src/api/routes/ and identify which ones are missing 
rate limiting. Generate a checklist of endpoints to update."

"I'm getting rate limit errors in development. Can you check if the rate limiter
is configured correctly for local development? We should have higher limits locally."
```

### **Code Generation Guidelines**
When generating code involving APIs:
1. Always include rate limiting middleware
2. Default to 'authenticated' tier unless specified
3. Include rate limit headers in responses
4. Document rate limits in API response schemas
5. Add tests for rate limit behavior

## Monitoring & Alerts

### **Metrics to Track**
```typescript
// Add to monitoring dashboard
const rateLimitMetrics = {
  // How many requests are being rate limited?
  'rate_limit.exceeded': counter,
  
  // Which endpoints are hit hardest?
  'rate_limit.requests_by_endpoint': histogram,
  
  // Are users hitting limits frequently?
  'rate_limit.users_limited': gauge,
  
  // Distribution of tier usage
  'rate_limit.tier_usage': distribution
};

// Alert conditions
if (rateLimitExceededRate > 5%) {
  alert('High rate limit rejection rate - possible attack or limits too strict');
}

if (premiumTierUsage > 80%) {
  alert('Premium tier approaching limits - consider upsell conversation');
}
```

## Testing Strategy

```typescript
// tests/integration/rateLimiter.test.ts
describe('Rate Limiter Integration', () => {
  let app: Express;
  let redis: Redis;
  
  beforeEach(async () => {
    redis = await setupTestRedis();
    app = createTestApp(redis);
  });
  
  afterEach(async () => {
    await redis.flushall();
    await redis.quit();
  });
  
  it('should allow requests within limit', async () => {
    // Make 10 requests (well under 100 limit)
    for (let i = 0; i < 10; i++) {
      const res = await request(app).get('/api/public/data');
      expect(res.status).toBe(200);
      expect(res.headers['x-ratelimit-remaining']).toBeDefined();
    }
  });
  
  it('should reject requests exceeding limit', async () => {
    // Make 101 requests to exceed anonymous limit
    let lastResponse;
    for (let i = 0; i < 101; i++) {
      lastResponse = await request(app).get('/api/public/data');
    }
    
    expect(lastResponse.status).toBe(429);
    expect(lastResponse.body.error).toBe('rate_limit_exceeded');
    expect(lastResponse.headers['retry-after']).toBeDefined();
  });
  
  it('should use different limits for authenticated users', async () => {
    const authToken = await generateTestToken();
    
    // Make 150 requests (exceeds anonymous but within authenticated limit)
    let lastResponse;
    for (let i = 0; i < 150; i++) {
      lastResponse = await request(app)
        .get('/api/users/profile')
        .set('Authorization', `Bearer ${authToken}`);
    }
    
    expect(lastResponse.status).toBe(200);
    expect(parseInt(lastResponse.headers['x-ratelimit-remaining'])).toBeGreaterThan(0);
  });
});
```

---

**Protocol Version:** v1.0.0 (Acme Corp Internal)  
**Extends:** Stackpole Consulting AI Agent Best Practices  
**Owned by:** Platform Engineering Team  
**Last Updated:** 2025-10-23
```

## Creating Custom Templates

### **Template Structure**
```markdown
# [Template Name]

> **[Your Organization] [Template Category]**  
> [Purpose of this template]

## When to Use This Template

[Describe scenarios where this template applies]

## Template

```[language]
[Your template content with placeholders]
```

## Customization Points

[List the parts that need customization]

## Examples

[Show 2-3 real examples]

## Integration with AI Agents

[How should AI agents use this template?]
```

### **Example: Microservice Bootstrap Template**

```markdown
# Microservice Bootstrap Template

> **Acme Corp Service Templates**  
> Standard structure for new microservices in our ecosystem

## When to Use This Template

Use this template when creating a new microservice that:
- Exposes a REST API
- Needs to integrate with our service mesh
- Requires database access
- Must emit metrics and logs to our observability stack

## Template

```typescript
// src/index.ts
import express from 'express';
import { setupDatabase } from './config/database';
import { setupMonitoring } from './config/monitoring';
import { setupHealthChecks } from './config/health';
import { router } from './api/routes';

const PORT = process.env.PORT || 3000;
const SERVICE_NAME = process.env.SERVICE_NAME || '[SERVICE_NAME]';

async function bootstrap() {
  const app = express();
  
  // Middleware
  app.use(express.json());
  app.use(correlationIdMiddleware);
  app.use(loggingMiddleware);
  
  // Database connection
  await setupDatabase();
  
  // Monitoring & observability
  setupMonitoring(app, SERVICE_NAME);
  
  // Health checks (required for k8s)
  setupHealthChecks(app);
  
  // API routes
  app.use('/api/v1', router);
  
  // Error handling
  app.use(errorHandlerMiddleware);
  
  // Start server
  app.listen(PORT, () => {
    logger.info(`${SERVICE_NAME} listening on port ${PORT}`);
  });
}

bootstrap().catch(err => {
  logger.error('Failed to start service', err);
  process.exit(1);
});
```

## Customization Points

1. **[SERVICE_NAME]**: Replace with your service name (e.g., "user-service", "order-service")
2. **Database setup**: Choose PostgreSQL, MongoDB, or DynamoDB configuration
3. **API routes**: Define your domain-specific endpoints
4. **Environment variables**: Add service-specific configuration

## Examples

### Example 1: User Service
```typescript
const SERVICE_NAME = 'user-service';
// Database: PostgreSQL
// Routes: users, authentication, profiles
```

### Example 2: Order Service  
```typescript
const SERVICE_NAME = 'order-service';
// Database: PostgreSQL + Redis cache
// Routes: orders, cart, checkout
```

## Integration with AI Agents

### **Bootstrap Prompt**
```
"Create a new microservice called [name]-service using our microservice bootstrap template.

Requirements:
- PostgreSQL database
- CRUD operations for [entity]
- Standard health checks and monitoring
- Rate limiting on public endpoints

Follow our microservice template from templates/microservice-bootstrap.md"
```

### **Code Generation Rules**
1. Always include health check endpoints
2. Always add correlation ID middleware
3. Always configure structured logging
4. Always add Prometheus metrics endpoints
5. Always include graceful shutdown logic
```

## Documenting Domain Knowledge

### **Pattern: Domain Glossary**

```markdown
# Domain Glossary: E-commerce

> **Acme Corp Domain Knowledge**  
> Shared understanding of business concepts

## Core Entities

### Order
**Definition**: A customer's request to purchase products

**Lifecycle States**:
- `pending`: Order created, payment not yet processed
- `confirmed`: Payment successful, ready for fulfillment
- `shipped`: Package handed to carrier
- `delivered`: Customer received package
- `cancelled`: Order cancelled before shipment
- `refunded`: Money returned to customer

**Business Rules**:
- Orders can only be cancelled before shipment
- Refunds require manager approval for amounts > $500
- International orders require customs documentation
- Subscription orders auto-renew monthly

**Code Representation**:
```typescript
interface Order {
  id: string;
  customerId: string;
  status: OrderStatus;
  items: OrderItem[];
  totalAmount: Money;
  shippingAddress: Address;
  billingAddress: Address;
  createdAt: Date;
  updatedAt: Date;
}

type OrderStatus = 
  | 'pending' 
  | 'confirmed' 
  | 'shipped' 
  | 'delivered' 
  | 'cancelled' 
  | 'refunded';
```

### SKU (Stock Keeping Unit)
**Definition**: Unique identifier for a specific product variant

**Format**: `{CATEGORY}-{PRODUCT}-{VARIANT}`
- Example: `SHIRT-POLO-BLUE-M` (Blue polo shirt, medium)

**Business Rules**:
- SKUs are immutable once created
- Each SKU has independent inventory tracking
- Discontinued SKUs remain in system for historical orders

## Key Workflows

### Checkout Process
```
1. Cart Review
   ↓
2. Shipping Address Entry
   ↓
3. Shipping Method Selection
   ↓
4. Payment Information
   ↓
5. Order Review
   ↓
6. Payment Processing
   ↓
7. Order Confirmation
   ↓
8. Confirmation Email
```

**Failure Points**:
- Payment declined → Return to payment step, preserve cart
- Out of stock → Remove item, notify customer, recalculate
- Shipping unavailable → Offer alternative methods or store pickup

## AI Agent Integration

When AI agents work on e-commerce features, they should:
1. Reference this glossary for correct terminology
2. Follow established business rules
3. Use standard entity representations
4. Respect workflow stages

### Example Usage
```
"Implement order cancellation following our e-commerce domain glossary.
Remember: orders can only be cancelled before shipment, and we need to 
restore inventory when cancelling."
```
```

## Contributing Your Patterns Back

### **Why Contribute?**
- Help others avoid the same problems
- Get feedback on your approach
- Build your professional reputation
- Improve the broader AI agent ecosystem

### **What to Contribute**
```markdown
✅ Good Contributions:
- Language-agnostic patterns
- Solved common problems
- Documented with examples
- Tested in real projects

❌ Poor Contributions:
- Company-specific secrets
- Untested ideas
- Overly specific to your setup
- Duplicate existing patterns
```

### **Contribution Process**

1. **Anonymize Your Pattern**
```markdown
Before:
"At Acme Corp, we handle Stripe webhooks by storing them in our 
AcmeWebhookTable in DynamoDB..."

After:
"For payment webhook processing, consider storing webhook events in a 
persistent queue (e.g., database table, SQS) before processing to ensure
no events are lost during processing errors..."
```

2. **Generalize the Solution**
```markdown
Before:
"We use this specific Redis configuration for our traffic patterns..."

After:
"For high-throughput caching scenarios, consider these Redis configuration
trade-offs: [persistence vs speed], [memory vs disk], [clustering vs single-node]"
```

3. **Create Pull Request**
```bash
# Fork the repository
git clone https://github.com/Stackpole-Consulting/ai-agent-best-practices
cd ai-agent-best-practices

# Create feature branch
git checkout -b feature/webhook-processing-protocol

# Add your protocol
# Edit protocols/webhook-processing.md

# Commit and push
git commit -m "Add webhook processing protocol

- Persistent queue pattern for webhook reliability
- Error handling and retry strategies  
- Idempotency key implementation
- Monitoring and alerting guidelines"

git push origin feature/webhook-processing-protocol

# Create pull request on GitHub
```

## Framework Evolution

### **Version Your Innovations**
```markdown
# My Custom Protocol

**Version:** v1.0.0  
**Created:** 2025-10-23  
**Last Updated:** 2025-10-23  
**Status:** Active  

## Changelog

### v1.1.0 (2025-11-15)
- Added support for async processing
- Improved error messages
- Added retry mechanism

### v1.0.0 (2025-10-23)
- Initial version
- Basic synchronous processing
```

### **Deprecation Strategy**
```markdown
# Old Pattern (DEPRECATED)

⚠️ **DEPRECATED as of 2025-11-01**  
**Replaced by:** protocols/new-improved-pattern.md  
**Reason:** Performance issues at scale, better alternative available  

## Migration Guide
[Step-by-step guide to migrate to new pattern]

## Supported Until
This pattern will be supported until 2026-01-01.
```

## Success Metrics

Track the effectiveness of your custom framework:

```markdown
### Adoption Metrics
- Number of services using the pattern
- Time to implement (before vs after template)
- Code review comments related to the pattern (should decrease)

### Quality Metrics
- Bugs related to the pattern (should decrease)
- Consistency across codebase (should increase)
- Time for new developers to understand pattern (should decrease)

### AI Agent Metrics
- Successful first-try implementations
- Pattern compliance in generated code
- Reduction in correction iterations
```

## Next Steps

1. **Identify Your First Pattern**: Review recent code reviews for repeated feedback
2. **Document It**: Use the templates in this guide
3. **Share Internally**: Get team feedback
4. **Iterate**: Improve based on real usage
5. **Consider Contributing**: Help the broader community

---

**Remember**: The best frameworks are living documents that evolve with your team's needs.

---

**Guide Version:** v1.0.0 (Stackpole Consulting AI Agent Best Practices)  
**Philosophy:** Build your unique patterns on proven foundations  
**Next:** Review `guides/working-with-agents.md` for practical AI collaboration techniques