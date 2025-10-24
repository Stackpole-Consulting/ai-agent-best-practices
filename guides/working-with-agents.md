# Working with AI Agents

> **Stackpole Consulting AI Agent Best Practices**  
> Practical techniques for effective collaboration with GitHub Copilot, Claude, Cursor, and other AI coding assistants

## The Mental Model

### **AI Agents as Pair Programming Partners**

Think of AI agents like an **exceptionally skilled but slightly forgetful junior developer**:
- ✅ Excellent at pattern recognition and code generation
- ✅ Fast at writing boilerplate and standard implementations  
- ✅ Great at applying consistent styles and conventions
- ❌ No memory of your project unless you provide context
- ❌ Can't infer unstated requirements or business rules
- ❌ Doesn't know which parts of your code are "sacred"

**Key Insight:** The quality of AI output is directly proportional to the quality of context you provide.

## Prompting Strategies

### **The Context Pyramid**
```
           Specific Request
          /                \
         /  Implementation  \
        /      Details       \
       /                      \
      /    Business Rules      \
     /                          \
    /    Project Architecture    \
   /                              \
  /        Domain Knowledge        \
 /________________________________\
        Technology Stack
```

Always work from the bottom up. The AI needs foundational context before it can handle specific requests.

### **Bad Prompt vs Good Prompt**

#### **❌ Bad: Vague and Context-Free**
```
"Add user authentication"
```

**Problems:**
- What kind of authentication? (JWT, sessions, OAuth?)
- What's the tech stack?
- What's the existing architecture?
- What are the security requirements?

**Result:** Generic boilerplate that doesn't fit your project

#### **✅ Good: Specific with Context**
```
"Add JWT-based user authentication to our Express.js API following our existing patterns:

CONTEXT:
- We use TypeScript with strict type checking
- Database: PostgreSQL with TypeORM
- Current auth: Simple API key (src/middleware/apiKey.ts)
- User model: src/models/User.ts

REQUIREMENTS:
- Login endpoint: POST /api/auth/login (email, password)
- Returns: JWT token with 24h expiration
- Store refresh tokens in database
- Use bcrypt for password hashing (already in package.json)
- Follow our error handling pattern (src/middleware/errorHandler.ts)

CONSTRAINTS:
- Don't modify existing endpoints yet
- Add tests following patterns in tests/integration/
- Use our logging utility (src/utils/logger.ts)

OUTPUT NEEDED:
1. Authentication service (src/services/AuthService.ts)
2. Auth middleware (src/middleware/auth.ts)
3. Login route (src/routes/auth.ts)
4. Integration tests (tests/integration/auth.test.ts)"
```

**Result:** Code that fits your architecture and follows your conventions

### **Prompt Templates for Common Tasks**

#### **Template 1: Feature Implementation**
```markdown
"Implement [FEATURE_NAME] with the following requirements:

CONTEXT:
- Tech stack: [list technologies]
- Relevant files: [list files that should inform the implementation]
- Existing patterns: [describe patterns to follow]

REQUIREMENTS:
- [Functional requirement 1]
- [Functional requirement 2]
- [Non-functional requirement: performance, security, etc.]

CONSTRAINTS:
- [What NOT to change]
- [Performance requirements]
- [Dependencies to avoid]

ACCEPTANCE CRITERIA:
- [How to verify it works]
- [Edge cases to handle]
- [Error conditions to test]

OUTPUT:
- [List specific files/functions needed]"
```

#### **Template 2: Debugging**
```markdown
"I'm seeing [ERROR/BEHAVIOR]. Here's the context:

EXPECTED BEHAVIOR:
[What should happen]

ACTUAL BEHAVIOR:
[What is happening]

RELEVANT CODE:
```[language]
[paste code snippet]
```

ERROR MESSAGES:
```
[paste error messages or logs]
```

ENVIRONMENT:
- [Runtime details: Node version, Python version, etc.]
- [Dependencies and versions]
- [Configuration details]

WHAT I'VE TRIED:
- [Attempt 1]
- [Attempt 2]

Please help me:
1. Identify the root cause
2. Suggest a fix
3. Explain why this happened"
```

#### **Template 3: Code Review**
```markdown
"Review this code for [SPECIFIC_CONCERNS]:

```[language]
[paste code]
```

REVIEW CHECKLIST:
- [ ] Follows our coding standards (reference: .copilot-instructions.md)
- [ ] Handles error cases properly
- [ ] Has adequate test coverage
- [ ] Performance considerations addressed
- [ ] Security vulnerabilities
- [ ] Edge cases handled

PROJECT CONTEXT:
- This code handles [critical/non-critical] functionality
- Expected load: [traffic/usage patterns]
- Security requirements: [authentication, authorization, data protection]

Please provide:
1. Issues found (critical vs nice-to-have)
2. Suggested improvements
3. Alternative approaches to consider"
```

## Agent-Specific Techniques

### **GitHub Copilot**

#### **Inline Suggestions: Comment-Driven Development**
```typescript
// Copilot excels at generating code from detailed comments

// Create a user validation function that:
// - Accepts email and password
// - Validates email format using regex
// - Ensures password is at least 8 characters
// - Checks password has uppercase, lowercase, and number
// - Returns validation errors as array of strings
function validateUser(email: string, password: string): string[] {
  // Copilot will generate implementation based on comments above
}
```

#### **Multi-line Completions: Pattern Recognition**
```typescript
// After implementing one endpoint, Copilot learns the pattern
app.post('/api/users', async (req, res) => {
  try {
    const { email, password } = req.body;
    const user = await userService.create({ email, password });
    res.status(201).json({ user });
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Start typing the next endpoint and Copilot suggests similar structure
app.post('/api/products', async (req, res) => {
  // Copilot suggests try-catch with same error handling pattern
});
```

#### **Copilot Chat: Iterative Refinement**
```markdown
// First pass
You: "Create a function to calculate order total"
Copilot: [generates basic calculation]

// Refine with specifics
You: "Add tax calculation based on shipping address. 
Tax rates: CA=9.5%, NY=8.875%, TX=8.25%, other=0%"
Copilot: [adds tax logic]

// Add edge cases
You: "Handle case where shipping address is null (use billing address).
Also add validation that at least one address exists."
Copilot: [adds validation and fallback logic]
```

### **Claude / Claude in Cursor**

#### **Leverage Long Context Window**
```markdown
"I'm going to share multiple files from our codebase. Please review them to understand our patterns before implementing the new feature.

[Share 3-5 related files]

Now, implement [feature] following the patterns you see in these files."
```

#### **Iterative Architecture Discussions**
```markdown
You: "I need to add real-time notifications to our app. Current stack:
- React frontend
- Express.js backend  
- PostgreSQL database
- Currently REST API only

What are the trade-offs between WebSockets, Server-Sent Events, and polling?"

Claude: [Provides analysis with pros/cons]

You: "We expect 10,000 concurrent users with notifications every 2-5 minutes.
Which approach would you recommend and why?"

Claude: [Recommends specific approach with reasoning]

You: "Great. Show me how to implement Server-Sent Events in our Express app,
following the patterns in [paste existing route code]"

Claude: [Generates implementation matching your patterns]
```

#### **Explaining Complex Code**
```markdown
"Explain this code to me like I'm new to the team:

```typescript
[paste complex code]
```

Include:
1. High-level purpose
2. Step-by-step walkthrough
3. Why certain decisions were made
4. Potential edge cases or gotchas
5. How it fits into our architecture"
```

### **Cursor**

#### **Codebase-Aware Refactoring**
```markdown
// Cursor indexes your codebase for better context

"Refactor UserService to use dependency injection. I can see we're already 
using DI in ProductService - follow that same pattern."

// Cursor will find ProductService and apply the same pattern
```

#### **Multi-File Edits**
```markdown
"I need to rename the 'status' field to 'orderStatus' across the codebase.

Files likely affected:
- src/models/Order.ts (type definition)
- src/services/OrderService.ts (business logic)
- src/api/routes/orders.ts (API endpoints)
- tests/unit/OrderService.test.ts (tests)

Please:
1. List all places where this field is used
2. Show me the changes needed in each file
3. Update database migration to rename column"
```

## Advanced Collaboration Patterns

### **Pattern 1: Rubber Duck Debugging**
```markdown
"I'm trying to understand why this function is slow. Let me explain my logic:

1. We fetch all users from database
2. For each user, we fetch their orders
3. For each order, we calculate total
4. Return aggregated results

[paste code]

Talk me through this - where might the performance bottleneck be?"
```

**Why This Works:** Explaining the problem helps you think through it, and the AI can spot the N+1 query problem you missed.

### **Pattern 2: Test-First Development**
```markdown
"Before we implement the feature, let's write tests that define the behavior:

Feature: User can update their profile

Tests needed:
1. Successfully update valid profile data
2. Reject invalid email format
3. Prevent updating another user's profile
4. Return 404 for non-existent user
5. Validate required fields present

Write these tests first, then I'll ask you to implement the code to make them pass."
```

### **Pattern 3: Progressive Enhancement**
```markdown
// Start simple
"Create a basic CRUD API for products with GET/POST/PUT/DELETE endpoints"

// Add validation
"Add Joi validation to the product endpoints:
- name: required, min 3 chars, max 100 chars
- price: required, positive number, max 2 decimals
- sku: required, alphanumeric, unique"

// Add business logic
"Add inventory management:
- Track stock quantity
- Prevent orders when out of stock
- Send low inventory alerts when < 10 items"

// Add optimization
"Add Redis caching for product listings:
- Cache for 5 minutes
- Invalidate on product updates
- Use cache-aside pattern"
```

### **Pattern 4: Code Archaeology**
```markdown
"I found this code and I'm not sure what it does or if it's still needed:

```typescript
[paste mysterious code]
```

Can you help me:
1. Understand what this code does
2. Find where it's being called from
3. Determine if it's safe to remove
4. Suggest tests if we need to keep it"
```

## Debugging with AI Agents

### **Effective Bug Reports**

#### **❌ Ineffective Bug Report**
```
"It's broken. Help!"
```

#### **✅ Effective Bug Report**
```markdown
"I'm seeing unexpected behavior in the user registration flow.

EXPECTED:
User registers → Email sent → User redirected to dashboard

ACTUAL:
User registers → No email sent → 500 error → User sees error page

ERROR MESSAGE:
```
Error: Connection refused to SMTP server at smtp.gmail.com:587
    at SMTPConnection._formatError (/node_modules/nodemailer/lib/smtp-connection/index.js:784:19)
    at SMTPConnection._actionEHLO (/node_modules/nodemailer/lib/smtp-connection/index.js:1348:34)
```

RELEVANT CODE:
```typescript
// src/services/EmailService.ts
async sendWelcomeEmail(email: string) {
  const transporter = nodemailer.createTransport({
    host: process.env.SMTP_HOST,
    port: parseInt(process.env.SMTP_PORT),
    secure: false,
    auth: {
      user: process.env.SMTP_USER,
      pass: process.env.SMTP_PASS
    }
  });
  
  await transporter.sendMail({
    from: 'noreply@example.com',
    to: email,
    subject: 'Welcome!',
    html: '<p>Welcome to our platform!</p>'
  });
}
```

ENVIRONMENT:
- Node.js v18.16.0
- nodemailer v6.9.1
- Running in Docker container
- SMTP_HOST=smtp.gmail.com
- SMTP_PORT=587

WHAT I'VE CHECKED:
- Environment variables are set correctly
- Credentials are valid (tested with Gmail directly)
- Port 587 is not blocked (can telnet to smtp.gmail.com:587)

Question: Is this a Docker networking issue? How can I debug SMTP connections from inside the container?"
```

### **Performance Debugging**
```markdown
"This endpoint is slow (>2 seconds average). Help me optimize:

ENDPOINT: GET /api/users/{id}/dashboard

CURRENT IMPLEMENTATION:
```typescript
async getDashboard(userId: string) {
  const user = await User.findById(userId);
  const orders = await Order.find({ userId });
  const products = await Product.find({ 
    id: { $in: orders.flatMap(o => o.items.map(i => i.productId)) }
  });
  const reviews = await Review.find({ userId });
  
  return {
    user,
    orderCount: orders.length,
    totalSpent: orders.reduce((sum, o) => sum + o.total, 0),
    recentOrders: orders.slice(0, 5),
    reviewedProducts: products.filter(p => 
      reviews.some(r => r.productId === p.id)
    )
  };
}
```

PERFORMANCE DATA:
- Database query time: 1.8s
- Data processing time: 0.3s
- Total time: 2.1s

DATABASE:
- PostgreSQL 14
- Users table: 50,000 rows
- Orders table: 500,000 rows (indexed on userId)
- Products table: 10,000 rows
- Reviews table: 100,000 rows (indexed on userId)

Questions:
1. What's causing the slowness?
2. How can I optimize the database queries?
3. Should I cache this data?
4. Are there any obvious N+1 query problems?"
```

## Handling AI Mistakes

### **When AI Gets It Wrong**

AI agents will make mistakes. Here's how to handle them effectively:

#### **Pattern: Specific Correction**
```markdown
❌ Don't: "That's wrong. Do it again."

✅ Do: "This implementation has an issue - it doesn't handle the case where 
the user is already logged in. According to our business rules, if a logged-in
user tries to login again, we should:
1. Return their existing session token
2. Update the lastLoginAt timestamp
3. Return 200 (not 201)

Please update the code to handle this scenario."
```

#### **Pattern: Constrain the Scope**
```markdown
❌ Don't: "Rewrite the entire authentication system"

✅ Do: "I only need you to modify the login function to add the duplicate
login check. Don't change the token generation logic, password hashing,
or database queries - those are working correctly. Just add the check
at the beginning of the function."
```

#### **Pattern: Provide Counter-Example**
```markdown
"The code you generated doesn't handle this edge case:

Input: email = 'user@example.com  ' (note trailing spaces)
Expected: Trim whitespace and process normally
Actual: Validation fails with 'invalid email'

Please update the validation to trim whitespace before checking email format."
```

### **Recognizing Hallucinations**

AI agents sometimes "hallucinate" - generating plausible-sounding but incorrect information.

#### **Warning Signs:**
```markdown
🚩 References to non-existent libraries or methods
🚩 Overly confident about unverified assumptions
🚩 Code that violates fundamental language rules
🚩 Security patterns that seem too simple
🚩 Performance claims without benchmarks
```

#### **Verification Strategy:**
```markdown
1. **For library methods:** Check official documentation
2. **For patterns:** Search GitHub for real usage examples  
3. **For security:** Consult OWASP or security-specific resources
4. **For performance:** Benchmark the code locally
5. **When in doubt:** Ask the AI to explain its reasoning
```

## Maximizing Productivity

### **Keyboard Shortcuts & Workflows**

#### **GitHub Copilot**
```
Tab           - Accept suggestion
Esc           - Dismiss suggestion
Alt+]         - Next suggestion
Alt+[         - Previous suggestion
Ctrl+Enter    - Open Copilot completions panel
```

#### **Efficient Workflow**
```markdown
1. Write descriptive comment explaining intent
2. Press Enter and let Copilot suggest implementation
3. Review suggestion (don't blindly accept!)
4. Tab to accept or Alt+] to see alternatives
5. Test the code immediately
6. Iterate if needed
```

### **Batch Processing Similar Tasks**

```markdown
"I need to create CRUD endpoints for 3 entities: Products, Categories, Tags.

All should follow this pattern:
- GET /api/{entity}           - List all
- GET /api/{entity}/:id       - Get one
- POST /api/{entity}          - Create
- PUT /api/{entity}/:id       - Update
- DELETE /api/{entity}/:id    - Delete

Start with Products (I'll paste the model), then I'll ask you to repeat
for Categories and Tags following the exact same pattern."
```

**Why This Works:** After seeing one complete example, the AI can replicate the pattern with high accuracy.

### **Documentation Generation**

```markdown
"Generate comprehensive JSDoc comments for this function:

```typescript
async function processPayment(orderId: string, paymentMethod: PaymentMethod, amount: number) {
  // [implementation]
}
```

Include:
- Purpose description
- Parameter descriptions with types
- Return value description
- Possible errors thrown
- Usage example
- Side effects (database updates, external API calls, etc.)"
```

## Team Collaboration with AI

### **Sharing Context Between Team Members**

```markdown
# .copilot-instructions.md

## Team-Specific Patterns

### Payment Processing
When implementing payment features, always:
1. Use Stripe test mode in development
2. Implement idempotency keys for all charges
3. Store webhook events before processing
4. Follow the pattern in src/services/PaymentService.ts

### Error Handling  
Follow our 3-tier error handling:
1. Service layer: Throw typed errors (ValidationError, NotFoundError, etc.)
2. Controller layer: Catch and convert to HTTP status codes
3. Middleware: Log errors and format responses

See: src/middleware/errorHandler.ts

[Team members can reference these shared patterns]
```

### **Code Review with AI**

```markdown
"Review this pull request for common issues:

[Paste diff or changed files]

Check for:
- Violations of our coding standards (.copilot-instructions.md)
- Missing error handling
- Potential security vulnerabilities
- Performance concerns
- Test coverage gaps
- Breaking changes to existing APIs

Provide specific line-by-line feedback."
```

## Common Pitfalls

### **Pitfall 1: Over-Reliance**
```markdown
❌ Problem: Accepting all suggestions without review
✅ Solution: Always review, test, and understand generated code

// Ask yourself:
- Do I understand what this code does?
- Are there edge cases not handled?
- Is this the right approach for our use case?
```

### **Pitfall 2: Insufficient Context**
```markdown
❌ Problem: "Add error handling"
✅ Solution: "Add error handling following our pattern in src/middleware/errorHandler.ts - 
use custom error classes and return consistent JSON error responses"
```

### **Pitfall 3: Ignoring Security**
```markdown
❌ Problem: Trusting AI-generated auth/security code without verification
✅ Solution: Always have security-critical code reviewed by a human expert

// Security checklist for AI-generated code:
- Input validation present?
- SQL injection prevention?
- XSS prevention?
- Authentication/authorization checks?
- Sensitive data properly encrypted?
- Secrets not hardcoded?
```

### **Pitfall 4: Not Testing**
```markdown
❌ Problem: Deploying AI-generated code without testing
✅ Solution: Test everything, especially edge cases

// Even if AI writes the tests, verify they actually test the right things
```

## Success Metrics

Track your AI collaboration effectiveness:

```markdown
### Code Quality
- First-attempt success rate (code works without modification)
- Time spent on corrections vs new code
- Bug rate in AI-assisted vs manual code

### Productivity
- Time to implement features (before/after AI)
- Lines of boilerplate written manually (should approach zero)
- Time spent on documentation (should decrease)

### Learning
- New patterns discovered through AI suggestions
- Time to onboard new technologies (should decrease)
- Understanding of generated code (should always be 100%)
```

---

**Remember:** AI agents are powerful tools, but they're tools that require skill to use effectively. Invest time in learning to collaborate with them, and you'll see massive productivity gains.

---

**Guide Version:** v1.0.0 (Stackpole Consulting AI Agent Best Practices)  
**Philosophy:** Clear context yields clear code  
**Next:** Review `guides/real-world-lessons.md` for case studies and practical insights